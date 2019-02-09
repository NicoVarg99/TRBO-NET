# ARS-E DAEMON AUTOMATIC REGISTRATION SERVICE EXTENDABLE  
### Installation Examples / Demos
[KB9MWR Video](https://youtu.be/gxjyrYZn3Ds)

[W8FSM Rasberry Pi Video](http://youtu.be/j7ItqeQou4k)

[KD8EYF Video](http://youtu.be/85EdiW7mbXQ)  

[KD8EYF Beagle Bone Control Station](http://i.imgur.com/9Uu0T.jpg)  


#### 2015-03-05 Updates
  Created SMS send function.
  Created NOAA message function.
  By Wodie XE1SWL.

#### 2013-04-14 Updates
  Created 'updates' branch with following updates. Will test and merge to master.
  Thanks to anonymous (non-working - still branched)

* fix tx msg charset encode
* can send group message
* fix gps negative lat lon, maybe work evrywhere now
* allow configure station symbol for every station
* reverse GeoCode address with "Who callsign" command
* send msgs given in files by other program
* less extra channel loading, do set ars_ping_interval to 0

## Install Instructions  
Assumes a Clean install of Debian Squeeze / Wheezy


Update system and install prereqs
```
sudo apt-get update  
sudo apt-get install build-essential openssh-server git libyaml-tiny-perl libdate-calc-perl libjson-perl  libtest-pod-coverage-perl libssl-dev
```

Download and install the Ham APRS module
```
mkdir ~/src  
cd ~/src  
wget http://search.cpan.org/CPAN/authors/id/H/HE/HESSU/Ham-APRS-FAP-1.20.tar.gz  
tar -zxvf ./Ham-APRS-FAP-1.20.tar.gz  
cd Ham-APRS-FAP-1.20  
perl Makefile.PL  
make  
sudo make install
cd ..  
```
Install the TRBO-NET arsed program  

```
git clone https://github.com/NicoVarg99/TRBO-NET.git  
cd TRBO-NET/  
perl Makefile.PL  
make  
sudo make install  
```

Install CPAN libraries
Used to access the Google gecoding API:

```
cpan Bundle::LWP
```

For Underground weather:

```
sudo cpan Weather::Underground
```

For e-mail sending:

```
sudo cpan Email::Send::SMTP::Gmail
cpan Net::SMTPS
```


Edit the config file by hand to include the DMR radio users you want to listen for.  
the mi5 network config is include as an example of what we did in michigan:  

```
sudo cp configs/arsed.mi5.conf /etc/arsed.conf  
sudo nano /etc/arsed.mi5.conf  
```

Wired network configuration:
Assuming Radio IP of 192.168.10.1 and PC ip of 192.168.10.1 - Write
`    
ip route add 12.0.0.0/8 via 192.168.10.1 dev usb0
`
to file /etc/dhcpcd.exit-hook

Bluetooth network configuration:
Assuming Radio IP of 192.168.11.1 and PC ip of 192.168.11.2 - Write
`    
ip route add 12.0.0.0/8 via 192.168.11.1 dev bnep0
`
to file /etc/dhcpcd.exit-hook

Plug your radio programming cable to the PC.
Now it is time to `reboot`

RUN THE PROGRAM!
```
arsed
```

To start automatically at system boot
add
/usr/local/bin/arsed
to
/etc/rc.local
```
vi /etc/rc.local
```

## Optional: web interface
### Install Apache webserver  
```
apt-get install apache2 libapache2-mod-php5  
cp ~/src/TRBO-NET/web/* /var/www/  
```

connect turbo radio to Linux box using USB  
Device should enumerate and automatically create a network interface. If not check kernel support  


## How To Use
- send text message to gateway radio's ID: it has 'who' command to list registered radios ('w' for short).  
- send 'aprs <callsign> <message>' to send text message to APRS-IS  

once radio starts sending positions, they will be sent to the APRS-IS too  !

View the WebStatus  
- http:/[IP ADDRESS OF SERVER]/state.php  

## Support
Please Feel free to email me, i will be more then happy to help get your installation configured.
