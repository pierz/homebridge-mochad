# homebridge-heyu

Supports X10 devices via heyu on the HomeBridge Platform. Tested with a CM11A
Module connected via a USB Serial adapter. For device configuration, it parses
the heyu configuration file /etc/heyu/x10.conf and creates an accessory for each
alias.  The accessory name is generated from the label ( underscores removed ),
and the device type is generated by the module type.  Currently the code only
recognizes StdLM ( Lamp ) and StdAM ( Outlet ).

Monitoring of changes by other remotes / devices is enabled, and updating back
to HomeKit. This is using the heyu monitor command, and watching for messages
with rcvi.

Please note that for device status to work correctly, please ensure that the heyu
engine is running.  ie have this in your x10.conf

START_ENGINE  AUTO

Also included are two macro's, "All Devices" and "All Lights", which map to the
commands allon/alloff and lightson/lightsoff on housecode A.

On, off, bright and dim commands can also be sent via the CM17A FireCracker module
by setting "useFireCracker" to **true** in the configuration settings.

# Installation

1. Install homebridge using: npm install -g homebridge
2. Install homebridge-heyu using: npm install -g homebridge-heyu
3. Update your configuration file. See sample-config.json in this repository
for a sample.
4. Assumes heyu is installed in /usr/local/bin/heyu, and already configured and
running
5. Homebridge must run under the same id as heyu

# Configuration

```
"platforms": [{
       "platform": "Heyu",
       "name": "Heyu",
       "heyuExec": "/usr/local/bin/heyu",   //optional - defaults to /usr/local/bin/heyu
       "x10conf": "/etc/heyu/x10.conf",     //optional - defaults to /etc/heyu/x10.conf
       "useFireCracker": false              //optional - If true, issues commands via the CM17A FireCracker module
       "cputemp": "cputemp"                 //optional - If present includes cpu TemperatureSensor
   }]
```

#cputemp

Is a shell script I have installed on all my machines to monitor CPU
temperature's.  This will showup with the name of the machine running homebridge.

I have this installed as /usr/local/bin/cputemp

```
#!/bin/bash
cpuTemp0=$(cat /sys/class/thermal/thermal_zone0/temp)
cpuTemp1=$(($cpuTemp0/1000))
cpuTemp2=$(($cpuTemp0/100))
cpuTempM=$(($cpuTemp2 % $cpuTemp1))

echo $cpuTemp1" C"
```

# ToDo
