# Captagent Init Scripts

The init.d script can be used to start/stop captagent in a nicer way. 

### Installation & Configuration

A sample of init.d script for captagent is provided at:
```
/usr/src/captagent/init/deb/debian/captagent.init
```

Just copy the init file to ```/etc/init.d/captagent``` and change the permisions:

```
  cp /usr/src/captagent/init/deb/debian/captagent.init /etc/init.d/captagent
  chmod 755 /etc/init.d/captagent 
```

then edit the file updating the $DAEMON and $CFGFILE values:

```
  DAEMON=/usr/local/captagent/bin/captagent
  CFGFILE=/usr/local/captagent/etc/captagent/captagent.xml
```

You need also setup a configuration file in the /etc/default/ directory:
```
  cp /usr/src/captagent/init/deb/debian/captagent.default /etc/default/captagent
```

When using systemd _(we all wish we were not)_ the service file might be required:
```
  cp /usr/src/captagent/init/deb/debian/captagent.service /etc/systemd/system/captagent.service
```

Once installed and before using, edit the default file to reflect your captagent path:
```
   RUN_CAPTAGENT=yes
   CFGFILE=/usr/local/captagent/etc/captagent/captagent.xml
```


### Automatic Startup
To execute automatically at startup:
```
   update-rc.d captagent defaults
```

To exclude the script from startup:
```
   update-rc.d -f  captagent remove
```
