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

**Create new features**

New features were created:

* rush_hour: Binary feature, it's equal to 1 if the train is traveling within a given rush hour. 0 otherwise.
* difference: Difference between delay at arrival and delay at departure.
* same_line: Binary feature, it's equal to 1 if the train arrives at the same line that departed. 0 otherwise.

**Outlier detection**

Outliers were detected and managed with a custom function to deal with them. The function calculates the Z score to detect if the given record is an outlier, then, the function calculates the mean of the records that were not marked as outliers, and finally, outliers are replaced with the calculated mean.

```python
def modify_outliers_mean(data,features):

  '''
  This function modifies outliers with mean.
  First, the function detects outliers with Z-score, then calculates the mean of
  the feature without outliers,and finally, replaces outliers with the calculated
  mean.
  Parameters:
  -------------------
  data: dataset to be analyzed
  features: List of numerical features in the dataset
  '''

  to_del=[]
  for i in features:

    #Initialize null lists
    ind_upper=[]
    ind_lower=[]
    ind=[]

    #Calculate Z score
    data['Z_score']=(data[i]-data[i].mean())/data[i].std()
    print(data[(data['Z_score']>3) | (data['Z_score']<-3)].shape[0],' outliers detected for ',i)
    to_del.append(data[(data['Z_score']>3) | (data['Z_score']<-3)].shape[0])
    
    #Identified outliers
    ind=data[(data['Z_score']>3) | (data['Z_score']<-3)].index
    
    #Calculate mean to replace outlier

    mean_to_replace=data[(data['Z_score']<3) & (data['Z_score']>-3)][i].mean()
    
    #Replacing outliers
    data.loc[ind,i]=mean_to_replace

  print('total outliers modified: ',sum(to_del))
  data.drop(columns='Z_score',inplace=True)
```

## **3. Modeling and Evaluation**

Because of the number of categories within every categorical feature, the total amount of features was 239. A first approach to modeling was using Principal Component Analysis (PCA) to reduce dimensionality and gain computer time performance, however, model performance was not better. The next table shows the comparison between regular approach Vs PCA approach for differente models:

From the above we can see that origial data perfoms better, computing time was smaller and performance had an good improvement.

|        Model                      | Computing Time (PCA) | RMSE (PCA) | Computing Time (No PCA) | RMSE (No PCA) |
|-----------------------------------|:--------------------:|:----------:|:-----------------------:|:-------------:|
| Linear Regression (Baseline Model)| < 1 seg              | 279 Seg    | 2.7 Seg                 | 3.43E12 Seg   |
| Random Forest                     | 39 Min               | 161 Seg    | 6 Min                   | 85 Seg        |
| XGBoost                           | 2 Min                | 152 Seg    | 44 Seg                  | 89 Seg        |
| Gradient Boosting                 | 13 Min               | 192 Seg    | 4 Min                   | 124 Seg       |



