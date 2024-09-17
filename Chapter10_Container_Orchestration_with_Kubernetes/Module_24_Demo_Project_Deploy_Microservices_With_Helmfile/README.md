# Chapter10
This module we will review Deploy of Microservices with Helmfile

# Name: Demo project: Deploy Microservices with Helmfile

# Description: 

Deploy Microservices with Helm Install

    Deploy in the K8 cluster

        Redis Chart

        MS Chart

        cat install.sh

        helm install -f values/redis-values.yaml rediscart ~/k8s-configuration/microservices/charts/redis

        helm install -f values/email-service-values.yaml emailservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/cart-service-values.yaml cartservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/currency-service-values.yaml currencyservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/payment-service-values.yaml paymentservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/recommendation-service-values.yaml recommendationservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/productcatalog-service-values.yaml productcatalogservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/shipping-service-values.yaml shippingservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/ad-service-values.yaml adservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/checkout-service-values.yaml checkoutservice ~/k8s-configuration/microservices/charts/microservice
        helm install -f values/frontend-values.yaml frontendservice ~/k8s-configuration/microservices/charts/microservice


        ls
        charts		config.yaml	install.sh	templates	values
        michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/k8s-configuration/microservices/microservice 

        % chmod u+x install.sh

        helm ls
        NAME                 	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART             	APP VERSION
        adservice            	default  	1       	2024-09-15 23:16:36.104494 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        cartservice          	default  	1       	2024-09-15 23:16:34.630433 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        checkoutservice      	default  	1       	2024-09-15 23:16:36.900072 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        currencyservice      	default  	1       	2024-09-15 23:16:34.790929 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        emailservice         	default  	1       	2024-09-13 18:46:40.58062 -0400 EDT 	deployed	microservice-0.1.0	1.16.0     
        paymentservice       	default  	1       	2024-09-15 23:16:34.93875 -0400 EDT 	deployed	microservice-0.1.0	1.16.0     
        productcatalogservice	default  	1       	2024-09-15 23:16:35.149309 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        recommendationservice	default  	1       	2024-09-15 14:47:53.960519 -0400 EDT	deployed	microservice-0.1.0	1.16.0     
        rediscart            	default  	1       	2024-09-15 22:46:17.775594 -0400 EDT	deployed	redis-0.1.0       	1.16.0     
        shippingservice      	default  	1       	2024-09-15 23:16:35.501213 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

        kubectl get pod - to view pods running

        There is no clean way to elegantly uninstall helm files by reversing the method, but there is another solution.

    What is a Helmfile?

        Helmfile is a declarative say of deploying helm charts.

        This is acheived by defining the desired state.

        helmfile.yaml

        
    Install Helmfile

        brew install helmfile

        helmfile sync


        helmfile list

        NAME                 	NAMESPACE	ENABLED	INSTALLED	LABELS	CHART              	VERSION
        rediscart            	         	true   	true     	      	charts/redis       	       
        emailservice         	         	true   	true     	      	charts/microservice	       
        cartservice          	         	true   	true     	      	charts/microservice	       
        currencyservice      	         	true   	true     	      	charts/microservice	       
        paymentservice       	         	true   	true     	      	charts/microservice	       
        recommendationservice	         	true   	true     	      	charts/microservice	       
        productcatalogservice	         	true   	true     	      	charts/microservice	       
        shippingservice      	         	true   	true     	      	charts/microservice	       
        adservice            	         	true   	true     	      	charts/microservice	       
        checkoutservice      	         	true   	true     	      	charts/microservice	       
        frontendservice      	         	true   	true     	      	charts/microservice


        kubectl get pod

        NAME                                     READY   STATUS    RESTARTS   AGE
        adservice-d7ff4646b-5m78s                1/1     Running   0          20h
        adservice-d7ff4646b-gz4gk                1/1     Running   0          20h
        cartservice-7496fc6c9b-8v5cn             1/1     Running   0          20h
        cartservice-7496fc6c9b-b8xb7             1/1     Running   0          20h
        checkoutservice-564cdbcbc5-dk5tb         1/1     Running   0          20h
        checkoutservice-564cdbcbc5-s2m74         1/1     Running   0          20h
        currencyservice-69c577d9f8-52plw         1/1     Running   0          20h
        currencyservice-69c577d9f8-875xt         1/1     Running   0          20h
        emailservice-c6b75bdfc-249j5             1/1     Running   0          3d
        emailservice-c6b75bdfc-6dg92             1/1     Running   0          3d
        frontend-7b5c58f8c-dss55                 1/1     Running   0          6m12s
        frontend-7b5c58f8c-tmzqt                 1/1     Running   0          6m12s
        mosquitto-76954b5c5f-lkfgg               1/1     Running   0          3d1h
        paymentservice-5b75576b54-8rq97          1/1     Running   0          20h
        paymentservice-5b75576b54-vb74x          1/1     Running   0          20h
        productcatalogservice-fbfc699d9-hdv4l    1/1     Running   0          20h
        productcatalogservice-fbfc699d9-skr8t    1/1     Running   0          20h
        recommendationservice-6c4fdb4f46-zvtbs   1/1     Running   0          28h
        recommendationservice-6c4fdb4f46-zx22j   1/1     Running   0          28h
        redis-95d54f78-bj6bj                     1/1     Running   0          6m12s
        shippingservice-79bc74547c-6rpht         1/1     Running   0          20h
        shippingservice-79bc74547c-xqqqx         1/1     Running   0          20h


    Uninstall Releases


        helmfile destroy

        helm ls
        NAME	NAMESPACE	REVISION	UPDATED	STATUS	CHART	APP VERSION
        michaelbradley@Michaels-iMac-2.local /Users/michaelbradley/k8s-configuration/microservices 
%  


    Wrap up

        1 Shared Helm Chart for all Microservices

        Deploy declaratively using helmfile

        Where to host the Helm Charts?

            Hosted in Git Repository

            Two options:

                With Application Code

                    app code + helm charts

                Separate Git Repository for Helm Charts

                    app code repo

                    helm charts repo



        How does it fit into the DevOps workflow?

                Typically the DevOps Engineer would configure K8s and Helm Charts for developers

                Once this is done you can hand this over as a blueprint to the developers

                DevOps Eng will be the creator of Microservices, configuration files, etc


                Developers will be the users of Helm Charts

                Only need to tweak values of variables

                Can be implemented by integrated into CI/CD pipeline


                Facilitators for Developers and Operations Team

                    Creating configurations and programs for these teams


                    Can easily be integrated into build tool












# Usage

chmod u+x install.sh

./install.sh
NAME: cartservice
LAST DEPLOYED: Sun Sep 15 23:16:34 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NAME: currencyservice
LAST DEPLOYED: Sun Sep 15 23:16:34 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NAME: paymentservice
LAST DEPLOYED: Sun Sep 15 23:16:34 2024
...

kubectl get pod
NAME                                     READY   STATUS    RESTARTS   AGE
adservice-d7ff4646b-5m78s                1/1     Running   0          19h
adservice-d7ff4646b-gz4gk                1/1     Running   0          19h
cartservice-7496fc6c9b-8v5cn             1/1     Running   0          19h
cartservice-7496fc6c9b-b8xb7             1/1     Running   0          19h
checkoutservice-564cdbcbc5-dk5tb         1/1     Running   0          19h
checkoutservice-564cdbcbc5-s2m74         1/1     Running   0          19h
currencyservice-69c577d9f8-52plw         1/1     Running   0          19h
currencyservice-69c577d9f8-875xt         1/1     Running   0          19h
emailservice-c6b75bdfc-249j5             1/1     Running   0          3d
emailservice-c6b75bdfc-6dg92             1/1     Running   0          3d
mosquitto-76954b5c5f-lkfgg               1/1     Running   0          3d
paymentservice-5b75576b54-8rq97          1/1     Running   0          19h
paymentservice-5b75576b54-vb74x          1/1     Running   0          19h
productcatalogservice-fbfc699d9-hdv4l    1/1     Running   0          19h
productcatalogservice-fbfc699d9-skr8t    1/1     Running   0          19h
recommendationservice-6c4fdb4f46-zvtbs   1/1     Running   0          28h
recommendationservice-6c4fdb4f46-zx22j   1/1     Running   0          28h
redis-5f99c49df4-brnmx                   1/1     Running   0          20h
redis-5f99c49df4-ngb5g                   1/1     Running   0          20h
shippingservice-79bc74547c-6rpht         1/1     Running   0          19h
shippingservice-79bc74547c-xqqqx         1/1     Running   0          19h
    

brew install helmfile
==> Auto-updating Homebrew...
Adjust how often this is run with HOMEBREW_AUTO_UPDATE_SECS or disable with
HOMEBREW_NO_AUTO_UPDATE. Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Auto-updated Homebrew!
Updated 4 taps (homebrew/services, mongodb/brew, homebrew/core and homebrew/cask).
==> New Formulae
bc-gh                    dra                      firefly                  http-server-rs           ktor                     packcc                   spidermonkey@115
bed                      dwarfs                   flexiblas                immich-cli               kubernetes-cli@1.30      pgcopydb                 spoofdpi
bump-my-version          fast_float               gabo                     js-beautify              libblastrampoline        pyupgrade                wush
cmrc                     fatal                    gollama                  jsbeautifier             mariadb@11.4             rpcsvc-proto
crow                     fierce                   grizzly                  kea                      oasdiff                  slackdump
==> New Casks
blood-on-the-clocktower-online      font-departure-mono                 font-server-mono                    oxygen-xml-developer                rize
choice-financial-terminal           font-ibm-plex-math                  gauntlet                            parallels@19                        truetree
crosspaste                          font-ibm-plex-sans-tc               gitlight                            photostickies                       wealthfolio
dfcf                                font-lxgw-simxihei                  kindle-create                       pronotes                            winbox
feedflow                            font-lxgw-simzhisong                microsoft-openjdk@21                replaywebpage                       xmenu
flutterflow                         font-scientifica                    neo-network-utility                 retcon                              zen-browser

You have 31 outdated formulae installed.

Warning: You are using macOS 12.
We (and Apple) do not provide support for this old version.
It is expected behaviour that some formulae will fail to build in this old version.
It is expected behaviour that Homebrew will be buggy and slow.
Do not create any issues about this on Homebrew's GitHub repositories.
Do not create any issues even if you think this message is unrelated.
Any opened issues will be immediately closed without response.
Do not ask for help from Homebrew or its maintainers on social media.
You may ask for help in Homebrew's discussions but are unlikely to receive a response.
Try to figure out the problem yourself and submit a fix as a pull request.
We will review it but may or may not accept it.

==> Downloading https://ghcr.io/v2/homebrew/core/helmfile/manifests/0.168.0
############################################################################################################################################################################ 100.0%
==> Fetching helmfile
==> Downloading https://ghcr.io/v2/homebrew/core/helmfile/blobs/sha256:121cf83efcf48be4c137d6e1f8212957fef8022eb5dcaf13d703bafbac9208fe
############################################################################################################################################################################ 100.0%
==> Pouring helmfile--0.168.0.monterey.bottle.tar.gz
==> Caveats
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/helmfile/0.168.0: 9 files, 111.7MB
==> Running `brew cleanup helmfile`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).


helmfile sync
  
Building dependency release=adservice, chart=charts/microservice
Building dependency release=recommendationservice, chart=charts/microservice
Building dependency release=currencyservice, chart=charts/microservice
Building dependency release=frontendservice, chart=charts/microservice
Building dependency release=rediscart, chart=charts/redis
Building dependency release=productcatalogservice, chart=charts/microservice
Building dependency release=cartservice, chart=charts/microservice
Building dependency release=shippingservice, chart=charts/microservice
Building dependency release=checkoutservice, chart=charts/microservice
Building dependency release=emailservice, chart=charts/microservice
Building dependency release=paymentservice, chart=charts/microservice
Upgrading release=productcatalogservice, chart=charts/microservice, namespace=
Upgrading release=shippingservice, chart=charts/microservice, namespace=
Upgrading release=paymentservice, chart=charts/microservice, namespace=
Upgrading release=cartservice, chart=charts/microservice, namespace=
Upgrading release=frontendservice, chart=charts/microservice, namespace=
Upgrading release=rediscart, chart=charts/redis, namespace=
Upgrading release=currencyservice, chart=charts/microservice, namespace=
Upgrading release=checkoutservice, chart=charts/microservice, namespace=
Upgrading release=adservice, chart=charts/microservice, namespace=
Upgrading release=emailservice, chart=charts/microservice, namespace=
Upgrading release=recommendationservice, chart=charts/microservice, namespace=
Release "shippingservice" has been upgraded. Happy Helming!
NAME: shippingservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^shippingservice$
Release "recommendationservice" has been upgraded. Happy Helming!
NAME: recommendationservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^recommendationservice$
Release "paymentservice" has been upgraded. Happy Helming!
NAME: paymentservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^paymentservice$
Release "emailservice" has been upgraded. Happy Helming!
NAME: emailservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^emailservice$
Release "currencyservice" has been upgraded. Happy Helming!
NAME: currencyservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^currencyservice$
Release "cartservice" has been upgraded. Happy Helming!
NAME: cartservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Release "adservice" has been upgraded. Happy Helming!
NAME: adservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^cartservice$
Listing releases matching ^adservice$
Release "productcatalogservice" has been upgraded. Happy Helming!
NAME: productcatalogservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^productcatalogservice$
Release "checkoutservice" has been upgraded. Happy Helming!
NAME: checkoutservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^checkoutservice$
Release "frontendservice" does not exist. Installing it now.
NAME: frontendservice
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

Listing releases matching ^frontendservice$
Release "rediscart" has been upgraded. Happy Helming!
NAME: rediscart
LAST DEPLOYED: Mon Sep 16 19:14:52 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None

Listing releases matching ^rediscart$
shippingservice	default  	2       	2024-09-16 19:14:52.575104 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

recommendationservice	default  	2       	2024-09-16 19:14:52.571937 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

currencyservice	default  	2       	2024-09-16 19:14:52.568503 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

productcatalogservice	default  	2       	2024-09-16 19:14:52.574572 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

paymentservice	default  	2       	2024-09-16 19:14:52.576232 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

emailservice	default  	2       	2024-09-16 19:14:52.568159 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

adservice	default  	2       	2024-09-16 19:14:52.573893 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

cartservice	default  	2       	2024-09-16 19:14:52.573419 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

frontendservice	default  	1       	2024-09-16 19:14:52.566204 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

checkoutservice	default  	2       	2024-09-16 19:14:52.575328 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

rediscart	default  	2       	2024-09-16 19:14:52.57332 -0400 EDT	deployed	redis-0.1.0	1.16.0     


UPDATED RELEASES:
NAME                    NAMESPACE   CHART                 VERSION   DURATION
shippingservice                     charts/microservice   0.1.0           1s
recommendationservice               charts/microservice   0.1.0           1s
paymentservice                      charts/microservice   0.1.0           1s
emailservice                        charts/microservice   0.1.0           1s
currencyservice                     charts/microservice   0.1.0           1s
cartservice                         charts/microservice   0.1.0           1s
adservice                           charts/microservice   0.1.0           1s
productcatalogservice               charts/microservice   0.1.0           1s
checkoutservice                     charts/microservice   0.1.0           1s
frontendservice                     charts/microservice   0.1.0           1s
rediscart                           charts/redis          0.1.0           1s

helmfile list

NAME                 	NAMESPACE	ENABLED	INSTALLED	LABELS	CHART              	VERSION
rediscart            	         	true   	true     	      	charts/redis       	       
emailservice         	         	true   	true     	      	charts/microservice	       
cartservice          	         	true   	true     	      	charts/microservice	       
currencyservice      	         	true   	true     	      	charts/microservice	       
paymentservice       	         	true   	true     	      	charts/microservice	       
recommendationservice	         	true   	true     	      	charts/microservice	       
productcatalogservice	         	true   	true     	      	charts/microservice	       
shippingservice      	         	true   	true     	      	charts/microservice	       
adservice            	         	true   	true     	      	charts/microservice	       
checkoutservice      	         	true   	true     	      	charts/microservice	       
frontendservice      	         	true   	true     	      	charts/microservice


helmfile destroy

Building dependency release=currencyservice, chart=charts/microservice
Building dependency release=adservice, chart=charts/microservice
Building dependency release=frontendservice, chart=charts/microservice
Building dependency release=emailservice, chart=charts/microservice
Building dependency release=shippingservice, chart=charts/microservice
Building dependency release=cartservice, chart=charts/microservice
Building dependency release=productcatalogservice, chart=charts/microservice
Building dependency release=paymentservice, chart=charts/microservice
Building dependency release=recommendationservice, chart=charts/microservice
Building dependency release=rediscart, chart=charts/redis
Building dependency release=checkoutservice, chart=charts/microservice
Listing releases matching ^frontendservice$
frontendservice	default  	1       	2024-09-16 19:14:52.566204 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^checkoutservice$
checkoutservice	default  	2       	2024-09-16 19:14:52.575328 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^adservice$
adservice	default  	2       	2024-09-16 19:14:52.573893 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^shippingservice$
shippingservice	default  	2       	2024-09-16 19:14:52.575104 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^productcatalogservice$
productcatalogservice	default  	2       	2024-09-16 19:14:52.574572 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^recommendationservice$
recommendationservice	default  	2       	2024-09-16 19:14:52.571937 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^paymentservice$
paymentservice	default  	2       	2024-09-16 19:14:52.576232 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^currencyservice$
currencyservice	default  	2       	2024-09-16 19:14:52.568503 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^cartservice$
cartservice	default  	2       	2024-09-16 19:14:52.573419 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^emailservice$
emailservice	default  	2       	2024-09-16 19:14:52.568159 -0400 EDT	deployed	microservice-0.1.0	1.16.0     

Listing releases matching ^rediscart$
rediscart	default  	2       	2024-09-16 19:14:52.57332 -0400 EDT	deployed	redis-0.1.0	1.16.0     

Deleting currencyservice
Deleting frontendservice
Deleting rediscart
Deleting adservice
Deleting shippingservice
Deleting paymentservice
Deleting recommendationservice
Deleting cartservice
Deleting emailservice
Deleting productcatalogservice
Deleting checkoutservice
release "productcatalogservice" uninstalled

release "currencyservice" uninstalled

release "frontendservice" uninstalled

release "rediscart" uninstalled

release "checkoutservice" uninstalled

release "cartservice" uninstalled

release "shippingservice" uninstalled

release "paymentservice" uninstalled

release "recommendationservice" uninstalled

release "emailservice" uninstalled

release "adservice" uninstalled


DELETED RELEASES:
NAME                    NAMESPACE   DURATION
productcatalogservice                     1s
currencyservice                           1s
frontendservice                           1s
rediscart                                 1s
checkoutservice                           1s
cartservice                               1s
shippingservice                           1s
paymentservice                            1s
recommendationservice                     1s
emailservice                              1s
adservice                                 1s