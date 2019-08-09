# analyze-tremor-bradykinesia-PD
A Python (2.7) package that enables digital measurements of **resting tremor** and **bradykinesia** in patients with Parkinson's Disease. These measurements utilize accelerometer data as input from a single wrist-worn wearable device located on the most affected side.

## Overview
Objective assessment of Parkinson’s disease symptoms during free-living conditions can provide valuable information for disease management and help accelerate the development of new therapies. Traditional assessments are episodic (require patients to come into a clinic at specified intervals for assessment), low-fidelity (assessments are paper-based rudimentary numerical rating scales), and subjective in nature (assessments are reported by clinicians and patients). Recent advances in wearable sensor technology have enabled the use of objective digital measurements to assess Parkinson's disease symptoms in free-living conditions. Current digital measurements often require the use of multiple devices or performance of prescribed motor activites, which is not optimal for monitoring free-living conditions.

Herein we present our source code used for the development and validation of a method aimed at objective assessment of **resting tremor** and **bradykinesia** (two common symptoms of Parkinson's disease) using accelerometer data captured with a single wrist-worn device during the performance of unscripted activities. Our method combines context detection and symptom assessment by using heuristic and machine learning models in a hierarchical framework to provide continuous monitoring by sequentially processing epochs of raw sensor data. Results of our analysis show that sensor derived continuous measures of resting tremor and bradykinesia achieve good to strong agreement with clinical assessment of symptom severity and are able to discriminate between treatment related changes in Parkinsonian motor states (ON/OFF).

## Software Requirements
There are 7 main packages used in this repository. The names of the packages and versions are listed below:

* ``pandas``: 0.23.4+
* ``scipy``: 1.2.2+
* ``scikit-learn``: 0.20.0+
* ``statsmodels``: 0.8.0+
* ``tsfresh``: 0.11.0
* ``numpy``: 1.16.4+

If necessary, the listed requirements can be installed as follows:
```
pip install -r requirements.txt
```

## Repository Contents
Our method for continuous objective of assessment of resting tremor and bradykinesia follows the hierarchical framework seen below:

<p align="center">
  <img width="500" height="450" src="https://raw.githubusercontent.com/NikhilMahadevan/analyze-tremor-bradykinesia-PD/update-readme/images/pd_analytics_diagram.png?token=ABFEV6QYQCBKJ36DQ2QQECK5JWNN2">
</p>

This system utilizes heuristic and machine learning models for context detection and symptom assessment. This repository contains the source code for each module. Currently the availablity of the data set used to support the findings of this work is restricted; the data set was used under contract for this study. All heuristic models are available, but for machine learning models only the code for generating the signal based features used as input into model training and model parameters are available. Users of source code will have to provide their own labeled data sets for training each of the machine learning models.

The repository is organized as follows:
* __classifiers__: code to generate classifiers in each node of the tree above

|File | Model Type | Description |
| --- | --- | --- |
| hand_movement_classifier.py | Heuristic | Binary classification of hand movement |
| resting_tremor_classifier.py | Machine Learning | Binary classification of resting tremor |
| gait_classifier.py | Machine Learning | Binary classification of gait |
| resting_tremor_amplitude.py | Heuristic | Compute tremor amplitude (during bouts of tremor) |
| hand_movement_features.py | Heuristic | Compute amplitude of hand movement and smoothness of hand movement (jerk metric) |

* __endpoints__: code to filter model predictions per the tree above and summarize measures of resting tremor and bradykinesia for a given period of time

|File| Description|
|---|---|
| filter_classifier_predictions.py | Filter model predictions per tree above |
| bradykinesia_endpoints.py | <ul><li>mean bouts of no hand movement</li><li>percentage of no hand movement</li><li>mean hand movement amplitude</li><li>95th percentile of smoothness of hand movement</li></ul> |
| bradykinesia_endpoints.py | Calculate mean bouts of no hand movement, percentage of no hand movement, mean hand movement amplitude, and 95th percentile of smoothness of hand movement |
| resting_tremor_endpoints.py | Calculate percentage of tremor (tremor constancy) and 85th percentile of tremor amplitude |

* __signal_preprocessing__: signal preprocessing functions applied on accelerometer data prior to feature extraction
* __features__: signal features extracted from accelerometer data used to train supervised learning machine learning models