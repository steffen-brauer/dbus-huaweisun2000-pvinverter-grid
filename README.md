# dbus-huaweisun2000-pvinveter-grid

dbus driver for victron cerbo gx / venus os for huawei sun 2000 inverter & huawei smart meter

## Purpose

This script is intended to help integrate a Huawei Sun 2000 inverter and the measure smart meter values into the Venus OS and thus also into the VRM portal to control ac side battery charge & discharge on a victron inverter

Tested with the following setup:

Huawei SUN 2000 Invert with PV connected power
Victron Inverter with LV Battery connected to AC only.
Raspberry PI running Venus OS connected via LAN to the home network and directly connecting through wifi to huawei inverter internal WiFi. 

To further use the data, the mqtt broker from Venus OS can be usedd


## Useful hints:
While testing the scripts I noticed that requesting a lot of modbus register at the same time from the external huawei supplied WiFi dongle, that the dongle can not handle all of those requests, resulting in connection timeouts or modbus transfer errors.

To mitigate that kind of problems connect to internal WiFi of your inverter.

Modbus was listening on port ```6607```
Modbus slave id was ```0```


## Installation

1. Copy the full project directory to the /data/etc folder on your venus:

    - /data/dbus-huaweisun2000-grid/

   Info: The /data directory persists data on venus os devices while updating the firmware

   Easy way:
   ```
   wget https://github.com/steffen-brauer/dbus-huaweisun2000-pvinverter-grid/archive/refs/heads/main.zip
   unzip main.zip -d /data
   mv /data/dbus-huaweisun2000-pvinverter-grid-main /data/dbus-huaweisun2000-pvinverter-grid
   chmod a+x /data/dbus-huaweisun2000-pvinverter-grid/install.sh
   rm main.zip
   ```


3. Edit the config.py file

   `nano /data/dbus-huaweisun2000-pvinveter-grid/config.py`

5. Check Modbus TCP Connection to gridinverter

   `python /data/dbus-huaweisun2000-pvinterter-grid/connector_modbus.py`

6. Run install.sh

   `sh /data/dbus-huaweisun2000-pvinverter-gid/install.sh`

### Debugging

You can check the status of the service with svstat:

`svstat /service/dbus-huaweisun2000-pvinverter-grid`

It will show something like this:

`/service/dbus-huaweisun2000-pvinverter-grid: up (pid 10078) 325 seconds`

If the number of seconds is always 0 or 1 or any other small number, it means that the service crashes and gets
restarted all the time.

When you think that the script crashes, start it directly from the command line:

`python /data/dbus-huaweisun2000-pvinverter-grid/dbus-huaweisun2000-pvinverter.py`

Also useful:

`tail -f /var/log/dbus-huaweisun2000/current | tai64nlocal`

### Stop the script

`svc -d /service/dbus-huaweisun2000-pvinverter-grid`

### Start the script

`svc -u /service/dbus-huaweisun2000-pvinverter-grid`


### Restart the script

If you want to restart the script, for example after changing it, just run the following command:

`sh /data/dbus-huaweisun2000-pvinverter-grid/restart.sh`

## Uninstall the script

Run

   ```
sh /data/dbus-huaweisun2000-pvinverter-grid/uninstall.sh
rm -r /data/dbus-huaweisun2000-pvinverter-grid/
   ```

# Examples

ToDo: Insert Screenshots from remote console


# Thank you

## Used libraries

This is a fork of https://github.com/kcbam/dbus-huaweisun2000-pvinverter

adding the functionality to also transfer smart meter information to Venus OS on dbus

## this project is inspired by

https://github.com/kcbam/dbus-huaweisun2000-pvinverter

https://github.com/RalfZim/venus.dbus-fronius-smartmeter

https://github.com/fabian-lauer/dbus-shelly-3em-smartmeter.git

https://github.com/victronenergy/velib_python.git
