# I have a graph database. Now what?

# Abstract

Graph data models have been a hot topic in security for a few years but analysis of cyber graphs is still largely driven by visual assessments and rudimentary link analysis. Graph’s can do a lot more than just paint pretty pictures. In our talk we discuss how to develop cyber specific graph models and utilize probabilistic analysis to help operators make better decisions. We have also released Project Balerion to the community, a security operator focused tool that utilizes our framework in combination with the Fidelis Barncat dataset to help operators with RAT hunting.  

# Graph databases: The gap between Possibilities and Reality

Increasingly cybersecurity professionals are starting to look like scientists - we collect evidence, set up a hypothesis, analyze it and drive to a conclusion and decision. This requires technical capabilities that can utilize pattern matching, exploration and statistical inference based on the underlying relationships between data. We see graph technologies as a core component of making these analysis techniques more accessible to operators. But there is still a way to go towards making it a reality. Starting from the first step of developing a data model to repurposing your bespoke analysis methods, there are plenty of challenges along the way. Initiatives like CyBOX and STIX that represent cyber data in a standardized taxonomy and ontology are steps in the right direction. Increasingly cyber data representation is moving towards a standardized taxonomy and ontology, like CyBOX and STIX, that can be easily represented in a graph data model. In our talk we discuss how to address the challenges of designing a practical data model and implementing it using Neo4j’s graph technology. In the following section, we will explain how this data model is operationalized for Project Balerion. 

# Project Balerion Approach

In July 2016 Fidelis Cybersecurity released the Barncat Intelligence Data to security researchers and we applied our graph data model and analysis techniques to see what insights we could derive. The Barncat data contains approximately 100,000 remote access tools (RAT) configurations from malware samples gathered and analyzed by Fidelis. We covered our graph data model and our framework that applies Bayes rule [^1] to infer relationships between various RAT configurations. In the analysis, we divided the data into two categories: the input indicators and the classification indicators. The input indicators (Hashes, IPs, URLs etc.) can be used to infer the classification indicators (RAT type or Campaign name). The use of the Bayes formula stems from the non-uniqueness of the classification into a class indicator type using input indicators [^2]. That is why a probabilistic framework, fundamentally relying on Bayes Theorem, is unavoidable. 

As a quick review of the theorem and its application here, the posterior probability obtained from Bayes rule gives the probability of classifying into a specific RAT/Campaign name given we observe a specific input indicator. The posterior depends on two quantities: the likelihood function and the prior probability. The likelihood is the probability of observing an input indicator given we observe a specific RAT/Campaign name. The prior is the probability of observing a specific RAT/Campaign within the total population of RATs/Campaigns. The main assumption relies in the modeling of the prior probability. There are two possible approaches. The first one computes the prior from the Barncat dataset which means the prior becomes the frequency of observing a specific RAT/Campaign name. The second one assumes they are equally likely. Both approaches are implemented in Project Balerion [^3] and both of them can have pros and cons. The first approach takes into consideration the real world data of the frequencies of observation of specific RAT types. But one should be aware that it could be biased with respect to the infrastructure used by Fidelis as it might not be capturing the true population’s distribution. On the other hand, the second approach is more idealistic as it assumes that all RAT/Campaigns are equally likely. The argument for the equally likely assumption is that it is more objective, in the sense that it avoids any potential infrastructure bias. In our opinion, given that some RATs or Campaigns are way more popular or frequent than others it would be more realistic to use the prior computed from the Barncat dataset. We have released the framework so researchers can apply this to their own datasets of RAT configurations in the following open source project [^3].

# Future Work

What is ubiquitous in any scientist’s toolkit, namely statistical inference frameworks, is generally missing from a cyber operator’s analysis toolkit. But there hasn’t been a focus on how to make these statistical frameworks more pragmatic and operator friendly. We see graph technologies as a critical component of enabling computationally intensive processes and also providing a more operator friendly view of the data. We plan on releasing more tools to the community that helps us think about data analysis in a more consistent and logical way. 

# References

[^1] Bayes, T. (1763). http://mathworld.wolfram.com/BayesTheorem.html

[^2] Kseib, N. (2016). https://blog.trustar.co/increase-your-chances-of-catching-a-rat-199bcb439f93#.p1qvz7az9

[^3] Kseib, N. (2016). https://github.com/trustar/trustar-balerion

#### Metadata

* **Primary Author Name**: Nicolas Kseib
* **Primary Author Affiliation**: Data Science Lead, TruSTAR Technology
* **Primary Author Email**: nkseib@trustar.co

* **Additional Author Name**: Shimon Modi
* **Additional Author Affiliation**: Director of Product & Technology, TruSTAR Technology
* **Additional Author Email**: smodi@trustar.co