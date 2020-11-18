---
title: "Application of computer vision on laboratory sandbox investigations"
date: 2020-06-23
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
The work was published under the title: [Optimizing Laboratory Investigations of Saline Intrusion by Incorporating Machine Learning Techniques](https://www.mdpi.com/2073-4441/12/11/2996)

## Project overview
Deriving saltwater concentrations from the values of light intensity is a long-established image processing practice in laboratory scale investigations of saline intrusion. The current repository presents a novel methodology that employs the predictive ability of machine learning algorithms in order to determine saltwater concentration fields. The proposed approach consists of three distinct parts, image pre-processing, ground profile classification (bead structure recognition) and saltwater field generation (regression). It minimizes the need for aquifer-specific calibrations, significantly shortening the experimental procedure by up to 50% of the time required. 

<img src="{{ site.url }}{{ site.baseurl }}/images/Comp.Vision.Figures/Fig1.PNG" alt="l try">
A graphical outline of the investigation

## Image pre-processing

Current part filters out the impact of back lighting in the experimental images by formulating a novel variable name Mean Homogenization Factor. This significantly helps neural training in the next stages of the investigation.

Scripts: [MeanHomoFactorCalculator.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/1%20Image%20pre-processing/MeanHomoFactorCalculator.m)

Datasets: subset1.mat, subset2.mat, subset3.mat, subset4.mat

## Classification

This part derives the heterogeneous structure (strata) of the test aquifers by conducting classification analysis on freshwater-only test images.

### a) [Classification training](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/2%20Classification/2.1%20ClassificationTraining)

Scripts: [ClassificationTrainingData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.1%20ClassificationTraining/ClassificationTrainingData.m) (prepares data for neural training), [ANNClassifiationGenerator.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.1%20ClassificationTraining/ANNClassifiationGenerator.m) (trains on parallel a deep classification ANN)

Data: subset4.mat (used as the training dataset, containing 3 freshater-only aquifer images, one for every utilized bead size)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

### b) [Classification prediction](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/2%20Classification/2.2%20Classification%20Prediction)

Scripts: [ClassificationData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ClassificationData.m) (prepares data for testing),  [ANNPrediction.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ANNPrediction.m) (executes the neural prediction), [ANNPredictionProbability.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/2%20Classification/2.2%20Classification%20Prediction/ANNPredictionProbability.m) (executes the neural prediction while further post-processing it to get optimum results)

Data: ANNclassification.mat (pre-trained deep classificstion neural network), testdataset.mat (3 stratified test aquifers)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

## Regression
This part is executed after the classification analysis and it derives the saltwater concnentration fields in the investigated test aquifers.

### a) [Regression training](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.1%20Regression%20Training)

Scripts: [RegressionTrainingData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.1%20Regression%20Training/RegressionTrainingData.m) (prepares data for neural training), [ANNTrainingRegression.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.1%20Regression%20Training/ANNTrainingRegression.m) (trains on parallel a regression ANN)

Data: Cal780G.mat, Cal1090G.mat, Cal1325G.mat (green (G) light intensity values for each one of the utilized bead sizes, each subset includes the 8 calibration concentration images) 

### b) [Regression prediction](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.2%20Regression%20Prediction)

Scripts: [RegressionTestData.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.2%20Regression%20Prediction) (prepares data for testing),  [ANNPredictionRegression.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.2%20Regression%20Prediction/ANNPredictionRegression.m) (executes the neural prediction),

Data: ANNregression.mat (pre-trained regression neural network), Str10907801090.mat (aquifer heterogenous structure derived from the classification analysis of the previous section), Layered3SW0.mat, Layered3SW100.mat, Layered3Test1.mat (Green (G) light instensity values of saline intrusion test images in a heterogeneous aquifer)

Necessary variables: MeanHomofactorRGB.mat derived from the execution of MeanHomoFactorCalculator.m

### c) [Genetic algorithm optimization](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/tree/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization)
This part determines the optimum combination between the Perfect C=0% and Predect C=100% generated by the ANN in the previous section.

Scripts: [GeneticAlgorithmSWCombination.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/GeneticAlgorithmSWCombination.m) (main script executing the genetic algorithm iotimization), [ObjectiveSWcombination.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/ObjectiveSWcombination.m) (the objective function that was optimmized), [gaplotbestcustom.m](https://github.com/GeorgiosEtsias/Computer-Vision-Sandbox-Images/blob/main/3%20Regression/3.3%20Genetic%20Algorithm%20Optimization/gaplotbestcustom.m) (ploting function)

Data: PredictionC0.m, PredictionC100.m (ANN prediction results for the 24 calibration images,corresponding to 3 homogeneous aquifers), RealSW,mat (the actual calibration concentration of the 24 images, used to evaluate the preformance of the neural prediction)

