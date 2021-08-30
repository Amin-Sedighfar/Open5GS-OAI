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
