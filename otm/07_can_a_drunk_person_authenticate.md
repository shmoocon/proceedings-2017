# Can a Drunk Person Authenticate Using Brainwaves? # NotAlcoholicsJustResearchers

# Abstract

Biometric authentication is a technique to identify and authorize end-users to computing systems. An electroencephalogram (EEG) as an authentication system has shown initial successes, but few studies have investigated impaired brain activity. Additionally, external influences, such as alcohol or caffeine, can impact brainwave activity thus, confusing the authentication system. In this paper, we propose an approach to authenticate end-users influenced by alcohol using EEG readings and machine learning techniques. We evaluated our methodology using an existing dataset and a live test subject. Our results demonstrated the accuracy of our proposed system.

# Introduction

Biometric authentication using an electroencephalogram (EEG) has been recently studied as a new paradigm for identifying end-users. The EEG usage has shown versatility in authenticating an end-user [^1][^2][^3][^4] due to the uniqueness presented in brainwave measurements. However, alcohol consumption has been shown to impact brainwave activity [^5], which presents challenges for end-user authentication.

To combat such challenges, this paper presents techniques to authenticate impaired end-users using various machine learning models: Support Vector Machines (SVM), K-Nearest Neighbor (KNN), Artificial Neural Network (ANN), and Deep Convolutional Neural Network (DCNN). We evaluate our approach using an existing dataset [^6] and live experiments. Our evaluations utilized the EMOTIV Insight 5-channel EEG [^7] and measured levels of inebriation with Blood Alcohol Content (BAC) readings using a Vastar AB130 breathalyser [^8].

We outline the contributions of this work as follows.
* We carefully studied levels of inebriation as an approach to authenticate end-users using an EEG.
* We evaluated the accuracy of our machine learning approaches.

# Methodology

## Data Acquisition

To set up the experiment, participants consumed alcohol in controlled quantities and time increments. Using the EEG and breathalyser, the participants’ brainwaves and BAC were recorded. Using sensors positioned around a subject’s head, brainwave data can be collected from different regions of the brain. These specific signals closely estimated varying degrees of impairment to verbal memory (AF3), attention (T7), emotional memory (T8), and judgement (AF4), with one signal used as a baseline (Pz). Collecting the BAC data was important because it gave a quantitative measure of the participants’ levels of impairment.

## Modeling and Prediction

Using the brainwave measurements from the EEG device, several machine learning models were trained to identify an individual user from a database of users with existing data samples. Different models, such as SVM, KNN, ANN, and DCNN were used to compare results for each approach. Using a parameter grid search and 5-fold cross validation, a best configuration was found for each model type. The best models, based on validation accuracies, were then used to report results on a separate testing dataset.

# Results

## Examination of Brainwave Activity

In a careful examination of using an EEG, a study was conducted to evaluate the severity of the influence of alcohol towards brainwave activity. In our analysis, we conducted a series of measurements using a live test subject where alcohol was consumed at intervals of 30 minutes over a period of time. The alcohol of choice for our study was one fluid oz of Fireball [^9] per interval with BAC readings at every half interval. It was demonstrated that alcohol applied a direct influence to brainwave activity as shown in *Figure 1*.

![](imgs/fig1.png)

*Figure 1.* EEG measurements on channels AF3, T7, Pz, T8, and AF4 at corresponding subcharts ordered top to bottom at various BAC levels.

## Evaluating the EEG Paradigm

To evaluate our approach, we utilized the UCI EEG dataset [^6] to measure the accuracy of our authentication scheme. The dataset consisted of two splits: training and testing. Moreover, the UCI dataset had a population size of 20 participants, a comparison of subjects influenced by alcoholism, and a larger channel range with a high end EEG. Although we collected EEG measurements from a live participant, the UCI dataset had more data samples in addition to its other benefits. Therefore, it was considered a better solution for our analysis. The UCI dataset does not measure EEG signals from inebriated users, but instead uses data from some participants who are affected by alcoholism and others who are not. Using the UCI dataset, *Table 1* shows the accuracy of our authentication approach.

*Table 1.* Performance comparison of different machine learning models on user identification.

<table>
    <tr>
        <td>Model Type</td>
        <td>KNN</td>
        <td>SVM</td>
        <td>ANN</td>
        <td>DCNN</td>
    </tr>
    <tr>
        <td>Validation Score</td>
        <td>29.66667%</td>
        <td>23.33333%</td>
        <td>28.83333%</td>
        <td>12.50000%</td>
    </tr>
    <tr>
        <td>Testing Score</td>
        <td>27.83333%</td>
        <td>25.00000%</td>
        <td>34.66667%</td>
        <td>13.16670%</td>
    </tr>
</table>

To find the best machine learning models, a brute-force parameter sweep was performed for each model type. Using 5-fold cross validation, an average prediction accuracy on the training data, called the validation score, was used to identify the highest performing model configuration. Once the best configuration was chosen, the trained model was used to collect the prediction accuracy on a separate testing dataset. For each model type, the values shown in the table denote the validation scores and accuracies of the best parameter configurations on testing data.

# Conclusion

It was demonstrated that our work showed encouraging results for using an EEG as an authentication system for impaired users. Our preliminary measurements showed that alcohol consumption greatly affected brainwave activity. This introduced a need for an identification system that was robust to varying levels of inebriation. Our approach achieved approximately 34.667% accuracy in user identification, which has shown to be a more effective approach in comparison to other solutions that utilized the UCI dataset. In the future, we plan to conduct experiments with a larger sample population in addition to varying levels of influences to brainwave measurements.

# Acknowledgement

The authors would like to thank Bruce and Heidi Potter for their extensive dedication and work. We also thank the reviewers for their time and consideration. Furthermore, we would like to thank the audience who witnessed our first ShmooCon presentation. Lastly, #NotAlcoholicsJustResearchers

# References

[^1] J. Chuang, et al. "I think, therefore i am: Usability and security of authentication using brainwaves." In International Conference on Financial Cryptography and Data Security. Springer Berlin Heidelberg, 2013.

[^2] J. Klonovs, et al.. "Id proof on the go: Development of a mobile eeg-based biometric authentication system." IEEE Vehicular Technology Magazine 8, no. 1 (2013)

[^3] S. Marcel and J. Millán. "Person authentication using brainwaves (EEG) and maximum a posteriori model adaptation." IEEE transactions on pattern analysis and machine intelligence 29, no. 4 (2007).

[^4] Q. Li, et al. "Brain-computer interface applications: Security and privacy challenges." In Communications and Network Security (CNS), 2015 IEEE Conference on, IEEE, 2015.

[^5] K. L. Son, et al. "Neurophysiological features of Internet gaming disorder and alcohol use disorder: a resting-state EEG study." Translational psychiatry 5, no. 9 (2015): e628.

[^6] https://archive.ics.uci.edu/ml/datasets/EEG+Database

[^7] https://www.emotiv.com/product/emotiv-insight-5-channel-mobile-eeg/

[^8] https://www.amazon.com/Vastar-Professional-Breathalyzer-Semi-conductor-Mouthpieces/dp/B01N1GQY38/ref=sr_1_8?ie=UTF8&qid=1486791093&sr=8-8

[^9] http://fireballwhisky.com/

#### Metadata

Tags: Authentication,Electroencephalography (EEG),Machine Learning (ML),Alcohol

* **Primary Author Name**: Tommy Chin
* **Primary Author Affiliation**: Grimm (SMFS, Inc.)
* **Primary Author Email**: tommy.chin@ieee.org
* **Additional Author Name**: Peter Muller
* **Additional Author Affiliation**: Rochester Institute of Technology
* **Additional Author Email**: pete.m.muller@gmail.com
