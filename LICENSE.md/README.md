# INTRODUCTION

This README gives details of fresh installation and upgrade of Cloud CPE solution.

This is applicable for both MANO/CSO and NSC packages.

The Cloud CPE solution consists of several infrastructure services and micro-services that are deployed in two data centers which are primarily called "central" and "regional". In a deployment environment, there should be one central and multiple regional data centers.

Cloud CPE solution consists of two scenarios - Centralized (aka "vCPE") and Distributed (aka "uCPE"). The vCPE scenario uses Contrail while uCPE solution uses NFX device to create network services.

#FRESH INSTALLATION:
# Cloud CPE solution components

## Infrastructure Services
    elasticsearch, cassandra, zookeeper, mariadb, keystone, rabbitmq, elk_kibana, elk_logstash, swift, redis,
    dms, haproxy_confd, etcd, kubemaster, kubeminion, sim_cluster(icinga), sim_client, arangodb, contrail_analytics

## Microservices
    admin-portal-ui,csp-activation-service-central,csp-ams,csp-cms-central,csp-cms-central-core,csp-data-view-central,csp-design-tools,csp-dms-central,csp-dms-central-core,csp-ems-central,csp-fmpm-collector,csp-fmpm-provider,
    csp-fmpm-provider-core,csp-hybrid-access,csp-hybrid-access-core,csp-iamsvc,csp-iamsvc-noauth,csp-ims-central,csp-ims-central-core,csp-inv-central,csp-inv-central-core,csp-ipam,csp-job-service,csp-job-service-core,
    csp-policy-mgmt,csp-policy-mgmt-core,csp-pslam,csp-pslam-core,csp-routing-manager,csp-routing-manager-core,csp-schema-svc,csp-schema-service,csp-shared-object,csp-shared-object-core,csp-signature-manager,
    csp-signature-manager-core,csp-sse,csp-template-service,csp-topology-service,csp-topology-service-core,csp-tssm,csp-tssm-core,csp-vim,csp-vim-core,logspout,ne,ne-core,nsd-ui,nso,nso-core,secmgt-sm,secmgt-jingest,
    secmgt-monitoring,vnfm,vnfm-core,reporting-service

    cadvisor,csp-telemetry-agent,csp-activation-service,csp-cms-regional,csp-cms-regional-core,csp-config-service,csp-device-connectivity,csp-dms-regional,csp-dms-regional-core,
    csp-ems-regional,csp-fmpm-collector,csp-iamsvc-regional,csp-iamsvc-regional-core,csp-ims-regional,csp-ims-regional-core,csp-inv-regional,csp-inv-regional-core,csp-job-service,
    csp-job-service-core,csp-vim,csp-vim-core,logspout,telemetry-converter,csp-schema-svc

**Note**

1. Generally vCPE uses contrail keystone and uCPE/SDWAN uses standalone keystone. But there are some special cases where vCPE can use only standalone keystone and not a contrail keystone
2. vCPE can use contrail node as contrail analytics node, uCPE/SDWAN requires a seperate contrail analytics installation
3. Space Node is required to be deployed only for vCPE

# Ubuntu VMs

To have a common OS platform for CSO/NSC solution is recommendation to use Ubuntu cloud Image. The Steps are given in the provision vm section.


# Downloading CSO/NSC from CSO Downloader for UI

1. Refer CSO Install/Upgrade guide from juniper site.
    https://www.juniper.net/support/downloads/?p=cso#docs


# Downloading CSO/NSC from CSO Downloader for CLI

1. CSO Downloader for CLI based, Please use following URL to download latest version of downloader for your testing.

    Url: https://www.juniper.net/support/downloads/?p=cso

    CSO Downloader for Windows       : cso_downloader-4.0.1.exe
    CSO Downloader for MacOS         : cso_downloader-4.0.1.dmg
    CSO Downloader for Linux Desktop : cso_downloader-4.0.1.deb
 
2. Install the downloaded downloader on your mac/windows/linux desktops, Users login with their juniper credentials.
 
3. CLI based deployment - Fresh
                - select your hypervisor KVM/ESXI
                - select installation New
                - select software (Contrail Service Orchestrator / CSO Network Service controller)
                - select install later option will download two files CSO/NSC and Hypervisor bundle which contains preinstalled OSS, VRR, and ubuntu cloud image.
                - once the images are downloaded then copy those to your hosts or installer vm then extract cso/nsc tarball after that copy ESXi-4.0.1.tar.gz or KVM-4.0.1.tar.gz into <tar>/artifacts/ location and then continue with asusual deployment.
                - In case if your bandwidth is slow and taking more time to download then use below urls to download the directly in the hosts or installer vm
                  Url: https://www.juniper.net/support/downloads/?p=cso
                       Contrail_Service_Orchestration_4.0.1.tar.gz (or) CSO_Network_Service_Controller_4.0.1.tar.gz
                       KVM-4.0.1.tar.gz (or) ESXi-4.0.1.tar.gz
 
5. CLI based deployment - Upgrade
                - select your hypervisor KVM/ESXI
                - select installation Upgrade
                - select software (Contrail Service Orchestrator / CSO Network Service controller)
                - select upgrade now option will download a files CSO/NSC into installer vm.
                - once the images are downloaded then copy those to installer vm to continue with asusual upgrade.
                - In case if your bandwidth is slow and taking more time to download then use below urls to download the directly in the hosts or installer vm
                  Url: https://www.juniper.net/support/downloads/?p=cso
                       Contrail_Service_Orchestration_4.0.1.tar.gz (or) CSO_Network_Service_Controller_4.0.1.tar.gz
                       
#Large Deployment

    * 9 Physical servers (6 Physical Servers for CSO installation and 3 Physical Servers for Contrail Analytics), 38 Virtual Machines including Space, Virtual Route Reflector are required for deploying all the services in central and regional regions
    * 42 IPs including 1 VIP for space web GUI and 3 VIP for HAPROXY (Central, Regional and Regional Southbound) are required for this installation.

| S.No   | Components                       | Nodes                     | CPU(Cores)/Memory(GB)
| ------ |:--------------------------------:|:-------------------------:|:-----------------------:|
| 1      | Installer VM                     | csp-installer-vm          | 4/32G
| 2      | Central Infrastructure services  | csp-central-infravm1      | 16/64G
| 3      | Central Infrastructure services  | csp-central-infravm2      | 16/64G
| 4      | Central Infrastructure services  | csp-central-infravm3      | 16/64G
| 5      | Central LB VM                    | csp-central-lbvm1         | 4/32G
| 6      | Central LB VM                    | csp-central-lbvm2         | 4/32G
| 7      | Central LB VM                    | csp-central-lbvm3         | 4/32G
| 8      | Central Microservices            | csp-central-msvm1         | 16/64G
| 9      | Central Microservices            | csp-central-msvm2         | 16/64G
| 10     | Central Microservices            | csp-central-msvm3         | 16/64G
| 11     | Regional Infrastructure services | csp-regional-infravm1     | 16/64G
| 12     | Regional Infrastructure services | csp-regional-infravm2     | 16/64G
| 13     | Regional Infrastructure services | csp-regional-infravm3     | 16/64G
| 14     | Regional Microservices           | csp-regional-msvm1        | 16/64G
| 15     | Regional Microservices           | csp-regional-msvm2        | 16/64G
| 16     | Regional Microservices           | csp-regional-msvm3        | 16/64G
| 17     | Regional LB VM                   | csp-regional-lbvm1        | 4/32G
| 18     | Regional LB VM                   | csp-regional-lbvm2        | 4/32G
| 19     | Regional LB VM                   | csp-regional-lbvm3        | 4/32G
| 20     | Space VM                         | csp-space-vm              | 4/32G (Optional)
| 21     | Central ELK VM                   | csp-central-elkvm1        | 4/32G
| 22     | Central ELK VM                   | csp-central-elkvm2        | 4/32G
| 23     | Central ELK VM                   | csp-central-elkvm3        | 4/32G
| 24     | Regional ELK VM                  | csp-regional-elkvm1       | 4/32G
| 25     | Regional ELK VM                  | csp-regional-elkvm2       | 4/32G
| 26     | Regional ELK VM                  | csp-regional-elkvm3       | 4/32G
| 27     | Regional SBLB VM                 | csp-regional-sblb1        | 4/32G
| 28     | Regional SBLB VM                 | csp-regional-sblb2        | 4/32G
| 29     | Regional SBLB VM                 | csp-regional-sblb3        | 4/32G
| 30     | Virtual Route Reflector VM       | csp-vrr-vm1               | 4/32G
| 31     | Virtual Route Reflector VM       | csp-vrr-vm2               | 4/32G
| 32     | Virtual Route Reflector VM       | csp-vrr-vm3               | 4/32G
| 33     | Virtual Route Reflector VM       | csp-vrr-vm4               | 4/32G
| 34     | Virtual Route Reflector VM       | csp-vrr-vm5               | 4/32G
| 35     | Virtual Route Reflector VM       | csp-vrr-vm6               | 4/32G
| 36     | Contrail Analytics server        | csp-contrailanalytics-1   | 48/256G
| 37     | Contrail Analytics server        | csp-contrailanalytics-2   | 48/256G
| 38     | Contrail Analytics server        | csp-contrailanalytics-3   | 48/256G


# Medium Deployment
    * 4 Physical servers, 23 Virtual Machines including Space, Virtual Route Reflector are required for deploying all the services in central and regional regions
    * 27 IPs including 1 VIP for space web GUI and 3 VIP for HAPROXY (Central, Regional and Regional Southbound) are required for this installation.

| S.No   | Components                       | Nodes                     | CPU(Cores)/Memory(GB)
| ------ |:--------------------------------:|:-------------------------:|:-----------------------:|
| 1      | Installer VM                     | csp-installer-vm          | 4/32G
| 2      | Central Infrastructure services  | csp-central-infravm1      | 16/64G
| 3      | Central Infrastructure services  | csp-central-infravm2      | 16/64G
| 4      | Central Infrastructure services  | csp-central-infravm3      | 16/64G
| 5      | Central LB VM                    | csp-central-lbvm1         | 4/32G
| 6      | Central LB VM                    | csp-central-lbvm2         | 4/32G
| 7      | Central LB VM                    | csp-central-lbvm3         | 4/32G
| 8      | Central Microservices            | csp-central-msvm1         | 16/64G
| 9      | Central Microservices            | csp-central-msvm2         | 16/64G
| 10     | Central Microservices            | csp-central-msvm3         | 16/64G
| 11     | Space VM                         | csp-space-vm              | 4/32G (Optional)
| 12     | Central ELK VM                   | csp-central-elkvm1        | 4/32G
| 13     | Central ELK VM                   | csp-central-elkvm2        | 4/32G
| 14     | Central ELK VM                   | csp-central-elkvm3        | 4/32G
| 15     | Regional SBLB VM                 | csp-regional-sblb1        | 4/16G
| 16     | Regional SBLB VM                 | csp-regional-sblb2        | 4/16G
| 17     | Virtual Route Reflector VM       | csp-vrr-vm1               | 4/32G
| 18     | Virtual Route Reflector VM       | csp-vrr-vm2               | 4/32G
| 19     | Virtual Route Reflector VM       | csp-vrr-vm3               | 4/32G
| 20     | Virtual Route Reflector VM       | csp-vrr-vm4               | 4/32G
| 21     | Contrail Analytics server        | csp-contrailanalytics-1   | 16/64G
| 22     | Contrail Analytics server        | csp-contrailanalytics-2   | 16/64G
| 23     | Contrail Analytics server        | csp-contrailanalytics-3   | 16/64G


# Small Deployment 
    * 1 Physical servers, 7 Virtual Machines including Space, Virtual Route Reflector are required for deploying all the services in central and regional regions
    * 7 IPs are required for this installation.

| S.No   | Components                       | Nodes                     | CPU(Cores)/Memory(GB)
| ------ |:--------------------------------:|:-------------------------:|:-----------------------:|
| 1      | Installer VM                     | csp-installer-vm          | 4/32G
| 2      | Central Infrastructure services  | csp-central-infravm       | 8/48G
| 3      | Central Microservices            | csp-central-msvm          | 8/48G
| 4      | Central K8 Master                | csp-central-k8mastervm    | 4/8G 
| 4      | Space VM                         | csp-space-vm              | 4/16G (Optional)
| 5      | Regional SBLB VM                 | csp-regional-sblb         | 4/8G
| 6      | Virtual Route Reflector VM       | csp-vrr-vm                | 4/32G
| 7      | Contrail Analytics server        | csp-contrailanalytics-1   | 8/48G


# trial Environment
    * 3 Physical servers, 25 Virtual Machines including a Space, Virtual Route Reflector, Contrail Analytics are required for deploying all the services in central and regional regions
    * 29 IPs including 1 VIP for space web GUI and 3 VIP for HAPROXY (Central, Regional and Regional Southbound) are required for this installation.


| S.No   | Components                       | Nodes                     | CPU(Cores)/Memory(GB)
| ------ |:--------------------------------:|:-------------------------:|:-----------------------:|
| 1      | Installer VM                     | csp-installer-vm          | 4/48G
| 2      | Central Infrastructure services  | csp-central-infravm1      | 4/32G
| 3      | Central Infrastructure services  | csp-central-infravm2      | 4/32G
| 4      | Central Infrastructure services  | csp-central-infravm3      | 4/32G
| 5      | Central LB VM                    | csp-central-lbvm1         | 4/16G
| 6      | Central LB VM                    | csp-central-lbvm2         | 4/16G
| 7      | Central LB VM                    | csp-central-lbvm3         | 4/16G
| 8      | Central Microservices            | csp-central-msvm1         | 8/64G
| 9      | Central Microservices            | csp-central-msvm2         | 8/64G
| 10     | Central Microservices            | csp-central-msvm3         | 8/64G
| 11     | Regional Infrastructure services | csp-regional-infravm1     | 4/32G
| 12     | Regional Infrastructure services | csp-regional-infravm2     | 4/32G
| 13     | Regional Infrastructure services | csp-regional-infravm3     | 4/32G
| 14     | Regional Microservices           | csp-regional-msvm1        | 8/32G
| 15     | Regional Microservices           | csp-regional-msvm2        | 8/32G
| 16     | Regional Microservices           | csp-regional-msvm3        | 8/32G
| 17     | Regional LB VM                   | csp-regional-lbvm1        | 4/16G
| 18     | Regional LB VM                   | csp-regional-lbvm2        | 4/16G
| 19     | Regional LB VM                   | csp-regional-lbvm3        | 4/16G
| 20     | Space VM                         | csp-space-vm              | 4/16G (Optional)
| 21     | Regional SBLB VM                 | csp-regional-sblb1        | 4/24G
| 22     | Regional SBLB VM                 | csp-regional-sblb2        | 4/24G
| 23     | Virtual Route Reflector VM       | csp-vrr-vm1               | 4/8G
| 24     | Virtual Route Reflector VM       | csp-vrr-vm2               | 4/8G
| 25     | Contrail Analytics VM            | csp-contrailanalytics-1   | 16/48G

# OS and Data partitions
    
    * Starting from 4.0.1, the feature of separate OS and Data partitions for CSO deployments is introduced.
    
    * Refer provision vm example configurations for disk sizes.
    
    * KVM OS and Data Partitions are automated whereas for ESXI, create a separate partition based on below specifications
        
    * Size of the OS Partition is 100G, Data partition is 400G with "/mnt/data" as mount path for Data partition and Swap Partition is 64G
 


# ASSUMPTIONS/PREREQUISITIES

    * Network machines (routers) are already configured with required configurations.

    * All target machines are in same subnet.

    * All physical servers where kvm VMs are provisioned should have ubuntu 14.04.5 LTS

    * All VMs (if created NOT using provision_vm.sh)  where csp components are deployed should have Ubuntu 14.04.5 LTS OS

    * Refer deployment guide for VM Specifications and provision the VM with the recommended configuration only.

    * Make sure the DNS server configuration on the nodes are accurate.

    * The installer tool is run from the same subnet as the target machines.

    * All the machines are reachable between each other

    * Pre-installed contrail cloud is required and Keystone Admin Token is required for vCPE deployment

    * All the operations/installations are to be run as "root" user.

    * All machines should have FQDN set correctly(hostname -f)

# DEPLOYMENT STEPS

    * If user does not already have Space and VMs (KVM/ESXi) already provisioned, then "Provision VM" can be used to spin up Ubuntu and Space cluster on Virtual Machines. Refer to the "Provision VM" section below for more details.

    * At high level, deployment has following steps -
        - CLEANUP EXISITING DEPLOYMENT (Based on the need)
        - PROVISION VM
        - SETUP ASSIST (Run from the csp-installer-vm)
        - DEPLOYMENT OF INFRASTRUCUTRE SERVICES
        - DEPLOYMENT OF MICRO SERVICES
        - POST DEPLOYMENT STEPS
           * For vCPE create cspadmin user
        - LOAD DATA (Load initial/default data into the services)

## CLEANUP EXISITING DEPLOYMENT

In case, you already have a deployment and want to deploy afresh do the following. This only applies if you want to completely reset and deploy again
(THIS DOES NOT APPLY IF YOU WANT TO UPGRADE THE EXISTING DEPLOYMENT)

 - Delete all old VMs on the physical server(s)

    * To check existing VM's,

        $ virsh list --all

    * To clean the VM's,

        $ virsh destroy <Name>
      	$ virsh undefine <Name>

    * Delete Ubuntu Source & vm
        $ rm -rf /root/disks
        $ rm -rf /root/disks_can
        $ cd /root/ubuntu_vm
        $ rm -rf <vm directory>

    * Delete old salt keys

        $ salt-key -D

## PROVISION VM

# Note: Provision VM is mandatory for all type of deployments (KVM and Esxi)

# Space:

1. By default Space image is not packaged along with the CSO/NSC tar.
2. According to your use case if there is a need to have space (space-15.1R1.11.qcow2) in your CSO/NSC deployments please download space from   below link and keep in "<project_root>/artifacts" and then continue below steps.
    URL: https://webdownload.juniper.net/swdl/dl/secure/site/1/record/59889.html

# Esxi:

1. Have a Ubuntu VM with kernel version 4.4.0-31-generic with 8GB RAM and 2 vcpu with internet access.

2. Copy the tarball to this VM and untar it.

3. Based on the deployment mode(trial/production) and whether the deployment is small/medium/large, 
    - copy confs/cso4.0.1/<deployment_mode>/<ha/nonha>/provision_vm/provision_vm_example_ESXI.conf to confs/provision_vm.conf.

4. Open the file provision_vm.conf with a text editor.

5. In the [TARGETS] section, specify the following values for the network on which CSO resides
   • installer_ip: The IP of this VM.
   • ntp_servers—Comma-separated list of fully qualified domain names (FQDN) of Network Time Protocol (NTP) servers. For networks within firewalls, specify NTP servers specific to your network.
   • Don’t edit the physical and server names
   
6. Specify the following configuration values for each ESXi host that you specified in Step 5.

    • management_address—IP address of the Ethernetmanagement (primary) interface in classless Internet domain routing (CIDR) notation  for ESXi host
    • gateway—VM Network's gateway address
    • dns_search—Domain for DNS operations
    • dns_servers—Comma-separated list of DNS name servers, including DNS servers specific to your network
    • [hostname]—Hostname of the ESXi host.
    • [username]—Username for logging in to the ESXI host. Should be root user or should have root access.
    • password—Password for logging in to the ESXi host
    • vmnetwork— Labels for each virtual network adapter which is used to identify which virtual network adapter is associated with which physical network.
 
    - The vmnetwork data for each VM is available in the Summary tab of each VM in the vSphere Client. You must not specify vmnetwork data within double quotes.
        • datastore—Datastore value to save all VMs files.
          
    - The datastore data for each VM is available in the Summary tab of each VM in the vSphere Client. You must not specify datastore data within double quotes
        • After running the provision_vm_ESXI.sh, it will bring up the CSP VMs with the configuration provided in the files. However, VRR provisioning is not supported and needs to be brought up manually.
        
# Esxi - VRR Manual provision steps:
1. Download VRR (.ova format) from below URL and lauch the VRR using vcenter or vsphere tool.
   URL: https://webdownload.juniper.net/swdl/dl/secure/site/1/record/72406.html
   
2. Apply below configurations once the VRR able to do ssh access.

        $ configure
        $ delete groups global system services ssh root-login deny-password
        $ set system root-authentication plain-text-password
        $ set system services sshg
        $ set system services netconf ssh
        $ set routing-options rib inet.3 static route 0.0.0.0/0 discard
        $ commit
        $ exit
    
# KVM:

Note: Physical servers should have a internet access to download the libvirt packages and perform the prerequisites which are given below.
 
- Provision VM feature is used to create Virtual machines (ubuntu14.04.5, space, contrail analytics node) in given physical server to install CSO.
   Also the physical server should run ubuntu 14.04.5 LTS

- The PROVISION VM tool can be run from the physical server to create all the Virtual machines including the installer vm. Then the same tarball can be moved to the csp-installer-vm for running the installation.

- A basic setup is required in physical server before launching VM's.


- Follow below steps to complete the basic setup to create a bridge interface

   * apt-get update
   * ifconfig (can see the primary interface eg: "eth0")
   * apt-get install libvirt-bin
   * ifconfig (can see the virtual interface "virbr0")

   * Open "/etc/network/interfaces" and update the config likewise, The given config is assuming "eth0" is going to map with "virbr0".
   If you have more than one interface, create a similar configuration
   for bridging virbr0 to the desired interface. This is required for the VMs to communicate to the network.

        <pre>
        # This file describes the network interfaces available on your system
        # and how to activate them. For more information, see interfaces(5).

        # The loopback network interface
        auto lo
        iface lo inet loopback

        # The primary network interface
        auto eth0
        iface eth0 inet manual
          up ifconfig eth0 0.0.0.0 up

        auto virbr0
        iface virbr0 inet static
                bridge_ports eth0
                address 192.168.1.2
                netmask 255.255.255.0
                network 192.168.1.0
                broadcast 192.168.1.255
                gateway 192.168.1.1
                dns-nameservers 192.168.10.1
                dns-search example.net
         </pre>

   * Edit default.xml of virsh using below command and refer example, (From CSO 4.0.1 version VM network on bridge group
     is supported. Set the bridge group in the field management_interface in provision_vm.conf. Editing the default.xml is not required)

     - virsh net-edit default
         - Change ip address, stp='off', netmask
         - remove nat and dhcp sections

     Example:

     Before edit default.xml
     -----------------------

         <network>
              <name>default</name>
              <uuid>0f04ffd0-a27c-4120-8873-854bbfb02074</uuid>
              <forward mode='nat'/>
              <bridge name='virbr0' stp='on' delay='0'/>
              <ip address='192.168.1.2' netmask='255.255.255.0'>
                <dhcp>
                  <range start='192.168.1.1' end='192.168.1.254'/>
                </dhcp>
              </ip>
         </network>


     After edit default.xml
     -----------------------

         <network>
              <name>default</name>
              <uuid>0f04ffd0-a27c-4120-8873-854bbfb02074</uuid>
              <bridge name='virbr0' stp='off' delay='0'/>
              <ip address='192.168.1.2' netmask='255.255.255.0'>
              </ip>
         </network>


   * Reboot physical server

   * After reboot we can see the "eth0" interface is mapped with "virbr0"


          # brctl show
          bridge name bridge id       STP enabled interfaces
          virbr0      8000.0cc47a010808   no      em1
                                                  vnet1
                                                  vnet2


- Once the bridge interface is ready, now can give the details of VM and Host in the provision_vm.conf and start provisioning VM's.

   * Refer deployment guide and introduction section for VM Specifications and provision the VM with the recommended configuration only.
   * Enter the configuration in confs/provision_vm.conf (The config file is self explanatory, all fields are mandatory for successful VM provisioning)

### PROVISION VM CONFIGURATION 

 - Large 
   * copy confs/cso4.0.1/production/ha/provisionvm/provision_vm_example.conf to
     confs/provision_vm.conf
   * provision_vm_example.conf has the recommended configuration from the deployment guide.

 - Medium 
   * copy confs/cso4.0.1/production/ha/provisionvm/provision_vm_collocated_example.conf to
     confs/provision_vm.conf
   * provision_vm_example.conf has the recommended configuration from the deployment guide.

 - Small 
   * copy confs/cso4.0.1/trial/nonha/provisionvm/provision_vm_collocated_example.conf to
     confs/provision_vm.conf
   * provision_vm_example.conf has the recommended configuration from the deployment guide.

 - Trial HA Environment
   * copy confs/cso/trial/ha/misc/provisionvm/provision_vm_example.conf to
     confs/provision_vm.conf
   * provision_vm_example.conf has the recommended configuration from the deployment guide.

- Refer deployment guide and introduction section for VM Specifications and provision the VM with the recommended configuration only.
- Enter the configuration in confs/provision_vm.conf (The config file is self explanatory, all fields are mandatory to successfull VM provisioning)


- Once the Config file is ready, start VM provisioning using below command.
   * go to project_root
  <pre> ./provision_vm.sh
  </pre>

- Logs for VM provisioning.
   Path : <project_root>/logs/provision_vm.log
          <project_root>/logs/provision_vm_console.log
          <project_root>/logs/provision_vm_error.log

## If CSO is deployed behind NAT, below are NAT rules to access Admin Portal UI, Kibana UI, Rabbitmq Management console etc..
   For example, 
   # Central.
   * The Public IP of NAT central region is 20.13.5.180
   * Northbound Private virtual IP in case of Haproxy HA is 15.15.15.41
   * Then the following rules need to be applied on central NAT servers

            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.180 --dport 443 -j DNAT --to-destination 15.15.15.41:443
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.180 --dport 35357 -j DNAT --to-destination 15.15.15.41:35357
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.180 --dport 5601 -j DNAT --to-destination 15.15.15.41:5601
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.180 --dport 9200 -j DNAT --to-destination 15.15.15.41:9200
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.180 --dport 1947 -j DNAT --to-destination 15.15.15.41:1947

   # Regional.
   * The public IP of NAT regional region is 20.13.5.182
   * Southbound load balancer - private virtual IP in case of Haproxy HA is 15.15.15.42
   * Northbound load balancer - private virtual IP in case of Haproxy HA is 15.15.15.43
   * Regional management interface is 15.15.15.2
   * Then the following rules need to be applied on regional NAT servers
   
            iptables -t nat -A POSTROUTING -o virbr0 -p tcp --dport 5601 -d 15.15.15.43 -j SNAT --to-source 15.15.15.2  # For enabling second host with SNAT
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.182 --dport 5601 -j DNAT --to-destination 15.15.15.43:5601
            iptables -t nat -A POSTROUTING -o virbr0 -p tcp --dport 7804 -d 15.15.15.43 -j SNAT --to-source 15.15.15.2
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.182 --dport 7804 -j DNAT --to-destination 15.15.15.43:7804
            iptables -t nat -A POSTROUTING -o virbr0 -p tcp --dport 3514 -d 15.15.15.42 -j SNAT --to-source 15.15.15.2
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.182 --dport 3514 -j DNAT --to-destination 15.15.15.42:3514
            iptables -t nat -A POSTROUTING -o virbr0 -p tcp --dport 514 -d 15.15.15.42 -j SNAT --to-source 15.15.15.2
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.182 --dport 514 -j DNAT --to-destination 15.15.15.42:514
            iptables -t nat -A POSTROUTING -o virbr0 -p tcp --dport 443 -d 15.15.15.43 -j SNAT --to-source 15.15.15.2
            iptables -t nat -A PREROUTING -p tcp -d 20.13.5.182 --dport 443 -j DNAT --to-destination 15.15.15.43:443
            iptables -t nat -A PREROUTING -d 10.213.5.182/32 -p tcp -m tcp --dport 2216 -j DNAT --to-destination 15.15.15.42:2216
            iptables -t nat -A POSTROUTING -d 15.15.15.42/32 -o virbr0 -p tcp -m tcp --dport 2216 -j SNAT --to-source 15.15.15.2



## Copying the Installer Package to the Installer VM
 
After you have provisioned the VMs, move the installer package from the central server to the installer VM.

1. Copy the installer package file from the central CSO server to the installer VM.
2. Log in to the installer VM as root.
3. Expand the installer package.
For example, if the name of the installer package is csoVersion.tar.gz:
root@host:~/# tar –xvzf csoVersion.tar.gz
The contents of the installer package are placed in a directory with the same name as the installer package. In this example, the name of the directory is csoVersion.
4. If you have created an installer VM using the provisioning tool, you must copy the /csoVersion/confs/provision_vm.conf file from the Ubuntu VM to the
/csoversion/confs/provision_vm.conf directory of the installer VM.
5. Open the file provision_vm.conf with a text editor.
6. For installer_ip in the [TARGETS] section, specify the IP address of the installer VM.
7. Save the file.

## SETUP ASSIST

This tool assists you in installing/updating your CSO/NSC solution.

First it sets up the installer by downloading packages from the local repo (debian and pip) in installer vm.

Then it starts an interactive script which asks you questions to prepare the configuration files. At the end of the script,
it will provide the commands to be executed to complete the installation.
This script is re-runnable. It will remember the values from your previous run. In case you need to change any configuration in your deployment you can just re-run this script.


# On-Perm
  <pre> ./setup_assist.sh
  </pre>

# AWS
  <pre> ./setup_assist_aws.sh
  </pre>

### CONFIGURATION CREATION (TOPOLOGY & ROLES CONF)

1. Setup assist will populate user interactive question to feed the inputs for topolgy and will create the roles.conf w.r.t the topology

2. Major Classifications of user inputs in setup assist.

    - Do you have an external private repository (y|n):
        * All the libraries required for the cso3.3 solution will be downloaded from private repository
        * Private repository can be created either on the installer VM or on the external server
        * Entering 'n' will create the private repository in the installer VM

        How to create external private repository
           * Copy cso4.0.1 tar file in a server which runs os version ubuntu 14.04.5 LTS
           * Extract the cso4.0.1 tar file (tar -xvzf Contrail_Service_Orchestration_4.0.1.tar.gz)
           * Execute the script ./create_private_repo.sh located under directory /root/Contrail_Service_Orchestration_4.0.1
           * While running setup assist in the installer VM, answer 'y' to the question "Do you have an external private repo(y|n)"

    - The deployment environment that you want to setup. (production):
        * Default is production. 

    - Select the deployment type. (small/medium/large):
        * Input any required deployment type. Topology will be picked up based on this input. Default is small.
        
    - Do you need a HA deployment (y/n):
        * (y) - will pickup the topology w.r.t HA and based on above given environment
        * (n) - will pickup the topology w.r.t NONHA and based on above given environment

    - Installer IP Address (This machine's address)
        * Provide the Management IP address from where setup assist script executes

    - Timezone for the servers in topology [America/Los_Angeles]:
        * mostly keep it as default, other wise refer ubuntu timezone servers and feed the correct value

    - List of ntp servers (comma seperated):
        * Provide the correct ntp address (eg: ntp.ubuntu.net)

    - Common password for all infra components [passw0rd]:
        * provide the password which will apply for all infra components except mariadb admin and cluster password

    - Public DNS name that will be used to access CSO Administration Portal (example: jcs.juniper.net)[]:
        * provide the correct DNS name, if you want to access admin portal using single sign-on method, otherwise for local authentication please provide dummy hostname

    - Do you want to enable TLS mode of authentication between device to FMPM services?. (y/n) []:
        * (y) - this will enable tls settings in haproxy of sblb for communication between device and FMPM services
        * (n) - this will disable tls settings in haproxy of sblb for communication between device and FMPM services

    - The Autonomous System Number for BGP [64512]:
        * provide the Autonomous System number 
        
    - Provide the regional region name(s) (For centralized deployment,if more than one region, provide comma separated list of region names) [regional]:
        * In case of large deployment, provide regional region names. This does not apply for small/medium deployments. 
        
    - Do you have an external keystone (y/n):
        * (y) - Is applicable only for contrail keystone and provide the correct details of contrail keystone
        * (n) - this will install standalone keystone and provide the user specific details for keytone

    - Do all your VMs have same password for root [y]:
        * (y) - The password entered will be used for central, regional and contrail analytics VMs
        * (n) - Password will be prompted for all the VMs

    - Enter the subnet where all CSO VMs resides, give dummy value for vcpe (network cidr)):
         The LOAD DATA step sets up the static route value in topology microservice to be installed in NFX for csp-dc subnets using the MX interface ip (connected to NFX’s WAN0) as next-hop. Provide this network value here.

    - Is CSO behind NAT (y/n) [n]:
        * (y) - is applicable only for VMs are in private network
          - if (y)
            * The public IP of NAT region central[]:  - Provide the public ip which is used for NAT between public and private hosts or VMs
            * The public IP of NAT region regional[]: - Provide the public ip which is used for NAT between public and private hosts or VMs
          - Note: the public ip will not do any NATing, NATing should be done manually but the above IP's used for Swift internal and external data upload

        * (n) - by default option is (n) and most of the deployments are with public accessible vms.

    - Enter the primary interface for all VMs [eth0]:
        * don't edit the about default input unless there is a change in interface on your host or vms

    - Topology Details. Region: central/regions(regional)
        * Provide the management IP's with cidr of server/vm
        * Provide the FQDN based hostname of server/vm
        * Provide the password for root user of server/vm

    - Kubernetes overlay network cidr used by the microservices - (central/regions(regional))
        * By default it is 172.16.0.0/16. if this value clash with your network range then change it to a different /16 subnet

    - Kubernetes Services overlay network range (cidr) used by the microservices [192.168.3.0/24]:

    - Kubernetes Service APIServer IP which is in above cidr range [192.168.3.1]:

    - Kubernetes Cluster DNS IP used by skydns which should be in the Kubernetes Services overlay network range [192.168.3.1]:

    - Enter the tunnel interface unit range which can be used by cso [4000,6000]
        * This is the tunnel interface unit range which will be used by cso

    - The FQDN exposed by load balancer to access the solution (IP Address or domain, virtual IP in case of Haproxy HA) - (central/regions(regional))
        * Non HA - provide lbvm ipaddress to access solution
        * HA - Provide lb VIP to access solution

    - Number of replicas of each microservice:
        * NON HA - default is 1
        * HA - Production Env: 3, trial Env:2

  #### At the end, Topology, roles.conf will be generated w.r.t above given inputs. On completion setup assist will provide the list of commands to be executed to proceed with the installation.


## Deploy Infraservices

This step deploys the Infrastructure services based on the topology.

For small/medium setup, 

    $ ./deploy_infra_services.sh

For large setup, 

    $ DEPLOYMENT_ENV=central ./deploy_infra_services.sh
    $ DEPLOYMENT_ENV=regional ./deploy_infra_services.sh

# Note: central and regional infra deployment can run in parallel with atleast 10 mins of time gap between central first and then regional

## Deploy Microservices

This step deploys the micro services on top of the infrastructure.

# On-Premises

For small/medium setup, 

    $ ./deploy_micro_services.sh
 
For large setup, 
   
    $ DEPLOYMENT_ENV=central ./deploy_micro_services.sh
    $ DEPLOYMENT_ENV=regional ./deploy_micro_services.sh

# AWS
    Enable RP Filter for Proxy Nodes
    $ ./scripts/manage_rp_filter.sh add

    $ DEPLOYMENT_ENV=central ./deploy_micro_services.sh
    $ DEPLOYMENT_ENV=regional ./deploy_micro_services.sh


### Different usages of deploy Micro services

    - To restart all pods (This will delete pods and recreate it)


    $ DEPLOYMENT_ENV=central ./deploy_micro_services.sh --restart_containers
    $ DEPLOYMENT_ENV=regional ./deploy_micro_services.sh --restart_containers


    - To cleanup all the databases and reset the entire the kubernetes cluster


    $ DEPLOYMENT_ENV=central ./deploy_micro_services.sh --reset_databases
    $ DEPLOYMENT_ENV=regional ./deploy_micro_services.sh --reset_databases


    - To cleanup entire the kubernetes cluster

    $ DEPLOYMENT_ENV=central ./deploy_micro_services.sh --reset_cluster
    $ DEPLOYMENT_ENV=regional ./deploy_micro_services.sh --reset_cluster

## LOAD DATA

  Before running load data make sure all microservices are up and running.

# On-Premises
   <pre>
   ./load_services_data.sh
   </pre>

# AWS
   <pre>
   ./load_services_data.sh
   </pre>

    Disable RP Filter for Proxy Nodes
    $ ./scripts/manage_rp_filter.sh remove

## POST DEPLOYMENT STEPS

- For vCPE with contrail keystone use case - create cspadmin user to access Admin Portal and it is validated only for Contrail 3.0.2

1. Run below commands in any of the contrail controller which is using the cso3.3 solution

     # source /etc/contrail/keystonerc
     If above source path is not available then use the keystonerc configured path during contrail installation.

     # keystone user-create --name="cspadmin" --pass="contrailadmin_password" --tenant="admin"
     # keystone tenant-create --name="default-project" --description="Default Tenant"
     # keystone user-role-add --tenant default-project --user admin --role admin
     # keystone user-role-add --tenant default-project --user cspadmin --role admin
     # keystone role-create operator
     # keystone role-create tenant-operator

2. Created groups including ‘admin’, ‘_member_’, ‘operator’ if keystone does not have
   ** <ip> - central-ms-vm ip or central VIP IP incase of HA setup **

   curl -H "x-auth-token:<AdminToken>" -H "content-type:application/json" -d '{"group": {"name": "operator", "domain_id": "default"}}' -XPOST http://<ip>:5000/v3/groups

   curl -H "x-auth-token:<AdminToken>" -H "content-type:application/json" -d '{"group": {"name": "admin", "domain_id": "default"}}' -XPOST http://<ip>:5000/v3/groups

   curl -H "x-auth-token:<AdminToken>" -H "content-type:application/json" -d '{"group": {"name": "_member_", "domain_id": "default"}}' -XPOST http://<ip>:5000/v3/groups

3. Add ‘admin’ and ‘cspadmin’ to both ‘admin’ group and ‘_member_’ group
   ** <ip> - central-ms-vm ip or central VIP IP incase of HA setup **

   curl -g -i -X PUT http://<ip>:5000/v3/groups/<group-id>/users/<user-id>  -H "Accept: application/json" -H "X-Auth-Token:<AdminToken>"

4. Add system_user property to cspadmin, admin & neutron users
   ** <ip> - central-ms-vm ip or central VIP IP incase of HA setup **

   curl -X PATCH -H "X-Auth-Token:<AdminToken>" http://<ip>:35357/v3/users/<user-id> -d '{"user": {"system_user": 1 }}'

Note: "contrailadmin_password" is keystone admin_password which is given in roles.conf for vCPE with contrail keystone use case

## LOAD DATA

  Before running load data make sure all microservices are up and running.

   <pre>
   ./load_services_data.sh
   </pre>

# UPGRADE:
Upgrade to CSO Release 4.0 from Release 3.3.1 is independent of the deployment type (HA or non-HA), environment type (trial or production) and the hypervisor type(KVM or ESXi)

## Pre-requisites:
* Installer VM should be up and running.
* It is mandatory to have provision_vm.conf under confs/ folder of CSO3.3.1 for the upgrade script to check VM health and take VM snapshot. 
* In case of KVM, this file is present already. 
* In case of ESXi, the user can refer example file according to the type of deployment and create the same. 
    For example, if the type is large, the example file can be found at 
        - confs/cso4.0.1/production/ha/provisionvm/provision_vm_example_ESXI.conf
* Need to have provisioned as many VMs as mentioned in the example provision_vm.conf file. 
    For example, if CAN VM is not provisioned, upgrade will fail, because external CAN is not supported in case of upgrade.

#### Run ./upgrade.sh to start the upgrade process.

* Check if VMs are reachable and ensure if there is enough free disk space on the host servers (>100 GB)
* Put CSO to maintenance mode, which means there will be no incoming traffic. Accessing admin portal UI will redirect you to the maintenance page.
* Ask user choice if a snapshot of the VMs (except installer and VRR) is to be taken. This question is skipped in case of production HA setup, if provision_vm.conf file is present.
  Otherwise, the user has to manually take VM snapshots. 
* After snapshot, the user can manually check if the snapshot is successful.
    * SSH to the host server and run below command.
      $ virsh list --all
      
      For example, if central-infra-vm is the name of the VM in the list, execute below command 
      $ qemu-img snapshot -l /root/ubuntu_vm/central-infra-vm/central-infra-vm.img
* Once the upgrade process is successful, CSO maintenance mode is disabled. Sample output for a successful upgrade:  


    INFO     ===============================================
    INFO                Overall Upgrade Summary            
    INFO     ===============================================
    INFO      Configuration Upgrade : success 
    INFO      System Health Check : success 
    INFO      CSO Health-Check Before Upgrade : success 
    INFO      CSO Maintenance Mode Enabled : success 
    INFO      Kernel Upgrade : NA 
    INFO      VM Snapshot : success 
    INFO      Selective Infra Components Upgrade : NA 
    INFO      Central Infra Upgrade : success 
    INFO      Regional Infra Upgrade : success 
    INFO      Microservices pre-deploy scripts execution : success 
    INFO      Central Microservices Upgrade : success 
    INFO      Regional Microservices upgrade : success 
    INFO      Microservices post-deploy scripts execution : success 
    INFO      CSO Health-Check after Upgrade : success 
    INFO      Enable CSO Services : success 
    INFO      Load Microservices Data : success 
    INFO     ============================================
    INFO     CSO is successfully upgraded to Release Contrail_Service_Orchestration_4.0.1
    INFO     ============================================  
    
    The status for each of the sub-process is stored in upgrade/upgrade.conf 
    
* If the upgrade process is unsuccessful, run revert.sh to go back to CSO3.3.1.
  
  Note: Snapshot availability is a pre-requisite for revert.sh
  
    Sample output for a successful revert:
    
    
    INFO ===============================================  
    INFO Overall Revert Summary  
    INFO ===============================================   
    INFO Revert VM Snapshots : success  
    INFO Revert Kernel Upgrade : NA
    INFO Revert Salt Master Configuration: success  
    INFO Post-revert processes: success  
    INFO CSO Health-Check after Revert: success  
    INFO Start Kubernet Pods: success  
    INFO Enable CSO Services: success  
    INFO ================================================  
    INFO CSO successfully reverted to the previously installed version  
    INFO ================================================  

## Limitations:
* There are no changes to the device image or device configurations.
* If upgrade is successful, reverting back to the previous installed version of CSO is not supported.


ACCESSING THE UI's:
===================

** Note **

Desgin tools (design-tools) microservices is not applicable for NSC package

- Accessing Admin-portal-ui

	https://central-ms-vm/admin-portal-ui/index.html

	central VIP IP incase of HA setup.

	https://central-vip-ip/admin-portal-ui/index.html

	Creds: use keystone credentials used in roles.conf. After first login you have to change the password.

- Accessing NSD-UI

	https://central-ms-vm:83/nsd-ui/index.html

	Creds: use the password that was changed in the previous step.


- Accessing SIM

	http://central-ms-vm:1947/icingaweb2

	Default Creds: icinga/ <common infra password given during setup assit>

- Accessing KIBANA (No Credentials are required to access the UI)

	http://central-infra-vm:5601

	http://regional-infra-vm:5601

	Please click on the CREATE tab under 'settings' on 	kibana UI in order to create the 'csplogs*' index.

- Infra and Microserices - Haproxy status page

	http://central-haproxyip:1936

	http://regional-haproxyip:1936

    haproxyip - for HA use VIP and for non ha use direct microservices vm ip.

Import the Visualizations to Kibana:
************************************

- Go to Kibana UI --> Settings --> Indices

	Please click on the green colored "CREATE" button under 'settings' on kibana UI in order to create the 'csplogs*' index.


- cd <package_name>/deploy_manager/export.json

   copy export.json to your desktop from where file can be imported to the kibana UI.( Note: Please make sure not to change the format of the json file).

- Now on Kibana UI : Go to Settings --> Objects
   Click 'Import'
   Navigate to choose the 'export.json' file from the location where we just copied the file to.

- Refresh Kibana UI, go to dashboards to view the visualizations.

   Once NS-instantitaion is made, logs will pop up in the kibana dashboard.

# Components health check:

* To check health of infrastructure components
    $ ./components_health.sh
    
* To check health of central infrastructure components
    $ ./components_health.sh central
    
* To check health of regional infrastructure components
    $ ./components_health.sh regional
    
* This health check is called internally after deploy infra services


FAQ:
----

### Upgrade:

* If your upgrade is stuck with the message ‘Going to sync salt’ for a long time, 
    * Open another instance of Installer VM. 
    * Execute _salt ‘*’ test.ping_. 
    * If you get the message, ‘Salt request timed out. The master is not responding. If this error persists after verifying the master is up,worker_threads may need to be increased’, 
    execute, _service salt-master restart_. 
    * Execute _salt ‘*’ test.ping_ again. It should return True for all the VMs.  
    
* If you get the error message ‘Could not free cache on host server <servername>’,
    * SSH to the host server. 
    * Execute _free && sync && echo 3 > /proc/sys/vm/drop_caches && free_ 
    
* If CSO health check reports, ‘One or more kube-system pods are not running’
    * SSH to the Micro service VM. 
    * Execute _kubectl get pods –namespace=kube-system_
    * If kube-proxy is not in “Running” state, execute, _Kubectl apply –f /etc/kubernetes/manifests/kube-proxy.yaml_
    
* If CSO health check reports, ‘One or more nodes down’ for Kubernetes, 
    * SSH to the Micro service VM
    * Execute _kubectl get nodes_
    * Check the node’s IP that is in “NotReady” state. 
    * SSH to the corresponding VM
    * Execute _service kubelet restart_. 
    * Execute _kubectl get nodes_ again and verify if all the nodes are in “Ready” state. 
    
* If upgrade fails during snapshot and the Overall Upgrade Summary looks like below: 

        Configuration Upgrade: success
        System Health Check: success
        CSO Health-Check before Upgrade: success
        CSO Maintenance Mode Enabled: success
        VM Snapshot: failure
    
    * If upgrade fails during snapshot with the following error: ‘Could not shut down the following VMs [< >, < >…]’,
        * SSH to host server
        * Execute _virsh shutdown \<vm name>_ for each of the VMs in the above list. 
        * If it is successful, you can continue upgrade by rerunning the script. 
        * If it fails, snapshot can’t be taken. 
    * If upgrade fails during snapshot with the following error: ‘Could not take a snapshot of the following VMs [< >,  < >..]’, then snapshot can’t be taken. 

    * In both the failure cases, 
        * SSH to the host server. 
        * Execute virsh list –all 
        * You will see the list of VMs that are shut off
        * Execute _virsh start <vm name>_. 
        * Maintain an order when starting VMs manually this way – ['infra', 'lbvm', 'sblb', 'contrail', 'k8', 'msvm']
        * On the installer VM, execute, _cat upgrade/upgrade.conf | grep clear_cache_pods_
        * If you get _clear_cache_pods: success_, execute, _./python.sh vm_snapshot/scale_pods.py revert_
        * SSH to central infra VM. 
        * In case of HA, log in to any of the central infra VMs. 
        * Execute, _etcdctl set /lb/maintenance false_


### Recovery:

The recovery utility will recover following infra components in HA environment in case of cluster issues.
1. Redis
2. Mariadb
3. Rabbitmq

Run below command that will prompt the options to select the components,

$ ./recovery.sh

### Enabling Prometheus and cadvisior for On-Premises

By default prometheus and cadvisior is disabled in the CSO setup. To enable it run the below command from the installer VM


<region> - denotes central and regional regions ( incase of multi region regional can be multiple and apply the command for each region)

$ kubectl --kubeconfig=/etc/kube/<region>/kubeconfig apply -f cadvisor.yaml
 
$ kubectl --kubeconfig=/etc/kube/<region>/kubeconfig apply -f prometheus.yaml

 
 
