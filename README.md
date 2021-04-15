# ElasticSIEM Build Guide

[![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/dwyl/esta/issues) 
 
 [![Anurag's github stats](https://github-readme-stats.vercel.app/api?username=watsoninfosec&theme=blue-green)](https://github.com/anuraghazra/github-readme-stats)

----
### Creating an ElasticSIEM Overview!

These setup guides are going to show you how I setup my ElasticSIEM Server, this process can take a bit to complete, but once you are done. You will have a working ElasticSIEM and be able to start using it at once! This is a simple but advanced layout for you to follow. So if you are new, then don't worry this guide is for you too! 

-----------------------
Note:
- This build was setup on a VMware ESXI 6.7.OU3B and the virtual machines are running 24/7. 
- Now if you wanna use that process then this setup guide is still the same.

-----------------------


I am trying to make this process simple are straight to the point. So that you can follow along and re-create the same setup that I have created.

## Resource References:

-----------------------

What is Elasticsearch?
> https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-intro.html

What is Kibana?
> https://www.elastic.co/guide/en/kibana/current/introduction.html

-----------------------

## Required Software

- Hosting Server Software 

- ubuntu Server 18.04 & 20.04 LTS **#Option 3**
> https://ubuntu.com/download/server

- The Hypervisor that you use is up to you but, the process is still the same.
- You can use linux or windows for the base hypervisor install.

-----------------------
- VirtualBox for Windows or Linux Installs

- Oracle VirtualBox 6.1.16 
> https://www.virtualbox.org/wiki/Downloads

- Oracle VirtualBox Guest Extension Pack 
> https://download.virtualbox.org/virtualbox/6.1.16/Oracle_VM_VirtualBox_Extension_Pack-6.1.16.vbox-extpack

-----------------------
- VMware for Windows or Linux Installs

- VMware Workstation 16.1.0 Player Free 
> https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=PLAYER-1610&productId=1039&rPId=55792

These two are optional below.
You will need physical hardware to install.

- Vmware ESXI 6.7.OU3B 
> https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=ESXI67U3B&productId=742&rPId=56014

- Vmware ESXI 7.0 
> https://my.vmware.com/web/vmware/evalcenter?p=free-esxi7

-----------------------
# Installation Guide ElasticSIEM Build

- Install Elastic Stack Guide: 
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Installation-Guide/Installation-Guide.md

- Security Configuration Guide: 
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Security-Module/Security-Module.md

- Elastic Beats Module Guide:
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Beats-Setup/packetbeats.Guide.md

- Elastic Fleet-Module Guide: 
> https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/Fleet-Module.md

- Elastic Fleet-Agent-Enrollment Guide:
> https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/Fleet-Agent-Enrollment.md
-----------------------



## License

This project is licensed under the MIT License - see the **LICENSE** file for details

MIT © WatsonInfoSec