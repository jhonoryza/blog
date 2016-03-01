---
layout: post
title:  "how to speedup linux network connection"
date:   2016-02-28 08:06:16 +0000
categories: linux
---
Hello, i write this tutorial after i get a problem when i install ubuntu server and try to use internet connection but it's too
slow so here are a few tricks to fix that problem..

1. Disable IPv6 support   
    * sudo su vim /etc/sysctl.conf    
    * press insert    
    * add text below    
    * #disable ipv6    
    * net.ipv6.conf.all.disable_ipv6 = 1     
    * net.ipv6.conf.default.disable_ipv6 = 1      
    * net.ipv6.conf.lo.disable_ipv6 = 1      
    * press escape and ":wq"  
2.  DSA    
```javascript
 var s = 0;
```
