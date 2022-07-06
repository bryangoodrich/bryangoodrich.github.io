---
layout: post
title: Applied Linear Statistical Models (ALSM)
date: 2022-07-06
excerpt: Python and R code for ALSM Examples
tags: [code, statistics, machine learning]
comments: true
image: ../assets/img/alsm.jpg
---

![Book logo](../assets/img/alsm.jpg "Applied Linear Statistical Models (5th edition)")

# Applied Linear Statistical Models

## Background
It has been over a decade since I learned R from scratch, long before Stackoverflow was there to help. With a background in statistical theory, I did not have much experience with application. Even though I was good enough at coding in R, I did not fully grasp statistical programming. 

The UC Davis statistics program uses ALSM for their 106 and 108 courses on design of experiments and linear regression. This prompted me to buy the text, teach myself the content, and replicate all the examples with the included sample data using R. The results of this labor were the first iteration of the [ALSM codebase](https://github.com/bryangoodrich/ALSM), which the output can still be found at [RPubs](https://rpubs.com/bryangoodrich).

## A Python Update
Now as a data engineer, I live and breathe Python. In late 2021 we ran into a non-linear regression problem that prompted me to run through the entire exercise again&mdash;much easier this time around. Using Jupyter notebooks makes this a lot easier because the code and results are together in the codebase and can be displayed directly from GitHub. 

## Lessons
While I did not go as far as the RPubs examples, I have covered the majority of the linear regression content. Chapters 5 and 13 needs to be finished and Chapter 14 has not even been started. Additional descriptions and notation could be added to the sections where no code or examples are available to really tie the book content into the notebooks. I may or may not get to the design of experiments chapters. I know it is invaluable to learn!

If any interested parties want to help maintain this codebase, feel free to submit pull requests.

Cheers

## Table of Contents

[Chapter 1](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter01.ipynb) &ndash; Linear Regression with One Predictor Variable

[Chapter 2](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter02.ipynb) &ndash; Inferences in Regression and Correlation Analysis

[Chapter 3](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter03.ipynb) &ndash; Diagnostics and Remedial Measures

[Chapter 4](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter04.ipynb) &ndash; Simultaneous Inferences and Other Topics in Regression Analysis

[Chapter 5](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter05.ipynb) &ndash; Matrix Approach to Simple Linear Regression Analysis

[Chapter 6](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter06.ipynb) &ndash; Multiple Regression I

[Chapter 7](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter07.ipynb) &ndash; Multiple Regression II

[Chapter 8](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter08.ipynb) &ndash; Regression Models for Quantitative and Qualitative Predicators

[Chapter 9](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter00.ipynb) &ndash; Building the Regression Model I: Model Selection and Validation

[Chapter 10](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter10.ipynb) &ndash; Building the Regression Model II: Diagnostics

[Chapter 11](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter11.ipynb) &ndash; Building the Regression Model III: Remedial Measures

[Chapter 12](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter12.ipynb) &ndash; Autocorrelation in Time Series Data

[Chapter 13](https://github.com/bryangoodrich/ALSM/blob/main/notebooks/chapter13.ipynb) &ndash; Introduction to Nonlinear Regression and Neural Networks

Chapter 14 &ndash; Logistic Regression, Poisson Regression, and Generalized Linear Models

