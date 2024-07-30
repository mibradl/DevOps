# Chapter9
This module we will explore the AWS CLI

# Name: Introduction to AWS CLI - Part 1

# Description: 

AWS CLI - Part 1

Previously we created an EC2 instance using the AWS UI

    What if you need programmatic Access

    Better to execute with script instead of manually


AWS Command Line Interface

    Powerful command line tool

    Commands for every AWS service and resource(s)

        Automation and speed            Version Control

        Efficiency, Batch Operations    Reprocibility

    Learn how to:

        Install AWS CLI

        Use AWS CLI

        Perform actions completed previously to create AWS EC2 instance


    Created via AWS UI

        EC2 instance

        1 Security Group

        SSH key pair



    Create new via CLI

        Create new EC2 Instance

        Create new Security Group

        Create new SSH key pair



Install and Configure AWS Command Line Tool

    On Mac OS

        brew install aswcli

        check AWS installation guide - docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

            Linux

            Windows

            Mac OS

        Install AWS CLI version 2 or higher

        Version

            aws --version
            aws-cli/2.17.0 Python/3.11.8 Darwin/21.2.0 exe/x86_64

    Configure AWS CLI to connect to AWS account

        Connect with an AWS User

        Created admin with Administrator access, via UI

        UI Access through password

        CLI Access through Access key ID and Secret Access key

        Which AWS account do you want to use?

        Which AWS User do you want to use?


        When an AWS CLI command is executed it will use these settings to run the command

            Configuration is stored in your home directory under / .aws





Command Structure

        How AWS CLI works

        For every service there is a subset of commands

            aws <command> <subcommand>  [options and parameters]

            AWS service     which operation?

            EC2             run-instance            configure key pair

                            create-security-group



            aws = the base call to the aws program

            command = the AWS service

            subcommand = specifies which operation to perform


            aws ec2 aa - list all commands available to the ec2 service

            aws iam bb - list all commands available to the iam service

            aws ec2 run-instances
             --image-id  ami-0427090fd1714168b
             --count 1
             --instance-type t2.micro 
             --key-name MyKpCli 
             --security-group-ids sg-0f4537d84501a4c35
             --subnet-id subnet-02b7aac21a8211968

             We will create a key pair and security group

             We will reuse the subnet


Create a Security Group

            aws ec2 describe-security-groups


            Security Group needs to be created inside a VPC

            aws ec2 describe-vpcs


            aws ec2 create-security-group --group-name my-sg --description "My SG" --vpc-id vpc-0c6d0155c83f552e8
            {
                "GroupId": "sg-0f4537d84501a4c35"
            }


            aws ec2 describe-security-groups --group-id sg-0f4537d84501a4c35
            {
                "SecurityGroups": [
                    {
                        "Description": "My SG",
                        "GroupName": "my-sg",
                        "IpPermissions": [],
                        "OwnerId": "211125391769",
                        "GroupId": "sg-0f4537d84501a4c35",
                        "IpPermissionsEgress": [
                            {
                                "IpProtocol": "-1",
                                "IpRanges": [
                                    {
                                        "CidrIp": "0.0.0.0/0"
                                    }
                                ],
                                "Ipv6Ranges": [],
                                "PrefixListIds": [],
                                "UserIdGroupPairs": []
                            }
                        ],
                        "VpcId": "vpc-0c6d0155c83f552e8"
                    }
                ]
            }



            Who (which IP address) is able to access port 22

            Only 1 IP address is permitted to access port 22


            aws ec2 authorize-security-group-ingress \
>           --group-id sg-0f4537d84501a4c35 \
>           --protocol tcp \
>           --port 22 \
>           --cidr 44.203.54.164/32


            aws ec2 describe-security-groups --group-id sg-0f4537d84501a4c35



Create Key Pair


            aws ec2 create-key-pair \
            --key-name MyKpCli \
            --query 'KeyMaterial' \
            --output text > MyKpCli.pem


            michaelbradley@Michaels-iMac-2 ~ % ls -la ~/.ssh           
            total 56
            drwx------   8 michaelbradley  staff   256 Jul 29 14:12 .
            drwxr-x---+ 40 michaelbradley  staff  1280 Jul 29 14:07 ..
            -rw-r--r--   1 michaelbradley  staff     0 Jul 29 14:12 MyKpCli.pem


Create EC2 Instance

            aws ec2 run-instances \
>           --image-id  ami-0427090fd1714168b \
>           --count 1 \
>           --instance-type t2.micro \
>           --key-name MyKpCli \
>           --security-group-ids sg-0f4537d84501a4c35 \
>           --subnet-id subnet-02b7aac21a8211968


            aws ec2 describe-instances
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-0427090fd1714168b",
                    "InstanceId": "i-089031a5d0081270a",
                    "InstanceType": "t2.micro",
                    "KeyName": "MyKpCli",
                    "LaunchTime": "2024-07-29T19:00:45+00:00",
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "Placement": {
                        "AvailabilityZone": "us-east-1a",
                        "GroupName": "",
                        "Tenancy": "default"
                    },
                    "PrivateDnsName": "ip-172-31-23-228.ec2.internal",
                    "PrivateIpAddress": "172.31.23.228",
                    "ProductCodes": [],
                    "PublicDnsName": "ec2-34-234-95-103.compute-1.amazonaws.com",
                    "PublicIpAddress": "34.234.95.103",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
                    "StateTransitionReason": "",
                    "SubnetId": "subnet-02b7aac21a8211968",
                    "VpcId": "vpc-0c6d0155c83f552e8",
                    "Architecture": "x86_64",
                    "BlockDeviceMappings": [
                        {
                            "DeviceName": "/dev/xvda",
                            "Ebs": {
                                "AttachTime": "2024-07-29T19:00:46+00:00",
                                "DeleteOnTermination": true,
                                "Status": "attached",
                                "VolumeId": "vol-05f561d5afe8427b2"
                            }
                        }









    



# Usage


michaelbradley@Michaels-iMac-2 ~ % aws --version
aws-cli/2.17.0 Python/3.11.8 Darwin/21.2.0 exe/x86_64


michaelbradley@Michaels-iMac-2 ~ % ls ~/.aws/
config		credentials


michaelbradley@Michaels-iMac-2 ~ % aws ec2 authorize-security-group-ingress \
> --group-id sg-0f4537d84501a4c35 \
> --protocol tcp \
> --port 22 \
> --cidr 44.203.54.164/32




aws ec2 create-key-pair \
--key-name MyKpCli \
--query 'KeyMaterial' \
--output text > MyKpCli.pem




Created instances


michaelbradley@Michaels-iMac-2 ~ % aws ec2 describe-instances
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-0427090fd1714168b",
                    "InstanceId": "i-089031a5d0081270a",
                    "InstanceType": "t2.micro",
                    "KeyName": "MyKpCli",
                    "LaunchTime": "2024-07-29T19:00:45+00:00",
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "Placement": {
                        "AvailabilityZone": "us-east-1a",
                        "GroupName": "",
                        "Tenancy": "default"
                    },
                    "PrivateDnsName": "ip-172-31-23-228.ec2.internal",
                    "PrivateIpAddress": "172.31.23.228",
                    "ProductCodes": [],
                    "PublicDnsName": "ec2-34-234-95-103.compute-1.amazonaws.com",
                    "PublicIpAddress": "34.234.95.103",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
                    "StateTransitionReason": "",
                    "SubnetId": "subnet-02b7aac21a8211968",
                    "VpcId": "vpc-0c6d0155c83f552e8",
                    "Architecture": "x86_64",
                    "BlockDeviceMappings": [
                        {
                            "DeviceName": "/dev/xvda",
                            "Ebs": {
                                "AttachTime": "2024-07-29T19:00:46+00:00",
                                "DeleteOnTermination": true,
                                "Status": "attached",
                                "VolumeId": "vol-05f561d5afe8427b2"
                            }
                        }
                    ],
                    "ClientToken": "a83c2af8-5805-4da0-bc76-f74396ab7e2c",
                    "EbsOptimized": false,
                    "EnaSupport": true,
                    "Hypervisor": "xen",
                    "NetworkInterfaces": [
                        {
                            "Association": {
                                "IpOwnerId": "amazon",
                                "PublicDnsName": "ec2-34-234-95-103.compute-1.amazonaws.com",
                                "PublicIp": "34.234.95.103"
                            },
                            "Attachment": {
                                "AttachTime": "2024-07-29T19:00:45+00:00",
                                "AttachmentId": "eni-attach-089bed5a6f7edfc02",
                                "DeleteOnTermination": true,
                                "DeviceIndex": 0,
                                "Status": "attached",
                                "NetworkCardIndex": 0
                            },
                            "Description": "",
                            "Groups": [
                                {
                                    "GroupName": "my-sg",
                                    "GroupId": "sg-0f4537d84501a4c35"
                                }
                            ],
                            "Ipv6Addresses": [],
                            "MacAddress": "0a:ff:c8:35:5e:a1",
                            "NetworkInterfaceId": "eni-019c399339be950f9",
                            "OwnerId": "211125391769",
                            "PrivateDnsName": "ip-172-31-23-228.ec2.internal",
                            "PrivateIpAddress": "172.31.23.228",
                            "PrivateIpAddresses": [
                                {
                                    "Association": {
                                        "IpOwnerId": "amazon",
                                        "PublicDnsName": "ec2-34-234-95-103.compute-1.amazonaws.com",
                                        "PublicIp": "34.234.95.103"
                                    },
                                    "Primary": true,
                                    "PrivateDnsName": "ip-172-31-23-228.ec2.internal",
                                    "PrivateIpAddress": "172.31.23.228"
                                }
                            ],
                            "SourceDestCheck": true,
                            "Status": "in-use",
                            "SubnetId": "subnet-02b7aac21a8211968",
                            "VpcId": "vpc-0c6d0155c83f552e8",
                            "InterfaceType": "interface"
                        }
                    ],
                    "RootDeviceName": "/dev/xvda",
                    "RootDeviceType": "ebs",
                    "SecurityGroups": [
                        {
                            "GroupName": "my-sg",
                            "GroupId": "sg-0f4537d84501a4c35"
                        }
                    ],
                    "SourceDestCheck": true,
                    "VirtualizationType": "hvm",
                    "CpuOptions": {
                        "CoreCount": 1,
                        "ThreadsPerCore": 1
                    },
                    "CapacityReservationSpecification": {
                        "CapacityReservationPreference": "open"
                    },
                    "HibernationOptions": {
                        "Configured": false
                    },
                    "MetadataOptions": {
                        "State": "applied",
                        "HttpTokens": "required",
                        "HttpPutResponseHopLimit": 2,
                        "HttpEndpoint": "enabled",
                        "HttpProtocolIpv6": "disabled",
                        "InstanceMetadataTags": "disabled"
                    },
                    "EnclaveOptions": {
                        "Enabled": false
                    },
                    "BootMode": "uefi-preferred",
                    "PlatformDetails": "Linux/UNIX",
                    "UsageOperation": "RunInstances",
                    "UsageOperationUpdateTime": "2024-07-29T19:00:45+00:00",
                    "PrivateDnsNameOptions": {
                        "HostnameType": "ip-name",
                        "EnableResourceNameDnsARecord": false,
                        "EnableResourceNameDnsAAAARecord": false
                    },
                    "MaintenanceOptions": {
                        "AutoRecovery": "default"
                    },
                    "CurrentInstanceBootMode": "legacy-bios"
                }
            ],
            "OwnerId": "211125391769",
            "ReservationId": "r-072e58b5be19da0d4"
        },
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-0b72821e2f351e396",
                    "InstanceId": "i-02045bde58b74bf07",
                    "InstanceType": "t2.micro",
                    "KeyName": "docker-server",
                    "LaunchTime": "2024-07-21T20:47:51+00:00",
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "Placement": {
                        "AvailabilityZone": "us-east-1d",
                        "GroupName": "",
                        "Tenancy": "default"
                    },
                    "PrivateDnsName": "ip-172-31-89-89.ec2.internal",
                    "PrivateIpAddress": "172.31.89.89",
                    "ProductCodes": [],
                    "PublicDnsName": "ec2-44-203-54-164.compute-1.amazonaws.com",
                    "PublicIpAddress": "44.203.54.164",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
                    "StateTransitionReason": "",
                    "SubnetId": "subnet-0e279712db9fec20d",
                    "VpcId": "vpc-0c6d0155c83f552e8",
                    "Architecture": "x86_64",
                    "BlockDeviceMappings": [
                        {
                            "DeviceName": "/dev/xvda",
                            "Ebs": {
                                "AttachTime": "2024-07-21T20:47:52+00:00",
                                "DeleteOnTermination": true,
                                "Status": "attached",
                                "VolumeId": "vol-0a41bca2d5caa0987"
                            }
                        }
                    ],
                    "ClientToken": "7ca09be5-e26d-43b7-bb26-ea5644e61262",
                    "EbsOptimized": false,
                    "EnaSupport": true,
                    "Hypervisor": "xen",

    