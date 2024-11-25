## NAT
When the SDP details are reporting masqueraded or private IP ranges, the ```nat-mode``` switch can be enabled to force Captagent to use the received address for matching media sessions. Settings enabled by module ```database_hash``` should be applied in file ```database_hash.xml``` :

```
<?xml version="1.0"?>
<document type="captagent_module/xml">
    <module name="database_hash" description="HASH Database" serial="2014010402">
	<profile name="database_hash" description="HASH RTCP" enable="true" serial="2014010402">
	    <settings>
		<param name="timer-expire" value="80"/>
		 <!-- enable if you have nat devices and masqaraded ip -->
		 <param name="nat-mode" value="true"/>		    
	    </settings>
	</profile>
    </module>
</document>
```
