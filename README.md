# Open5GS-OAI
Open5GS contains a series of software components and network functions that implement the 4G/ 5G NSA and 5G SA core functions.
The main difference of NSA (Non-Standalone Architecture) and SA (Standalone Architecture) is that NSA anchors the control signaling of 5G Radio Networks to the 4G Core, while the SA scheme connects the 5G Radio directly to the 5G core network, and the control signaling does not depend on the 4G network at all. NSA, as the name suggests, is a 5G service that does not ‘stand alone’ but is built over an existing 4G network. SA, on the other hand, allows completely independent operation of a 5G service without any interaction with an existing 4G core.

 

Per 3GPP TR 21.915, Two deployment options are defined for 5G: 

    1. The “Non-Stand Alone” (NSA) architecture, where the 5G Radio Access Network (AN) and its New Radio (NR) interface is used in conjunction with the existing LTE and EPC infrastructure Core Network (respectively 4G Radio and 4G Core), thus making the NR technology available without network replacement. In this configuration, only the 4G services are supported, but enjoying the capacities offered by the 5G New Radio (lower latency, etc). The NSA is also known as “E-UTRA-NR Dual Connectivity (EN-DC)” or “Architecture Option 3”.
    2. The “Stand-Alone” (SA) architecture, where the NR is connected to the 5G CN. Only in this configuration, the full set of 5G Phase 1 services are supported. 
