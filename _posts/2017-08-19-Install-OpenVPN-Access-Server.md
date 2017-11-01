---
published: true
layout: post
title: Install and Configure OpenVPN Access Server
---
A while ago I wrote [an introduction to virtual private networks]({{site.baseurl}}/VPN-Intro). I have an odd love for this technology and the wide array of uses that come with implementing it. What used to be a tool for enterprise use is now almost a commonly known term. Just this evening, [NPR ran a story]("http://www.npr.org/sections/alltechconsidered/2017/08/17/543716811/turning-to-vpns-for-online-privacy-you-might-be-putting-your-data-at-risk"] about normal people using VPNs to deter spying by internet providers and the government. They recommend the safest way to use a VPN is to host it yourself. Today it is easier than ever to configure a quality VPN server.

Before OpenVPN Access Server, I used [Streisand]("https://github.com/jlund/streisand") and [PiVPN]("http://www.pivpn.io/") as my main VPN setup tools. They're both quick to fully setup and easy to manage. I don't think I've ever SSH'd into my Streisand instance. Access Server is a front end for a OpenVPN server. For an administrator, it can manage users and settings. PiVPN allows for easy creation and removal of clients through the command line. Access Server adds functionality to change settings and even authenticate through a LDAP or RADIUS server. Clients can also log in to the web server to download operating specific installers and their connection profile (the .ovpn file).

## Setup

Access Server is available for Amazon Cloud or as a Virtual Appliance. There are also software packages for all major Linux distributions. I used an Ubuntu 16.04 virtual machine on Proxmox. First, download the software package file from the OpenVPN website.

`wget http://swupdate.openvpn.org/as/openvpn-as-2.1.9-Ubuntu16.amd_64.deb`

Then install the package

`sudo dpkg -i openvpn-as-2.1.9-Ubuntu16.amd_64.deb`

The install finishes with telling you to change the password of the <code>openvpn</code> user. This is the initial administrator account.

`sudo passwd openvpn`

Finally, access the login page at: `https://&lt;server-ip&gt;:943/admin/`.

![Web Login]({{site.baseurl}}/images/Install-OpenVPN-Access-Server/web-login.png)

## Authentication

### Local

Local authentication uses profiles created in the interface. Profiles can be found under `User Management -&gt; User Permissions`.

![Permissions]({{site.baseurl}}/images/Install-OpenVPN-Access-Server/homepage.png)

Enter a new username and then click `Show` to set a password for the user. After making any changes, you will need to update the running server by clicking the button that appears. Clients can then access their profile at `https://&lt;server-ip&gt;:943/`. Users can log in and download the OS specific program, and OpenVPN profile. User's access can be revoked or modified in this area too.

After creating a user and password, the setup is complete and you can connect locally. To connect from the internet, forward port 1194 through your router.
## Links

[OpenVPN Access Server Downloads]("https://openvpn.net/index.php/access-server/overview.html")