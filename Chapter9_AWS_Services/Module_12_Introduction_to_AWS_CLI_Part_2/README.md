# Chapter9
This module we will continue exploring AWS CLI 

# Name: AWS CLI - Part 2

# Description: 

Filter and Query - describe command option

        aws <command> describe-             Filter and Query

        Listed certain components - such as ec2 instances, security groups, vpcs, etc

        The describe also allows you to run --filters

        Filter your components/resources            only instances - type with t2.micro

                                                    only instances - in subnet 134

                                                    only instances - in VPC 123

        Filter the output with --query options

        What information/attributes do you want?


        Filter = picks components

        Query = picks specific attributes of components


            Filter

            aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro"

            With Query

            aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query "Reservations[].Instances[].InstanceId"
[
            "i-089031a5d0081270a",
            "i-070f04717825eef4f",
            "i-02045bde58b74bf07"
]

            aws ec2 describe-instances --filters "Name=tag:Type,Values=web-server-with-docker"

            aws ec2 describe-instances --filters "Name=tag:Type,Values=web-server-with-docker" --query "Reservations[].Instances[].InstanceId"
            [
                "i-02045bde58b74bf07"


Using IAM command

        Create a User, a Group and assign permissions


        IAM - CLI Commands

            Create AWS User


            Create User Group

                System Administrator Group

                K8s Admin Group

                Jenkins Admin Group

            Assign Policy (permissions) to that User Group

                Give user permissions for the EC2 service

                We need a policy identifier (ARN)


            Create Group Name
            aws iam create-group --group-name MyGroupCli

            Create User Name
            aws iam create-user --user-name MyUserCli

            Add User to Group 
            aws iam add-user-to-group --user-name MyUserCli --group-name MyGroupCli

            Get Group and associated users
            aws iam get-group --group-name MyGroupCli

            Assign Policy to User Group
            aws iam attach-group-policy --group-name MyGroupCli --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess

            List attached policies for Group
            aws iam list-attached-group-policies --group-name MyGroupCli
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonEC2FullAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AmazonEC2FullAccess"
{


Create Credentials for new User

            Create Credentials and require password reset
            aws iam create-login-profile --user-name MyUserCli --password MyPassword! --password-reset-required


Create Policy and assign to Group


            Create policy to change password
            aws iam create-policy --policy-name changePwd --policy-document file://changePwdPolicy.json

            Attach policy to group MyGroupCli
            aws iam attach-group-policy --group-name MyGroupCli --policy-arn arn:aws:iam::211125391769:policy/changePwd

            Change Password using old and new password in UI


Create Access Keys for a new User

            Create access key for MyUserCli
            aws iam create-access-key --user-name MyUserCli




Switch AWS Users for AWS CLI commands

            Admin           Use aws configure to change default User

            Use set to change default configuration
            aws configure set


            export AWS_ACCESS_KEY_ID=

            export AWS_S?????_ACCESS_KEY=

            export AWS_DEFAULT_REGION=


Delete AWS resources

            aws ec2

            delete-carrier-gateway                  
            delete-client-vpn-endpoint               | delete-client-vpn-route                 
            delete-coip-cidr                         | delete-coip-pool                        
            delete-customer-gateway                  | delete-dhcp-options                     
            delete-egress-only-internet-gateway      | delete-fleets                           
            delete-flow-logs                         | delete-fpga-image                       
            delete-instance-connect-endpoint         | delete-instance-event-window            
            delete-internet-gateway                  | delete-ipam                             
            delete-ipam-pool                         | delete-ipam-resource-discovery          
            delete-ipam-scope                        | delete-key-pair                         
            delete-launch-template                   | delete-launch-template-versions         
            delete-local-gateway-route               | delete-local-gateway-route-table        
            delete-local-gateway-route-table-virtual-interface-group-association | delete-local-gateway-route-table-vpc-association
            delete-managed-prefix-list               | delete-nat-gateway                      
            delete-network-acl                       | delete-network-acl-entry                
            delete-network-insights-access-scope     | delete-network-insights-access-scope-analysis
            delete-network-insights-analysis         | delete-network-insights-path            
            delete-network-interface                 | delete-network-interface-permission     
            delete-placement-group                   | delete-public-ipv4-pool                 
            delete-queued-reserved-instances         | delete-route                            
            delete-route-table                       | delete-security-group                   
            delete-snapshot                          | delete-spot-datafeed-subscription       
            delete-subnet                            | delete-subnet-cidr-reservation          
            delete-tags                              | delete-traffic-mirror-filter            
            delete-traffic-mirror-filter-rule        | delete-traffic-mirror-session           
            delete-traffic-mirror-target             | delete-transit-gateway                  
            delete-transit-gateway-connect           | delete-transit-gateway-connect-peer     
            delete-transit-gateway-multicast-domain  | delete-transit-gateway-peering-attachment
            delete-transit-gateway-policy-table      | delete-transit-gateway-prefix-list-reference
            delete-transit-gateway-route             | delete-transit-gateway-route-table      
            delete-transit-gateway-route-table-announcement | delete-transit-gateway-vpc-attachment   
            delete-verified-access-endpoint          | delete-verified-access-group            
            delete-verified-access-instance          | delete-verified-access-trust-provider   
            delete-volume                            | delete-vpc                              
            delete-vpc-endpoint-connection-notifications | delete-vpc-endpoint-service-configurations
            delete-vpc-endpoints                     | delete-vpc-peering-connection           
            delete-vpn-connection                    | delete-vpn-connection-route             
            delete-vpn-gateway




            aws iam

            delete-access-key                        | delete-account-alias                    
            delete-account-password-policy           | delete-group                            
            delete-group-policy                      | delete-instance-profile                 
            delete-login-profile                     | delete-open-id-connect-provider         
            delete-policy                            | delete-policy-version                   
            delete-role                              | delete-role-permissions-boundary        
            delete-role-policy                       | delete-saml-provider                    
            delete-ssh-public-key                    | delete-server-certificate               
            delete-service-linked-role               | delete-service-specific-credential      
            delete-signing-certificate               | delete-user                             
            delete-user-permissions-boundary         | delete-user-policy                      
            delete-virtual-mfa-device 













# Usage

michaelbradley@Michaels-iMac-2 ~ % aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro"                                                
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

michaelbradley@Michaels-iMac-2 ~ % aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query "Reservations[].Instances[].InstanceId"
[
    "i-089031a5d0081270a",
    "i-070f04717825eef4f",
    "i-02045bde58b74bf07"
]


michaelbradley@Michaels-iMac-2 ~ % aws ec2 describe-instances --filters "Name=tag:Type,Values=web-server-with-docker"
{
    "Reservations": [
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


michaelbradley@Michaels-iMac-2 ~ % aws ec2 describe-instances --filters "Name=tag:Type,Values=web-server-with-docker" --query "Reservations[].Instances[].InstanceId"
[
    "i-02045bde58b74bf07"



michaelbradley@Michaels-iMac-2 ~ % aws iam create-group --group-name MyGroupCli
{
    "Group": {
        "Path": "/",
        "GroupName": "MyGroupCli",
        "GroupId": "AGPATCKAN2GMSNXJUE7CP",
        "Arn": "arn:aws:iam::211125391769:group/MyGroupCli",
        "CreateDate": "2024-07-30T13:00:53+00:00"
    }
}


michaelbradley@Michaels-iMac-2 ~ % aws iam create-user --user-name MyUserCli 
{
    "User": {
        "Path": "/",
        "UserName": "MyUserCli",
        "UserId": "AIDATCKAN2GMSWV55SUSQ",
        "Arn": "arn:aws:iam::211125391769:user/MyUserCli",
        "CreateDate": "2024-07-30T13:04:35+00:00"
    }
}


michaelbradley@Michaels-iMac-2 ~ % aws iam add-user-to-group --user-name MyUserCli --group-name MyGroupCli


michaelbradley@Michaels-iMac-2 ~ % aws iam get-group --group-name MyGroupCli
{
    "Users": [
        {
            "Path": "/",
            "UserName": "MyUserCli",
            "UserId": "AIDATCKAN2GMSWV55SUSQ",
            "Arn": "arn:aws:iam::211125391769:user/MyUserCli",
            "CreateDate": "2024-07-30T13:04:35+00:00"
        }
    ],
    "Group": {
        "Path": "/",
        "GroupName": "MyGroupCli",
        "GroupId": "AGPATCKAN2GMSNXJUE7CP",
        "Arn": "arn:aws:iam::211125391769:group/MyGroupCli",
        "CreateDate": "2024-07-30T13:00:53+00:00"
    }
}


michaelbradley@Michaels-iMac-2 ~ % aws iam attach-group-policy --group-name MyGroupCli --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess



michaelbradley@Michaels-iMac-2 ~ % aws iam list-attached-group-policies --group-name MyGroupCli
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonEC2FullAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AmazonEC2FullAccess"
        }
    ]
}



michaelbradley@Michaels-iMac-2 ~ % aws iam create-login-profile --user-name MyUserCli --password MyPassword! --password-reset-required
{
    "LoginProfile": {
        "UserName": "MyUserCli",
        "CreateDate": "2024-07-30T14:08:53+00:00",
        "PasswordResetRequired": true
    }
}


michaelbradley@Michaels-iMac-2 ~ % aws iam create-policy --policy-name changePwd --policy-document file://changePwdPolicy.json
{
    "Policy": {
        "PolicyName": "changePwd",
        "PolicyId": "ANPATCKAN2GMY4IV5F3IX",
        "Arn": "arn:aws:iam::211125391769:policy/changePwd",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2024-07-30T15:00:39+00:00",
        "UpdateDate": "2024-07-30T15:00:39+00:00"
    }
}


michaelbradley@Michaels-iMac-2 ~ % aws iam create-access-key --user-name MyUserCli
{
    "AccessKey": {
        "UserName": "MyUserCli",
        "AccessKeyId": "AKIATCKAN2GMQVEWPJ66",
        "Status": "Active",
        "S?????AccessKey":
        "CreateDate": "2024-07-30T15:11:49+00:00"
    }
}



