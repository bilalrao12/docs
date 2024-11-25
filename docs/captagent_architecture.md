## Abstract
CaptAgent has been around for a while and went through several redesign phases.

The current version (6) has been completely redesigned from the ground up and ships with new internal architecture expanding its performance range and core capabilities to handle more network protocols, the ability to relay and aggregate traffic for remote agents, parse and handle RTCP-XR and RTP statistics, RPC centralized control and much more in a modular architecture.

## Internal Architecture
##### Sockets, Pipes and Plans

Captagent 6 features a fully modular design enabling users to design and program their packet capture and processing logic, leveraging specialized functionality provided on-demand via loadable dynamic modules.

Core module types include:

| type  | description  |
|:--|:--|
| socket  | responsible for capturing ingress packets according to settings _(ie: PCAP, RAW)_ |
| protocol  | responsible for processing/dissecting/parsing protocol data _(ie: SIP, RTCP)_ |
| transport  | responsible for providing egress transport for generated data _(ie: HEP, JSON)_|
| function  | responsible for providing additional functionality _(ie: database, etc)_ |

Core modules are loaded via the main ```captagent.xml``` configuration file and can be easily concatenated to create multiple, independent capture chains:

<img src="http://i.imgur.com/GalHujZ.png">

In the above example:

```SOCKET``` -> ```PROFILE``` -> ```CAPTURE PLAN``` <--> ```MODULES (functions)```

-------------


###### CAPTURE CHAINS
For each chain, the logic and functionality is managed using a "capture-plan" which defines the behavior of the packet processing pipe. Capture plans are defined within the socket configuration alongside the general capture settings. An example for PCAP socket follows:
```
            <settings>
		<param name="dev" value="any"/>
		<param name="promisc" value="true"/>
		<param name="reasm" value="false"/>
		<param name="capture-plan" value="sip_capture_plan.cfg"/>
		<param name="filter">
		    <value>portrange 5060-5091</value>
		</param>
	    </settings>
```

In the above example, packets captured by the socket would be processed by *capture-plan* in  ```sip_capture_plan.cfg```:

```
# PCAP socket module
capture[pcap] {
        # PROTO SIP module
	# Ie: check source/destination IP/port, message size, etc.
	if(msg_check("size", "100")) {
	    # Parse SIP Protocol
	    if(parse_sip()) {
		# use HEP TRANSPORT module (transport_hep.xml)	
		if(!send_hep("hepsocket")) {
		    clog("ERROR", "Error sending HEP!");
		}
            }
       }
}
```

The capture-plan can access all functions provided by the loaded modules globally.


### Main Features
* Multiple incoming sockets (PCAP, RAW, PF_RING, RX-RING, FILE)
* Multiple outgoing types (HEP, JSON, CSV)
* HTTP JSON API for statistics, config changes etc
* RTCP-XR collector module
* RTCP output module (output in raw or json format)
* Capture scenario configuration (pseudo scripting via flex, bison)
* Call transaction tracking
* TCP/UDP reassembling and defragmentation.
* applying and change capture filter on demand
* LUA scripting (JITLua) (experimental)
* V7 Javascripting sandbox (experimental)
* SIPFIX Support (experimental)
* Websocket encapsulation support
