# háček: computing a hacker experience

### An inside peek at the development of the háček, an interactive art exhibit that leverages synthesized attack data, and a discussion of the technical aspects emerging in computational art, the hackers of the art scene. 

# Abstract

Today’s artists and musicians  are using computational techniques and tools to reshape boundaries between engineering and art, extracting compelling immersive experiences out of emerging social and networked systems. In this paper we explain how we conceptualized, coded and created háček, a unique interdisciplinary artwork designed to explore the narratives of hackers.

# Content

We live within data; using it to describe who we are, provide context what has happened, and filter current events. We can also leverage this data to build new experiences and environments such as háček. Our multidisciplinary team was able to create a new vision of cyberspaces / cybersecurity by combining a reactive physical installation with a VR experience that visualized, sonified and gamified data. From simple server logs, the artists wrote scripts, shaders and patches to transform the IP addresses, timing information and data types into sound and moving image, creating an immersive installation that encompasses metaphors of networked landscape, security and wayfinding. Here we discuss the framework, techniques, and narrative approach used to craft a compelling artistic experience from 134,635 lines of alphanumeric data. 


## Introduction

Peter Naur coined the term “data science” in 1960, but it didn’t take off in popularity until the 1990s. Initially used as a synonym for computer science, its meaning has become more nuanced—data science is now recognized as an interdisciplinary field linking traditional statistical methodology, modern computer technology, and the knowledge of domain experts in order to convert data into information and knowledge. In A Taxonomy of Data Science Hilary Mason and Chris Wiggins write “Data science is clearly a blend of the hackers’ arts… statistics and machine learning… and the expertise in mathematics and the domain of the data for the analysis to be interpretable… It requires creative decisions and open-mindedness in a scientific context.” As computers have become faster, and tools for analysis of data have become easier to access, the creative decision side of data science has been growing. Now artists and musicians can convert data into artistic experiences; art created from data is distinct from data visualization or sonification, because the ultimate goal is a pleasing aesthetic experience, instead of insight into the data. For háček we wanted to embody the experience of being part of a DDOS attack —using data extracted from the race to get into ShmooCon, and creating a VR experience of what it feels like to join thousands of people, scripts and bots during the annual challenge to log into shmoocon.org on launch day, to compete for a ticket. 


##Envisioning Experiences

The team started by analyzing at the data from the server logs: we received a text file with 2.5 seconds of data which held 134,635 lines with a 1) a timestamp; 2) a four-digit IP;  and 3) an event type tag, for example:

```
2015-11-01 12:12:09.965560 162.17.208.26 get_hold: 12-9c583367-d3ae-00a4-1e2d-8af113f16a19 Queueing
```

Initially we thought we could create a visually compelling piece which would also give a sense of the scale of the data by using the location of the IP addresses to graph locations, but the locations were very unevenly distributed and we didn’t think it would work well aesthetically. The most interesting piece of information seemed to be the timing; by following the microseconds it was clear that each IP took a unique path through the process of registration. After focusing on the timing data, we thought of creating a VR race where you picked an IP to “play” and  the finish line was getting through the final captcha. Most of the players wouldn’t get through, showing the intense competition for access. The initial sketches were mountain ranges and we knew we wanted a landscape-like formation based on the data. To create the range we parsed the data so that IPs were translated into unique IDs starting at 1. After that we culled the first and last 165 rails because they produced awkward looking visualizations when plotted with all the others. The rails were then uniformly distributed around in a circle away from the center -- which is defined as the goal position. From there, the rails were given several midpoints between their start position and final goal, representing stages in buying a ticket. The time it took to finish a state both determined the speed AND the height of the rail; and the mountain range was created by hand around the pattern that this created. 

Sonically, the majority of the sounds heard are from hummingbird samples slowed down 100-10,000 times. Since we were stretching 2.5 seconds of data into a 3.5 minute experience slowing down very fast sounds was consistent with our approach to the data. Everything audiences hear in and out of the VR was converted into MIDI triggers  by the data. For example the IP address dictated the following: 

```
IP1
	IP2
	IP3
	IP4
	PITCH
	VELOCITY
	DURATION
	PITCH 2
	

	DURATION 2
	VELOCITY 2
```	

	
Essentially, for each IP address two notes were created with inverse velocity and duration. Since there were 16 events types, it was very natural to assign the MIDI channel, or instrument, to the event type because there are 16 MIDI channels. The sound files produced were very dense, so we also programmed in “packet loss” as a way to perforate the data in a way that was consistent with the source. We also programmed a way to constrain the range of the pitches being triggered. Other sounds were motivated by the visual brief—the IP “rails” were glassy trails which inspired the use of a glass harmonica.


All scripts can be found on https://github.com/artsdotcodes. 


##Tools

The team used two custom Python scripts to parse the data to be used by the VR engine and the audio programs. In the visual domain we used Unity to program the VR, and then sent a composite image to a second computer running Resolume to video map five channels of video projection onto a sculpture. These projections were a way to “spy on” the user in VR, creating an artistic commentary on the security of data. In  the audio domain we used Max/MSP to read a parsed text file created by the Python script. Using internal MIDI routing, Max/MSP then controlled a combination Ableton Live, and Kontakt Player. Ableton and Kontakt then sent audio through Soundflower to Logic. In the physical installation we had three speakers playing back looped sound files of different lengths, creating an ever-changing soundscape to accompany the projected video. 

## Conclusion

Hackers and artists have always had a lot in common; a willingness to tinker and experiment with new technologies and systems, and an interest in how things work. As artists and researchers in new media it was fascinating to  get a front-row seat into the hacker experience, to see the reflection of that world via data left behind in logs. Similarly, we share back with you a view of that experience rendered back again, through our own layers of interpretation and expression. Computational artists, the hackers of the art world, are charting our own course through the ether.


#References

* H. Mason & C. Wiggins “A Taxonomy of Data Science” Posted: September 25th, 2010. Accessed: Feb 25 2017. http://www.dataists.com/2010/09/a-taxonomy-of-data-science/


#### Metadata

* **Primary Author Name**:  Allison Miller
* **Primary Author Twitter**:  @selenakyle
* **Primary Author Email**: allymiller@gmail.com

* **Additional Author Name**: Melissa Clarke
* **Additional Author Affiliation**: Stony Brook University
* **Additional Author Email**: mfcgrp@gmail.com

* **Additional Author Name**: Margaret Schedel
* **Additional Author Affiliation**: Stony Brook University
* **Additional Author Email**: margaret.schedel@stonybrook.edu
