---
published: false
---
---
published: true
layout: post
title: Opening My Smart TV to the World
---

This year I moved into a new apartment and __had__ to get a TV for the living room. I ultimately got this [TCL Roku TV](https://www.bestbuy.com/site/tcl-55-class-led-4-series-2160p-smart-4k-uhd-tv-with-hdr-roku-tv/5878703.p?skuId=5878703) which my journey of getting is whole nother story. I powered it on, selected my language and country, then it was time to configure the network connection. **Shoot.**

You see, RIT gives students public IP addresses for everything besides mobile devices on the Wi-Fi. This is great for hosting content on my servers. It's not so great when you want to connect an [IoT device](https://blog.radware.com/uncategorized/2018/03/history-of-iot-botnets/) like an Amazon Echo, test Raspberry Pi project, or smart TV. I have plans to make a hidden Wi-Fi network using a Cisco access point behind a firewall and router to hopefully secure my devices better. Until then, I still need to watch Netflix. Reluctantly I registered it with the university and got the green checkmark on the TV indicating I could connect to the internet (and the internet could connect to me).

But what exactly am I exposing to the internet? As soon as the setup was done, I headed to nmap on my laptop to see if the TV really was reachable. I started with a ping and eventually worked my way up to a banner grab of the only listening port, 9080.

![NMAP Results](https://puu.sh/BktCG/637959323e.png)

From a quick search it looks like a RESTful API that can be used to interact with the TV. It has legitimate uses like the Roku mobile app which can take control of the TV simply by putting in the IP address. It can also be used with [Home Assistant](https://www.home-assistant.io/components/media_player.roku/) to switch to your security camera feed when someone rings the doorbell [(Source)](https://www.reddit.com/r/Roku/comments/2usq8o/whats_up_with_the_open_ports_on_roku_3/cobeg7k).

I had never heard of the [Mongoose](https://cesanta.com/) embedded web server before so I decided to look it up on [Shodan](https://www.shodan.io/search?query=mongoose). There are currently 4,470 results. Many of them appear to be part of a "SpectrumAnalizer" project, Z-Wave Smart Home servers in Germany, or a simple site with an error message in a foreign language.

![Mongoose Servers](https://puu.sh/BkuuU/a445e0116a.png)

That's all I have time for tonight. I'm excited to climb further down this rabbit hole and maybe find some other cool smart appliances on the internet.