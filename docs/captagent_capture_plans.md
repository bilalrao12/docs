### Capture Plans
Capture Plans are pipelines attached to capture sockets and utilized to define processing logic, 
using global functions and methods exported by the loaded modules as defined in ```captagent.xml```.

The socket is defined in each capture plan, supporting the following `capture[...]` types:

* pcap
* tzsp
* collector

##### Example Configuration Chain
```socket_pcap``` -> ```{profile}``` -> ```capture_plan```

##### Example Pointer
/usr/local/etc/captagent/socket_pcap.xml
```
        <profile name="socketspcap_sip" description="HEP Socket" enable="true" serial="2014010402">
	    <settings>
		<param name="dev" value="eth0"/>
		<param name="promisc" value="true"/>
		<param name="reasm" value="false"/>
		<param name="tcpdefrag" value="false"/>
		<param name="capture-plan" value="sip_capture_plan.cfg"/>
		<param name="filter">
		    <value>portrange 5060-5091</value>
		</param>
	    </settings>
	</profile>
```

##### Example Capture Plan
/usr/local/etc/captagent/captureplans/sip_capture_plan.cfg
```
capture[pcap] {
	# here we can check source/destination IP/port, message size
	if(msg_check("size", "100")) {
	    #Do parsing
	    if(parse_sip()) {
                # Drop unwanted methods
                if(sip_check("rmethod","OPTIONS") || sip_check("rmethod","NOTIFY")) {
                  drop;
                } 

		#Multiple profiles can be defined in transport_hep.xml	
		if(!send_hep("hepsocket")) {
		    clog("ERROR", "Error sending HEP!!!!");
		}
	    }
	}
	drop;
}
```
