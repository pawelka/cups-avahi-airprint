# gmkey/cups-avahi-airprint
Fork from [chuckcharlie/cups-avahi-airprint](https://github.com/chuckcharlie/docker-cups-airprint) which was forked 
from [quadportnick/docker-cups-airprint](https://github.com/quadportnick/docker-cups-airprint)

This Alpine-based Docker image runs a CUPS instance with HPLIP drivers that is meant as an AirPrint relay for printers that are already on the network but not AirPrint capable. The other images out there never seemed to work right. I forked the original to use Alpine instead of Ubuntu and work on more host OS's.

## Configuration

### Volumes:
* `/config`: where the persistent printer configs will be stored
* `/services`: where the Avahi service files will be generated

### Variables:
* `CUPSADMIN`: the CUPS admin user you want created
* `CUPSPASSWORD`: the password for the CUPS admin user

### Ports/Network:
* Must be run on host network. This is required to support multicasting which is needed for Airprint.

## Building

```
git clone https://github.com/Jack187/cups-avahi-airprint.git
cd cups-avahi-airprint
docker build \
  --no-cache \
  --pull \
  -t cups-avahi-airprint .
```

## Running

### Example run command:
For testing purposes. Container will be removed once stopped (--rm).

```
docker run -d --rm --name cups --net <your macvlan> \
  -v <your services dir>:/services \
  -v <your config dir>:/config \
  -e CUPSADMIN="<username>" \
  -e CUPSPASSWORD="<password>" \
  -p 631:631/tcp \
  cups-avahi-airprint
```

## Add and set up printer:
* CUPS will be configurable at http://[host ip]:631 using the CUPSADMIN/CUPSPASSWORD.
* Make sure you select `Share This Printer` when configuring the printer in CUPS.
* ***After configuring your printer, you need to close the web browser for at least 60 seconds. CUPS will not write the config files until it detects the connection is closed for as long as a minute.***




