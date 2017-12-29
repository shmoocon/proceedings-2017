# Plug-in Electric Vehicle Fingerprinting

### Authentication for Plug-in Electric Vehicles

## Abstract

Hacking vehicles is a hot topic. However, there is one aspect of vehicle vulnerability that is underappreciated, mostly because it only applies to a minute percentage of the vehicles on the road today. Plug-in electric vehicles (PEVs) make up a tiny portion of the overall vehicle market, but they are becoming more common.

PEV charging involves authentication, payments, and, increasingly, communication for managing power flow to stabilize the electric power grid. In the design of charging systems, security is not always emphasized. In this talk we propose a means for charging stations to identify the type of PEV connected to charge without explicit communication of this information from the PEV. We report the results of initial testing, and outline future work.


## Introduction

The hypothesis, or driving idea behind the PEV fingerprinting project is that different vehicle types exhibit slight differences in their charging behaviors that EVSE could detect and use to identify the type of PEV plugged in to charge. PEV type is defined by manufacturer, model and year. Charging behaviors comprise a variety of aspects of PEV charging that are described below. PEV fingerprinting is similar in concept to OS fingerprinting and various device identification experiments completed by researchers [1-4]. 

## Background and Motivation

PEV Fingerprinting would serve multiple purposes. The ability to distinguish between different vehicle types could facilitate troubleshooting incompatibilities between PEV and EVSE types. Parties controlling EVSE can better manage the PEV charging when they have access to information about the charging capabilities of the vehicles connected to the EVSE in the system. Explicit self-identification between the EVSE and PEV is supported by relatively new standards, and some research groups have implemented advanced communication between PEVs and EVSE. However, these standards and communication methods have not been widely adopted. Even when advanced communication becomes common, the party managing the EVSE may want additional methods to verify information provided by the PEV for security purposes.

PEVs are vulnerable to the same security risks as internal combustion engine vehicles as well as some that are unique to PEVs. The common risks generally comprise two factors: the extent to which systems in the vehicle are computer-controlled, and vehicles’ interconnectivity  [5 - 6]. Security issues unique to PEVs generally relate to the role PEVs are expected to play in the smart grid. PEVs are a significant additional load to the electric power grid, as a PEV can easily draw power in the same range as a typical single-family residence [7]. As PEVs become more common, their charging must be managed in a way that at least protects, and at best benefits the grid. Such management requires communication systems that become targets for potential attacks targetted at the vehicle or the electric power grid  [8-10]. In this environment, PEV fingerprinting could serve purposes related to the security of PEV charging, possibly as a supplemental tool for authentication.


## Methods

Experiments exploring the fingerprinting hypothesis use the SAE J1772 standard for AC Level 1 and Level 2 charging.  The SAE J1772 defines the interface between EVSE and PEVs: both the physical connectors and the communication  [11]. This interface is illustrated in Figure 1, and described in Table 1. 


![J1772 Interface Image](imgs/J1772v2.png)
<imgcaption>**Figure 1** J1772 connector; PEV side.</imgcaption>

![J1772 Interface Explained](imgs/Table1.png)
<imgcaption>**Table 1** J1772 connector elements</imgcaption>

The power lines and control pilot provide parameters for fingerprinting vehicles. Through voltage levels and PWM over the control pilot, the EVSE and PEV exchange information to set up and maintain a charging session. The PEV contains components of the control pilot circuit. The SAE J1772 defines the nominal values of these components, and allows a 3% tolerance. Differences in the exact values of the components used would cause variations in the voltage levels on the pilot. These voltage levels, as well as the timing of an action taken by the PEV to change the voltage level at one point during the set up phase of a charging session could serve as fingerprinting parameters.

The current drawn by a PEV also provides several parameters. According to the SAE J1772, while EVSE specify the maximum current allowed, PEVs make the final decision regarding how much current to draw (within the specified max), and at what rate to ramp up the current drawn. The final current and the features of the ramping serve as fingerprinting parameters. 

Data for the fingerprinting project came from EVSE at the University of Delaware. In these EVSE, the control pilot is sampled by a microcontroller that provides timing and voltage measurements. Information regarding power flow comes from an EKM Omni V.4 pulse meter. The pilot and power information is sent to a server, from which it is pulled and formatted to be used as input to data mining software. 

To generate data, EV fingerprinting team members repeatedly plugged and unplugged PEVs at EVSE reporting fingerprinting data with 2 minutes between each action. Multiple tests yielded 130 total charge events (instances of a car starting to charge and charging at least 2 minutes) from 7 different vehicles. None of the vehicles were of the same type. 


## Results

Using Weka data mining software, the data was used to generate models for classification based on the aforementioned parameters. The two classifiers used were Naïve Bayes and a J48 Decision Tree. The best performance was that of the J48 decision which correctly classified 129 of the 130 PEVs in the data. The Naïve Bayes classifier correctly classified 127. Figures 2a and 2b show the confusion matrices of both classifiers. 

![Confustion Matrices](imgs/ConfusionMatrices.png)
<imgcaption>**Figure 2a.** Results of J48 classifier: 129 of 130 instances correctly classified.  **Figure 2b** Results of Naive Bayes Classifier: 127 of 130 instances correctly classified.</imgcaption>

## Future Work

In addition to the small amount of data and lack of multiple PEVs of the same type, the fingerprinting work is limited by certain assumptions. Most significantly, battery state of charge and state of health, as well as ambient temperature have been ignored up to this point. Thus, while generating a larger volume of data with multiple PEVs of the same type is the primary concern for future work, consideration must be given to these factors. 

## Conclusion

This discussion of the EV fingerprinting project has described initial work in developing a means of identifying the types of PEVs connected to EVSE using only the information available through a common charging standard. Results thus far suggest that fingerprinting is feasible. More work will determine the extent to which PEV fingerprinting is possible and practical.

## References

[^1] Kohno, T., Broido, A.,Claffy, K.C., "Remote Physical Device Fingerprinting," IEEE Trans.Dependable and Secure Comput.IEEE Transactions on Dependable and Secure Computing, vol. 2, pp. 93-108, 2005.   

[^2] J. M. Allen, "OS and application fingerprinting techniques", SANS Reading Room, 2008. Available: https://www.sans.org/reading-room/whitepapers/authentication/os-application-fingerprinting-techniques-32923.  

[^3] "About Panopticlick." Available: https://panopticlick.eff.org/about.  
[^4] Bojinov, H., Boneh, D., Michalevsky, Y., "Mobile device identification via sensor fingerprinting", Onlinel. Available: https://crypto.stanford.edu/gyrophone/sensor_id.pdf.  

[^5] C. Thuen, "Commonalities in vehicle vulnerabilities", Online, 2016. Available: http://www.ioactive.com/pdfs/Commonalities_in_Vehicle_Vulnerabilities_WP.pdf.  

[^6] MOTOR VEHICLES INCREASINGLY VULNERABLE TO REMOTE EXPLOITS. (March 17, 2016). Available: https://www.ic3.gov/media/2016/160317.aspx.  

[^7] S. Barker et al, "SmartCap: Flattening peak electricity demand in smart homes," in 2012 IEEE International Conference on Pervasive Computing and Communications, 2012, pp. 67-75.  

[^8] H. Chaudhry and T. Bohn, "Security concerns of a plug-in vehicle," in 2012 IEEE PES Innovative Smart Grid Technologies (ISGT), 2012, pp. 1-6.  

[^9] M. A. Mustafa et al, "Smart electric vehicle charging: Security analysis," in Innovative Smart Grid Technologies (ISGT), 2013 IEEE PES, 2013, pp. 1-6.  

[^10] A. C. F. Chan and J. Zhou, "On smart grid cybersecurity standardization: Issues of designing with NISTIR 7628," IEEE Communications Magazine, vol. 51, pp. 58-65, 2013.   

[^11] Society of Automotive Engineers, "J1772," 2012. 

* **Primary Author Name**: Rebekah Houser,
* **Primay Author Affiliation**: University of Delaware,
* **Primay Author Email**: rlhouser@udel.edu,

* **Additional Author Name**: Willet Kempton,
* **Additional Author Affiliation**: University of Delaware,
* **Additional Author Email**: willett@udel.edu,

* **Additional Author Name**: Rodney McGee,
* **Additional Author Affiliation**: University of Delaware,
* **Additional Author Email**: tmcgee@udel.edu,

* **Additional Author Name**: Fouad Kiamilev,
* **Additional Author Affiliation**: University of Delaware,
* **Additional Author Email**: kiamilev@udel.edu,

* **Additional Author Name**: Nick Waite,
* **Additional Author Affiliation**: University of Delaware,
* **Additional Author Email**: ernwa@udel.edu