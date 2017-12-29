# WaveConverter

### An Open Source Tool for RF Reverse Engineering

# Abstract

WaveConverter is a tool that extracts digital data from RF transmissions that have been captured via Software Defined Radio (SDR). You can think of it as a virtual receiver, in which you define the modulation parameters, framing, encoding and error correction. Armed with this protocol information, WaveConverter can then process input I-Q files and extract the payload data from any transmissions contained within. WaveConverter then displays statistical information on this payload data, allowing you to uncover the function of each payload bit.
WaveConverter makes the process of reverse engineering signals easier and more error-proof. Because WaveConverter includes the ability to store and retrieve signal protocols (the combination of modulation and encoding parameters), we have begun generating a database of protocols that we can use to iteratively attack unknown signals.

# What WaveConverter Is and Isn't

WaveConverter is designed for reverse engineering relatively simple devices such as keyfobs, tire pressure monitor (TPM) sensors, fan controllers and the like. Such devices typically use OOK or FSK modulations and reasonably simple encoding schemes. WaveConverter is not built to handle higher order modulations like PSK-4 (or higher) or hybrid modulations like QAM.

# Why I Built It

In 2015, I had been working on a series of automotive security projects, focusing on keyfobs and TPM sensors. These projects required me to reverse engineer a large number of similar transmissions. At first, I accumulated a set of gnuradio flowgraphs (for demodulation), each with a corresponding python script (for extracting the payload from the demodulated signal). Before long my flowgraphs and scripts multiplied, and it grew increasingly difficult to keep track of them all.

Next, I built a command line tool that accessed a set of configuration text files containing the protocol definitions. For a while, this kept the chaos under control. Eventually, though, I realized that managing the manually edited configuration file was getting unwieldy as well. At that point, I decided to build a more formal tool that collected all the aspects of reverse engineering under one roof.

# How It Works

WaveConverter breaks down the RF reverse engineering process into five discrete tasks. WaveConverter's UI contains five tabs, each containing the controls for one of those tasks.

For example, you'll start on the *RF Demod* tab, where you enter the modulation type, frequency and other parameters necessary to demodulate the input IQ file. After correctly setting up this tab and clicking the "Demod" button, you'll see the underlying digital waveform.

![](imgs/s6_id_043.png)

Next, you'll move to the *Framing* and *Payload Decode* tabs to extract the binary data from this waveform. Because WaveConverter shows you the digital waveform, you don't need to use Audacity or any other waveform viewing tool to observe the signal timings. 

![](imgs/s6_id_045.png)

Finally, you can move on to the "Payload Stats" tab to view individual bit probabilities and other useful statistical information. This will help you work out the function of each of the payload bits.

At any point in this process, you can save all of the parameters you've entered as a *protocol* and retrieve them later.

Please see the User Guide for a detailed description of how to operate WaveConverter.

# Why You Should Use It

WaveConverter provides several advantages over other reverse engineering processes.

* If you are new to RF reverse engineering the tabbed interface will guide you through the steps in the process, providing you with key feedback at each stage so you know whether you're on track.

* If you have a large number of devices and protocols that you're working with, WaveConverter can keep them straight by allowing you to save and retrieve signal definitions from the *Protocol Library*. 

* WaveConverter includes a programmable glitch filter, which allows it to extract data from noisy signals that would otherwise not be recoverable.

* Many simple Python scripts for decoding digital waveforms do not handle non-uniform or asymmetric timing. Because WaveConverter uses arbitrary timing definitions, it can easily handle preambles with highly irregular timing as well as non-uniform symbol durations like those used in Pulse Interval Encoding (PIE).

# A Tricky Thing I Learned

WaveConverter uses a gnuradio flowgraph to demodulate the input I-Q file and then passes the demodulated waveform to a set of functions that deframe and decode the digital waveform. At first, I simply sent the digital waveform to a File Sink and then had the decoding functions read the file back in. Given the inefficiency of this process, I set out to directly hand off data from the flowgraph to the rest of the Python code. 

This turned out to be a major challenge, as I could not find any clear documentation or examples of how to do this. After much trial and error, I was able to use a Message Sink in the flowgraph to pass the waveform data as a list of binary-valued strings. The code below shows how this can be done:

```
# create flowgraph object and execute
flowgraphObject = ook_flowgraph()
flowgraphObject.run()

# get message queue object from flowgraph
queue = flowgraphObject.sink_queue

# run through each message in the queue, pulling out each byte
basebandList = []
for n in xrange(queue.count()):
    messageSPtr = queue.delete_head() # remove the front-most message
    messageString = messageSPtr.to_string() # convert message to a string
    # for each character in the string, get binary 1 or 0
    for m in xrange(len(messageString)):
        if messageString[m] == b'\x00':
            basebandList.append(0)
        elif messageString[m] == b'\x01':
            basebandList.append(1)
        else:
             print "Fatal Error: flowgraph output not binary"
             exit(1)
             
return basebandList
```

# Conclusion

WaveConverter has helped me a lot, and I hope it makes life easier for you as well. You can get the code at:

[https://github.com/paulgclark/waveconverter](https://github.com/paulgclark/waveconverter)

If you do develop some protocol definitions, please export them to a text file and attach the file to a new issue in GitHub. I'll add your protocol to the database and include it in the next release.