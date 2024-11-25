In order for captagent to correlate and pair SIP SDP Sessions to RTCP packets, the following configuration block should be uncommented in the ```sip_capture_plan.cfg```:
```
if(sip_has_sdp())
	{
		#Activate it for RTCP checks
		if(!check_rtcp_ipport())
		{
			clog("ERROR", "SDP SESSION ALREADY EXISTS!");
		}
	}
```

This will produce a match for the ```is_rtcp_exist()``` function in the ```rtcp_capture_plan.cfg``` scope to inject a correlation ID and return results in HOMER.
