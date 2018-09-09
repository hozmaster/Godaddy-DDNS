# Godaddy DDNS Script

## Introduction
This script dynamically udpates GoDaddy DNS A record. I have to use Godaddy for a domain and I wanted a way to update the A record whenever the IP changed. This is particularly useful for home networks hosting websites where the domain is hosted at godaddy. This script uses the godaddy api. You can obtain information here as well as keys https://developer.godaddy.com/. Make sure you generate production keys, do not use the test key/secret.


## Dependencies
I build this with the python libraries already installed on the pfSense (FreeBSD) so I would not have to install any additional libraries in pfsense (Hence urllib2, i prefer requests)

- python3.6
- urllib5
- json

## Usage
go_daddy-dns.py [settings].json

Give name of settings json for application. Don't make changes directly to settings.json.example file, copy the example json file to another location path and rename to prefer name.

optionally you can run 'go_daddy-dns.py -h' for these same instructions.

## Cron It!
Obviously you do not want to have to run this script every time the IP changes. Thats what cron is for. You can try it cmd line with crontab -e, Or you can go the easy way and install the Cron pkg in pfSense. Once the package is installed, upload your script to your pfSense box (and remember where you placed it). Then schedule your cron job. Below is a sample cron job:


2	\*	\*	\*	\*	root	/usr/local/bin/python3.6 [/path/to/]/go_daddy-dns.py [source].json 


## HAProxy
Final note, I use HAProxy and noticed that when my IP changed, HAProxy saw the new IP attached to my WAN, but would not allow connections until HAProxy was reloaded. This script reloads HAProxy. If you do not need that function, you can remove line 108.

There is a checkbox 'Reload behaviour' in HAProxy that may do this function. I have not tested, as my IP hasn't changed at the time of this writing. 

## Other

Since this application is bound by MIT license, you can use or fork this application pretty much freely. However, you
should delete any encrypted json files found this project from you copy of this repository.