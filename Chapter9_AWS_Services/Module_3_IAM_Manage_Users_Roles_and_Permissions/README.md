# Chapter9
This module we will configure Identity & Access Management (IAM)

# Name: Identity & Access Management (IAM)

# Description: 

IAM

Need to manage access to AWS services and resources

    Manages who is allowed to access services and resources

    Create and manage AWS users and groups

    Assign policies (set of permissions)


    By default you have a root user

    root user has unlimited privileges

    

Best practice

    Create an admin user with less privileges

        Security, Identity, & Compliance

            IAM

            Manage Access

                Create Users, Groups and Permissions

                Manage access to 

                    Compute     Storage

                    Database    App Services

                Admin user to manage the whole AWS account, but won't have access to billing and credit card information

                    Can administer system accounts, like Jenkins, etc

                
                System  Users

                    E.g. Jenkins deploys Docker container on AWS

                    E.g. Jenkins pushes Docker images to AWS Docker Repo

                    Do something on your AWS cloud



                IAM Dashboard displays IAM resources


                    User groups

                        For granting access to multiple IAM users        
                    
                    Users - Human Users, System Users

                        You give access to these IAM users           
                    
                    Roles

                        Multple services may require roles

                        Granting AWS services to other AWS services

                            EC2 ------> S3

                        
                    IAM Role versus IAM User

                        Human or System Users - IAM EC2 Full Access Policy

                                |
                                |
                                V

                               EC2  <--------------------- AWS Service - will be granted access to a service

                                                            Policies cannot be assigned to AWS Services directly

                                                            Assign AWS services 
                                                            a IAM Role

                                                            Then assign policies to the Role

                        Create IAM User - 

                        Assign Policy to IAM User


                                                                  |-----------------------ECS (R)      Create IAM Role
                                EC2 ------------------------------|
                                                                  |-----------------------EKS (R)      Assign Policy to IAM Role

                                                                                                        Two Step Process

                                                                                                        1) Assign Role to AWS Service

                                                                                                        2) Attach Policies to that Role


                        Create an IAM User

                        Admin User

                            Add User

                            Specify user details

                            User name: admin

                            * Provide user access to the AWS Management Console - optional - Select which access you allow UI or CLI or both

                            * Users must create a new password at next sign-in - Recommended

                            Next



                            UI - AWS Management Console - email and password

                            AWS CLI - Access Key Pair

                            AWS User has separate credentials for each type


                            Set Permissions

                            For Admin user select - Attach policies directly

                            Permission Policies - Best Practice: Least Privilege Principle

                                AdministrationAccess

                                {
                                    "Version": "2012-10-17"
                                    "Statement": [
                                        {
                                            "Effect": "Allow",
                                            "Action": "*",
                                            "Resource": "*"
                                        }
                                    ]
                                }

                            Next

                            Create User


                        Login with Admin User

                        Programmatic Access

                            Under admin click dropdown

                            Select Security Credential

                            Create Access Key

                                Leave description blank



                            


                                                            





# Usage


    