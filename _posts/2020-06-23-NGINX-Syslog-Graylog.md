---
title: NGINX Logs to Graylog - Quick and Dirty
date: 2020-06-23 00:00:00 Z
layout: post
---

Sometimes I have a casual personal project where I just want to get the logs into something where I can easily run queries and create visualizations. The popular content packs on the [Graylog Marketplace](https://marketplace.graylog.org/) seem to be deprecated or designed for the NGINX Docker container. This guide will get NGINX logs forwarding to Graylog in just a few minutes. Security and reliability are not guaranteed.

## Configuring NGINX

Updated: Since many NGINX content packs are outdated and do not target baremetal servers, I created my own. These instructions have been modified to use my version. 

Edit `/etc/nginx/nginx.conf` and add the following lines in the `Logging Settings` section. Replace `logging.example.com` with the domain or IP address of your Graylog server. Restart the service.

```jsx
log_format graylog_json escape=json '{ "timestamp": "$time_iso8601", '
       '"remote_addr": "$remote_addr", '
       '"connection": "$connection", '
       '"connection_requests": $connection_requests, '
       '"pipe": "$pipe", '
       '"body_bytes_sent": $body_bytes_sent, '
       '"request_length": $request_length, '
       '"request_time": $request_time, '
       '"response_status": $status, '
       '"request": "$request", '
       '"request_method": "$request_method", '
       '"host": "$host", '
       '"upstream_cache_status": "$upstream_cache_status", '
       '"upstream_addr": "$upstream_addr", '
       '"http_x_forwarded_for": "$http_x_forwarded_for", '
       '"http_referrer": "$http_referer", '
       '"http_user_agent": "$http_user_agent", '
       '"http_version": "$server_protocol", '
       '"remote_user": "$remote_user", '
       '"http_x_forwarded_proto": "$http_x_forwarded_proto", '
       '"upstream_response_time": "$upstream_response_time", '
       '"nginx_access": true }';

access_log syslog:server=logging.example.com:12401 graylog_json;

access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```

## Graylog Content Pack

Download this [content pack](https://raw.githubusercontent.com/scriptingislife/graylog-content-pack-nginx-syslog/main/content_pack.json) and upload it to Graylog by going to `System → Content Packs`. This will create the `nginx-syslog` input. The extractors attached to the input parse the JSON in the syslog message and also replaces the `message` field with a short readable summary.

## Wrap Up

Make sure logs are flowing in using the `Show received messages` for the `nginx-syslog` input. You can now search fields such as `http_user_agent`, `request_method`, and `response_status`.

## Resources

[Logging to syslog - NGINX Docs](https://nginx.org/en/docs/syslog.html)

[Graylog NGINX Content Pack](https://github.com/scriptingislife/graylog-content-pack-nginx-syslog)

[Graylog Marketplace](https://marketplace.graylog.org/)