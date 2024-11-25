## CaptAgent: Configuration

This section provides guidance to configure Captagent and its core modules on your system.

#### Configuration Logic

Understanding of CaptAgent configuration structure is key - read [this](https://github.com/sipcapture/captagent/wiki/About#sockets-pipes-and-plans) section carefully!


-------------

#### Basic Configuration

The following are the default file locations _(unless otherwise specified during configuration)_:

* Configuration: ```/usr/local/captagent/etc```
* Capture Plans: ```/usr/local/captagent/etc/captureplans```
* Modules: ```/usr/local/captagent/lib/modules```


##### Configuration tree
The default directory should contains the following using default profiles and plans:
```
captagent.xml
captureplans/
    sip_capture_plan.cfg
    rtcp_capture_plan.cfg
    rtcpxr_capture_plan.cfg
    tzsp_capture_plan.cfg
protocol_rtcp.xml
protocol_sip.xml
protocol_rtcpxr.xml
protocol_diameter.xml
socket_pcap.xml
socket_tzsp.xml
socket_collector.xml
transport_hep.xml
output_json.xml

```

##### Main Configuration
To begin, edit and validate the configuration and the module paths in ```/usr/local/etc/captagent/captagent.xml``` to match your actual captagent config/lib path:

```
<configuration name="core.conf" description="CORE Settings" serial="2014024212">
            <settings>
                <param name="debug" value="3"/>
                <param name="version" value="2"/>
                <param name="serial" value="2014056501"/>
                <param name="uuid" value="00781a4a-5b69-11e4-9522-bb79a8fcf0f3"/>
                <param name="daemon" value="false"/>
                <param name="syslog" value="false"/>
                <param name="pid_file" value="/var/run/captagent.pid"/>
                <param name="module_path" value="/usr/local/lib/captagent/modules"/>
                <param name="config_path" value="/usr/local/etc/captagent"/>
                <param name="capture_plans_path" value="/usr/local/etc/captagent/captureplans"/>
                <param name="backup" value="/usr/local/etc/captagent/backup"/>
                <param name="chroot" value="/var/lib/captagent"/>
            </settings>
        </configuration>
```

### Next:

* Configure [Socket Modules](https://github.com/sipcapture/captagent/wiki/Socket-Modules)
* Configure [Transport Modules](https://github.com/sipcapture/captagent/wiki/Transport-Modules)
* Configure [Capture Plans](https://github.com/sipcapture/captagent/wiki/Capture-Plans)
