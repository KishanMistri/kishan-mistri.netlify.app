---
draft: false
slides: example
url_pdf: ""
title: Personalized Cancer Diagnosis
subtitle: Classify the 9 cancer classes from text and categorical data.
date: 2022-12-20T07:12:41.499Z
summary: Classify the 9 cancer classes from text and categorical data.
url_video: ""
featured: true
external_link: https://github.com/KishanMistri/Personalise-Cancer-Diagnosis/blob/main/README.md
url_slides: ""
tags:
  - NLP
categories:
  - Machine Learning
links:
  - url: https://kmistri-personalise-cancer-diagnosis.streamlit.app/
    name: Demo
    icon_pack: fas
image:
  caption: Diagnosis Cancer Workflow
  focal_point: Smart
  filename: featured.jpg
url_code: ""
---
From the expert, the time to diagnosis of cancer takes a lot of time as it includes new studies/papers, which makes this a time-consuming and exhaustive process. With machine learning, we can fast-track the majority of scenarios and help the expert get updated details.

I have used below classical machine learning algorithms for the problem.

1: Naive Bayes

2: K Nearest Neighbors

3: Logistic Regression

4: Support Vector Machine (SVM)

5: Random Forest Classifier

6: StackedClassifier (Ensemble)

7: MaxVoting Classifier (Ensemble)

As you might know, these algorithms have their limitation and advantages, I have tried to incorporate the best use of them by remediating the problems. Like

* The curse of dimensionality has been addressed by Response Coding.
* Class imbalance can be tuned with stratified splits and using the Class weight parameter whenever exploitable.
* Compute intensive Hyper-Tuning with parallelism when needed.
* At last, the beautiful interface & ton of integration of streamlit used.