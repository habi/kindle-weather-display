# Kindle Wall display

Based on [the Kindle Weather Display](https://mpetroff.net/2012/09/kindle-weather-display/) I want to repurpose my old Kindle.
This repository aims to show how I did it from start to finish

# Prerequisites
- My Kindle has the Model No. D01200, and thus is a [Kindle Touch (2012 model)](https://www.google.ch/search?safe=off&source=hp&q=kindle+d01200&oq=kindle+d01200&gs_l=psy-ab.3..0i203k1j0l5j0i67k1j0l3.901.4255.0.4510.14.13.0.0.0.0.163.1285.6j6.13.0....0...1.1.64.psy-ab..1.12.1283.0..35i39k1j0i131k1j0i20i263k1.51.CE7nds_AkUs).
- Jailbreak the Kindle to allow for access, based on [the *current universal method* from the Mobileread Wiki](https://wiki.mobileread.com/wiki/Kindle_Touch_Hacking#CURRENT_UNIVERSAL_METHOD)

# Setup
Since I don't want to 'pay' for running a server, I thought about using a Raspberry Pi in my home network as a server running the server scripts from the [original kindle-weather-display repository from Matthew Petroff](https://github.com/mpetroff/kindle-weather-display).
Probably even easier is to use the rewrite of the orginal repository into a [Go](https://golang.org)-powered, dockered, [Darksky API](https://darksky.net/dev/docs)-backed thingamajig.

This repository covers what I did from start to finish.

# Raspberry Pi
- Buy a [Raspberry Pi Zero W](https://www.pi-shop.ch/raspberry-pi-zero-w)
- Print an [enclosure](https://www.thingiverse.com/thing:2588175)
- Download the [Raspbian Stretch lite](https://www.raspberrypi.org/downloads/raspbian/)
- Burn the image onto a SD card I had lying around with [Etcher](https://etcher.io)
- Mount the SD card on the mac
- In the terminal
  -  `cd /Volumes/boot`
  -  `vi wpa_supplicant.conf` and add your credentials like below (based on [this guide](https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html)).

  
  ````
  network={
    ssid="SSID"
    psk="password"
    key_mgmt=WPA-PSK
}
  ````
  - `touch ssd` to enable SSH access (also based on [this guide](https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html)).
- Unmount the SD card from the Mac and plug it into the Raspberry Pi Zero W
- Power up the RPi (temporarily with a 5V USB power bank until the [power supply](https://www.aliexpress.com/item/AC-100-240V-DC-5V-2A-10W-EU-Plug-USB-Switching-Power-Supply-Adapter-Charger-B119/32807869027.html) arrives)

----

STRUGGLE A LOT HERE!

----


- Wait a while and then `ssh pi@raspberrypi.local`

## kindle-weather-display
* A derivative of
  [https://github.com/mpetroff/kindle-weather-display](https://github.com/mpetroff/kindle-weather-display)

From:
[http://www.mpetroff.net/archives/2012/09/14/kindle-weather-display/](http://www.mpetroff.net/archives/2012/09/14/kindle-weather-display/)

## Refreshed
* Rebuilt using golang for the purposes of learning
* Uses the  (requires a key)

## Dockerfile for Server
* [https://hub.docker.com/r/jtslear/kindle-weather/](https://hub.docker.com/r/jtslear/kindle-weather/)
* Server requires two environment variables:
  * `DARKSKY_API_KEY`
  * `GPS_COORDINATES` - a comma separated string example: `GPS_COORDINATES='37.8267,-122.4233'`
* The shell script will output the file at `/var/lib/www/weather-script-output.png`
  * Do with this as you please

### Example Run Server
* `docker run -e DARKSKY_API_KEY='thisIsADarSkyApiKey' -e GPS_COORDINATES='37.8267,-122.4233' -v /var/lib/www:/var/lib/www jtslear/kindle-weather`
