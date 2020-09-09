---
published: true
layout: post
title: Diving into the Deep End - Sinkholing an Active Botnet
---

I'm not an expert in reverse engineering or threat intelligence. I spend my days as a security analyst trying to make sense of things I don't know much about. Those things might be a system admin setting up a new application or it could be a multi-stage malware attack. In one case it was the latter.

I'm not an expert but I do love to learn. When I saw that the malware I found was an emerging threat and a domain it used wasn't registered, I jumped on it.

## By The Numbers

Spoiler alert! Here are the results after 3 months.

**7,500-15,000** requests a day.

**912** IP addresses.

**92** ASNs.

## Setup

There isn't much to this part. I bought the domain for $12. I spun up a server in AWS. While NGINX installed I made a DNS record. Before I could take a breath I was a real threat researcher.

<div class="tenor-gif-embed centered" data-postid="9097480" data-share-method="host" data-width="50%" data-aspect-ratio="1.564102564102564"><a href="https://tenor.com/view/boy-realboy-pinnochio-borg-im-areal-boy-gif-9097480">Boy Realboy GIF</a> from <a href="https://tenor.com/search/boy-gifs">Boy GIFs</a></div><script type="text/javascript" async src="https://tenor.com/embed.js"></script>

That's easy enough, but what can you do with lines and lines of HTTP requests in a file? Luckily I already had a Graylog server running. Getting NGINX logs into it [took a little time](https://scriptingis.life/NGINX-Syslog-Graylog/) but it was well worth the effort as it transformed what was just text into a beautiful command center.

![Command Center]({{site.baseurl}}/images/Sinkholing-Botnet/cmdcenter-example.png)

## Analysis

After the logs started flowing I could start to visualize the data. I'll be honest, the raw logs aren't that interesting. The C2 script checks in with a GET request to `/api.aspx` with a few encoded parameters.  I added extractors to the stream for GeoIP and ASN data.

You know that first room in Willie Wonka's chocolate factory? The one with the chocolate river and everything is made out of candy? I felt like I was touring it when I started making different charts and graphs.

<div class="tenor-gif-embed centered" data-postid="15265109" data-share-method="host" data-width="50%" data-aspect-ratio="1.4351585014409223"><a href="https://tenor.com/view/charlie-and-the-chocolate-factory-willy-wonka-wonka-willy-charlie-gif-15265109">Charlie And The Chocolate Factory Willy Wonka GIF</a> from <a href="https://tenor.com/search/charlieandthechocolatefactory-gifs">Charlieandthechocolatefactory GIFs</a></div><script type="text/javascript" async src="https://tenor.com/embed.js"></script>

### Geography

The first thing I noticed is that this campaign targeted the US. After three months I have yet to receive a request from another country. These results are (mostly) in line with the [Cybereason report](https://www.cybereason.com/blog/valak-more-than-meets-the-eye) from May 2020 stating Valak specifically targets enterprises in the US and Germany. Again, I only saw US activity.

![Map of Connections]({{site.baseurl}}/images/Sinkholing-Botnet/connections-map.png)

### Attack Vector

Speaking of enterprises, Valak infects employee workstations using phishing. This can be seen in the data. The most requests happen during weekdays rising starting at 8am EST and falling around 5pm — corresponding with when work laptops are turned on and off. If servers were infected we'd expect the script to be calling back 24/7.

![Daily Flow]({{site.baseurl}}/images/Sinkholing-Botnet/daily-flow.png)

### Persistence

The C2 script calls out every 6 minutes. This can also be seen in the data. Except in the case of multiple infections, IP addresses will show exactly 10 requests sent every hour.

![Regular Intervals]({{site.baseurl}}/images/Sinkholing-Botnet/regular-intervals.png)

<p class="caption">Regular intervals indicate a scheduled task.</p>

![Summer Vacay]({{site.baseurl}}/images/Sinkholing-Botnet/summer-vacay.png)

<p class="caption">It's good to see someone taking PTO!</p>

### Researcher Interest

Are you in these logs? Although Valak infects workstations through phishing, there were requests from hosting providers like DigitalOcean and AWS. These could be proxies, sandboxes, or general detonation/reversing machines. Activity slowly trickled off and ended almost completely July 23rd. This could be due to new samples being found or an overall disinterest after everything has been published.

![Cloud IPs]({{site.baseurl}}/images/Sinkholing-Botnet/cloud-ips.png)

To compare, requests from everywhere else continue to come in at a steady rate.

![Total Requests]({{site.baseurl}}/images/Sinkholing-Botnet/total-requests.png)

## Future Work

Dwell time, the length of time from infection to eradication, could be calculated by looking at when an IP started and stopped sending requests. I have a feeling I'll be very disappointed.

Multiple infections at the same company could be found by looking at which IP addresses send more than 10 requests an hour.

## Conclusion

This was my first serious step into threat research and malware analysis. I didn't know what would come of purchasing a domain used by malware. Luckily it's been a few months and I have yet to receive an abuse report from AWS. There are an overwhelming number of things to look and many different ways to expand on this setup. I'm not an expert but maybe I will be soon.