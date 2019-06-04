---
title: Save Time On The Command Line With These Two Tools
date: 2017-01-19 00:00:00 Z
layout: post
---

Command-line interfaces (CLI) give users the ability to quickly execute commands and change virtually anything about a machine. However, sometimes remembering  or researching the write command or syntax can take more time than clicking through a GUI. These two tools will help save you time by simplifying commands and making your everyday work more efficient.

## TLDR

The popular shorthand notation for the phrase "Too long; didn't read" simplies long and extensive text into a few main points. [TLDR Pages]("https://tldr-pages.github.io/) does the same for man pages. Installation via npm is very easy and quick. Afterwards, typing `tldr $command` will show a straightforward man page for `$command`.

![TLDR Example]({{site.baseurl}}/images/Save-Time-On-CLI/tldr-page-ex.png)

TLDR pages work on linux and OS X. There is also a [web app interface]("https://tldr.ostera.io/") for less immediate results. For OS X specific commands type `osx/$command`. Man pages are still useful for more extensive options, but tldr is handy for quick lookups.

## Climate

TLDR is useful for remembering, but [Climate]("https://github.com/adtac/climate") actually shortens and runs commands. Installation requires cloning the Github repository and running the install script. Climate commands are grouped into categories such as monitoring performance, SSHing to upload and download files, and network tools to run a speed test or view used ports. There are some small but useful commands such as a console clock, getting the weather, and a nice overview for system performance. Learning the real commands for normal functions is imperative, but these shortcuts still add convenient features.

![Climate Example]({{site.baseurl}}/images/Save-Time-On-CLI/climate-ex.png)

### Resources

[Climate Github Page](https://github.com/adtac/climate)

[TLDR Github Page](https://github.com/tldr-pages/tldr)

[TLDR Web App](https://tldr.ostera.io/)

[NODE Video on Simplifying CLI](https://www.youtube.com/watch?v=kWmJcVdCdLo)
