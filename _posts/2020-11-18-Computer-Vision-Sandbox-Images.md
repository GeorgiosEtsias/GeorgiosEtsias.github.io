---
title: "Application of computer vision on laboratory sandbox investigations"
date: 2020-11-15
tags: [neural networks, image processing, regression, classification, genetic algothim]
header:
  
excerpt: "Neural networks, Image Analysis, Classification, Regression, MatLab"
mathjax: "true"
---

GitHub repository [link](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images)

## Table of contents
- [Citing this work](#citing-this-work)

- [Project overview](#project-overview)

- [Image pre-processing](#image-pre-processing)

- [Classification](#classification)

- [Regression](#regression)

## Citing this work

This project is part of the Saline Intrusion in Coastal Aquifers ([SALINA](https://gow.epsrc.ukri.org/NGBOViewGrant.aspx?GrantRef=EP/R019258/1)) research project. 

This work was originally published under the title: [Optimizing Laboratory Investigations of Saline Intrusion by Incorporating Machine Learning Techniques](https://www.mdpi.com/2073-4441/12/11/2996) in the Water MDPI journal.

## Project overview
Deriving saltwater concentrations from the values of light intensity is a long-established image processing practice in laboratory scale investigations of saline intrusion. The current repository presents a novel methodology that employs the predictive ability of machine learning algorithms in order to determine saltwater concentration fields. The proposed approach consists of three distinct parts, image pre-processing, ground profile classification (bead structure recognition) and saltwater field generation (regression). It minimizes the need for aquifer-specific calibrations, significantly shortening the experimental procedure by up to 50% of the time required. 

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig1.PNG" alt="l try">
A graphical outline of the investigation

The methodology introduced in this investigation comprises three sections. The first part involves experimental image pre-processing via both established and novel procedures. The second part treats the task of recognizing aquifer heterogeneity as a multinomial pixel wise classification problem. Lastly the third section derives the saltwater concentration fields via regression analysis on the values of light intensity (LI) and the corresponding bead sizes. 

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig2.png" alt="l try">
Flowchart of the proposed methodology depicting its three distinct parts a) image pre-processing, b) bead structure recognition (classification) and c) saltwater field generation (regression).

Three homogeneous aquifers served as the training datasets for all the machine learning algorithms. The remaining aquifers were utilized for quantifying the models’ generalization ability. It was established that one homogeneous aquifer per bead size used, generated sufficient data for the successful implementation of the method. After the training of the neural learners was complete, saltwater concentration fields could be successfully generated with no further calibration effort for any number of new homogeneous or heterogeneous aquifers.

## Image pre-processing

Data pre-processing significantly affects the generalization performance of supervised machine learning algorithms. Τhe images of homogeneous aquifers were characterized by significant irregularity in the distribution of light intensity values. This can be attributed to two distinct factors: the different optical attributes of each specific glass bead and the lighting conditions. While the first cause creates a random variation in the observed LI values, the second is responsible for a distinct distribution, where the images appeared lighter in the centre of the sandbox. 

This lighting pattern was efficiently filtered out by developing the Mean Homogenization Factor (MHF) presented in this equation:

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig3.PNG" alt="l try">

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig4.png" alt="l try">
Light intensity distribution of the a) original and b) homogenized training images.

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig5.png" alt="l try">
Impact of Mean Homogenization Factor on a single homogeneous aquifer photo

The application of MHF contributed to a significant improvement in the training and generalization performance of the machine learning algorithms tested in the next section.

#### Folder material description

Scripts: [MeanHomoFactorCalculator.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/1%20Image%20pre-processing/MeanHomoFactorCalculator.m)

Datasets: subset1.mat, subset2.mat, subset3.mat, subset4.mat (containg 12 freshwater-only images of homogenoeus aquifers)

## Classification

This part gives the user the ability to recognize the size of individual beads and thereby any given heterogeneous structure through a soft-computing method based on supervised machine learning. The technique the automatic recognition of more complex, including totally random, experimental aquifers, while minimizing the time needed for recreating each one of them. This part of the investigation is based on images of the 15 aquifers taken prior to saltwater intrusion. The pixels of three homogeneous aquifers served as the training subset. The remaining twelve images were used to evaluate the effectiveness of the method. The training subset was deliberately kept as small as possible, in order to ensure the easy implementation of the approach.

### a) [Classification training](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/2%20Classification/2.1%20ClassificationTraining)

Scripts: [ClassificationTrainingData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.1%20ClassificationTraining/ClassificationTrainingData.m) (prepares data for neural training), 

[ANNClassifiationGenerator.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.1%20ClassificationTraining/ANNClassifiationGenerator.m) (trains on parallel a deep classification ANN)

Data: subset4.mat (used as the training dataset, containing 3 freshater-only aquifer images, one for every utilized bead size)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

### b) [Classification prediction](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/2%20Classification/2.2%20Classification%20Prediction)

Scripts: [ClassificationData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ClassificationData.m) (prepares data for testing),  

[ANNPrediction.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ANNPrediction.m) (executes the neural prediction), 

[ANNPredictionProbability.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ANNPredictionProbability.m) (executes the neural prediction while further post-processing it to get optimum results)

Data: ANNclassification.mat (pre-trained deep classificstion neural network), 

testdataset.mat (3 stratified test aquifers)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig6.png" alt="l try">
The four prediction steps executed in [ANNPredictionProbability.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ANNPredictionProbability.m)

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig7.png" alt="l try">
Heterogeneous aquifers (i) and the (ii) bead structure generated by a multi-layered Artificial Neural Network (ANN).

## Regression

The image processing methods that have been presented up to now were based on the same principle. A number of calibration images were acquired for each aquifer, these data were used to formulate a relationship between LI values and SW concentrations via regression analysis. While relatively accurate, these techniques were disadvantaged by the need to acquire new calibration images for every investigated aquifer, making them slow and labour intensive. Eliminating this need by the use of machine learning, will represent a step change in the ability to run significant laboratory investigations.

### a) [Regression training](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.1%20Regression%20Training)

A traditional MLT approach would feed the raw data from the eight calibration images (with saltwater concentrations of 0%, 5%, 10%, 20%, 30%, 50%, 70%, and 100%), of one homogeneous aquifer for each utilized bead size, into a sufficiently strong learner that would automatically establish a relationship between LI and SW%. Nevertheless, it is well documented that experimental data of saltwater intrusion generated in sandbox setups are characterized by a high percentage of irregularity, mostly due to the deviating light absorbing attributes of the individual beads. 

The nature of this type of data constitutes their straightforward use for neural training problematic.
Instead of the actual images, a feedforward ANN with one 10-neuron hidden layer was trained on the mean values of monochromatic green LI for each specific aquifer using Levenberg–Marquardt as its training algorithm.

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig8.JPG" alt="l try">
Average values of green light intensity of the three homogeneous aquifers for each corresponding calibration concentration.

#### Folder material description

Scripts: [RegressionTrainingData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.1%20Regression%20Training/RegressionTrainingData.m) (prepares data for neural training), 

[ANNTrainingRegression.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.1%20Regression%20Training/ANNTrainingRegression.m) (trains on parallel a regression ANN)

Data: Cal780G.mat, Cal1090G.mat, Cal1325G.mat (green (G) light intensity values for each one of the utilized bead sizes, each subset includes the 8 calibration concentration images)

### b) [Regression prediction](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.2%20Regression%20Prediction)

In order to minimize the impact of noisy data, caused by the presence of beads of varying colour tones, a quantification of the optical irregularities of the glass bead medium was essential. This procedure took place in the freshwater only images of the tested aquifers. To do so the LI values of each individual pixel were divided with the corresponding measured average LI value of its bead size.  In doing so, a Bead Optical Irregularity Metric (BOIM) was derived characterising each specific bead of the investigated aquifers as shown in Equation 2, assigning higher and lower BOIM values to lighter and darker beads respectively.

By dividing the freshwater only image of any aquifer with its corresponding BOIM matrix, the mean LI values the feedforward ANN was trained upon are returned. When these values were fed on the neural network the 0 % SW concentration fields was perfectly recreated. The same procedure was repeated for saltwater-only images of the aquifers, deriving the BOIM corresponding to the 100% saltwater percentage. This leading to perfect prediction of the 100% SW concentration components of the testing images. Dividing the SWI test images with BOIM0 and BOIM100 creates two data subsets that, when fed to the neural network, generate a perfect prediction of the two main concentration regions. 

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig9.png" alt="l try">
Effect of the implementation of BOIM0 and BOIM100 on a SWI image of the Layered2 aquifer.

A crucial step towards the correct representation of the mixing zone on the salt-freshwater interface was the proper combination of values of the two, BOIM0 and BOIM100-modified, SW concentration fields. The next figure illustrates the mean squared error (MSE) generated by the ANN for the actual data of the homogeneous calibration images, expressing the difference between the calculated and the real SW%. For concentrations closer to 0%, the BOIM0-modified subsets generate less MSE. The same applies to the BOIM100-modified figures for bigger SW concentrations. A prediction combination based on this error distribution was formulated. 

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig10.JPG" alt="l try">
Mean squared error generated by the BOIM0 and BOIM100 modified subsets for the three homogeneous aquifers and the proposed optimum combination of the predictions

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig11.png" alt="l try">
Experimental images (left) and saltwater concentration fields (right) generated by the proposed ML technique 

#### Folder material description

Scripts: [RegressionTestData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.2%20Regression%20Prediction/RegressionTestData.m) (prepares data for testing),  

[ANNPredictionRegression.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.2%20Regression%20Prediction/ANNPredictionRegression.m) (executes the neural prediction),

Data: ANNregression.mat (pre-trained regression neural network), 

Str10907801090.mat (aquifer heterogenous structure derived from the classification analysis of the previous section), 

Layered3SW0.mat, Layered3SW100.mat, Layered3Test1.mat (Green (G) light instensity values of saline intrusion test images in a heterogeneous aquifer)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

### c) [Genetic algorithm optimization](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization)
This part determines the optimum combination between the Perfect C=0% and Predect C=100% generated by the ANN in the previous section.

The objective function optimized using the Genetic algorithm
<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig12.PNG" alt="l try">

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig13.png" alt="l try">
Calculation of weights for the weighted averaging of generated Mean Squared Erro

Scripts: [GeneticAlgorithmSWCombination.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/GeneticAlgorithmSWCombination.m) (main script executing the genetic algorithm iotimization), 

[ObjectiveSWcombination.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/ObjectiveSWcombination.m) (the objective function that was optimmized), 

[gaplotbestcustom.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/gaplotbestcustom.m) (ploting function)

Data: PredictionC0.mat, PredictionC100.mat (ANN prediction results for the 24 calibration images,corresponding to 3 homogeneous aquifers), 

RealSW.mat (the actual calibration concentration of the 24 images, used to evaluate the preformance of the neural prediction)

