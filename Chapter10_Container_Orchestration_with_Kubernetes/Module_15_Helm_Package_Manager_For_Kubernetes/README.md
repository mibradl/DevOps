# Chapter10
This module we will explore Helm Package Manager

# Name: Helm Package Manager for Kubernetes

# Description: 

Helm - Package Manager

    Main concepts of Helm

        Helm changes a lot from version to version

        Important to understand the basic common principles and use cases


    Helm Explained

        Overview

            What is Helm?

            What are Helm Charts?

            How to use them? 

            When to use them?


    What is Helm?

        Package Manager for Kubernetes

        To package YAML Files and distribute them in public and private repositories

        Suppose you want to install Elastic Search in you cluster

            You would need Stateful Set for stateful apps 

            You would need a Config Map with external configuration

            You would need Secret for secret data

            K8s users with permissions

            Services

            If you were to set these all up it would be a tedious job

            
            
            Bundle of YAML files are called Helm Charts

            Create your own Helm Charts with Helm

            Push them to Helm Repository

            Download and use existing ones



            Helm Charts - Already

                Database Apps - MySQL, MongoDB, ElasticSearch

                Monitoring Apps - Prometheus

                So you can reuse that configuration 


                Sharing Helm Charts

                    Helm search <keyword>

                    Public Registries: Artifact Hub - to search for service there

                    Private Registries: Share in organizations


            Second Feature:

                Templating Engine

                micro-svc   micro-svd   micro-svc   micro-svc   micro-svc

                Deployment and Service configurations almost the same

                YAML Config

                apiVersion: v1
                kind: Pod
                metadata:
                  name: my-app
                spec:
                  containers:
                  - name: my-app-container
                  - image: my-app-image
                  - port: 9001

                Without Helm you would write separate configuration files for each



                With Helm

                    1) Define a common blueprint 

                    2) Dynamic values are replaced by placeholders

                        That would be a template file

                apiVersion: v1
                kind: Pod
                metadata:
                  name: {{ .Values.name }}
                spec:
                  containers:
                  - name: {{ .Values.container.name }}
                  - image: {{ .Value.container.image }}
                  - port: {{ .Value.container.port }}

                {{ .Values }} values.yaml

                Object - which is created based on the values defined

                Values defined either via yaml file or --set flag

                Instead of having many YAML files you have just one

                Practical for CI / CD

                In your Build you can replace the values on the fly



                Another Use Case for Using Helm Features

                    When you deploy the same applications across different environments

                    Development         Staging         Production


                    Instead of deploying the individual yaml files separately in each cluster package them up to make own chart and use them to redeploy with one command, which will make the process easier

                Helm Chart Structure

                    Directory structure:

                    mychart/            The top level folder - name of the Chart
                      Chart.yaml        Chart.yaml - meta info about chart (name, dependencies, version)
                      values.yaml       values for the template files - default values you can overwrite
                      charts/           chart dependencies inside
                      templates/        the actual template files

                    helm install <chartname>

                    template will be filled with the values from values.yaml

                    Optionally, other files like ReadMe or license file

                    Values Injections into a template

                    
                    values.yaml

                    imageName: myapp
                    port: 8080
                    version: 1.0.0

                    default values can be overwritten

                    helm install --values=my-values.yaml <chartname>

                    
                    my-values.yaml  - override values

                    version: 2.0.0


                    result

                    imageName: myapp
                    port: 8080
                    version: 2.0.0

                    .Value object


                    Or on Command Line: helm install -set version=2.0.0


                Release Management

                    Keeping track of all chart executions:

                    Revision            Request

                    1                   Installed Chart

                    2                   Upgraded to v 2.0.0


                    helm install <chartname>

                    helm upgrade <chartname>

                    Upgrade - Changes are applied to existing deployment instead of creating a new one

                    Handling rollbacks

                    Specific version rollback

                    helm rollback <chartname>





















# Usage


    