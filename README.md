# Train-delay-prediction

# Project Overview

**Objective:** Predict delays at departure in Belgium Railway system.

**Data:** CSV file

* Records: 72,609

* Features:18

## Code and Resources used:

**Python Version**: 3.7

**Packages**: pandas, numpy, sklearn, matplotlib, seaborn, streamlit

## **1.  Exploratoy Data Analysis (EDA)**

*  Loading data
*  Statistics of the data frame
*  Data visualization
*  Data Cleaning
*  Fixing formats

### Data Visualization

<img src= "https://github.com/jsroa15/Train-delay-prediction/blob/main/images/arrivalperhour.jpg" width="400"/>
<img src= "https://github.com/jsroa15/Train-delay-prediction/blob/main/images/histandbox.jpg" width="600"/>
<img src= "https://github.com/jsroa15/Train-delay-prediction/blob/main/images/scatter.jpg" width="400"/>

Histogram and Box Plots show that there are many outliers in the continous features.

### Data Cleaning (Missing values and formats)

<img src= "https://github.com/jsroa15/Train-delay-prediction/blob/main/images/missig%20values.jpg" width="300"/>

Missing values were handled with imputation based on some rules that were present in the dataset. For example, some features presented the same value of another feature.

<img src= "https://github.com/jsroa15/Train-delay-prediction/blob/main/images/missin%20values%20function.jpg" width="800"/>

## **2.  Feature Engineering**

New features were created:

* rush_hour: Binary feature, it's equal to 1 if the train is traveling within a given rush hour. 0 otherwise.
* difference: Difference between delay at arrival and delay at departure.
* same_line: Binary feature, it's equal to 1 if the train arrives at the same line that departed. 0 otherwise.


