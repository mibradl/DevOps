# Chapter15
This module we will explore modules and collections in Ansible

# Name: Modules and Collections in Ansible 

# Description:

Let's look at Ansible documentation

    docs.ansible.com/ansible/latest/index.html

        Scroll down to "Indexes of all modules and plugins"

            Everything is shown as one huge list

            E.g. community.mongodb.mongodb_user - Adds or removes a user from MongoDB database

            Click inside on how to use the module, parameters, values

        Back to the main list of modules

            E.g. ansible.builtin.shell - Execute shell commands on targets

        There modules that work with scripts, directories and files

            ansible.builtin.shell - Execute shell commands on targets

            ansible.builtin.unarchive - Unpacks an archive after (optionally) copying it from the local machine



        What is a Collection?

            A package format for bundling and distributing Asnbile content

            Can be released and installed independent of other collections

            Collection

                Playbook

                Plugin

                Module

                Documentation

                A single bundle

                    e.g. Docker Collection to work with Docker containers


            Collection & Collection Index

                All modules are part of a collection

                amazon.aws

                ansible.builtin

                ansible.netcommon

                ansible.posix

                ansible.utils

                ansible.windows

                arista.eos

                awx.awx

                azure.azcollection

                check_point.mgmt

                chocolatey.chocolatey

                cisco.aci

        Ansible Plugins

            Pieces of code that add to Ansible's functionality or modules

            You can also write your own plugins

                Plugin indexes

                Index of all Become Plugins
                Index of all Cache Plugins
                Index of all Callback Plugins
                Index of all Cliconf Plugins
                Index of all Connection Plugins
                Index of all Filter Plugins
                Index of all Httpapi Plugins
                Index of all Inventory Plugins
                Index of all Lookup Plugins
                Index of all Modules
                Index of all Netconf Plugins
                Index of all Roles
                Index of all Shell Plugins
                Index of all Strategy Plugins
                Index of all Test Plugins
                Index of all Vars Plugins


        Ansible Galaxy - Online hub for finding and sharing Ansible community content

            Command-line utility to install individual collections

            ansible-galaxy install <collection name>

            ansible-galaxy collection list

            ** amazon.aws collection in home directory following upgrade

            ansible-galaxy collection install amazon.aws --upgrade

            # /Users/michaelbradley/.ansible/collections/ansible_collections
            Collection                               Version
            ---------------------------------------- -------
            amazon.aws                               8.1.0  

            # /usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/ansible_collections



            ansible-galaxy collection list


            Modules and Plugins are just pieces of code, which it allows you to download it to your local machine

            Where is this content hosted and shared?

            Where do I get Ansible Collections?


        Create own Collection

            For bigger Ansible projects

             Plugins / Modules / Playbooks

             If you want to distribute and share that as a bundle

             Collections follow a simple data structure
             
             Required:

             a galaxy.xml file (containting metadata)
             at the root level of the collection

                collection/
                    docs/
                    galaxy.xml
                    meta/
                        modules/
                            module1.py
                        inventory/
                        .../
                    README.md
                    roles/
                        roles1/
                        roles2/
                        .../
                    playbooks/
                        files/
                        vars/
                        templates/
                        tasks/
                    tests/


        Ansible Namespaces

         <namespace>.<collection>.<module name>

         <namespace>.<collection>.<plugin name>

         Fully Qualified Collection Name (FQCN)

         FQCN to specify the exact source of a module/plugin/ ...

         - name: Pull an image
           community.docker.docker_image:
              name: pacur/centos-7
              source: pull
              # Select platform for pulling. If not specified, will pull whatever docker prefers.
              pull:
                platform: amd64


        ansible.builtin = default namespace and collection name

       
       
        Start using the fully qualified name

        e.g. the backwards compatibility may be removed in future versions

            

# Usage


   ansible-galaxy collection list

# /usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/ansible_collections
Collection                               Version
---------------------------------------- -------
amazon.aws                               8.0.1  
ansible.netcommon                        6.1.3  
ansible.posix                            1.5.4  
ansible.utils                            4.1.0  
ansible.windows                          2.4.0  
arista.eos                               9.0.0  
awx.awx                                  24.5.0 
azure.azcollection                       2.4.0  
check_point.mgmt                         5.2.3  
chocolatey.chocolatey                    1.5.1  
cisco.aci                                2.9.0  
cisco.asa                                5.0.1  
cisco.dnac                               6.16.0 
cisco.intersight                         2.0.9  
cisco.ios                                8.0.0  
cisco.iosxr                              9.0.0  
cisco.ise                                2.9.2  
cisco.meraki                             2.18.1 
cisco.mso                                2.6.0  
cisco.nxos                               8.1.0  
cisco.ucs                                1.10.0 
cloud.common                             3.0.0  
cloudscale_ch.cloud                      2.3.1  
community.aws                            8.0.0  
community.ciscosmb                       1.0.9  
community.crypto                         2.20.0 
community.digitalocean                   1.26.0 
community.dns                            3.0.1  
community.docker                         3.10.4 
community.general                        9.1.0  
community.grafana                        1.9.1  
community.hashi_vault                    6.2.0  
community.hrobot                         2.0.1  
community.library_inventory_filtering_v1 1.0.1  
community.libvirt                        1.3.0  
community.mongodb                        1.7.4  
community.mysql                          3.9.0  
community.network                        5.0.3  
community.okd                            3.0.1  
community.postgresql                     3.4.1  
community.proxysql                       1.5.1  
community.rabbitmq                       1.3.0  
community.routeros                       2.16.0 
community.sap_libs                       1.4.2  
community.sops                           1.6.7  
community.vmware                         4.4.0  
community.windows                        2.2.0  
community.zabbix                         2.5.1  
containers.podman                        1.15.2 
cyberark.conjur                          1.3.0  
cyberark.pas                             1.0.25 
dellemc.enterprise_sonic                 2.4.0  
dellemc.openmanage                       9.3.0  
dellemc.powerflex                        2.5.0  
dellemc.unity                            2.0.0  
f5networks.f5_modules                    1.28.0 
fortinet.fortimanager                    2.5.0  
fortinet.fortios                         2.3.6  
frr.frr                                  2.0.2  
google.cloud                             1.3.0  
grafana.grafana                          5.2.0  
hetzner.hcloud                           3.1.1  
ibm.qradar                               3.0.0  
ibm.spectrum_virtualize                  2.0.0  
ibm.storage_virtualize                   2.3.1  
ieisystem.inmanage                       2.0.0  
infinidat.infinibox                      1.4.5  
infoblox.nios_modules                    1.6.1  
inspur.ispim                             2.2.3  
inspur.sm                                2.3.0  
junipernetworks.junos                    8.0.0  
kaytus.ksmanage                          1.2.2  
kubernetes.core                          3.2.0  
lowlydba.sqlserver                       2.3.3  
microsoft.ad                             1.6.0  
netapp.cloudmanager                      21.22.1
netapp.ontap                             22.11.0
netapp.storagegrid                       21.12.0
netapp_eseries.santricity                1.4.0  
netbox.netbox                            3.19.1 
ngine_io.cloudstack                      2.3.0  
ngine_io.exoscale                        1.1.0  
openstack.cloud                          2.2.0  
openvswitch.openvswitch                  2.1.1  
ovirt.ovirt                              3.2.0  
purestorage.flasharray                   1.28.1 
purestorage.flashblade                   1.17.0 
sensu.sensu_go                           1.14.0 
splunk.es                                3.0.0  
t_systems_mms.icinga_director            2.0.1  
telekom_mms.icinga_director              2.1.2  
theforeman.foreman                       4.0.0  
vmware.vmware_rest                       3.0.1  
vultr.cloud                              1.13.0 
vyos.vyos                                4.1.0  
wti.remote                               1.0.5   



 ansible-galaxy collection install amazon.aws --upgrade
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Downloading https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/amazon-aws-8.1.0.tar.gz to /Users/michaelbradley/.ansible/tmp/ansible-local-43463_8i4q68e/tmpwiimdx8w/amazon-aws-8.1.0-fjh9k2_9
Installing 'amazon.aws:8.1.0' to '/Users/michaelbradley/.ansible/collections/ansible_collections/amazon/aws'
amazon.aws:8.1.0 was installed successfully


ansible-galaxy collection list

# /Users/michaelbradley/.ansible/collections/ansible_collections
Collection                               Version
---------------------------------------- -------
amazon.aws                               8.1.0  

# /usr/local/Cellar/ansible/10.1.0/libexec/lib/python3.12/site-packages/ansible_collections
Collection                               Version
---------------------------------------- -------
amazon.aws                               8.0.1  
ansible.netcommon                        6.1.3  
ansible.posix                            1.5.4  
ansible.utils                            4.1.0  
ansible.windows                          2.4.0  
arista.eos                               9.0.0  
awx.awx                                  24.5.0 
azure.azcollection                       2.4.0  
check_point.mgmt                         5.2.3  
chocolatey.chocolatey                    1.5.1  
cisco.aci                                2.9.0  
cisco.asa                                5.0.1  
cisco.dnac                               6.16.0 
cisco.intersight                         2.0.9  
cisco.ios                                8.0.0  
cisco.iosxr                              9.0.0  
cisco.ise                                2.9.2  
cisco.meraki                             2.18.1 
cisco.mso                                2.6.0  
cisco.nxos                               8.1.0  
cisco.ucs                                1.10.0 
cloud.common                             3.0.0  
cloudscale_ch.cloud                      2.3.1  
community.aws                            8.0.0  
community.ciscosmb                       1.0.9  
community.crypto                         2.20.0 
community.digitalocean                   1.26.0 
community.dns                            3.0.1  
community.docker                         3.10.4 
community.general                        9.1.0  
community.grafana                        1.9.1  
community.hashi_vault                    6.2.0  
community.hrobot                         2.0.1  
community.library_inventory_filtering_v1 1.0.1  
community.libvirt                        1.3.0  
community.mongodb                        1.7.4  
community.mysql                          3.9.0  
community.network                        5.0.3  
community.okd                            3.0.1  
community.postgresql                     3.4.1  
community.proxysql                       1.5.1  
community.rabbitmq                       1.3.0  
community.routeros                       2.16.0 
community.sap_libs                       1.4.2  
community.sops                           1.6.7  
community.vmware                         4.4.0  
community.windows                        2.2.0  
community.zabbix                         2.5.1  
containers.podman                        1.15.2 
cyberark.conjur                          1.3.0  
cyberark.pas                             1.0.25 
dellemc.enterprise_sonic                 2.4.0  
dellemc.openmanage                       9.3.0  
dellemc.powerflex                        2.5.0  
dellemc.unity                            2.0.0  
f5networks.f5_modules                    1.28.0 
fortinet.fortimanager                    2.5.0  
fortinet.fortios                         2.3.6  
frr.frr                                  2.0.2  
google.cloud                             1.3.0  
grafana.grafana                          5.2.0  
hetzner.hcloud                           3.1.1  
ibm.qradar                               3.0.0  
ibm.spectrum_virtualize                  2.0.0  
ibm.storage_virtualize                   2.3.1  
ieisystem.inmanage                       2.0.0  
infinidat.infinibox                      1.4.5  
infoblox.nios_modules                    1.6.1  
inspur.ispim                             2.2.3  
inspur.sm                                2.3.0  
junipernetworks.junos                    8.0.0  
kaytus.ksmanage                          1.2.2  
kubernetes.core                          3.2.0  
lowlydba.sqlserver                       2.3.3  
microsoft.ad                             1.6.0  
netapp.cloudmanager                      21.22.1
netapp.ontap                             22.11.0
netapp.storagegrid                       21.12.0
netapp_eseries.santricity                1.4.0  
netbox.netbox                            3.19.1 
ngine_io.cloudstack                      2.3.0  
ngine_io.exoscale                        1.1.0  
openstack.cloud                          2.2.0  
openvswitch.openvswitch                  2.1.1  
ovirt.ovirt                              3.2.0  
purestorage.flasharray                   1.28.1 
purestorage.flashblade                   1.17.0 
sensu.sensu_go                           1.14.0 
splunk.es                                3.0.0  
t_systems_mms.icinga_director            2.0.1  
telekom_mms.icinga_director              2.1.2  
theforeman.foreman                       4.0.0  
vmware.vmware_rest                       3.0.1  
vultr.cloud                              1.13.0 
vyos.vyos                                4.1.0  
wti.remote                               1.0.5 




