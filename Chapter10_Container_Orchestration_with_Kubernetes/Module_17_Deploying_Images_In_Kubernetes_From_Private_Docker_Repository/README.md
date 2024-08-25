# Chapter10
This module we will look closely at deploying images in Kubernetes from private Docker repository

# Name: Deploying Images in Kubernetes from private Docker repository

# Description: 

Deploy App in Kubernetes cluster from Private Docker Registry

    Common workflow for your App                                                     docker image

    user ---> git ---> triggers CI build ---> Jenkins ---> pushed to registry  --->  private registry

                                            packages        docker image             Nexus

                                             applicaiton                             AWS

                                             docker image

    How to get Docker image on Kubernetes Cluster?

        Images like Mongodb, elastic live in a Public Docker repository, like Docker Hub

        Docker image (my-app) resides in private registry - needs explicit access!

    
    
    
    Steps to pull image from private registry

    1) Create Secret component

        Contains credentials for Docker registry authentication

    2)  Configure Deployment / Pod

        Use Secret using imagePullSecrets


    You can use Docker Hub private repo instead

    Private docker repo hosted on AWS also

    Simple Nodejs application

    Amazon Elastic Container Registry - my-app


Docker Login 

    create config.json file for Secret  


    Description

    Log in to a registry.

    Options
    Option	Default	Description
    -p, --password		Password
    --password-stdin		Take the password from stdin
    -u, --username		Username

    View push commands - Amazon ECR ---> Private registry ---> Repositories ---> my-app


    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 211125391769.dkr.ecr.us-east-1.amazonaws.com
    Login Succeeded

    cat .docker/config.json 
{
	"auths": {                          ** Authentication token
		"157.230.238.59:8083": {},                                      ** Endpoints
		"174.138.89.62:8083": {},
		"211125391769.dkr.ecr.us-east-1.amazonaws.com": {},             ** URI Repository
		"https://index.docker.io/v1/": {}
	},
	"credsStore": "desktop",
	"currentContext": "desktop-linux",
	"plugins": {
		"-x-cli-hints": {
			"enabled": "true"
		},
		"debug": {
			"hooks": "exec"
		},
		"scout": {
			"hooks": "pull,buildx build"
		}
	},
	"features": {
		"hooks": "true"
	}
}%                                       

    Because kubernetes/docker is being run on minikube must perform login to aws repo by sshing from minikube

    Docker can't access the credstore


        minikube ssh

        login to repo from minikube by manually entering the token block

        docker login --username AWS -p xxx 
        211125391769.dkr.ecr.us-east-1.amazonaws.com

        WARNING! Using --password via the CLI is insecure. Use --password-stdin.
        WARNING! Your password will be stored unencrypted in /home/docker/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded
        docker@minikube:~$ 


        docker@minikube:~$ ls -la
        total 36
        drwxr-x--- 1 docker docker 4096 Aug 21 13:03 .
        drwxr-xr-x 1 root   root   4096 May  8 23:53 ..
        -rw-r--r-- 1 docker docker  220 May  8 23:53 .bash_logout
        -rw-r--r-- 1 docker docker 3771 May  8 23:53 .bashrc
        drwx------ 2 docker docker 4096 Aug 21 13:03 .docker
        -rw-r--r-- 1 docker docker  807 May  8 23:53 .profile
        drwxr-xr-x 1 docker docker 4096 Aug  8 03:08 .ssh
        -rw-r--r-- 1 docker docker    0 Aug  8 03:08 .sudo_as_admin_successful
        docker@minikube:~$ 


    The following file will be used to create the Secret

        docker@minikube:~$ cat .docker/config.json 
        {
            "auths": {
                "211125391769.dkr.ecr.us-east-1.amazonaws.com": {
                    "auth": "QVdTOmV5SndZWGxzYjJGa0lqb2lkRzFR ...
                }
            }
        }docker@minikube:~$ 

    
    Create Security Component

        minikube cp minikube:/home/docker/.docker/config.json /Users/michaelbradley/.docker/config.json
        michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley 
        % cat .docker/config.json 
        {
            "auths": {
                "211125391769.dkr.ecr.us-east-1.amazonaws.com": {
                    "auth": "QVdTOmV5SndZWGxzYjJGa0lqb2lkRzFRVTFOVFoyRm1kRkZVYm...


        Not as secure as credStore, but using for minikube use case.

        cat docker-secret.yaml

        kind: Secret
        metadata:
        name: my-registry-key
        data:
        .dockerconfigjson: ewoJImF1dGhzIjogewoJCSIyMTExMjUzOTE3NjkuZGtyLmVjci51cy1lYXN0LTEuYW1hem9uYXdzLmNvbSI6IHsKCQkJImF1dGgiOiAiUVZkVE9tVjVTbmRaV0d4ellqSkdhMGxxYjJsYWJH
        type: kubernetes.io/dockerconfigjson



        kubectl create secret generic my-registry-key \
        --from-file=.dockerconfigjson=.docker/config.json \
        --type=kubernetes.io/dockerconfigjson


        kubectl get secret
        NAME                    TYPE                             DATA   AGE
        mongodb-secret          Opaque                           2      16d
        mosquitto-secret-file   Opaque                           1      11d
        my-registry-key         kubernetes.io/dockerconfigjson   1      99s

        Note: Can be used multiple times


    Second Option

        kubectl create secret docker-registry my-registry-key-two \
        --docker-server=211125391769.dkr.ecr.us-east-1.amazonaws.com \
        --docker-username=AWS
        --docker-password=********hash

        Note: Can only be used once in the registry


Create Deployment Component

    cat my-app-deployment.yaml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: my-app
    labels:
        app: my-app
    spec:
    replicas:
    selector:
        matchLabels:
        app: my-app
    template:
        metadata:
        labels:
            app: my-app
        spec:
        imagePullSecrets:
        - name: my-registry-key
        containers:
        - name: my-app
            image: 211125391769.dkr.ecr.us-east-1.amazonaws.com/my-app
            imagePullPolicy: Always
            ports:
            - containerPort: 3000




    kubectl apply -f my-app-deployment.yaml 
    deployment.apps/my-app created

    kubectl get pod
    NAME                                  READY   STATUS             RESTARTS        AGE
    mongo-express-6cfbc86cb6-ph22k        1/1     Running            3 (2d14h ago)   16d
    mongodb-deployment-585bb4fddc-fnshv   1/1     Running            3 (2d14h ago)   16d
    mosquitto-76954b5c5f-xj9ff            1/1     Running            1 (2d14h ago)   11d
    my-app-7778cc6ffd-rjw4h               1/1     Running            1               16s


Secret must be in same namespace as Deployment

    






        














# Usage

minikube ssh

docker@minikube:~$ ls -la
total 32
drwxr-x--- 1 docker docker 4096 Aug  8 03:08 .
drwxr-xr-x 1 root   root   4096 May  8 23:53 ..
-rw-r--r-- 1 docker docker  220 May  8 23:53 .bash_logout
-rw-r--r-- 1 docker docker 3771 May  8 23:53 .bashrc
-rw-r--r-- 1 docker docker  807 May  8 23:53 .profile
drwxr-xr-x 1 docker docker 4096 Aug  8 03:08 .ssh
-rw-r--r-- 1 docker docker    0 Aug  8 03:08 .sudo_as_admin_successful


docker@minikube:~$ docker login --username AWS -p eyJwYXlsb2FkIjoidG1QU1NTZ2FmdFFUbmd3L0FlZG8vTWRPajZJclFaMGJoZC8vclR3MGdhN0NJQUtSeUh1VWsrSTk5djdQcmJQZ1M1cjRZUXlWd2s2SE5GM20rd0srUkdlUVYwdlBpODdpK2h5c3hCaWQ2aitrNzNNaFNOZm5KMlhYNEtDMXdmSWRRVW1TdExtbGpuaFo4dXNSZDZuWVgreituU0lJZWM5UW1vaEFDZVNNYUtTS25zUVU4Sit4R1pTTkJOdTc4Z2c1Z051M1FBcy8ydXMvdTVGU2F1OG53NkpoT0lQSkU0enpzVzZhcnF6MTBaUGtvaGZyWWFMbmo5T0xYVjlUVDczTW5JM0poRjlEZlFDNHpDd28vVzU5dXB2OGhPVFFaMGZZcmludHhMcHF3WlV4eEhWYVcrZFh6ejA1YldEaS81Z3B6ZEFBeUJucUNzUXRyZkR3YmFIQ0N4UytSMFRwampOU3p2QW5JOGNBanAvQ2toVk1XMEtia0xpbUxOcGFnQmdNZFZwdFEvVFVwMDg2a2VWMlFEak5CSFV5d0xYMUdnWlBqNlV3NEc0ZWludGJnMGtPSFo3UVo2U0I0WmpDT1BVV0tQMnlVNWlLMHU2TGpwelovZVJaQTNnVzI1SjFiY0tYQ2JTSnBEYXNDWkk3cXZJSEszYi9ra1ZHelk4T0pobXJab1dZcVNTUUFJSmJlRjE3YnpGVnBKZU5QOEJNNGl5elNHdlltOUUvamZZNUNBWVVFZm5GUlNwS0FFL0U5R1J5NmdkUG8yMlp6K1dRZWhrbkNOb093c2hzU2FyczVnS0J0a3JrMi82TlZYcFUxSFJ0Q0R1RFJHeWwzNTlGZVk5b1lVU0lYV1VUMFBoS3ZabThkSFZGdUxWNG9jS29saHNyUFNRSm9VamtZN0NINWk5cUJBbm9RQmhCb2dCOXZFTWIwUGZ5WUd6RDhZVTlsSzFKVU5ZU0V4aC8vNmRTdTB4R1dYcDIzdXdSTno5SzBXWG1jOTZxTUFDY2pMRjV3WG5HK1RqM1gvVmVmQmVOakM0VXBKdjhDRVQ3RHZ5VU8ya28yc2dGaXNkRnI3RGg4M3JjL25OQWhXUHRDVGp6cTdjeks5OEkyaGxiZlBWWmdXM2JaZk5VNnh1Tk9DWHI3dzFValBIVmhSQmhaalA1N3AyaWJzYWREN2pNNm9wd0ZmUmQxUzkyVzVoZ1pDN0szSUxvcHM0czBOMjdDek91eWk4NDNnbjRiVDd6ZE1UQXhwckN0NUxOQkdrOUJnOHVRU1V0NXRhUExEandiWU1tRFphVmNnPT0iLCJkYXRha2V5IjoiQVFFQkFIaHdtMFlhSVNKZVJ0Sm01bjFHNnVxZWVrWHVvWFhQZTVVRmNlOVJxOC8xNHdBQUFINHdmQVlKS29aSWh2Y05BUWNHb0c4d2JRSUJBREJvQmdrcWhraUc5dzBCQndFd0hnWUpZSVpJQVdVREJBRXVNQkVFREpOOGYzWkV2bGgrS21LR3R3SUJFSUE3WExIQjd4L3NFd3BaelZBMGVua29JK2xKK0F2K3l2Yks3Yk5nTHlaclVxM1lLSC9JWmUrQnhmRnNNNzZRSWgwaWZwdGppR0kzaEpwSDFKRT0iLCJ2ZXJzaW9uIjoiMiIsInR5cGUiOiJEQVRBX0tFWSIsImV4cGlyYXRpb24iOjE3MjQyNjY5MzJ9 211125391769.dkr.ecr.us-east-1.amazonaws.com
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /home/docker/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded




kubectl create secret generic my-registry-key \
--from-file=.dockerconfigjson=.docker/config.json \ 
--type=kubernetes.io/dockerconfigjson

secret/my-registry-key created


kubectl get secret
NAME                    TYPE                             DATA   AGE
mongodb-secret          Opaque                           2      16d
mosquitto-secret-file   Opaque                           1      11d
my-registry-key         kubernetes.io/dockerconfigjson   1      99s



kubectl get secret -o yaml
apiVersion: v1
items:
- apiVersion: v1
  data:
    mongo-root-password: cGFzc3dvcmQ=
    mongo-root-username: dXNlcm5hbWU=
  kind: Secret
  metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"v1","data":{"mongo-root-password":"cGFzc3dvcmQ=","mongo-root-username":"dXNlcm5hbWU="},"kind":"Secret","metadata":{"annotations":{},"name":"mongodb-secret","namespace":"default"},"type":"Opaque"}
    creationTimestamp: "2024-08-09T02:29:01Z"
    name: mongodb-secret
    namespace: default
    resourceVersion: "50808"
    uid: c9167262-02c4-4f90-ad32-ba7b33dcd446
  type: Opaque
- apiVersion: v1
  data:
    secret.file: VGVjaFdvcmxkMjAyMyEgLW4K
  kind: Secret
  metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"v1","data":{"secret.file":"VGVjaFdvcmxkMjAyMyEgLW4K\n"},"kind":"Secret","metadata":{"annotations":{},"name":"mosquitto-secret-file","namespace":"default"},"type":"Opaque"}
    creationTimestamp: "2024-08-14T03:59:45Z"
    name: mosquitto-secret-file
    namespace: default
    resourceVersion: "246447"
    uid: 210b2fa2-423b-4ab9-857e-97a8b1299063
  type: Opaque
- apiVersion: v1
  data:
    .dockerconfigjson: ewoJImF1dGhzIjogewoJCSIyMTExMjUzOTE3NjkuZGtyLmVjci51cy1lYXN0LTEuYW1hem9uYXdzLmNvbSI6IHsKCQkJImF1dGgiOiAiUVZkVE9tVjVTbmRaV0d4ellqSkdhMGxxYjJsYWJHUlhWVEk1Y0U0d1ZrMWhRM1JTVmtWNE1rc3paSFpUU0d0MlVUQjNNMWRIUm5CUmEzZDJUMGRhV0ZKV1RYWmtTRkpTWlZSU2JrNHljekJWVkVadFVUSndUMUpGWkRaVWFtaDBVV3BhYTAxclVsZFdSa3BEVXpCV2FGWlhlRTVoYmtwdll6SldlRlJFVm14aFZUUjVZekkwTTJKRVNubGtSM04yVEROQ2IxWldiM2RpTWpCNlZsTjBSV0l4VGxWaFZXUkhUbGM0TVdWVlZsVlhiWFJJWWxaUmVsbHFRa2xUVjBwWFVXdGtSMkZFVWtoVFUzUndUREZXU2xSc2JIVlBRemx4VFcwNVJtTlhaSEZPYlUxNFZqRlpkbUZFV1hwaVJUbHlWak5SZDJKSFZreGxSbWhNVTBoQ05tRXlZelJWUlVaeFUydGtURTR6UlhabGJuQm9VVzVzYVdGVk5IWlZhMlJ1WTBkamRsUkhTakJYU0U1elZsWnNVMU5JUWpSTldHOTRVbXRLV0dWVk5WSmFNbFpNV2xka1EwNHdTakZoUm14T1lteE9VMk50ZHpKVGFsWkpWVEl4UTFJemFEQk9WV1J2VFVjME1WZFhXbmxXYWtseVZXMVZNMDVyY0VsT00xWkNZa1JXUWxVeWJ6Sk1lbHBwV210ME5Vd3dlRkZVVlRWeVRsUk9WbGRyU25GaU1HaHJUVVJXYlZWSGNHbFRSbFpoVjIxd2VsZFhjM1phUXpsdVZFaG9NMVZFYkZSTldGSnpUbFJhUTA5WGNIQlBWMnhHVDBNNWIxRXdSVEZVV0U1V1pGaEJlVlZ1Um0xVE1VcEVaSHBXTUZOc1JqVmFNREZ6VVcxb1VXUnNTa0paYm1SQ1RrVndVbFZwT1ZWa1IxVXpZVmQwYlUweVpFVlpNMDVTVG10U01tSlhaRVpVUmxFeFRVWmtlVmRUT1d0alYxcEVZa1pPU1dKVk9YUk9SRUp2Wkd4d1dHRXdlSFppTTJ3MlUwZDBTVlZZYkZaVk1GWXhWVVpzYlZWRVVucFRWbEYyWkROYVNtUldhRWhoZW1STldtc3dNRkpzUlhsTU1XaENWVVJvZDA1c1pIRmthM1JIV25sMGMySXdaSFJrYTBrelkxaHJNbGRZYkhkU2JsWklXV3hhZWxRd2J6Qk9SbVJFWVRKWk5WTlZSbmhqVnpWclkwVm5kMkZGY0hOWk1sWXpVVlZrVUZreVl6UlJXR2hyVWtoT01scHRXVE5UV0dNMVRVVm9ORXN5VmtaaFdGSm9UVWhXZEZZeldrTmxSMVpYWVc1b1dsTnNSbWxoVjBrMVlrZGtTbE5IWnpKaVJHeHRZbGhvUlZGVlZqRmphM2hYV1dwa1dtRXpiRE5oYTJoRlpVWm9XR0ZzVm5KU2Vsb3dWbFUxV2xKVlozaE9XRTR3VXpOak1tVnNaR3RsYW1oSVpIcFNjMVZGU2pKaFYzaFdZbGRTVmxwdFZtaFZTRkl4VTFaS1JtTXhXak5qVmxwNFdteG9jbEpzV1RGU01EVXdVbTFHYm1Kck1UWkxlbWcwWkhwVk5WcHVjRzVOV0d4R1VsaEdXR1ZWT1hOaFZYUldWMVpHZVZVeGNETmxVemxyU3pCd1dGUkdRak5oTVVJMlkxZG9jV0pFVGpaaVZYZ3dUVmRvYjFWdFVYZFBWbVJ1Vm01a05rc3pTbEZPUmxveFlWUktkbEpzYkc1T2VsRTBXVmhvY0dNeldqQlRTRUp0Vkd0NFRFc3dVWEppYVhOeVZFZHdkMVJ0Vm5CbFUzUnBWMnBhVVUxRk9UQlNhemxYWkZoTmVsRXdhRE5hU0UxMlRtdEtkbEZZV25oa1IzQXpVa1ZhYkZOV1NrWlVXRkkxVVcxR1MxbHRWWEpsYTFaNFZGWlJORlV5TlRaVFZtaG9WVVZKTldGSFpFaGxiRUkyVFVkU1dGTjZZelZQUm1SS1dXNW9WbGRWVVRWTU1WbDZVekpLTlZWdWJHOVNSMVpLVWtab1JGZHVVblZoTTJSSVZFVXdNVmRWV25kVk1tc3lXakJWTlZSRlZqTmlTR2MwWTFock1sSkZkRXBaYlZZMldsaEdOVmxWY0RWVlZFSlZUVEZvZDFWVmNIaFRNamxXVFVNNVYyUlliRE5PYVRsdlVsWlNiRmRWV25WVWJXZ3lZVzVhTmswelpFSk9NREYwWTBWNGExZEVUbGhPYTNScVQxWkdURTFHVWpaak1XaHhVVlJTZEZwSVkzcFZSazVzWVVkT1VHRXdWak5SV0d4MFVXNUNhbEZ0VG5oUFJscFhWVVJXV0U1Vk5WTlJiWGN3VDBkMGJsUlZhRTVUVlhkM1ZWUXdPVWxwZDJsYVIwWXdXVmQwYkdWVFNUWkphMFpTVWxWS1FsTkhhRE5pVkVKYVdWVnNWRk50VmxOa1JYQjBUbGMwZUZKNldqRmpWMVpzWVRGb01XSXhhRmxWUjFVeFZsVmFhbHBVYkZOalZHZDJUVlJTTTFGVlJrSlRSRkl6V210R1dsTnJkSFpYYTJ4dlpHMU9UMUZXUm1wU01qbElUMGhrYVZWVmJFTlJWVkpEWWpCS2JtRXpSbTloTW14SVQxaGpkMUZyU2pOU1dHUkpXakZzUzFkVmJHRlRWVVpZVmxWU1ExRlZWakZVVlVwR1VsVlNSRTFYVGs5TmJFNTZVbGQzTWxrd1ZUTmpSVnB4WkRCc1ExSlZiRUpPTVdReVQwUmpOVmRYYUROVFNHTXlWRmRhZVdOVVVqSkxNVUpDVkRCak1WUnNaekpaVkVKNlVsZFJlR1Z0WjNkU2VtUllUa1ZTUlZkdVNsQmtNR2cxWWpOa2NGWnVSbGhqUlRseFYxaHZNV0pFWkZOVVZUZ3hVMGhOTW1ScmFFdGliVkpzVFVoT2Qwc3dSVGxKYVhkcFpHMVdlV015YkhaaWFVazJTV3BKYVV4RFNqQmxXRUpzU1dwdmFWSkZSbFZSVmpsTVVsWnJhVXhEU214bFNFSndZMjFHTUdGWE9YVkphbTk0VG5wSk1FNUVSVE5QVkVreVpsRTlQUT09IgoJCX0KCX0sCgkiY3JlZHNTdG9yZSI6ICJkZXNrdG9wIiwKCSJjdXJyZW50Q29udGV4dCI6ICJkZXNrdG9wLWxpbnV4IiwKCSJwbHVnaW5zIjogewoJCSJkZWJ1ZyI6IHsKCQkJImhvb2tzIjogImV4ZWMiCgkJfSwKCQkic2NvdXQiOiB7CgkJCSJob29rcyI6ICJwdWxsLGJ1aWxkeCBidWlsZCIKCQl9Cgl9LAoJImZlYXR1cmVzIjogewoJCSJob29rcyI6ICJ0cnVlIgoJfQp9
  kind: Secret
  metadata:
    creationTimestamp: "2024-08-25T13:17:11Z"
    name: my-registry-key
    namespace: default
    resourceVersion: "607305"
    uid: 887ff152-16d0-44f6-9a27-dbbfd93657d1
  type: kubernetes.io/dockerconfigjson
kind: List
metadata:
  resourceVersion: ""


    