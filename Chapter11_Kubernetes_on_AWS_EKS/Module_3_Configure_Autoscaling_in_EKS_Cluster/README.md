# Chapter11
This module we will discuss the concepts of DevOps.

# Name: Configure Autoscaling in EKS Cluster

# Description: 

Create EKS cluster manually (UI) - Part II

    Configure Autoscaling

        AWS doesn't automatically autoscale our resources!

        We need to use: K8s Autoscaler, Auto Scaling Group  (Min: 3, Max: 3, Desired:3)

        If there are 5 instances and they are underutilized in the cluster, the autoscaler will reduce the number of instances and combine pods to less utilized intances

        Conversely, if there are not enough resources available on an instance(s) the autoscaler would add another instance and move pod to that instance.


    There are a couple of things to do:

        Auto Scaling Group

        Create custom policy and attach to Node Group IAM Role

        Deploy K8s Autoscaler

    Search IAM

        Select Policies

        Create Policy in json

        Specify Permissions

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}

        Next

            Name: node-group-autoscale-policy

            Create Policy

            ** Policy node-group-autoscale-policy created.

        Attach New Policy to existing Node Group IAM Role

            Select Role

            * eks-node-group-role

            Click this role

            Add Permission

                Attach Policy

                Search for policy

                node-group-autoscale-policy

            Add Permission

                Now there are three AWS Managed Policies and one Custom Managed











# Usage


    