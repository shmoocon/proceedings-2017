# User-Focused Security at Netflix: Stethoscope

# Abstract

User-Focused Security is an approach we are using to address employee
information security at Netflix. If we provide employees with the right
information and low-friction tools, we believe they can get their devices into
a more secure state without heavy-handed policy enforcement.

Letting people retain control over their devices means that they can maintain
flexibility and productivity and address security recommendations as
appropriate to their levels of access. This approach will only be successful,
though, if we can provide recommendations for clear and specific actions and
make it easy to do the right thing.

Stethoscope is a web-based tool that gives Netflix employees a view into the
security state of their devices, with specific recommendations regarding disk
encryption, firewalls, and other device settings. The website, in conjunction
with email alerts, gives Netflix employees a straightforward way to see what
actions they should take to remain safe.

# User-Focused Security

The notion of “User-Focused Security” acknowledges that attacks against
corporate users (e.g., phishing, malware) are the primary mechanism leading to
security incidents and data breaches, and it’s one of the core principles
driving our approach to corporate information security. It’s also reflective of
our philosophy that tools are only effective when they consider the true
context of people’s work.

![Stethoscope's Giraffe Logo](imgs/giraffe.png "Stethoscope")

Stethoscope is a web application that collects information for a given user’s
devices and gives them clear and specific recommendations for securing their
systems. If we provide employees with focused, actionable information and
low-friction tools, we believe they can get their devices into a more secure
state without heavy-handed policy enforcement.

## Software that treats people like people, not like cogs in the machine

We believe that Netflix employees fundamentally want to do the right thing,
and, as a company, we give people the freedom to do their work as they see fit.
As we say in the Netflix Culture Deck, responsible people thrive on freedom,
and are worthy of freedom. This isn’t just a nice thing to say–we believe
people are most productive and effective when they they aren’t hemmed in by
excessive rules and process.

That freedom must be respected by the systems, tools, and procedures we design,
as well. By providing personalized, actionable information–and not relying on
automatic enforcement–Stethoscope respects people’s time, attention, and
autonomy, while improving our company’s security outcomes.

This approach isn't just applicable within Netflix, however. By open-sourcing
Stethoscope, we aim to help organizations with similar values achieve their
security goals while respecting employee freedom. Further, we hope other
organizations find value in our approach and in Stethoscope and that we can
improve both by working together.

## Education, not automatic enforcement

It’s important to us that people understand what simple steps they can take to
improve the security state of their devices, because personal devices–which we
don’t control–may very well be the first target of attack for phishing,
malware, and other exploits. If they fall for a phishing attack on their
personal laptop, that may be the first step in an attack on our systems here at
Netflix.

We also want people to be comfortable making these changes themselves, on their
own time, without having to go to the help desk. To make this self service, and
so people can understand the reasoning behind our suggestions, we show
additional information about each suggestion, as well as a link to detailed
instructions.

## Security practices

We currently track the following device configurations, which we call
“practices”:

- Disk encryption
- Firewall
- Automatic updates
- Up-to-date OS/software
- Screen lock
- Not jailbroken/rooted
- Security software stack (e.g., Carbon Black)

Each practice is given a rating that determines how important it is. The more
important practices will sort to the top, with critical practices highlighted
in red and collected in a top banner.

## Implementation and data sources

Stethoscope is powered by a Python back-end and a React front-end. The web
application doesn’t have its own data store, but directly queries various data
sources for device information, then merges that data for display.

The various data sources are implemented as plugins, so it should be relatively
straightforward to add new inputs. We currently support LANDESK (for Windows),
JAMF (for Macs), and Google MDM (for mobile devices). We also support ownership
attribution via bitFit (an inventory system).

## Notifications

In addition to device status, Stethoscope provides an interface for viewing and
responding to notifications. For instance, if you have a system that tracks
suspicious application accesses, you could choose to present a notification
to the user when they login to which they can respond.

## Lessons so far

Building and deploying Stethoscope within Netflix has already taught us a few lessons from which we hope others will be able to learn. In particular, the quality of the data that feeds into a system like Stethoscope has to be high; buggy and incorrect ownership records plagued our early attempts to link employees with devices. Second, different users want different levels of context with respect to security recommendations: many users just want to “make it turn green,” but others want in-depth technical explanations and assurances that our recommendations won’t adversely impact their work. 

## What’s next?

We’re excited to work with other organizations to extend the data sources that
can feed into Stethoscope. [osquery](https://osquery.io/) is next on our list,
and there are many more possible integrations.

## Getting started

Stethoscope is available [now](https://github.com/Netflix/stethoscope) on
GitHub.


#### Metadata

Tags: information security, security education, open-source software, usable security

**Primary Author Name**: Andrew M. White
**Primary Author Affiliation**: Netflix
**Primary Author Email**: andreww@netflix.com
**Additional Author Name**: Jesse Kriss
**Additional Author Affiliation**: Netflix
**Additional Author Email**: jkriss@netflix.com
