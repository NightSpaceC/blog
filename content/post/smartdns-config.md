---
author: "Night Space"
title: SmartDNS自用配置
date: 2023-04-02T11:54:00+08:00
lastmod: 2023-04-02T11:54:00+08:00
tags:
    - 笔记
categories:
    - 笔记
---
```
bind [::]:53
bind-tcp [::]:53

cache-size 32768

force-qtype-SOA 65

dualstack-ip-selection yes

response-mode fastest-ip

log-level info

server 114.114.114.114
server 8.8.8.8
server 180.76.76.76 # Baidu
server 52.80.52.52 # OneDNS

server-https https://dns.alidns.com/dns-query
server-https https://doh.opendns.com/dns-query
server-https https://1.0.0.1/dns-query
server-https https://dns.pub/dns-query # DNSPod
server-https https://9.9.9.10/dns-query
server-https https://doh.360.cn/dns-query
```