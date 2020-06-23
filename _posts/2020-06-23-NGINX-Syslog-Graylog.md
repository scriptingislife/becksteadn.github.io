---
title: NGINX Logs to Graylog - Quick and Dirty
date: 2020-06-23 00:00:00 Z
layout: post
---

Sometimes I have a casual personal project where I just want to get the logs into something where I can easily run queries and create visualizations. The popular content packs on the [Graylog Marketplace](https://marketplace.graylog.org/) seem to be deprecated or designed for the NGINX Docker container. This guide will get NGINX logs forwarding to Graylog in just a few minutes. Security and reliability are not guaranteed.

## Configuring NGINX

Edit `/etc/nginx/nginx.conf` and add the following lines in the **Logging Settings** section. Restart the service.

```jsx
				log_format graylog2_json escape=json '{ "timestamp": "$time_iso8601", '
                     '"remote_addr": "$remote_addr", '
                     '"body_bytes_sent": $body_bytes_sent, '
                     '"request_time": $request_time, '
                     '"response_status": $status, '
                     '"request": "$request", '
                     '"request_method": "$request_method", '
                     '"host": "$host",'
                     '"upstream_cache_status": "$upstream_cache_status",'
                     '"upstream_addr": "$upstream_addr",'
                     '"http_x_forwarded_for": "$http_x_forwarded_for",'
                     '"http_referrer": "$http_referer", '
                   '"http_user_agent": "$http_user_agent" }';

        access_log syslog:server=<domain-or-ip>:12301 graylog2_json;
        error_log syslog:server=<domain-or-ip>.life:12302;
```

## Graylog Content Pack

Download this [content pack](https://raw.githubusercontent.com/lewisgeorge-innoscale/graylog3-content-pack-nginx-docker/master/content_pack.json) and upload it to Graylog by going to `System → Content Packs`. This will create the `nginx access log` and `nginx error log` inputs as well as several streams and other components. 

## Wrap Up

Make sure logs are flowing in. You can now search fields such as `http_user_agent`, `request_method`, and `response_status`.

## Resources

[Logging to syslog - NGINX Docs](https://nginx.org/en/docs/syslog.html)

[Graylog3 NGINX Content Pack](https://github.com/lewisgeorge-innoscale/graylog3-content-pack-nginx-docker)

[Graylog Marketplace](https://marketplace.graylog.org/)