---
title: Centralize Cowrie Honeypot Logs with Graylog in AWS
date: 2018-05-16 00:00:00 Z
layout: post
---

This summer I want to do more botnet analysis. I've done some before with my [rootonyour.webcam](https://rootonyour.webcam) SSH log analysis [sensor](https://github.com/becksteadn/Log-Sensor) and [server](https://github.com/becksteadn/Mapper-Server) projects. This really only included geolocation and linked to Shodan information on running services. Cowrie is a medium interaction honeypot that can log login credentials and command execution and also capture downloaded files. I don't prioritize availability with my self-hosted servers, so I'd rather put everything in the cloud.

## Graylog in AWS

### AMI

Graylog, Inc. maintains a full-stack installation AMI for every version of Graylog. Search for it in the public AMIs page of the EC2 section.

![AMI](https://lambda.sx/ffM.png)

Launch with `t2.medium` at least. Edit the EBS storage for however big you want. Cost is roughly $1 per 10Gb.

### Security Group

The Graylog web interface uses ports 80 (HTTP), 443 (HTTPS), and 9000 (TCP). The default Cowrie syslog configuration uses port 8514 (UDP).

### Initial Configuration

Launch and connect to the box via SSH.

#### Graylog-Reconfigure

To start complete the Graylog installation, you'll need to configure a few settings using the `graylog-ctl` command.

```
sudo graylog-ctl set-external-ip https://<public ip>:443/api
sudo graylog-ctl enforce-ssl
sudo graylog-ctl set-admin-password <password>
sudo graylog-ctl set-timezone <zone acronym>
sudo graylog-ctl reconfigure
```

After the configuration completes, log in as admin at `https://<public ip>`.

#### Input

Once logged in, navigate to `System -> Inputs`. At the top of the page select `Syslog UDP` and launch the new input then configure it. Use Port 8514.

## Honeypot Setup

We'll assume a Cowrie honeypot is already running. First, add the following lines to `cowrie.cfg`.

```
[output_localsyslog]
enabled = true
facility = USER
format = text
```

Then create the file `/etc/rsyslog.d/85-graylog.conf` with the following:

```
$template GRAYLOGRFC5424,"<%pri%>%protocol-version% %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msg%\n"
*.* @<graylog-server-ip>:8514;GRAYLOGRFC5424
```

Finally, restart cowrie and rsyslog.

```
bin/cowrie restart

service rsyslog restart
```

## Graylog Configuration

After messages start flowing, we can customize Graylog for honeypot logs.

### Extractors

Extractors can pick out additional information from a message. For this case, we'll be pulling IP addresses, usernames, and passwords. Hit `Manage extractors` to go to the extractors menu.


#### IP Addresses

Hit `Get started` and paste a Message ID from a message with an attacker's IP address and use the default index `graylog_0` then load the message. Next to the `message` field select `Select extractor type` and choose `Regular Expression`.

Feel free to use your own IP regex, but I use `(\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b)`. Set it to always try to extract or choose to only extract when `New Connection` appears. Store the field as `ip`. Set the `Extraction strategy` to copy.

#### Login Credentials

Select a message with a login attempt containing the username and password. Here I'll just give you the field I use and the regex to match it.

 ```
field : regex

attempt_username : login attempt \[(.*)\/
attempt_password : \/(.*)\]

 ```

These fields can now be used in searches.

![Login Regex Extractor](https://lambda.sx/nYM.png)

### Streams

Messages can be routed into streams based on a set of rules. This is done is realtime so it can be used for alerting or forwarding to another system.

I have one stream called `Honeypot Data` with rules that it matches a hostname of one of my sensors. The other is called `SSH Login Attempts` and matches when a message contains the phrase `login attempt`. 

### Dashboards

Create a new dashboard then go back to the search page. Search for `login attempt` in the last 1 day. On the left under **Search result**  select `Add count to dashboard`. This will show the number of login attempts in the last 24 hours. You can also use the time filter keyword `today midnight` to show the attempts in the current day. Finally, add the histogram to the dashboard as well.

#### Extra Credit

Also from the left-hand side, you can select any field and choose `Quick Values` to get a chart of values found in the search. Try it for `attempt_username`.

![Attempt Username Chart](https://lambda.sx/ulM.png)

## Conclusion

I'm just getting started with Cowrie and Graylog. I hope to automate honeypot setup with Ansible soon and get more valuable analytics from Graylog.

## Additional Notes

- Configuration files are located in `/opt/graylog/` for the AMI images.
- Set up dynamic DNS for the server to be used in honeypot syslog configuration.

## Resources

[Graylog AWS Documentation](http://docs.graylog.org/en/2.4/pages/installation/aws.html)

[cyberdefense.pl Tutorial](https://cyberdefense.pl/2016/10/30/graylog-and-aws-quick-start/)

[Cowrie Graylog Documentation](https://github.com/micheloosterhof/cowrie/tree/master/doc/graylog)

[Drools - Something to look into](http://docs.graylog.org/en/2.4/pages/drools.html)
