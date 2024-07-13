# Chapter8
This module we will Wrap Up - Jenkins Jobs.

# Name: Wrap Up - Jenkins Jobs

# Description: 

We created three different types of build jobs:

1. Freestyle job

    Executing a single task as a standalone job

        Test    Build   Deploy

        Each would be a Freestyle job


2. Pipeline job - is meant for on branch

    Better solution for CI/CD

        Test    Build   Deploy

        A single pipeline for running all stages. Pipeline is like a supercede, an umbrella for tasks that were previously defined by freestyle jobs.


3. Multibranch-Pipeline job - if you want to build multiple branches you need a multibranch pipeline. 

        It allows us to define the configuration once to create one build and under the hood it creates the regular pipelines for each branch that matches the regular expression in the configuration. So, these individual pipelines have the same view as a regular pipeline when you click inside them, the console output, replay, etc. You can just test, without commiting the changes using the Jenkinsfile.  Also, you can restart from stage.

    Parent of Pipeline jobs




# Usage


    