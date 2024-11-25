### Protocol Modules
Protocol modules are used by captagent to process specific protocol pipelines. 
Protocol modules are loaded at startup by the ```captagent.xml``` general configuration.


```
<configuration name="modules.conf" description="Modules">
	    <modules>
	
		...
		<load module="protocol_sip" register="local"/>
		<load module="protocol_rtcp" register="local"/>		                                  
		...
		
	    </modules>
</configuration>
```

### Functions to use in modules
#### protocol_sip

**Note:** This list is probably incomplete by the time you read it. You can get all functions available here: [protocol_sip.c](https://github.com/sipcapture/captagent/blob/master/src/modules/protocol/sip/protocol_sip.c)

| Function | Description |
| ---| --- |
| `sip_check("method", "INVITE")` | only INVITE requests |
| `sip_check("rmethod", "REGISTER")` | REGISTER requests and answers (CSeq) |
| `sip_check("response", "200")` | only 200 responses                |
| `sip_check("response_gt", "400")` | only responses >= 400          |
| `sip_check("response_lt", "199")` | only responses <= 199          |
| `sip_check("from_user_suffix", "tail")` |  check if from_user has "tail" as suffix   |
| `sip_check("to_user_suffix", "tail")` |  check if to_user has "tail" as suffix   |
| `sip_check("from_user_prefix", "head")` |  check if from_user has "head" as prefix   |
| `sip_check("to_user_prefix", "head")` |  check if to_user has "head" as prefix   |
| `msg_check("size", "100")` | only packets > 100 bytes              |
| `msg_check("src_ip", "10.0.0.1")` | only packets from 10.0.0.1     |
| `msg_check("destination_ip", "10.0.0.1")` | only packets to 10.0.0.1 |
| `msg_check("src_port", "10000")` | only packets from port 10000    |
| `msg_check("src_port_gt", "10000")` | only packets from ports >= 10000 |
| `msg_check("src_port_lt", "10000")` | only packets from ports <= 10000 |
| `msg_check("dst_port", "10000")` | only packets to port 10000      |
| `msg_check("dst_port_gt", "10000")` | only packets to ports >= 10000 |
| `msg_check("dst_port_lt", "10000")` | only packets to ports <= 10000 |
| `sip_is_method()` | check whether this is a request                |
| `parse_sip()` | check for valid SIP message                        |
| `light_parse_sip()` | check whether Call-ID Header is valid        |
| `parse_full_sip()` | check for valid SIP and parse Contact and Via header |
| `sip_has_sdp()` | check whether sip packet has sdp                 |
| `is_flag_set("1", "0")` | check whether flag named "1" has value "0" |
| `send_reply("200", "OK")` | generate a reply from Captagent        |
| `clog("ERR", "foo")` | generate error log line                     |
| `clog("NOTICE", "foo")` | generate notice log line                 |
| `clog("foo", "bar")` | generate debug log line                     |
