# Chapter9
This module we will explore the Virtual Private Cloud

# Name: Virtual Private Cloud - Manage Private Network on AWS

# Description: 

In the Networking & Content Delivery

    VPC - Isolated Cloud Resource


    Virtual Private Cloud

        VPC for each Region

        VPC is your own isolated network is the cloud

        VPC spans all the AZs (Subnet) in that Region

        Private network isolated from others

    For Corporations

        Multiple VPCs in different regions

        A Virtual representation of network infrastructure

        Setup of servers, network configuration moved to cloud

        Whenever you create an EC2 instance it has to run in a VPC


        VPC is the entire Private Network

        Subnet for each Availability Zone

        Private and Public Subnets

            In a configuaration you can not say I want a Private Subnet and a Public Subnet

            You configure the Firewall rule which makes it either a private or a public network

                When you block all the traffic from the internet you are creating a Private Subnet

                    A use case for this is for you database use

                Other services inside the VPC still ahve access

                You have Public subnet grants access from external traffic, such as the web.

                Your web environment would be accessible from you externally on the the internet, while being able to 
                connect with the database on the private network, because they are in the same VPC.

            IP Addresses
                
                Internal IP range on VPC level based on CIDR

                Not for outside web traffic

        Controlling Access

            Internet Gateway

                Internet Gateway connects the VPC to the outside internet


                For communication inside the VPC

                Secure your components

                Control access to your VPC

                Control access to your individual server instances, using a firewall


        Security

            Configure access on subnet level using NACL - Network Access Control List

            Configure access on a instance level - Security Group






            


# Usage


    