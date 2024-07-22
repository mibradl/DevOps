# Chapter9
This module we will explore Regions & Availability Zones.

# Name: Regions and Availability Zones

# Description: 


Cloud providers have physical data centers

    When you configured your server at AWS it is running at some data center.

Data Centers are all over the world

    Doesn't make sense to have all servers in one location because when users login from another location they will experience latency.

    Thus, AWS has its data centers split up by regions

    Region = a physical location where data centers are clustered

        30 + regions

    You have to select which region you want your server to run

    Availability Zone = 1 or more discrete data centers

        90 + Availability Zones

        There are up to 5 availability zones (data centers) that allows you to replicate

    Region

        Availability Zone

            EC2 Instance                        RDS Instance

        Some Companies

            HQ - Seattle, USA               Customers are located in Asia

        Solution:

            Host your application, where your customers are located.

            If your customers are distributed you may replicate your work in multiple locations.


# Usage


    