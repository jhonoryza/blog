---
layout: post
title:  "how to speedup linux network connection"
date:   2016-02-28 08:06:16 +0000
categories: linux
---
Hello, i write this tutorial after i get a problem when i install ubuntu server and try to use internet connection but it's too
slow so here are a few tricks to fix that problem..

1\. Disable IPv6 support:

```sh
    $ sudo su vim /etc/sysctl.conf  
    $ press insert   
    $ add text below  
    $ #disable ipv6  
    $ net.ipv6.conf.all.disable_ipv6 = 1  
    $ net.ipv6.conf.default.disable_ipv6 = 1  
    $ net.ipv6.conf.lo.disable_ipv6 = 1  
    $ press escape and ":wq"   
```

2\.  Fix bug in Debian Avahi-daemon    
Open a terminal and use the following command:

```sh
    sudo gedit /etc/nsswitch.conf
```

This will open the configuration file in gedit so that you could edit it easily in GUI. In here, look for the following line:

```shell
    hosts:          files mdns4_minimal [NOTFOUND=return] dns mdns4
```

If you find this file, replace it with the following line:

```shell
    hosts:          files dns
```

Save it, close it, restart your computer
