# Chapter10
This module we will review creating Helm Charts for Microservices

# Name: Create Helm Chart for Microservices 

# Description: 

Create Helm Chart for Microservices

    Introduction

        We have ten deployments and ten services configurations

        Wouldn't it be great if we had one blueprint for deployment and service for all microservices

        And only need to set the values for individual microservices

        We can achieve this with helm charts

    Benefits of Helm Charts

        Reusable K8s configuration

        Microservices is one of the best use cases for this

    Two ways of building Helm Charts for Microservices

        1) Helm Chart for each microservice

            When configurations are very different

        2) Shared Helm Chart for all Microservices

            You can also use a combination of both options

            A shared chart for similar applications

            Separate charts for completely different applications

    We will create one shared Helm Chart 


    Basice Structure of Helm Chart

            helm create microservice

                directory structure is auto-generated

                chart.yaml = meta info about chart

                    apiVersion: v2
                    name: microservice
                    description: A Helm Chart for Kubernetes

                chart folder = chart dependencies

                .helmignore = files you don't want to include in your helm chart

                    You can build a helm chart as an archive and host it on a helm repository

                template folder = where the K8s YAML files (template files) are created - the core of helm charts

                values.yaml = values for the template files

            Structure the same as K8s YAML files

                Instead of hard-coded values we have place-holders defined in {{ }}

                Helm replaces the placeholders with the values later

                Used to define variables and adding values

                We will remove all templates, except deployment, service and values yaml files.

                We will remove content from both files and start from scratch

    Create Microservices Helm Chart

            Create Basic Template File

            name: {{ .Values.varName }}


            Values Object

                A built-in Object

                By default, Value is empty

                Values are passed into template from 3 sources:

                    the values.yaml file in chart

                    user supplied file passed with -f flag

                    parameter passed with --set flag

                Built-in Objects

                    Several objects are passed into a template from the template engine

                    Examples: "Release", "Files", "Values", ...

                    helm.sh/docs/chart_template_guide/builtin_objects/

                Variable Naming Convention

                    Names should begin with lowercase letter

                    Separated with camelcase

                First usage

                    {{ .Values.appName }}

                    Use it as the Name value in the deployment.yaml and service.yaml


Deployment Component

apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.appName }}
spec:
  replicas: {{ .Values.appReplicas }}
  selector:
    matchLabels:
      app: {{ .Values.appName }}
  template:
    metadata:
      labels:
        app: {{ .Values.appName }}
    spec:
      containers:
      - name: {{ .Values.appName }}
        image: "{{ .Values.appImage }}:{{ .Values.appVersion }}"
        ports:
        - containerPort: {{ .Values.containerPort }}
        env:
        {{- range .Values.containerEnvVars}}
        - name: {{ .name }}
          value: {{ .value | quote }}
        {{- end}}


Service Component

apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.appName }}
spec:
  type: {{ .Values.serviceType }}
  selector:
    app: {{ .Values.appName }}
  ports:
  - protocol: TCP
    port: {{ .Values.servicePort }}
    targetPort: {{ .Values.containerPort }}



        Flat or Nested Values

            Values may be flat or nested deeply

            Best practice is to use flat structure, which is simpler



            Values.yaml  - Flat

            aapName: myapp
            aapReplicas: 1


            Values.yaml - Nested

            aap:
            name: myapp
            replicas: 1


        Create Microservices Helm Charts

        Dynamic Environment Variables


            Range

            Provides a for each  -style loop

            To reiterate through or "range over" a list


            Piping or Pipelines

            Same concept as Unix

            Tool for chaining together template commands

            env:
            {{- range .Values.containerEnvVars}}
            - name: {{ .name }}
            value: {{ .value | quote }}                         because it's a string syntax to enclose in quotes
            {{- end}}                                               end for each



        Create Microservices Helm Chart

        Set Values


            Default - values.yaml file

            appName: servicename
            appImage: gcr.io/google-samples/microservices-demo/servicename
            appVersion: v0.0.0
            appReplicas: 1
            containerPort: 8080
            containerEnvVars: 
            - name: ENV_VAR_ONE
            value: "valueone"
            - name: ENV_VAR_TWO
            value: "valuetwo"

            servicePort: 8080
            serviceType: ClusterIP          this is the default, so not needed


            helm template =

            render chart templates locally and display the output

        helm template -f email-service-values.yaml ~/k8s-configuration/microservices/microservice

            helm template does not run template, only determines if clean

        
        Helm Rendering Process

            When Helm evaluates a Chart it will send all the files defined in templates directory thru Helm rendering Template Engine

            /templates 

            The Engine replaces the variables with the actual values (from the 3 different sources)

            i.e. - helm template -f email-service-values.yaml --set aapReplicas 3 ~/k8s-configuration/microservices/microservice

            Helm will then collect results of rendering of those templates and send it to K8s when install command is run


        Helm lint command

            examines a chart for possible issues

            ERROR = issue that will cause chart to fail installation

            WARNING = issues that break with convention or recommendations

            helm lint -f email-service-values.yaml ~/k8s-configuration/microservices/microservice

            A way to validate syntax


        Deploy EmailService

        Helm install command

            helm install        -f myvalues.yaml        emailservice        microservice

            installs a chart    override values from    release name        chart name
                                a file



        helm install -f email-service-values.yaml emailservice ~/k8s-configuration/microservices/microservice

        helm ls

        kubectl get pod ** shows two replicas
        NAME                           READY   STATUS    RESTARTS   AGE
        emailservice-c6b75bdfc-249j5   1/1     Running   0          6m15s
        emailservice-c6b75bdfc-6dg92   1/1     Running   0          6m15s

    Create Microservices Helm Chart

        Create values file for all microservices

        ls -lstr

        total 120
        8 -rw-r--r--  1 michaelbradley  staff   1148 Sep  5 22:15 Chart.yaml
        8 -rw-r--r--  1 michaelbradley  staff   2365 Sep  5 22:15 values.yaml
        0 drwxr-xr-x  2 michaelbradley  staff     64 Sep  5 22:15 charts
        8 -rw-r--r--  1 michaelbradley  staff    209 Sep 12 15:33 frontend-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    209 Sep 12 15:33 email-service-values.yaml
        24 -rw-r--r--  1 michaelbradley  staff  11046 Sep 13 10:41 config.yaml
        0 drwxr-xr-x  4 michaelbradley  staff    128 Sep 13 10:59 templates
        8 -rw-r--r--  1 michaelbradley  staff    306 Sep 13 19:02 shipping-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    306 Sep 13 19:02 recommendation-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    306 Sep 13 19:02 currency-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    306 Sep 13 19:02 cart-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    306 Sep 13 19:02 ad-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    269 Sep 13 19:14 productcatalog-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    257 Sep 13 19:48 payment-service-values.yaml
        8 -rw-r--r--  1 michaelbradley  staff    610 Sep 13 19:54 checkout-service-values.yaml


        cat recommendation-service-values.yaml 

        appName: recommendationservice
        appImage: gcr.io/google-samples/microservices-demo/recommendationservice
        appVersion: v0.8.0
        appReplicas: 2
        containerPort: 8080
        containerEnvVars:
        - name: PORT
            value: "8080"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
            value: "productcatalogservice:3550"

        servicePort: 8080%  


        helm install -f recommendation-service-values.yaml recommendationservice ~/k8s-configuration/microservices/microservice

        
        
        helm ls |grep rec

        recommendationservice	default  	1       	2024-09-15 14:47:53.960519 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

    Create Redis Helm Chart

        /Users/michaelbradley/k8s-configuration/microservices/microservice/charts 
        % helm create redis
            Creating redis


        Create deployment.yaml

        apiVersion: apps/v1
        kind: Deployment
        metadata:
        name: {{ .Values.appName }}
        spec:
        replicas: {{ .Values.appReplicas }}
        selector:
            matchLabels:
            app: {{ .Values.appName }}
        template:
            metadata:
            labels:
                app: {{ .Values.appName }}
            spec:
            containers:
            - name: {{ .Values.appName }}
                image: "{{ .Values.appImage }}:{{ .Values.appVersion }}"
                ports:
                - containerPort: {{ .Values.containerPort }}
                livenessProbe:
                initialDelaySeconds: 5
                tcpSocket:
                    port: {{ .Values.containerPort }}
                periodSeconds: 5
                readinessProbe:
                initialDelaySeconds: 5
                tcpSocket:
                    port: {{ .Values.containerPort }}
                periodSeconds: 5
                resources:
                requests: 
                    cpu: 70m
                    memory: 200Mi
                limits:
                    cpu: 125m
                    memory: 300Mi
                volumeMounts:
                - name: {{ .Values.volumeName }}
                mountPath: {{ .Values.containerMountPath }}
            volumes:
            - name: {{ .Values.volumeName }}
                emptyDir: {}



        Create service.yaml

        apiVersion: v1
        kind: Service
        metadata:
        name: {{ .Values.appName }}
        spec:
        type: ClusterIP
        selector:
            app: {{ .Values.appName }}
        ports:
        - protocol: TCP
            port: {{ .Values.servicePort }}
            targetPort: {{ .Values.containerPort }}


        Create values.yaml - results templates Rendering Engine

        appName: redis
        appImage: redis
        appVersion: alpine
        appReplicas: 1
        containerPort: 6379
        volumeName: redis-data
        containerMountPath: /data

        servicePort: 6379

        

        helm template -f values/redis-values.yaml charts/redis 

    Check generated manifest without installing the chart


        helm install --dry-run -f values/redis-values.yaml rediscart charts/redis

        Difference between dry-run and template

        --dry-run sends files to K8s cluster, while template only validates it locally









# Usage

michaelbradley@Michaels-iMac-2.attlocal.net /Users/michaelbradley/k8s-configuration/microservices 
% helm create microservice 
Creating microservice



helm template -f email-service-values.yaml ~/k8s-configuration/microservices/microservice 
---
# Source: microservice/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: emailservice
spec:
  type: 
  selector:
    app: emailservice
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 8080
---
# Source: microservice/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: emailservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: emailservice
  template:
    metadata:
      labels:
        app: emailservice
    spec:
      containers:
        - name: emailservice
          image: "gcr.io/google-samples/microservices-demo/emailservice:v0.8.0"
          ports:
            - containerPort: 8080
          env:
          - name: PORT
            value: "8080"


helm lint -f email-service-values.yaml ~/k8s-configuration/microservices/microservice    
==> Linting /Users/michaelbradley/k8s-configuration/microservices/microservice
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed



helm install -f email-service-values.yaml emailservice ~/k8s-configuration/microservices/microservice 
NAME: emailservice
LAST DEPLOYED: Fri Sep 13 18:46:40 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

 helm ls
NAME        	NAMESPACE	REVISION	UPDATED                            	STATUS  	CHART             	APP VERSION
emailservice	default  	1       	2024-09-13 18:46:40.58062 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/k8s-configuration/microservices/microservice

 kubectl get pod
NAME                           READY   STATUS    RESTARTS   AGE
emailservice-c6b75bdfc-249j5   1/1     Running   0          6m15s
emailservice-c6b75bdfc-6dg92   1/1     Running   0          6m15s


helm install -f recommendation-service-values.yaml recommendationservice ~/k8s-configuration/microservices/microservice 
NAME: recommendationservice
LAST DEPLOYED: Sun Sep 15 14:47:53 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

helm ls
NAME                 	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART             	APP VERSION
emailservice         	default  	1       	2024-09-13 18:46:40.58062 -0400 EDT 	deployed	microservice-0.1.0	1.16.0     
recommendationservice	default  	1       	2024-09-15 14:47:53.960519 -0400 EDT	deployed	microservice-0.1.0	1.16.0     


kubectl get pod
NAME                                     READY   STATUS    RESTARTS   AGE
emailservice-c6b75bdfc-249j5             1/1     Running   0          44h
emailservice-c6b75bdfc-6dg92             1/1     Running   0          44h
recommendationservice-6c4fdb4f46-zvtbs   1/1     Running   0          28m
recommendationservice-6c4fdb4f46-zx22j   1/1     Running   0          28m


/Users/michaelbradley/k8s-configuration/microservices/microservice/charts 
% helm create redis
    Creating redis

helm template -f values/redis-values.yaml charts/redis - actual values filled in

# Source: redis/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  type: ClusterIP
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
# Source: redis/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: "redis:alpine"
          ports:
            - containerPort: 6379
          livenessProbe:
            initialDelaySeconds: 5
            tcpSocket:
              port: 6379
            periodSeconds: 5
          readinessProbe:
            initialDelaySeconds: 5
            tcpSocket:
              port: 6379
            periodSeconds: 5
          resources:
            requests:
              cpu: 70m
              memory: 200Mi
            limits:
              cpu: 125m
              memory: 300Mi
          volumeMounts:
            - name: redis-data
              mountPath: /data
      volumes:
        - name: redis-data
          emptyDir: {}


helm install --dry-run -f values/redis-values.yaml rediscart charts/redis
NAME: rediscart
LAST DEPLOYED: Sun Sep 15 16:49:25 2024
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
HOOKS:
MANIFEST:
---
# Source: redis/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  type: ClusterIP
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
# Source: redis/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: "redis:alpine"
          ports:
            - containerPort: 6379
          livenessProbe:
            initialDelaySeconds: 5
            tcpSocket:
              port: 6379
            periodSeconds: 5
          readinessProbe:
            initialDelaySeconds: 5
            tcpSocket:
              port: 6379
            periodSeconds: 5
          resources:
            requests:
              cpu: 70m
              memory: 200Mi
            limits:
              cpu: 125m
              memory: 300Mi
          volumeMounts:
            - name: redis-data
              mountPath: /data
      volumes:
        - name: redis-data
          emptyDir: {}