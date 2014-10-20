Code for Large Scale Hierarchical Text Classification competition.

http://www.kaggle.com/c/lshtc

# Summary

a centroid-based flat classifier.

## Prediction

1. Selecting k-class from near the query with nearest centroid classifier.
2. Judging with binary classifier whether the query can be accepted to class.

(predict.cpp)

![predict1](https://raw.githubusercontent.com/nagadomi/kaggle-lshtc/master/figure/predict1.png)

Selecting k-candidate classes that centroid of class close to the query.
 
![predict2](https://raw.githubusercontent.com/nagadomi/kaggle-lshtc/master/figure/predict2.png)

Selecting classes that binary classifier of class returns p > 0.5. (Implementation of the binary classifier is logistic regression)

![predict3](https://raw.githubusercontent.com/nagadomi/kaggle-lshtc/master/figure/predict3.png)

## Training

For each data points..

1. Selecting k-class from near the data point with nearest centroid classifier.
2. Adding the data point as training data to dataset for each classes.

(prefetch.cpp)

For each classes..

1. Learning the binary classifier using own dataset.

(train.cpp)

![train1](https://raw.githubusercontent.com/nagadomi/kaggle-lshtc/master/figure/train1.png)
![train2](https://raw.githubusercontent.com/nagadomi/kaggle-lshtc/master/figure/train2.png)

## What are the feature 
using variant TF-IDF.
```
tf = log(number_of_term_occurs_in_document + 1)
idf = log(total_number_of_documents / (number_of_documents_containing_term + 1)) + 5
tfidf = tf * idf
```
and feature vector is normalized by L2 norm.
(code: tfidf_transformer.hpp)

## What are the metric for Centroid Classifier
using cosine similarity.

# Requirements

- Ubuntu 13.10
- g++ 4.8.1
- make
- 32GB RAM

# How to Generate the Solution

please edit SETTINGS.h first.

    make
    ./prefetch
    ./train
    ./predict

NOTE: ./prefetch is very slow. probably processing time exceeds 15 hours.

# MISC programs

## Running the Validation Test

    ./vt_prefech
    ./vt_train
    ./validation

## Simple k-NN baseline

running the validation test.

    ./vt_knn

generating the sumission.txt.

    ./knn

## Simple Nearest Centroid Classifier

running the validation test.

    ./vt_ncc

generating the sumission.txt.

    ./ncc

## Figure

<table>
  <tr>
     <th>Model</th><th>LBMaF</th><th>Training Time</th><th>Prediction Time</th>
  </tr>
  <tr>
    <td>k-NN</td><td>0.23088</td><td>n/a</td><td>10 minutes</td>
  </tr>
  <tr>
    <td>NCC</td><td>0.28931</td><td>80 seconds</td><td>2 hours</td>
  </tr>
  <tr>
    <td>NCC+BC</td><td>0.33025</td><td>15 hours</td><td>2 hours</td>
  </tr>
</table>
