# Optimizing an ML Pipeline in Azure

## Overview

This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and Scikit-learn Logistic Regression model.
This model is then compared to an Azure AutoML run.

## Summary

This dataset contains data about UCI bank marketing, we will be doing classfication to predict if the client will subscribe to a term deposit with the bank. First, we will create and optimize an Scikit Logistic Regression ML pipeline, the hyperparamaters of which we will optimize using Hyperdrive. Then we will be comparing with the Auto ML results. 

In our experiment, the best performing model is VotingEnsemble generated by AutoML.

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

1.  As part of our pipeline, we need a training script 'train.py'. Data is imported from the specified URL using 'TabularDatasetFactory'.
2.  After doing the necessary data clean up, we split the data into train and test sets. 
3.  First we need to get our 'workspace'  and 'experiment' objects running. Then we created a compute cluster with compute configuration 'vm_size' :'Standard_D2_V2' and   'max_nodes=4'.
4.  Conda_dependencies.yml file is used for creating Sckit-learn environment.
5.  Parameter Sampler - RandomSampling is used.
6.  Early stopping policy is specified for saving training time and resources.
7.  As Sklearn estimator is deprecated, ScriptRunConfig is used to define the configuration information needed to submit a run in Azure ML, including the script - 'train.py', compute target - 'compute_cluster', environment - 'sklearn_env' , and any distributed job-specific configs.
8.  With the help of HyperDriveConfig Class we define the configuration for hyperparameter space sampling, termination policy, primary metric, estimator, and the compute  target to execute the experiment runs on. 

**What are the benefits of the parameter sampler you chose?**

In our experiment, Random Sampling is used as it uses less budget as compared to Grid and Bayesian Sampling. In random sampling, hyperparameter values are randomly selected from the defined search space. It can be used for initial search and then refine the search space to improve the results.

Whereas Bayesian sampling is recommended if enough budget is available to explore the hyperparameter space. For best results, a maximum number of runs greater than or equal to 20 times the number of hyperparameters being tuned is recommended.

Grid sampling can be used if we have budget for exhaustive search  over the search space. It performs a simple grid search over all possible values. Thus in our case due to cost constraints Random Sampling is preferred.

**What are the benefits of the early stopping policy you chose?**

Early termination improves computational efficiency and when used with smaller allowable slack gives aggressive savings.Bandit Policy terminates runs where the primary metrics is not within the specified slack amount compared to the best performing run.

## AutoML
**Describe the model and hyperparameters generated by AutoML.**

AutoML generated VotingEnsemble with accuracy of 91.64.Traditional machine learning model development like the one we used above is resource-intensive, requiring significant domain knowledge and time to produce and compare dozens of models. With automated machine learning, we can accelerate the time it takes to get production-ready ML models with great ease and efficiency.

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

In the 1st model we did traditional hyperparameter tuning of Sklearn Logistic Regression by using Azure Hyperdrive. We used Random sampling and Bandit policy to further improve the performance. The pipeline gave us an accuracy of 90.86 with a regularization strength of 0.01. Whereas, the AutoML model outeperformed the traditional hyperparamater tuning with an accuracy of 91.64 using VotingEnsemble choosing LightGBM as the surrogate model.

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**

AutoML detected Class Imbalance. Imbalanced data can lead to a falsely perceived positive effect of a model's accuracy because the input data has bias towards one class.
This can be checked to improve the model performance.
