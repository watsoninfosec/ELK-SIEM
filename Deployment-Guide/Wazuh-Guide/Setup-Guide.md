# Wazuh Open Source Security Platform

Note: Information is taken from the website below:

- Welcome to Wazuh
> https://documentation.wazuh.com/4.0/index.html

- Virtual Machine (OVA)
> https://documentation.wazuh.com/4.0/virtual-machine/virtual-machine.html#virtual-machine

Wazuh is a free, open source and enterprise-ready security monitoring solution for threat detection, integrity monitoring, incident response and compliance.

This process is simple and easy to setup. All we have to do is import the **.ova** file into our Hypervisor and run the system.

- Virtual Appliance (OVA):
> https://packages.wazuh.com/4.x/vm/wazuh-4.0.2_1.11.0.ova

```json
System Components:

CentOS 7
Wazuh manager: 4.0.2
Open Distro for Elasticsearch: 7.9.1
Filebeat-OSS: 7.9.1
Kibana: 7.9.1
Wazuh Kibana plugin: 4.0.2-7.9.1
```

First, import the OVA in the virtualization platform and run the virtual machine. The user root password is wazuh and the username/password for the Wazuh API is wazuh-wui/wazuh-wui.

Note: You will need to login into the system and get the IP address from the server.

By default the type of the network interface is bridge. The VM will try to get an IP address from the network’s DHCP server. Alternatively, a static IP address can be set by configuring the proper network files on the CentOS operating system that the virtual machine is based on.

```json
To access the web interface:

URL: https://<wazuh_server_ip>
user: admin
password: admin
```

## To create an admin user: 

1. Click on the 3 line on the left panel.
2. Then go to security
3. Then go to Internal users
4. Create internal user
5. Go to Roles
6. Scroll down to: **all_access**
7. Click on **all_access**
8. Go to mapped users
9. Click on Manage mapping
10. Click on Add another external identity
11. In blank field, type user name
12. Then click on Map

New admin user has been created!

# Conclusion

We now have our working Wazuh system setup so that we can start our journey and ingest data from different end points like I did.

### Congratulations You Did IT!

# Enjoy!