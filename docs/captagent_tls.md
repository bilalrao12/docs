## WARNING!
*This is a work in Progress! Issues must be raised w/ full details + PCAP to reproduce.*

## Captagent TLS
When provided with the appropriate keying material, the TCP protocol module can attempt decryption TLS  connections and display the application data traffic in real-time. 

##### How decryption work 
Internally, the work is divided to resolve the _asymmetric_ and _symmetric_ encryption.
1. The _encrypted pre-master secret_ is captured during the key exchange between Client and Server, and decrypted to obtain a **Pre-Master Secret (PMS)**.
2. The **Master Secret (MS)** is recreated thanks to _PMS_, and used for the symmetric encryption of the keys. The _MS_ is necessary to build the _keys block_, that is dissected and leveraged to regenerate the required decryption keys used in the asymmetric part of the encryption. 
3. These keys (**MACs** or **IVs**, and **Write** keys) perform the final decryption of the _real_ data.

Decryption can only be attempted for scenarios including the full client-server handshake.

##### SUPPORTED:
* TLS_RSA_WITH_AES_256_GCM_SHA384
* TLS_RSA_WITH_AES_128_GCM_SHA256

  * RSA_PKCS1_PADDING

##### UNSUPPORTABLE:
* TLS_DH* (Diffie-Hellman)

-------------

### Requirements

* RSA PRIVATE KEY (max size _2048_ bit)
* libssl-dev
* libgcrypt 1.8 or higher

#### Debian
```
apt-get install -y libgcrypt20 libgcrypt20-dev libssl-dev
```

### Compile & Install
```
./build.sh
./configure --enable-tls --enable-ssl
make && sudo make install
```

### Configure

#### captagent.xml
Enable loading for ```protocol_tls``` under `/usr/local/captagent/etc/captagent/captagent.xml`
```
           [...]
            <load module="protocol_tls" register="local"/>    <---- our module
            <load module="socket_pcap" register="local"/>
           [...]
```
Remember to put `<load module="protocol_tls" register="local"/>` **before** `<load module="socket_pcap" register="local"/>` in order to activate the protocol functions in the correct way.

#### socket_pcap
Configure ```socket_pcap``` and enable the TLS profile block using the proper port or portrange:
```
<profile name="socketspcap_tls" description="TLS Socket" enable="true" serial="2014010402">
	    <settings>
		<param name="dev" value="any"/>
		<param name="promisc" value="true"/>
		<param name="reasm" value="false"/>
		<param name="tcpdefrag" value="true"/>
		<param name="capture-plan" value="tcp_capture_plan.cfg"/>
		<param name="filter">
		    <value>tcp port 5061</value>
		</param>
	    </settings>
	</profile>
```

#### protocol_tls
Configure ```protocol_tls``` with the full path to the required ```private-key``` to decrypt RSA/TLS traffic:
```
<?xml version="1.0"?>
<document type="captagent_module/xml">
  <module name="protocol_tls" description="TLS Protocol" serial="2014010402">
    <profile name="proto_tls" description="TLS PROTO" enable="true" serial="2014010402">
      <settings>
	<param name="flow-timeout" value="180"/>
	<!-- the value of private key refers to the absolute path of the private key (used for decryption) -->
	<param name="private-key-path" value="/path/to/cakey.pem"/>
      </settings>
    </profile>
  </module>
</document>
```

_That's all!_

### Dev Demo
Development [demo setup](https://github.com/lmangani/docker-opensips-hepclient/tree/tls) leverages the default [OpenSIPS rootCA](https://github.com/OpenSIPS/opensips/tree/master/etc/tls/rootCA).

##### TODO
For a complete scenarios it will be desirable to have other supported cipher suites for the RSA family

### Troubleshooting
The code is relatively fresh and might be impacted by TCP reassembly and many other challenging scenarios requiring fine-tuning. If your traffic is not decoded or you receive TLS parsing errors for traffic you can decode with Wireshark, please open and issue w/ full details + PCAP to reproduce.
