# A Context-Aware Kernel IPC Firewall for Android

# Abstract

BinderFilter is an open source Linux kernel message firewall for Android. It is written as a Linux kernel driver that implements parsing, logging, blocking, and modifying Android IPC messages. Android's Binder IPC system completely mediates all inter-application messages, including requests by applications for private user data. We give users control and visibility over all such IPC messages, including userland filtering, blocking, and logging of any IPC message in Android. Userland policy can be informed by the system's context, i.e. environmental data such as GPS location and wifi network, which addresses the current lack of native Android support for context-based security policies. BinderFilter can transparently modify Android IPC messages in real time, unbeknown to userland applications. We also expose messages to the command line to reveal information such as Intent and system sensor data that can be useful for Android system research. Links to the github code, technical report, and presentation slides can be found at binderfilter.org. 


# Binder

Binder has been extensively documented [here](https://github.com/dxwu/BinderFilter/tree/master/documentation/binder-code-analysis), [here](http://www.cs.dartmouth.edu/~sergey/binderfilter/Summercon2016-slides.pdf), and [here](http://binderfilter.org/binderfilter_tr.pdf). Please see the [slides](http://www.cs.dartmouth.edu/~sergey/binderfilter/Shmoocon2017-slides.pdf) for additional visualizations.

Communication between sandboxed applications is done via Binder inter-process communication (IPC). Binder replaces
Linux’s own IPC system in Android and enables uniquely identifying security tokens, death notifications, and (intrapackage) RPC. Intents, Messengers, and ContentProviders are all built on Binder. 

Binder is implemented as a Linux kernel driver (`/dev/binder`), which is exposed to userland processes using the `ioctl()`syscall. The Binder driver is also responsible for copying data between sandboxed user processes, including data buffers, file descriptors, and death notifications. The Binder driver `ioctl()` call takes as a parameter `binder_write_read`, which contains information about driver buffer consumption and pointers to marshaled user transaction data. The `write_buffer` and `read_buffer` fields point to `binder_transaction_data` objects. Those contain sender pid, receiver pid, uid information, and pointers to data buffers and offsets. Specifically, the `data.ptr.offsets` field points to `flat_binder_object` objects. Finally, `flat_binder_object` contains extra information such as file descriptors.

# BinderFilter

[BinderFilter](https://github.com/dxwu/BinderFilter) can be broken up into three distinct parts: the kernel code, the middleware, and the interactive code. The Kernel code includes the hook of Android's existing Binder Kernel driver and our modification code which intercepts and processes Android IPC messages. The middleware allows user space code to communicate with our kernel module through system calls and lives on the Android device itself. Finally, a command-line interface drives BinderFilter. Picky, an Android application which has all the functionality of the command-line tools, in addition to a simple, usable user interface, can be found at https://github.com/dxwu/picky. 

![](https://github.com/dxwu/BinderFilter/raw/master/documentation/bf_hook.png?raw=true) 

## The Kernel Module

BinderFilter is our hooking implementation of Binder. Compiled as a static kernel driver, the filter steals Binder messages and modifies them based on our IPC firewall policy. Being in the Binder allows us to have complete access to all IPC messages and gather context information directly from sensor hardware.

We hook binder.c in [one location](http://androidxref.com/kernel_3.18/xref/drivers/staging/android/binder.c#1520). At this point in a Binder call, the driver has just validated user buffer data and copied it into kernel address space, but has yet to act on it. Here we can steal the buffer and modify it in the kernel if needed. This hook gives our code full access to IPC message data, including buffer contents, sender and receiver UID, and any file descriptors being passed around. This is what lets us parse, log, and modify IPC message data in the kernel. BinderFilter analyzes Binder IPC message content examples much like network packets. The blocking implementation looks at Binder message string literals and wipes buffer contents if the message and context match our firewall policy.

BinderFilter applies all current policy rules to every Binder IPC message before passing it back to Android Binder's control. Policy rules may block (memset to zeros) or modify (memcpy user-specified data) messages before handing back control. These rules are stored in a file on the device to persist across boots.

## The Middleware

To facilitate user space - kernel space communication, a small module must be placed on the Android device to let external programs talk to the kernel module. In [Picky](https://github.com/dxwu/picky), the Android NDK provides JNI functionality that lets Android applications call C++ middleware that can in turn use C system calls into the kernel layer. With the command line tools, the middleware must be cross-compiled for Android and moved onto a privileged location within the device to be executed.

## The Interactive Interface

This is written in Python for ease of development, and gives a user an easy way to print various Binder logs, IPC message contents, and to read and set BinderFilter policy. It talks with the Middleware to set BinderFilter policy and reads BinderFilter data directly from the Kernel with `adb`.

# Functionality

Below are examples of some things you can do with BinderFilter.

Block all messages that contain the string "install" from the play store.

`./binderfilter.py -s -m "install" -u 10018 -a 1`


Block the Camera permission from the facebook messenger app.

`./binderfilter.py -s -m "android.permission.CAMERA" -u 10084 -a 1`


Block the Camera permission from the facebook app if wifi ssid is equal to "insecure-wifi-hotspot".

`./binderfilter.py -s -m "android.permission.CAMERA" -u 10084 -a 1 --context 2 --context-type 2 --context-value "insecure-wifi-hotspot"`


Remove a policy rule that blocks the Camera permission from the facebook messenger app.

`./binderfilter.py -s -m "android.permission.CAMERA" -u 10084 -a 2`

Modify any string sent to/from the spotify app that contains "spotify" with the string "awesomemusicapp". Note the "binderfilter.arbitrary." that prepends the filter message. Also note that the replacement string will be copied into the IPC buffer in memory for AT MOST the number of bytes of the original string".

`./binderfilter.py -s -m "binderfilter.arbitrary.spotify" -u 10082 --modify-data "awesomemusicapp"`

Print Android Kernel IPC buffers.

`./binderfilter.py --print-ipc-buffers-once`


#### Metadata

* **Primary Author Name**: David Wu
* **Primary Author Affiliation**: Dartmouth College
* **Primary Author Email**: DavidXiaohanWu@gmail.com
* **Additional Author Name**: Sergey Bratus
* **Additional Author Affiliation**: Dartmouth College
* **Additional Author Email**: sergey@cs.dartmouth.edu