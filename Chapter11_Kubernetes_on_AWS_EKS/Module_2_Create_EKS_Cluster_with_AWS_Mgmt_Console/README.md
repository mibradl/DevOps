# Chapter11
This module we will manually setup EKS Cluster using the UI.

# Name: Create EKS cluster with AWS Management Console

# Description: 

Create EKS Cluster Manually (UI) Part 1

    Create an EKS Cluster

    Steps to create an EKS Cluster

        1. Create EKS IAM Role

        2. Create VPC Worker Nodes

        3. Create an EKS Cluster (Control Plane Nodes)

        4. Connect kubectl with EKS Cluster


        5. Create EC2 IAM Role for Node Group

        6. Create Node Group and attach to EKS Cluster (Control Plane Nodes)


            Managed by AWS

            Using our AWS account


        7. Configure auto-scaling (auto-intelligence)

        8. Deploy our Application to our EKS Cluster (kubectl)



    Create EKS IAM Role in our AWS Account

        Policy IAM Role

    Assign Role to EKS Cluster managed by AWS

        AWS Service

    To allow AWS to create and manage components on our behalf


        IAM

            Roles

            IAM components are global for your whole account

            Create Role

                AWS Service

                EKS

                    Select EKS Cluster

                    Next

                    Permission Policy automatically selected (AmazonEKSClusterPolicy)

                        Expand to see extended permissions

                        {
                        "Version": "2012-10-17",
                        "Statement": [
                            {
                                "Effect": "Allow",
                                "Action": [
                                    "autoscaling:DescribeAutoScalingGroups",
                                    "autoscaling:UpdateAutoScalingGroup",
                                    "ec2:AttachVolume",
                                    "ec2:AuthorizeSecurityGroupIngress",
                                    "ec2:CreateRoute",
                                    "ec2:CreateSecurityGroup",
                                    "ec2:CreateTags",
                                    "ec2:CreateVolume",
                                    "ec2:DeleteRoute",
                                    "ec2:DeleteSecurityGroup",
                                    "ec2:DeleteVolume",
                                    "ec2:DescribeInstances",
                                    "ec2:DescribeRouteTables",
                                    "ec2:DescribeSecurityGroups",
                                    ...

                        Next

                        Role Details

                        Role Name: eks-cluster-role - Trust Policy

                        {
                        "Version": "2012-10-17",
                        "Statement": [
                            {
                                "Effect": "Allow",
                                "Principal": {
                                    "Service": [
                                        "eks.amazonaws.com"
                                    ]
                                },
                                "Action": "sts:AssumeRole"
                            }

            Create VPC for EKS Worker Nodes

                Why do we need another VPC?

                EKS cluster needs specific networking configuration, which is based on K8s

                K8s specific and AWS specific networking rules

                Default VPC not optimized for it

                It's a network that going to run you worker nodes

                VPCs have their own Subnets and these have their own firewall rules

                These are configured using Network Access Control Lists

                Inside the Subnet are EC2 instances, which can also have their own firewall rules

                These are defined by Security Groups

                So, your worker nodes will run in your VPC, need to have specific firewall configurations

                Control will run on another VPC

                For Control Plane / Worker Node communications

                AWS managed account

                All the necessary ports need to be configured correctly

                Best practice: configure Public subnets and Private subnets

                    Loadbalancer Service gets created - Private Subnet
                    
                    Cloud native Loadbalancer (Elastic Loadbalancer) - Public Subnet

                Through IAM Role you give Kubernetes permission to change VPC configurations

                    Nodeport Service - Open port on Worker Node

            How do we create this VPC with EKS specific configurations?

                AWS has templates with all this pre-configured

                AWS CloudFormation

                We will use infrastructure as Code in detail in next module

                Click Create Stack

                    Three options

                        All private subnets

                        All public subnets

                        Public and Private subnets - this solution is recommended by AWS

                    From docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html - use IPv4 template to create stack

                    https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml

                Next

                    Worker Network Configuration
                    VpcBlock
                    The CIDR range for the VPC. This should be a valid private (RFC 1918) CIDR range.
                    192.168.0.0/16
                    PublicSubnet01Block
                    CidrBlock for public subnet 01 within the VPC
                    192.168.0.0/18
                    PublicSubnet02Block
                    CidrBlock for public subnet 02 within the VPC
                    192.168.64.0/18
                    PrivateSubnet01Block
                    CidrBlock for private subnet 01 within the VPC
                    192.168.128.0/18
                    PrivateSubnet02Block
                    CidrBlock for private subnet 02 within the VPC
                    192.168.192.0/18

                Next

                Configure Stack Options - Keep Defaults

                Next

                Summary

                Review and Create

                Prerequisite - Prepare template
                Template
                Template is ready

                Template
                Template URL
                https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml
                Stack description
                Amazon EKS Sample VPC - Private and Public subnets

                Step 2: Specify stack details
                Edit
                Provide a stack name
                Stack name
                eks-worker-node-vpc-stack

                Parameters 

                PrivateSubnet01Block
                192.168.128.0/18
                PrivateSubnet02Block
                192.168.192.0/18
                PublicSubnet01Block
                192.168.0.0/18
                PublicSubnet02Block
                192.168.64.0/18
                VpcBlock

                SUBMIT



            Resources

                22 Resources were created



            Output

                SecurityGroups

                sg-0dea482a7eff538da
                Security group for the cluster control plane communication with worker nodes
                -

                SubnetIds

                subnet-042007f2512ab7c5e,subnet-0c3fecdb728101573,subnet-02cdaa8a42d18edc0,subnet-0753d57ad89351c92
                Subnets IDs in the VPC
                -

                VpcId

                vpc-0760148efa8b4fd84
                The VPC Id
                -

            You need this information when creating the EKS cluster (Control Plane)



            Create EKS Cluster

                Control Plane

                Search for EKS

                Click Elastic Kubernetes Service

                Create Cluster - Configure Cluster

                    Name: eks_cluster-test

                    Kubernetes version: default

                    Cluster Service Role: eks-cluster-role

                Next

                Specify Networking

                Select VPC - eks-worker-node-vpc-stack-VPC

                    You also get the subnets of that VPC

                Select Security Groups

                Cluster endpoint access

                    eks-worker-node-vpc-stack-ControlPlaneSecurityGroup-*

                    API Server is the entrypoint to the cluster

                    Accessing your cluster

                    Will run inside the EKS VPC, which means this is a managed VPC

                    Public = accessible from outside the VPC

                        You can access from kubectl

                        Accessible from your dashboard in the browser

                        Worker Plane will leave the VPC and connect to the endpoint


                    Private = accessible only within our VPC

                Select Public and Private for optimum use

                Next


                Configure Observability

                    Metrics

                Control plan logging

                        private mode will allow the Control Pland and Worker Nodes to communicate thru our VPC

                        will allow traffic to go from worker nodes, directly to the control plane within the VPC 


                    Public and Private = the cluster endpoint is accessible from outside your VPC. Worker Node traffic to the endpoint stay within the VPC.

                Select Add-ons

                    CoreDNS to use DNS within K8s - default service

                    kube-proxy - manage routing and networking in the node to allow communicatation to the pods that back a service - default

                    Amazon VPC CNI - Container Network Interface - using this VPC allows you to have the same IP for the pods as the VPC - default

                    Next

                    Check your selections

                    Next

                    Review and create - we can review summary and selections and create the EKS cluster

                    * Add-on(s) vpc-cni, coredns, kube-proxy, eks-pod-identity-agent successfully added to cluster eks_cluster-test

                    
            Connect EKS cluster locally with kubectl

                Create kubeconfig file

                aws configure list
                Name                    Value             Type    Location
                ----                    -----             ----    --------
                profile                <not set>             None    None
                access_key     ****************GSMS shared-credentials-file    
                secret_key     ****************6tqJ shared-credentials-file    
                **region                us-east-1      config-file    ~/.aws/config

                The region has already been created and this is where the cluster will run

                aws eks update-kubeconfig --name eks_cluster-test

                - context:
                    cluster: arn:aws:eks:us-east-1:211125391769:cluster/eks_cluster-test
                    user: arn:aws:eks:us-east-1:211125391769:cluster/eks_cluster-test
                  name: arn:aws:eks:us-east-1:211125391769:cluster/eks_cluster-test
                current-context: arn:aws:eks:us-east-1:211125391769:cluster/eks_cluster-test

                kubectl cluster-info
                Kubernetes control plane is running at https://127.0.0.1:50090
                CoreDNS is running at https://127.0.0.1:50090/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy


                API server endpoint

                https://1D310AB75469D3BF1E5FAD34B201388C.gr7.us-east-1.eks.amazonaws.com


                Now talking to the EKS Cluster and have a connection to the Control Node

                Next we create Worker Nodes so we can add pods

            Cluster Overview - Worker Nodes

                Under EKS > Compute are Nodes (EC2 instances you manage yourself), Node Groups and Fargate (managed worker nodes, on top of Control Plane Nodes)

                You can run Worker Nodes on EC2 instances or Fargate, which will provision the virtual machine to run the pods

                When you create a EKS Cluster only the Control Plane Nodes are added.

                We will create Node Group

                Under EKS > Networking

            Create EC2 IAM Role for our Node Group (Worker Nodes)

                Worker Nodes run Worker Processes

                    One of the process is kubelet

                    Kubelet is the main process - scheduling, managing Pods and communicate with other AWS Services

                    Kubelet on Worker Node (EC2 Instance) needs permission

                    Create Role for Node Group

                Go to IAM Service

                    IAM > Roles > Create Role > AWS Service

                        Service or Use Case - EC2

                        Select Next

                        Need 3 policies

                        1) AmazonEKSWorkerNodePolicy - Provides access to EC2

                        2) AmazonEC2ContainerRegistryReadOnly - Provides read-only access to Amazon EC2 Container Registry repositories.

                        3) AmazonEKS_CNI_Policy - This policy provides the Amazon VPC CNI Plugin (amazon-vpc-cni-k8s) the permissions it requires to modify the IP address configuration on your EKS worker nodes. This permission set allows the CNI to list, describe, and modify Elastic Network Interfaces on your behalf. More information on the AWS VPC CNI Plugin is available here: https://github.com/aws/amazon-vpc-cni-k8s  - Container Network Interface - this is the internal network for K8s.

                        Click Next

                        Role details

                            Role name: eks-node-group-role

                            Default description

                            Trusted entities: ec2.amazonaws.com

                            Includes the three policies

                                "Principal": {
                                    "Service": [
                                        "ec2.amazonaws.com"

                        Click Create Role

                    Add Node Group to EKS Group

                        EKS > Cluster > eks-cluster-test

                        Compute

                        Add Node Group

                        Node Group Configuration

                            Name: eks-node-group

                            Node IAM Role: eks-node-group-role (default)

                            Next

                    Set Compute and scaling configuration
                    
                        Node group compute configuration

                        AMI type
                        Info
                        Select the EKS-optimized Amazon Machine Image for nodes.
                        Amazon Linux 2 (AL2_x86_64) (Default)

                        Capacity type
                        Select the capacity purchase option for this node group.
                        On-Demand   (Default)

                        Instance types
                        Info
                        Select instance types you prefer for this node group.


                        t3.medium
                        vCPU: 2 vCPUs
                        Memory: 4 GiB
                        Network: Up to 5 Gigabit
                        Max ENI: 3
                        Max IPs: 18

                        Disk size
                        Select the size of the attached EBS volume for each node.
                        20
                        GiB



                        Node group scaling configuration

                        Desired size
                        Set the desired number of nodes that the group should launch with initially.
                        2
                        nodes
                        Desired node size must be greater than or equal to 0
                        
                        Minimum size
                        Set the minimum number of nodes that the group can scale in to.
                        2
                        nodes
                        Minimum node size must be greater than or equal to 0

                        Maximum size
                        Set the maximum number of nodes that the group can scale out to.
                        2
                        nodes
                        Maximum node size must be greater than or equal to 1 and cannot be lower than the minimum size


                        Node group update configuration Info

                        Maximum unavailable
                        Set the maximum number or percentage of unavailable nodes to be tolerated during the node group version update.

                        Number
                        Enter a number

                        Percentage
                        Specify a percentage

                        Value
                        1       (Default)
                        node
                        Node count must be greater than 0.

                        Next


                    Specify Networking - Worker Nodes must run in VPC that we have created

                        The Control Plane which is running in another VPC can connect to the Worker Nodes, running in different subnets

                        Configure remote access to nodes

                        Select enable

                        EC2 Key Pair
                        Select an EC2 key pair to allow secure remote access to your nodes. To create a new EC2 key pair, go to the corresponding page in the EC2 console.

                        Choose existing ssh key - docker-server

                        Allow remote access from
                        Configure the SSH client source IP ranges that can remotely access nodes.

                        Selected security groups
                        Specify security groups to restrict which source IPs can remotely access nodes.

                        *All
                        Do not restrict source IPs that can remotely access nodes.

                        Next


                    Review and create

                        Step 1: Node group
                        
                        Node group configuration

                        Name
                        eks-node-group

                        Node IAM role
                        arn:aws:iam::211125391769:role/eks-node-group-role


                    Step 2: Compute and scaling configuration

                        Node group compute configuration

                        Capacity type
                        On-Demand

                        AMI type
                        Amazon Linux 2 (AL2_x86_64)

                        Instance types
                        t3.small

                        Disk size
                        20


                        Node group scaling configuration

                        Desired size
                        2 nodes

                        Minimum size
                        2 nodes

                        Maximum size
                        2 nodes

                    
                        Node group update configuration

                        Maximum unavailable
                        1 node



                    Step 3: Networking

                        Node group network configuration

                        Subnets
                        subnet-0c3fecdb728101573
                        subnet-042007f2512ab7c5e
                        subnet-0753d57ad89351c92
                        subnet-02cdaa8a42d18edc0

                        Configure remote access to nodes
                        on

                        EC2 Key Pair
                        docker-server
                    
                        Allow remote access from
                        All

                        Create



                    eks-node-group


                        Node group configuration Info

                        Kubernetes version
                        1.30

                        AMI type
                        Info
                        AL2_x86_64

                        Status
                        Creating

                        AMI release version
                        Info
                        1.30.4-20240928

                        Instance types
                        t3.small

                        Disk size
                        20 GiB


                    EC2 Dashboard

                        Resources


                        You are using the following Amazon EC2 resources in the US East (N. Virginia) Region:

                        Instances (running) 2

                        Auto Scaling Groups 1

                        Capacity Reservations 0

                        Dedicated Hosts 0

                        Elastic IPs 2

                        Instances 2

                        Key pairs 5

                        Load balancers 0

                        Placement groups 0

                        Security groups 11

                        Snapshots 0

                        Volumes 2


                    Instances

                    Name
	

            Worker Processes needs to be installed

            With Node Group all necessary processes are installed

                Container-d

                kubelet

                Node

                k-proxy


            Save infrastructure costs with an AutoScaler

                Node group scaling configuration

                    Desired size
                    Minimum size
                    Maximum size


# Usage

aws eks update-kubeconfig --name eks_cluster-test
Added new context arn:aws:eks:us-east-1:211125391769:cluster/eks_cluster-test to /Users/michaelbradley/.kube/configs




% kubectl get ns
NAME                   STATUS   AGE
default                Active   5m
kube-node-lease        Active   5m
kube-public            Active   5m
kube-system            Active   5m

kubectl cluster-info
Kubernetes control plane is running at https://1D310AB75469D3BF1E5FAD34B201388C.gr7.us-east-1.eks.amazonaws.com
CoreDNS is running at https://1D310AB75469D3BF1E5FAD34B201388C.gr7.us-east-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy


% kubectl get nodes 
NAME                                STATUS   ROLES      AGE   VERSION
ip-192-168-54-175.ec2.internal      Ready    <nodes>    37s   v1.30.4-eks-a737599
ip-192-168-96-248.ec2.internal      Ready    <nodes>    39s   v1.30.4-eks-a737599

