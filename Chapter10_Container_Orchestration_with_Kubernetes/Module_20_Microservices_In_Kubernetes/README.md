# Chapter10
This module we will explore Microservices in Kubernetes


# Name: Introduction to Microservices


# Description: 

Overview

Main Benefits of Microservices Applications

What you need to know to deploy Microservice Applications in Kubernetes Cluster

    Kubernetes emerged as a Platform for Microservices Applications

    From monolith to microservices

        The monolith typical application evolved into applications that are made up multiple small applications

        That can be developed, deployed, and managed independently

        Big complicate Applications like: Linked in

        App             App                 App             App

        User            Messenger           Job             Blog
        Service         Service             Service         Service


        Each business functionality is encapsilated into own Microservice

        Smaller independent applications that developers manage

        Each Microservice can be developed, packaged and released independently

        Can make changes in 1 MS doesn't affect other MS

        Cleaner code and less interconnected logic

        Each MS can be developed by a separate Developer team


How do MS communicate?

    They communicate via interfaces called Service-to-Service API Calls

       E.g. Code in User Service sends API request to Messenger Service

       Communication code  is inside the Microservices applications

    Another way is thru Message-Based Communication

        Known as a Message Broker

        MS sends message to Message Broker and requests that message be sent to another MS and Message Broker manages communication between them


            Example products are Redis, Rabbit


    Another architecture that is becoming popular in Kubernetes world is service-mesh

        Instead of a single Message Broker, each MS has its own helper application and handles the communication for that MS

        Helper application is a SideCar container in Kubernetes

        One of the popular service mesh is Istio


    What you need to know as a DevOps Engineer

        What part of MS is your responsibility?

        Normal tasks:

            Deploy existing MS Application in a Kubernetes cluster

            Developers developed these MS

            DevOps Eng needs to know what information you need from developers?

                What MS you need to deploy?

                Which MS is talking to which MS?

                How are they communicating?

                    Directly using the API?

                    Message Broker?

                    Service Mesh?

                Which database are they using?  3rd Party Services depend on to run successfully

                On which port does each MS run?

            Prepare K8s environment

                Deploy any 3rd party apps

                Create Secrets and ConfigMap for Microservices

                Create Deployment and Service for each MS, and if it runs in the same namespace or separately

                











# Usage


    