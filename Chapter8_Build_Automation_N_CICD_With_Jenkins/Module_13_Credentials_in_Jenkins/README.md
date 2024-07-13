# Chapter8
This module we review credentials in Jenkins.

# Name: Credentials in Jenkins

# Description: 

In the previous module we learned about creation of username with password type, using Credentials Plugin to store and manage them centrally.

Currently we have one jenkins store and a global domain inside that store. All credentials belong to that store and the global doman.

From the Global Credentials you can create or add credentials.

    Add New Credential

    Credentials Scopes

        System - Only available on Jenkins servers. If you are a Jenkins Administrator you are going to create a system credential. This credential is not available for Jenkins jobs.

        Global - Is assessible everywhere and all the build jobs

    Credential Types (Kind)

        Username & Password

        Certificate

        Secret File

        New types based on Plugins

        Github has its own github user access token type

Create Global and System Credential

        Add credential

        Scope: Global / System

        Username: Global / System

        Password: password

        ID: Global / System


Under the Dashboard

        Click my-multi-branch-pipeline 

        Credential is on the left pane, which is a third scope and is limited to your folder/project, available only with multibranch. This allows you to credentials scoped to your project.

        Click Credential

            Dashboard ---> my-multi-branch-pipeline ---> credential ---> folder

            Click on domain - Global credentials

            Add Credentials - now in the project credentials domain

                Username: my-pipeline

                Password: password

                ID: my-pipeline

                Click create


        Now under

            Dashboard

            my-multi-branch-pipeline

            Credentials

            System credential is not accessible and there is a my-multi-branch-pipeline store






# Usage


    