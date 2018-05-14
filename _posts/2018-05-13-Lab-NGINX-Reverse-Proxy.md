---
published: true
layout: post
title: Configuring a Reverse Proxy in AWS for my Homelab
---

This summer my servers are behind a firewall that I don't control. I can request to have ports opened but it'd take a while and I like to have things under my control. I've tried setting up a reverse proxy before but the concept confused me and I never had a real use case for one.

## What is a reverse proxy?

A proxy takes requests from one computer and forwards them on to another, sometimes modifying the request before sending it on its way. A reverse proxy is a special kind of proxy that can fetch data from the appropriate backend server without the client knowing any of what goes on behind the scenes. This enables an infrastructure to include load balancing and caching among other features.

![FrontendBackend](https://i.imgur.com/G7hDEvr.jpg)

## Why use a reverse proxy?

Companies with popular services are able to use load balancing to ensure speed and availabily. I want to use it in conjuction with a VPN as a proxy to other services that are normally firewalled off and can't be reached directly. I also want to experiment with TLS termination so that I can have public facing web interfaces that are encrypted.

## VPN Connection

I have an OpenVPN Access Server running on my LAN. The VPN port is the only port that needs to be exposed to the internet for this to work. The server running NGINX in AWS will act as the OpenVPN client.

### Server Configuration

First, set up a new user with a passwordless profile. This `.ovpn` profile goes to the NGINX server which has the `openvpn` package installed. 

The server should also be set up to only route traffic destined for the VPN. Otherwise the VPS will not be reachable by the public internet when it's connected.

### Client Configuration

We want OpenVPN to connect on startup. The OpenVPN daemon has an option for this. In the `/etc/default/openvpn` uncomment the line `#AUTOSTART="all"`. This will start all `.conf` files located in the `/etc/openvpn` directory.

Move the `.ovpn` profile to this folder as `client.conf`. If the profile requires a username and password follow [this guide](https://www.smarthomebeginner.com/configure-openvpn-to-autostart-linux/).

If the OpenVPN service isn't already enabled do so now. Finally, restart the server.

If the VPN hasn't connected by the time NGINX starts, NGINX may fail if it can't resolve the internal DNS entries. I added these lines to `/etc/rc.local` just in case.

```
sleep 30
systemctl restart nginx
```

## TCP Servers

NGINX is normally used for HTTP pages, but it can also proxy straight TCP and UDP packets. The first TCP service I tried to proxy was a Starbound game server I run.

We'll put our configuration in a separate file to not complicate the default files. In `/etc/nginx/nginx.conf` add the line `include /etc/nginx/lab-streams.conf;` below the http section. The organization will look like:

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    ...
}

http {
    ...
}

include /etc/nginx/lab-streams.conf;

...
```

Now we need to make and edit `lab-streams.conf`. This will hold the TCP and UDP stream configurations. A basic TCP stream looks like:

```
stream {
        server {
                listen 21025;
                proxy_pass starbound.lan.scriptingis.life:21025;
        }
```

The definition is extremely simple. These lines tell NGINX to listen on port 21025 and forward everything to my internal server on the same port. Other packet streams can be added to the same `stream` section.

## HTTP Servers

These definitions go in `/etc/nginx/sites-available` and are symlinked to `/etc/nginx/sites-enabled`.

HTTP includes more data than raw TCP so we have more customization. A basic configuration, however, looks like this:

```
server {                             
        listen 8080;      
        server_name proxy.scriptingis.life;
        location / {
                proxy_pass web.lan.scriptingis.life;
        }                                        
}                                                 
```

This configuration is very similar to a TCP stream. NGINX listens on port 8080 and forwards traffic to `web.lan.scriptingis.life`. There a two HTTP specific directives however. `server_name` gives NGINX the domain names a server block is responsible for. For example, this server only responds when the HTTP Host header is `proxy.lan.scriptingis.life`. The `location` section is like `server_name` but for the file path section in the URL. For example, all images can be located separate from HTML and CSS files. These fields also support regular expressions.  

## Conclusion

I grew up using Apache but NGINX's dead simple configuration and usability has converted me. This was a really useful project and I'm glad I understand reverse proxies now. There are a ton of uses that I can get started on to explore my interest in infrastructure and devops. Next up, TLS termination.

## Resources

[NGINX Docs on TCP/UDP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/)

[NGINX Configuration Basics from Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-configure-the-nginx-web-server-on-a-virtual-private-server)

[More on Server Names and Locations](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)