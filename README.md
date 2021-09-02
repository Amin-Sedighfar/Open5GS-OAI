# Open5GS-OAI
Open5GS contains a series of software components and network functions that implement the 4G/ 5G NSA and 5G SA core functions.
The main difference of NSA (Non-Standalone Architecture) and SA (Standalone Architecture) is that NSA anchors the control signaling of 5G Radio Networks to the 4G Core, while the SA scheme connects the 5G Radio directly to the 5G core network, and the control signaling does not depend on the 4G network at all. NSA, as the name suggests, is a 5G service that does not ‘stand alone’ but is built over an existing 4G network. SA, on the other hand, allows completely independent operation of a 5G service without any interaction with an existing 4G core.

 
Per 3GPP TR 21.915, Two deployment options are defined for 5G: 

1. The “Non-Stand Alone” (NSA) architecture, where the 5G Radio Access Network (AN) and its New Radio (NR) interface is used in conjunction with the existing LTE and EPC infrastructure Core Network (respectively 4G Radio and 4G Core), thus making the NR technology available without network replacement. In this configuration, only the 4G services are supported, but enjoying the capacities offered by the 5G New Radio (lower latency, etc). The NSA is also known as “E-UTRA-NR Dual Connectivity (EN-DC)” or “Architecture Option 3”.
2. The “Stand-Alone” (SA) architecture, where the NR is connected to the 5G CN. Only in this configuration, the full set of 5G Phase 1 services are supported. 

![image](https://user-images.githubusercontent.com/87240174/131412506-1de0a508-a656-4b20-b0d9-41a76a8ff43e.png)
4G/ 5G NSA Core

The Open5GS 4G/ 5G NSA Core contains the following components:

    MME - Mobility Management Entity
    HSS - Home Subscriber Server
    PCRF - Policy and Charging Rules Function
    SGWC - Serving Gateway Control Plane
    SGWU - Serving Gateway User Plane
    PGWC/SMF - Packet Gateway Control Plane / (component contained in Open5GS SMF)
    PGWU/UPF - Packet Gateway User Plane / (component contained in Open5GS UPF)
# Install Open5GS
Ubuntu 20.04
Ubuntu makes it easy to install Open5GS as shown below,

 $ sudo apt update   
 $ sudo apt install software-properties-common   
 $ sudo add-apt-repository ppa:open5gs/latest   
 $ sudo apt update   
 $ sudo apt install open5gs   

# Install the WebUI of Open5GS
The WebUI allows you to interactively edit subscriber data. 

  $ sudo apt update   
  $ sudo apt install curl    
  $ curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -   
  $ sudo apt install nodejs   
  $ curl -fsSL https://open5gs.org/open5gs/assets/webui/install | sudo -E bash -   
 
 
 # Configure Open5GS
 ![image](https://user-images.githubusercontent.com/87240174/131413358-46d15cf1-1302-4082-8344-a621cdcd7a4a.png)

 There are some tweaks you will need to make to the config files
# Setup a 5G Core
Each VMs are as follows.  
 VM # 	SW & Role 	IP address 	OS 	Memory (Min) 	HDD (Min)  
 VM1 	Open5GS EPC C-Plane 	192.168.0.111/24 	Ubuntu 20.04 	1GB 	20GB  
 VM2 	Open5GS EPC U-Plane1 	192.168.0.112/24 	Ubuntu 20.04 	1GB 	20GB  
 VM3 	Open5GS EPC U-Plane2 	192.168.0.113/24 	Ubuntu 20.04 	1GB 	20GB  
 VM4 	OpenAirInterface UE / RAN 	192.168.0.120/24 	Ubuntu 18.04 	2GB 	40GB  
 OAI UE / RAN cannot be built on Ubuntu 20.04, so build it on Ubuntu 18.04  

Subscriber Information (other information is the same) is as follows.
  UE # 	IMSI 	APN 	OP/OPc  
  UE0 	001010000000100 	internet 	OPc  
  UE1 	001010000000101 	internet2 	OPc  
  UE2 	001010000000102 	internet2 	OPc  
  UE3 	001010000000103 	ims 	OPc  
  UE4 	001010000000104 	ims 	OPc  
 ## Changes in configuration files of Open5GS EPC C-Plane
 First, copy .yaml files to have a backup  
 then do all of the change like below and check with diff -u command  
 Modify /etc/open5gs/mme.yaml to set the S1AP IP address, PLMN ID, and TAC.  
 open5gs/install/etc/open5gs/mme.yaml  
 open5gs/install/etc/open5gs/sgwc.yaml  
 open5gs/install/etc/open5gs/smf.yaml  
 Then you can check the modifications with the below command:  
 $ diff -u /etc/open5gs/mme.yaml.old /etc/open5gs/mme.yaml   
 You can check differences between the original files and uploaded in Core folder.
 ## Changes in configuration files of Open5GS EPC U-Plane1 & 2  
 open5gs/install/etc/open5gs/sgwu.yaml  
 open5gs/install/etc/open5gs/upf.yaml   
 You can check differences between the original files and uploaded in U-plane1&2 folders.
 
 # Register Subscriber Information

Connect to http://localhost:3000 and login with admin account.  

    Username : admin
    Password : 1423

 # Network settings of Open5GS EPC and OAI UE / RAN
 ### Network settings of Open5GS EPC U-Plane1

First, uncomment the next line in the /etc/sysctl.conf file and reflect it in the OS.  
net.ipv4.ip_forward=1  
Next, configure the TUNnel interface and NAPT.  
ip tuntap add name ogstun mode tun  
ip addr add 10.45.0.1/16 dev ogstun  
ip link set ogstun up  
iptables -t nat -A POSTROUTING -s 10.45.0.0/16 ! -o ogstun -j MASQUERADE  
ip tuntap add name ogstun2 mode tun  
ip addr add 10.46.0.1/16 dev ogstun2  
ip link set ogstun2 up  
iptables -t nat -A POSTROUTING -s 10.46.0.0/16 ! -o ogstun2 -j MASQUERADE  

### Network settings of Open5GS EPC U-Plane2
First, uncomment the next line in the /etc/sysctl.conf file and reflect it in the OS.  
net.ipv4.ip_forward=1  
Next, configure the TUNnel interface and NAPT.  
ip tuntap add name ogstun3 mode tun  
ip addr add 10.47.0.1/16 dev ogstun3  
ip link set ogstun3 up  
iptables -t nat -A POSTROUTING -s 10.47.0.0/16 ! -o ogstun3 -j MASQUERADE   

# UE and eNB installation
$ git clone https://gitlab.eurecom.fr/oai/openairinterface5g/ enb_folder  
$ cd enb_folder  
$ git checkout -f v1.0.0  
$ cd ..  
$ cp -Rf enb_folder ue_folder  
## Changes in configuration files of UE

ue_folder/ci-scripts/conf_files/ue.nfapi.conf  
ue_folder/openair3/NAS/TOOLS/ue_eurecom_test_sfr.conf   
You can check differences between the original files and uploaded in UE/RAN folders.

## Changes in configuration files of RAN

enb_folder/ci-scripts/conf_files/rcc.band7.tm1.nfapi.conf  

sudo ifconfig lo: 127.0.0.2 netmask 255.0.0.0 up  

## Build the eNB

$ cd enb_folder  
$ source oaienv  
$ cd cmake_targets  
(If you test less than 16 UEs, type below command.)  
$ ./build_oai -I --eNB -t ETHERNET -c  
(If you test more than 16 UEs, type below command and this command also can be used in case of less than 16 UEs)  
$ ./build_oai -I --eNB -t ETHERNET -c --mu  

##  Build the UEs

$ cd ue_folder  
$ source oaienv  
$ cd cmake_targets  
(If you test less than 16 UEs, type below command.)  
$ ./build_oai -I --UE -t ETHERNET -c  
(If you test more than 16 UEs, type below command and this command also can be used in case of less than 16 UEs.)  
$ ./build_oai -I --UE  -x -t ETHERNET -c --musim  
$ cd ue_folder/targets/bin/  
$ cp .u* ../../cmake_targets/  
$ cp usim ../../cmake_targets/  
$ cp nvram ../../cmake_targets/  

# Run OAI RAN

cd ~/enb_folder/cmake_targets  
./lte_build_oai/build/lte-softmodem -O ../ci-scripts/conf_files/rcc.band7.tm1.nfapi.conf 2>&1 | tee enb.log  
# Run OAI UEs

$ cd ue_folder/cmake_targets/tools  
$ source init_nas_s1 UE  
cd ~/ue_folder/cmake_targets/tools  
source init_nas_s1 UE  
cd ..  
./lte_build_oai/build/lte-uesoftmodem -O ../ci-scripts/conf_files/ue.nfapi.conf --L2-emul 3 --num-ues 5 2>&1 | tee ue.log  

# Related references
https://open5gs.org/open5gs/docs/  
https://github.com/s5uishida/open5gs_epc_oai_sample_config  
https://gitlab.eurecom.fr/oai/openairinterface5g/-/wikis/l2-nfapi-simulator/l2-nfapi-simulator-w-S1-same-machine

# Help & Support
Having trouble?  
Just ask ! between 5 and 10 days I will respond.

