# Project 4 - West Nile Virus Prediction 

## Directory Structure
<details>
  <summary>Expand here</summary>

```
West Nile prediction
|__ codes
|   |__    01_Data_Cleaning.ipynb
|   |__    02_Feature_Engineering.ipynb
|   |__    03_Model.ipynb
|__ datasets
|   |__    train.csv
|   |__    test.csv   
|   |__    weather.csv 
|   |__    spray.csv 
|__ README.md
|__ requirement.txt
|__ Project4_presentation.pdf
```
</details>

# The Data Science Process

## Problem Statement

West Nile virus (WNV) is transmitted through the bite of an infected mosquito and has the potential to cause prolonged disability or death in people who are infected. 
Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever, to serious neurological illnesses that can result in death. 
West Nile Virus-related hospitalizations and follow-ups in the United States cost $778 million in health care expenses and lost productivity from 1999 through 2012.

Given that we do not have no data/information on how many counts of mosquitoes in the environment, we can only rely on trap data for inferences whether aerial spray or mosquitoes trap are effective to reduce West Nile Virus (WNV)
Our team aims to build a classifier model to predict presence of West Nile Virus in Chicago supporting the Chicago Department of Public Health in its efforts to prevent West Nile Virus, lowering 
the number of human infections. 

## Cleanup and Pre-processing with EDA

1. Cleanup Weather Dataset
    - Replace "M" & "-" as null value (because they are missing data) and "  T" as 0 (because T is insignificant value i.e 0.00001)
    - Fill null values
    - Cleanup CodeSum
    - Resolve duplicates 
2. Cleanup Spray Dataset
    - Drop duplicated data
    - Drop Time column
3. Cleanup Training Dataset
    - EDA
        - Presence of West Nile Virus (2007 - 2013)
        - Presence of West Nile Virus in traps - Month
        - Presence of West Nile Virus in traps - Year
        - Types of Mosquitoes captured in traps
        - Counts of traps with mosquitoes carrying West Nile Virus
        - Counts of species captured in traps over the years
        - Sum of Mosquitoes (Year, Month)
        - Sum of Mosquitoes (Year, Month) by Species
        - Traps
    - Resolve duplicates in the data due to the limits in data collection by summing the number of mosquitoes, as the number of mosquitoes captured for each row seems to be limited to 50. 
        

## Feature Engineering
1. Create Weather Elements
2. Add Feature  - rolling window
        - 7,15 and 30 days rolling window for PrecipTotal, TAvg, AvgSpeed
2. Feature for Date/Time
    - 2.1 Calculate the maximum day of the year that WNV was the highest, and create a feature that calculates the date difference.
3. Feature for Lat/Long
    - 3.1  Create a cluster using DBSCAN, and select the top 4 clusters as our hotspots. Calculate the distance using haversine.
4. Effectiveness of spraying for WnV carriers in 2011 and 2013
5. Create Dummies for Trap, Species and Street


## Model Evaluation/Metrics

We have imbalance dataset where the minority is our positive class (WNV is present), and model built with the original dataset is predicting almost everything as negative class (WNV is not present). We addressed the issue by using oversampling through the SMOTE technique.

We look at the coefficient and the top 10 features using RidgeClassifier,  some weather feature like WetBulb, Sunrise, TAvg, and featured engineered like days_from_max_wnv - where we calculate the date difference from the peak day of the year, and also the rolling window where we take the average for the last 7, 15 and 30 days for some weather features appeared as well in the top 10.

We built several models including deep learning with l1 regularizer, dropout and early stop, however this is still around 55% ROC.

From the pipeline using GridSearchCSV, ExtraTreesClassifier was chosen as best model in terms of ROC-AUC with 74%. We also use However, if we want to check the Precision, or how well it performs in terms of True Positive , the GradientBoost has the highest precision.  We decided to send the prediction of all models to Kaggle to see how is the ROC-AUC score for each model.

For future improvement, we can further analyse how to incorporate spray in our models, and this will improve the false negative if we have more spray data for all years.

| Model       | Public Score in ROC-AUC (Kaggle) |
| ----------- | -----------      |
| Ridge   | 0.68851             |
| LR   | 0.68589             |
| ET      | 0.64059            |
| ADA   |  0.64059             |
| RF   | 0.62510             |
| Gradient   | 0.61897             |


## Recommendation:

Develop and Strengthen Mosquito Control Program in Chicago

1) Continue to spray to reduce counts of mosquitoes as it is scientific proven to kill mosquitoes temporarily. Also, increase usage of traps and surveillance in locations near Trap T900 and Trap T115.

2) Remove all potential breeding areas near the most common traps with presence of west nile virus - T900, T115.
Remove, puncture or regularly drain all water-retaining objects such as tin cans, buckets, holes in trees, clogged gutters and down spouts, old tires, birdbaths, trash can lids and shallow fishless ponds. 
Fix dripping outside water faucets and sprinklers.  Monitor ponds and sources of water regularly for signs of mosquito larvae.

3) Chicago Department of Public Health can also support the mosquito control program by educating the residents on understanding of the mosquitoes and how they are able to prevent certain mosquito-borne diseases. 
The residents should also be aware of the pesticide spraying timings and the precautions that they can take to reduce breeding of these mosquitoes. In addition, education program can also be extending to schools to teach 
children at an early age best habits to moquito-proof their homes.

4) Strengthen Mosquito control officials who will be monitoring mosquito traps, investigating breeding sites, educating residents and schools abd obtaining feedback from the public.

5) Initiate studies on the specific species that are west nile virus carriers, better understand the behavior of these particular species to predict the mosquito larval occurence, flight behavior, offering insights to 
enable public health officials to further develop the Mosquito Control program.

## References:

1.  https://www.renesas.com/us/en/blogs/understanding-relative-humidity-and-dew-point
2.  https://www.sciencedaily.com/releases/2020/09/200915105932.htm#:~:text=West%20Nile%20virus%20spreads%20most,published%20today%20in%20eLife%20shows
3.  https://kestrelmeters.com/blogs/news/the-science-of-mosquito-abatement#:~:text=Wind%20works%20as%20a%20natural,MPH%20wind%20gust%20is%20substantial
4.  https://en.wikipedia.org/wiki/Rain#:~:text=Light%20rain%20%E2%80%94%20when%20the%20precipitation,50%20mm%20(2.0%20in)%20per