↩
# Digital tracking
## General
Tracking is a way to tell that some set of requests came from the same computer

This can be done in a few ways
 - Static Tracking
   - [IP Address ↩](#ip-address)
   - [User-Agent ↩](#http-request)
 - [Cookies ↩](#cookies)
 - System properties
   - [Installed apps ↩](#custom-protocols)
   - [Javascript object properties ↩](#javascript-object-properties)

Shortcuts
 - [VPN ↩](#vpn)
 - [Sorfware ↩](#software)
## TCP
TCP (Transmission control protocol) is a protocol that powers all of the web and most of the internet

A packet in TCP is an object that can be used to transmit data


a TCP packets consist of (not a complete list):
 - Source [IP + port ↩](#ip-address)
 - Destination [IP + port ↩](#ip-address)
 - Data

# IP Address

The IP (Internet protocol) Address is a way of distinquishing computers in a network

When your computer connects to a network (either via WiFi or Ethernet) it gets an IP Address that is unique to that device in this network. This is called a local IP Address

A local IP Address looks something like this: `192.168.100.75` (any number can only be between 0-255 and 0.0.0.0/255.255.255.255 are not valid ips)

But the number of devices connected to the internet is much bigger than the number of valid IP Addresses. And this is where NAT comes in

### NAT
NAT (Network Address Translation) is a way for IP Addresses to be reused by multiple devices

The router (The gateway) when performing NAT changes the source IP Address of the packet to the IP Address given to the router by it's parent while keeping track of the connection, the parent does the same.

The packet ends up at the ISP, gets sent to another ISP that own the destination IP and gets stuck. The ISP does not know to which of a thousand devices connected to them should they send the packet.

To resolve this Port Forwarding is used

## Port Forwarding
Port Forwarding is just a rule that says: `If the destination port of the packet is <...>, then set the destination ip to <...> and send this to <...>-th device connected to me`

This rule is set up in every router that leads to the website server you are trying to contact, and the packet finally gets to it.

This is how it looks something like this:
```
Source IP: 67.43.234.82 <-- your ISP's ip (Your public ip)
Source Port: 61001 <-- usually random
Destination IP: 192.168.70.45 <-- server's local ip
Destination port: 443 <-- standart https port (80 for http)
```

#### How IP Address can be used for tracking
The server could use a giant lookup table to determine your approximate location based on the source ip. This is the main (and the only) reason why [VPN's ↩](#vpn) say that they make your internet anonymous.

# VPN

VPN (Virtual Private Network) or Proxy is a type of software that is mainly used to hide your public ip. It works by modifying outgoing packets to point to the vpn server instead.

What your computer sends:
```
Source IP: 192.168.100.75 <-- Your local IP
Source Port: 61001 <-- Usually random
Destination IP: 89.2.100.98 <-- The website's public IP
Destination Port: 443 <-- Default https port
Data:
+-------encrypted to website--------+
| Password: somesupersecretpassword |
+---^-------------------------------+
    |
  Encrypted, can only be read by the website
```

VPN software changes it to:
```
Source IP: 192.168.100.75 <-- Your local ip
Source Port: 61001 <-- Usually random
Destination IP: 96.100.56.2 <-- VPN server's public IP
Destination Port: 3001 <-- Picked by the VPN provider
Data:
+--------encrypted to vpn server--------+
| Send that to 89.2.100.98 with data:   |
| +--------encrypted to website-------+ |
| | Password: somesupersecretpassword | |
| +-----------------------------------+ |
+---------------------------------------+
```

Your ISP sees:
```
Source IP: 10.20.56.128 <-- The ip of the router 1 step below the ISP
Source Port: 61001 <-- Usually random
Destination IP: 96.100.56.2 <-- VPN server's public IP
Destination Port: 3001 <-- Picked by the VPN provider
Data:
+-------------------------------+
|           ENCRYPTED           |
+-------------------------------+
```

What the VPN server sees:
```
Source IP: 67.43.234.82 <-- Your public IP
Source Port: 61001 <-- Usually random
Destination IP: 192.168.45.2 <-- VPN server's local IP
Destination Port: 3001 <-- Picked by the VPN provider
Data:
 websites public ip ↓
Send that to 89.2.100.98 with data:
+-----------------------------+
|          ENCRYPTED          |
+-----------------------------+
```

When the VPN server recieves that packet it sends this one:
```
Source IP: 192.168.45.2 <-- VPN server's local IP
Source Port: 61001 <-- Usually random
Destination IP: 89.2.100.98 <-- Website's public IP
Destination Port: 443 <-- Default https Port
Data:
+-----------------------------+
|           ENCRYPTED         |
+-----------------------------+
```

And this is that website sees:
```
Source IP: 96.100.56.2 <-- *VPN server's* public IP (Not yours!)
Source Port: 61001 <-- Usually random
Destination IP: 192.168.70.45 <-- Website's local IP
Destination Port: 443 <-- Default https Port
Data:
Password: somesupersecretpassword
```
Related:
 - [IP Address ↩](#ip-address)
 - [TCP ↩](#tcp)

Prevention:
 - [Tor ↩](#tor)
 - I can't recomend any other VPN

# Cookies
Cookies are an incredible technology that allows websites to store small text files on the client. Many websites with a login system give you a unique token that is saved into your cookies when you log-in.

But facebook for example could generate and save a unique string into your cookies, which they can later use to track what websites you visit, because to load the image of that little "like on facebook" widget, the browser sends a request to facebook containing the identification cookie and the website address you visited. And facebook does that

Prevention:
 - [Privacy badger ↩](#privacy-badger)

# User-Agent
One of the headers sent to a website when you connect to it is User-Agent, which give information about your systenm and browser

For example my user agent string is:
```
[REDACTED]
```
Which tells that I use [REDACTED] which is pretty rare

Prevention:
 - [Random User-Agent (Switcher) ↩](#random-user-agent-switcher)

# Custom protocols

Custom protocols is a way for websites to open desktop applications. For example on windows you could type `ms-calculator://` in your browser to open calculator.

Basically you could get a pretty unique device fingerprint by trying to open every existing application openable from the browser, this is called Scheme Flooding (Here's a [demo 🡥](https://schemeflood.com/))

Prevention:
 - [SchemefloodDisabler ↩](#SchemefloodDisabler)

# Javascript object properties
Websites can use certain html object (like canvas) to determine sertain properties of the system. I am not entirely sure how it works, but as far as I know, it has something to do with diffrent behaviour it diffrent systems

Prevention:
 - [ilGur Defenders ↩](#ilgur-defenders)

# Software
This section contains a list of software that can be used to prevent tracking:
 - [Tor ↩](#tor)
 - [Privacy badger ↩](#privacy-badger)
 - [Random User-Agent (Switcher) ↩](#random-user-agent-switcher)
 - [SchemefloodDisabler ↩](#SchemefloodDisabler)
 - [ilGur Defenders ↩](#ilgur-defenders)
 - [uBlock Origin ↩](#ublock-origin)

## Tor
Tor is not a VPN, but it is still used to hide your public IP

It is usually used in form of the Tor Browser which you can download at:

Download: [torproject.org 🡥](https://torproject.org)

## Privacy badger
This is an extention that can block almost all trackers. If it sees 3 diffrent websites making tha same request, it'll block it

Chrome: [Privacy badger 🡥](https://chrome.google.com/webstore/detail/privacy-badger/pkehgijcmpdhfbdbbnkijodmdjhbjlgp)

Firefox: [Privacy badger 🡥](https://addons.mozilla.org/en-US/firefox/addon/privacy-badger17/)

## Random User-Agent (Switcher)
This is an extention that randomly switched your [User-Agent ↩](#user-agent) every couple of seconds (Need manual configuration)

Chrome: [Random User-Agent (Switcher) 🡥](https://chrome.google.com/webstore/detail/random-user-agent-switche/einpaelgookohagofgnnkcfjbkkgepnp)

Firefox: [Random User-Agent (Switcher) 🡥](https://addons.mozilla.org/en-US/firefox/addon/random_user_agent/)

## SchemefloodDisabler
⚠️ This extention is more likely to be malicious that others, use this at your own risk

This extention can prevent [Scheme Flooding ↩](#custom-protocols)

Chrome: Unavailable

Firefox: [SchemefloodDisabler 🡥](https://addons.mozilla.org/en-US/firefox/addon/schemeflooddisabler/)

## ilGur Defenders
This a collection of Fingerpring defenders by ilGur

Chrome: [Font Fingerprint Defender 🡥](https://chrome.google.com/webstore/detail/font-fingerprint-defender/fhkphphbadjkepgfljndicmgdlndmoke)

Chrome: [WebGL Fingerprint Defender 🡥](https://chrome.google.com/webstore/detail/webgl-fingerprint-defende/olnbjpaejebpnokblkepbphhembdicik)

Chrome: [Canvas Fingerprint Defender 🡥](https://chrome.google.com/webstore/detail/canvas-fingerprint-defend/lanfdkkpgfjfdikkncbnojekcppdebfp)

Chrome: [AudioContext Fingerprint Defender 🡥](https://chrome.google.com/webstore/detail/audiocontext-fingerprint/pcbjiidheaempljdefbdplebgdgpjcbe)

Firefox: [Font Fingerprint Defender 🡥](https://addons.mozilla.org/en-US/firefox/addon/font-fingerprint-defender/)

Firefox: [WebGL Fingerprint Defender 🡥](https://addons.mozilla.org/en-US/firefox/addon/webgl-fingerprint-defender/)

Firefox: [Canvas Fingerprint Defender 🡥](https://addons.mozilla.org/en-US/firefox/addon/canvas-fingerprint-defender/)

Firefox: [AudioContext Fingerprint Defender 🡥](https://addons.mozilla.org/en-US/firefox/addon/audioctx-fingerprint-defender/)

## uBlock Origin

This is just a good adblocker

Chrome: [uBlock Origin 🡥](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm)

Firefox: [uBlock Origin 🡥](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)