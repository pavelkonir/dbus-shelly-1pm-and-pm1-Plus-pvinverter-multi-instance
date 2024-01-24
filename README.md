# dbus-shelly-1pm-pvinverter and shelly pm 1 plus multi-instance
Integrate Shelly 1PM, shelly pm 1 plus and shelly pm mini plus into Victron Energies Venus OS

## Purpose
With the scripts in this repo it should be easy possible to install, uninstall, restart a service that connects the Shelly 1PM to the VenusOS and GX devices from Victron.
Idea is inspired on @fabian-lauer project linked below.



## Inspiration
This project is fork from https://github.com/Halmand/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance


### Details / Process
As mentioned above the script is inspired by @fabian-lauer dbus-shelly-3em-smartmeter implementation.
So what is the script doing:
- Running as a service
- connecting to DBus of the Venus OS `com.victronenergy.pvinverter.http_{DeviceInstanceID_from_config}`
- After successful DBus connection Shelly 1PM is accessed via REST-API - simply the /status is called and a JSON is returned with all details
  A sample JSON file from Shelly 1PM can be found [here](docs/shelly1pm-status-sample.json)
- Serial/MAC is taken from the response as device serial
- Paths are added to the DBus with default value 0 - including some settings like name, etc
- After that a "loop" is started which pulls Shelly 1PM data every 750ms from the REST-API and updates the values in the DBus

Thats it 😄

### Pictures
![Tile Overview](img/venus-os-tile-overview.PNG)
![Remote Console - Overview](img/venus-os-remote-console-overview.PNG) 
![SmartMeter - Values](img/venus-os-shelly1pm-pvinverter.PNG)
![SmartMeter - Device Details](img/venus-os-shelly1pm-pvinverter-devicedetails.PNG)


## Install & Configuration
### Get the code
Just grap a copy of the main branche and copy them to a folder under `/data/` e.g. `/data/dbus-shelly-1pm-pvinverter`.
After that call the install.sh script.

instance1 The following script should do everything for  you:
```
wget https://github.com/pavelkonir/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance/archive/refs/heads/main.zip
unzip main.zip "dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main/*" -d /data
mv /data/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main /data/dbus-shelly-1pm-pvinverter01
chmod a+x /data/dbus-shelly-1pm-pvinverter01/install.sh
/data/dbus-shelly-1pm-pvinverter01/install.sh
rm main.zip
```


instance2 The following script should do everything for you:
```
wget https://github.com/pavelkonir/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance/archive/refs/heads/main.zip
unzip main.zip "dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main/*" -d /data
mv /data/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main /data/dbus-shelly-1pm-pvinverter02
chmod a+x /data/dbus-shelly-1pm-pvinverter02/install.sh
/data/dbus-shelly-1pm-pvinverter02/install.sh
rm main.zip
```



instance3 The following script should do everything for you:
```
wget https://github.com/pavelkonir/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance/archive/refs/heads/main.zip
unzip main.zip "dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main/*" -d /data
mv /data/dbus-shelly-1pm-and-pm1-Plus-pvinverter-multi-instance-main /data/dbus-shelly-1pm-pvinverter03
chmod a+x /data/dbus-shelly-1pm-pvinverter03/install.sh
/data/dbus-shelly-1pm-pvinverter03/install.sh
rm main.zip
```
⚠️ Check configuration after that - because service is already installed an running and with wrong connection data (host, username, pwd) you will spam the log-file

### Change config.ini
Within the project there is a file `/data/dbus-shelly-1pm-pvinverter01/config.ini` or `/data/dbus-shelly-1pm-pvinverter02/config.ini` or `/data/dbus-shelly-1pm-pvinverter03/config.ini` - just change the values - most important is the deviceinstance, custom name and phase under "DEFAULT" and host, username and password in section "ONPREMISE". More details below:


| Section  | Config vlaue | Explanation |
| ------------- | ------------- | ------------- |
| DEFAULT  | AccessType | Fixed value 'OnPremise' |
| DEFAULT  | SignOfLifeLog  | Time in minutes how often a status is added to the log-file `current.log` with log-level INFO |
| DEFAULT  | Deviceinstance | Unique ID identifying the shelly 1pm in Venus OS |
| DEFAULT  | CustomName | Name shown in Remote Console (e.g. name of pv inverter) |
| DEFAULT  | Phase | Valid values L1, L2 or L3: represents the phase where pv inverter is feeding in |
| DEFAULT  | Position | Valid values 0, 1 or 2: represents where the inverter is connected (0=AC input 1; 1=AC output; 2=AC input 2) |
| DEFAULT  | HwVersion | Valid values: PM1_PLUS, PM_MINI or empty for old PM1 |
| ONPREMISE  | Host | IP or hostname of on-premise Shelly 3EM web-interface |
| ONPREMISE  | Username | Username for htaccess login - leave blank if no username/password required |
| ONPREMISE  | Password | Password for htaccess login - leave blank if no username/password required |

## Uninstall:

instace 1 
```
/data/dbus-shelly-1pm-pvinverter01/uninstall.sh
rm -r /data/dbus-shelly-1pm-pvinverter01
```
instace 2
```
/data/dbus-shelly-1pm-pvinverter02/uninstall.sh
rm -r /data/dbus-shelly-1pm-pvinverter02
```
instace 3
```
/data/dbus-shelly-1pm-pvinverter03/uninstall.sh
rm -r /data/dbus-shelly-1pm-pvinverter03
```
