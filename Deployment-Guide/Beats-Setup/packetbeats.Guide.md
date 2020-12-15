# Elastic Module Packetbeats Setup Guide

#### **Overview of Packetbeats** ####


Packetbeat will give you geographical location data on your connections that are coming from your hosts that have this module installed.

This module will need to be installed on any system that you would want to monitor its connections.

> Documentation Here: 
> https://www.elastic.co/guide/en/beats/packetbeat/current/packetbeat-overview.html


- Link below for more information on **packetbeat**:point_down:
> **Link**: https://www.elastic.co/guide/en/beats/packetbeat/current/packetbeat-installation-configuration.html


For this Demo we will install the windows agent for **packetbeats**.

Before we start, we will need this file to be able to capture packets on our NIC interfaces. 

- Download this application and install it:
> https://nmap.org/npcap/dist/npcap-1.10.exe

- Next we will need the **packetbeats** zip file:point_down:.

- **packetbeat** zip file for windows: 
> https://artifacts.elastic.co/downloads/beats/packetbeat/packetbeat-7.10.1-windows-x86_64.zip

Now lets extract the contents.

- Extract the contents of the zip file into **C:\Program Files**.
- Then Rename the folder to: **packetbeat**.
  
Once that is done then we can configure the application.


- Open a PowerShell prompt as an Administrator (right-click the PowerShell icon and select Run As Administrator)

- First lets cd into that directory:

~~~
cd 'C:\Program Files\Packetbeat'
~~~
- Then enable this script:
~~~
PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-packetbeat.ps1
~~~

- You should see a warning message like this & type **R** to run:
~~~
PS C:\Program Files\Packetbeat> PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-packetbeat.ps1

Security warning
Run only scripts that you trust. While scripts from the internet can be useful, this script can potentially harm your
computer. If you trust this script, use the Unblock-File cmdlet to allow the script to run without this warning
message. Do you want to run C:\Program Files\Packetbeat\install-service-packetbeat.ps1?
[D] Do not run  [R] Run once  [S] Suspend  [?] Help (default is "D"): r


__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 5
PSComputerName   :

__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :

Status      : Stopped
Name        : packetbeat
DisplayName : packetbeat

~~~


Once that is done then we can look for this file called **packetbeat.yml**.
Then we will need to edit it.

Note: This will be the usernames & password you created once before in the **Security-Module** 

- Edit this portion in the file below :point_down: with your IP address:

~~~
output.elasticsearch:
  hosts: ["192.168.0.25:9200"]
~~~

- Now add this line under **hosts**:

~~~
pipeline: geoip-info
~~~

- It should look like this:
~~~
output.elasticsearch:
  hosts: ["192.168.0.25:9200"]
  pipeline: geoip-info
~~~
Note: This will be the usernames & password you created once before in the **Security-Module**
~~~
  username: "elastic"
  password: "YOUR_PASSWORD"
~~~

Note: Change the IP address in the **Kibana** field.
- Now we need to setup **Kibana** to be able to connect to it:
~~~
setup.kibana:
    host: "mykibanahost:5601"
~~~

This part will add **dns** information about the host.
- Now lets add this block of text under **processors** and don't forget to change your **Nameserver1,Nameserver2** to your own DNS of you choice.
  
~~~
processors:
- dns:
    type: reverse
    action: append
    transport: tls
    fields:
      server.ip: server.hostname
      client.ip: client.hostname
    success_cache:
      capacity.initial: 1000
      capacity.max: 10000
      min_ttl: 1m
    failure_cache:
      capacity.initial: 1000
      capacity.max: 10000
      ttl: 1m
    nameservers: ['Nameserver1', 'Nameserver2']
    timeout: 500ms
    tag_on_failure: [_dns_reverse_lookup_failed]
~~~


- Now lets check which Interface we need to enable:

- Examle Snapshot:
~~~
PS C:\Program Files\Packetbeat> .\packetbeat.exe devices

0: \Device\NPF_{75AA7527-E078-4BD5-948D-0E294EFA83A9} (Intel(R) 82574L 
Gigabit Network Connection) (fe80::4c2b:5913:3c16:92ed 192.168.80.129)

1: \Device\NPF_Loopback (Adapter for loopback traffic capture) (Not assigned ip address)
~~~

- Now check your Interface numeber:

~~~
.\packetbeat.exe devices
~~~

- Assign Interface number in **packetbeat.yml** file, mine is 0:

~~~
packetbeat.interfaces.device: 0
~~~

## Don't for get to save this file.

- Now lets setup the asset **packetbeats**:

Note: If you see some erros with API, but everything else is ok. Then it worked!
~~~
.\packetbeat.exe setup -e
~~~

Note: If you get an output like this, then it worked, except for **API** failed. I am still working on that.

~~~
PS C:\Program Files\Packetbeat> .\packetbeat.exe setup -e
2020-12-14T23:16:14.441-0600    INFO    instance/beat.go:645    Home path: [C:\Program Files\Packetbeat] Config path: [C:\Program Files\Packetbeat] Data path: [C:\Program Files\Packetbeat\
data] Logs path: [C:\Program Files\Packetbeat\logs]
2020-12-14T23:16:14.452-0600    INFO    instance/beat.go:653    Beat ID: 177a1706-955d-43a1-9726-56f73c37f52b
2020-12-14T23:16:14.453-0600    INFO    [beat]  instance/beat.go:981    Beat info       {"system_info": {"beat": {"path": {"config": "C:\\Program Files\\Packetbeat", "data": "C:\\Program F
iles\\Packetbeat\\data", "home": "C:\\Program Files\\Packetbeat", "logs": "C:\\Program Files\\Packetbeat\\logs"}, "type": "packetbeat", "uuid": "177a1706-955d-43a1-9726-56f73c37f52b"}}}
2020-12-14T23:16:14.453-0600    INFO    [beat]  instance/beat.go:990    Build info      {"system_info": {"build": {"commit": "1da173a9e716715a7a54bb3ff4db05b5c24fc8ce", "libbeat": "7.10.1"
, "time": "2020-12-04T22:46:34.000Z", "version": "7.10.1"}}}
2020-12-14T23:16:14.453-0600    INFO    [beat]  instance/beat.go:993    Go runtime info {"system_info": {"go": {"os":"windows","arch":"amd64","max_procs":2,"version":"go1.14.12"}}}
2020-12-14T23:16:14.465-0600    INFO    [beat]  instance/beat.go:997    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-12-14T20:22:32.92-06:00","name":
"WIN-1NF1QH839NK","ip":["fe80::4c2b:5913:3c16:92ed/64","192.168.80.129/24","::1/128","127.0.0.1/8","fe80::5efe:c0a8:5081/128","2001:0:34f1:8072:30aa:3d0d:3f57:af7e/64","fe80::30aa:3d0d:3f5
7:af7e/64"],"kernel_version":"10.0.14393.447 (rs1_release_inmarket.161102-0100)","mac":["00:0c:29:25:56:b6","00:00:00:00:00:00:00:e0","00:00:00:00:00:00:00:e0"],"os":{"family":"windows","p
latform":"windows","name":"Windows Server 2016 Standard","version":"10.0","major":10,"minor":0,"patch":0,"build":"14393.447"},"timezone":"CST","timezone_offset_sec":-21600,"id":"71c4b3c5-f
8f3-4a62-8589-257dddb19d68"}}}
2020-12-14T23:16:14.466-0600    INFO    [beat]  instance/beat.go:1026   Process info    {"system_info": {"process": {"cwd": "C:\\Program Files\\Packetbeat", "exe": "C:\\Program Files\\pack
etbeat\\packetbeat.exe", "name": "packetbeat.exe", "pid": 1112, "ppid": 2220, "start_time": "2020-12-14T23:16:14.219-0600"}}}
2020-12-14T23:16:14.466-0600    INFO    instance/beat.go:299    Setup Beat: packetbeat; Version: 7.10.1
2020-12-14T23:16:14.466-0600    INFO    [index-management]      idxmgmt/std.go:184      Set output.elasticsearch.index to 'packetbeat-7.10.1' as ILM is enabled.
2020-12-14T23:16:14.467-0600    INFO    eslegclient/connection.go:99    elasticsearch url: http://192.168.80.128:9200
2020-12-14T23:16:14.467-0600    INFO    [publisher]     pipeline/module.go:113  Beat name: WIN-1NF1QH839NK
2020-12-14T23:16:14.468-0600    INFO    procs/procs.go:105      Process watcher disabled
2020-12-14T23:16:14.471-0600    WARN    [cfgwarn]       sip/plugin.go:64        BETA: packetbeat SIP protocol is used
2020-12-14T23:16:14.486-0600    INFO    sniffer/device.go:92    Resolved device index 0 to device: \Device\NPF_{75AA7527-E078-4BD5-948D-0E294EFA83A9}
2020-12-14T23:16:14.487-0600    INFO    eslegclient/connection.go:99    elasticsearch url: http://192.168.80.128:9200
2020-12-14T23:16:14.498-0600    INFO    [esclientleg]   eslegclient/connection.go:314   Attempting to connect to Elasticsearch version 7.10.1
Overwriting ILM policy is disabled. Set `setup.ilm.overwrite: true` for enabling.

2020-12-14T23:16:14.530-0600    INFO    [index-management]      idxmgmt/std.go:261      Auto ILM enable success.
2020-12-14T23:16:14.660-0600    INFO    [index-management]      idxmgmt/std.go:274      ILM policy successfully loaded.
2020-12-14T23:16:14.667-0600    INFO    [index-management]      idxmgmt/std.go:407      Set setup.template.name to '{packetbeat-7.10.1 {now/d}-000001}' as ILM is enabled.
2020-12-14T23:16:14.667-0600    INFO    [index-management]      idxmgmt/std.go:412      Set setup.template.pattern to 'packetbeat-7.10.1-*' as ILM is enabled.
2020-12-14T23:16:14.669-0600    INFO    [index-management]      idxmgmt/std.go:446      Set settings.index.lifecycle.rollover_alias in template to {packetbeat-7.10.1 {now/d}-000001} as ILM
 is enabled.
2020-12-14T23:16:14.669-0600    INFO    [index-management]      idxmgmt/std.go:450      Set settings.index.lifecycle.name in template to {packetbeat {"policy":{"phases":{"hot":{"actions":{
"rollover":{"max_age":"30d","max_size":"50gb"}}}}}}} as ILM is enabled.
2020-12-14T23:16:14.671-0600    INFO    template/load.go:183    Existing template will be overwritten, as overwrite is enabled.
2020-12-14T23:16:14.778-0600    INFO    template/load.go:117    Try loading template packetbeat-7.10.1 to Elasticsearch
2020-12-14T23:16:14.981-0600    INFO    template/load.go:109    template with name 'packetbeat-7.10.1' loaded.
2020-12-14T23:16:14.994-0600    INFO    [index-management]      idxmgmt/std.go:298      Loaded index template.
2020-12-14T23:16:15.528-0600    INFO    [index-management]      idxmgmt/std.go:309      Write alias successfully generated.
Index setup finished.
Loading dashboards (Kibana must be running and reachable)
2020-12-14T23:16:15.544-0600    INFO    kibana/client.go:119    Kibana url: http://192.168.80.128:5601
2020-12-14T23:16:15.547-0600    ERROR   instance/beat.go:956    Exiting: error connecting to Kibana: fail to get the Kibana version: HTTP GET request to http://192.168.80.128:5601/api/stat
us fails: fail to execute the HTTP GET request: Get "http://192.168.80.128:5601/api/status": EOF. Response: .
Exiting: error connecting to Kibana: fail to get the Kibana version: HTTP GET request to http://192.168.80.128:5601/api/status fails: fail to execute the HTTP GET request: Get "http://192.
168.80.128:5601/api/status": EOF. Response: .
PS C:\Program Files\Packetbeat>
~~~

- Now lets run **packetbeats**:

~~~
Start-Service packetbeat
~~~

# Geoip Processor Setup Portion

Let's enable the **geoip info**.

Note: Remember to change **localhost** to your IP information.

- Login Into your **ELKSIEM** server and go to your Dev Tools and paste this command:
```json
PUT _ingest/pipeline/geoip-info
{
  "description": "Add geoip info",
  "processors": [
    {
      "geoip": {
        "field": "client.ip",
        "target_field": "client.geo",
        "ignore_missing": true
      }
    },
    {
      "geoip": {
        "field": "source.ip",
        "target_field": "source.geo",
        "ignore_missing": true
      }
    },
    {
      "geoip": {
        "field": "destination.ip",
        "target_field": "destination.geo",
        "ignore_missing": true
      }
    },
    {
      "geoip": {
        "field": "server.ip",
        "target_field": "server.geo",
        "ignore_missing": true
      }
    },
    {
      "geoip": {
        "field": "host.ip",
        "target_field": "host.geo",
        "ignore_missing": true
      }
    }
  ]
}
```
- Once you have run this commands in your Dev Tool in Kibana, you should see a status code like this:

~~~
{
  "acknowledged" : true
}
~~~
Note: Documentataion on Dev Console:

> https://www.elastic.co/guide/en/kibana/current/console-kibana.html

This will populate the maps field in the Kibana dashboard so that you can see cordinates of your connection on the map.

Note: This is my setup, you can setup the same with your own Dashboard!.

- Example maps taken from my Test-Server.

Note: Before you can create this dashboard, you will need to create an Index for **packetbeats** in the Stack Management Console.

> https://www.elastic.co/guide/en/kibana/current/index-patterns.html


![](/ELK-SIEM\Deployment-Guide\Beats-Setup\img\test.png)

# Congradulations packetbeats is installed!