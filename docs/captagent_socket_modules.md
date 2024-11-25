### Socket Modules
Socket modules are used by captagent to capture packets from the system available interfaces. By default, the ```pcap``` socket is enabled.

#### PCAP Socket
Within each socket, multiple profiles can be defined and configured as pipelines to process captured packets. By default the ```socketspcap_sip``` pipeline is enabled defining the interface and ports utilized for capturing and processing ```SIP``` packets via the dedicated [Capture Plan](https://github.com/sipcapture/captagent/wiki/Capture-Plans)

```
      <profile name="socketspcap_sip" description="HEP Socket" enable="true" serial="2014010402">
	    <settings>
		<param name="dev" value="eth0"/>
		<param name="promisc" value="true"/>
		<param name="reasm" value="false"/>
                <param name="websocket-detection" value="false"/>
		<param name="tcpdefrag" value="false"/>
                <!-- <param name="capture-filter" value="ip_to_ip"/> -->
		<param name="capture-plan" value="sip_capture_plan.cfg"/>
		<param name="filter">
		    <value>portrange 5060-5091</value>
		</param>
	    </settings>
	</profile>
```
### GRE-ERSPAN Example
When capturing GRE-ERSPAN Encapsulated traffic this needs to be setup
```
    <param name="erspan" value="true"/>
    <param name="filter">
       <value>proto GRE and len > 50</value>
    </param>
```

### Websocket encapsulation layer
Sometimes WebSocket subprotocol it is used as a reliable transport mechanism between Session Initiation Protocol (SIP) entities to enable use of SIP in web-oriented deployments. 

Captagent provide additional parsing for Websocket layer on TCP by enable the `websocket-detection` param (default is false)
```
   <param name="websocket-detection" value="true"/>
```

### IP-to-IP encapsulation layer
`IP-to-IP` is an IP tunneling protocol that encapsulates one IP packet in another IP packet. To encapsulate an IP packet in another IP packet, an outer header is added with source IP, the entry point of the tunnel, and the destination IP, the exit point of the tunnel.

Captagent now has the possibility to check and correctly parse this tunnel by adding a new `capture-filter` on portrange `5060-590`:
```
   <param name="capture-filter" value="ip_to_ip"/>
```
This filter perform also the internal check on portrange 5060-5090, so in the BPF filter you can simply add this
```
   <param name="filter">
       <value>ip</value>
   </param>
```
