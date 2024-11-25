### Transport Modules
Transport modules are used by captagent to send packets and reports to collectors using different methods and protocols. By default, the *HEP* method is activated.

#### HEP
The *HEP* module is used to define a HEP collector for captured packets, such as [HOMER](http://github.com/sipcapture/homer/wiki).

The critical parameters are:

* capture-host: defines the IP/hostname of the collector
* capture-port: defines the PORT to deliver HEP packets at the collector
* capture-proto: defines the transport protocol for HEP packets [udp/tcp]
* capture-id: defines a unique delivery HEP-ID to be used for filtering

NOTE: Parameters such as ```capt-password``` and ```payload-compression``` are currently only used in advanced deployments and can be ignored for standard setups.

##### Example HEP Configuration:
```
<?xml version="1.0"?>
<document type="captagent_module/xml">
    <module name="transport_hep" description="HEP Protocol" serial="2014010402">
        <profile name="hepsocket" description="Transport HEP" enable="true" serial="2014010402">
            <settings>
                <param name="version" value="3"/>
                <param name="capture-host" value="your.homer.ip"/>
                <param name="capture-port" value="9060"/>
                <param name="capture-proto" value="udp"/>
                <param name="capture-id" value="2016"/>
                <param name="capture-password" value="myHep"/>
                <param name="payload-compression" value="false"/>
            </settings>
        </profile>
    </module>
</document>
```



#### JSON
The *JSON* module is used to define a collector used by captagent to deliver JSON messages and statistics, such as [HOMER](http://github.com/sipcapture/homer/wiki).

The critical parameters are:

* capture-host: defines the IP/hostname of the JSON API
* capture-port: defines the PORT for the JSON API
* capture-proto: defines the transport protocol for HEP packets [udp|tcp]
* capture-id: defines a unique delivery HEP-ID to be used for filtering
* payload-send: defines if full packet payload should be included in API POST [true|false]

##### Example JSON Configuration:

```
<?xml version="1.0"?>
<document type="captagent_module/xml">
    <module name="output_json" description="JSON Protocol" serial="2014010402">
	<profile name="jsonsocket" description="Output JSON" enable="true" serial="2014010402">
	    <settings>
		<param name="capture-version" value="1"/>
		<param name="capture-host" value="127.0.0.1"/>
		<param name="capture-port" value="9061"/>
		<param name="capture-proto" value="udp"/>
		<param name="capture-id" value="2001"/>
		<param name="payload-send" value="true"/>
	    </settings>
	</profile>
    </module>
</document>
```
