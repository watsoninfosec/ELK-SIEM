# Install Guide & Build Instructions

This setup guide will show you how to create an Elasticsearch + Kibana ELK-SIEM.

I have listed some install steps below to help you re-create the same install build that I created. This will be a simple install out-of-box deployment.

## Requirements

- **Ubuntu** 18.04 or 20.04 LTS Linux distribution.
- **Java** 8
- **Git repository**
- **Elastic repository** 
- **kibana repository**
- **Putty or SSH Terminal**

- **Some of these Requiements are already installed in Ubuntu!**

## Ubuntu 18.04 & 20.04 LTS

 First we need to install ubuntu, this can be on a VMware or Oracle VirtualBox setup. Or if you have the physical hardware to spare that will work as well.

Note: Just make sure you have SSH access enabled once the server install is completed and know the IP address of the machine. Or that you can login through the GUI and open a Terminal.

Note: To configure a static IP address on your Ubuntu 18.04 & 20.04 LTS server you need to modify a relevant netplan network configuration file within **/etc/netplan/** directory.

- Configure a static IP address:

~~~
cd  /etc/netplan/
ls
~~~

Once you change into this directory and list the contents. We will need to look for our interface name. Remember to copy that Interface name, you will need it later.

- Display network info:

~~~
ifconfig -v
~~~

- Copy the name and clear the screen:

~~~
clear
~~~

Look for your interface name mines was **enp1s0** and change you values to match you network card info:

- You should see a file like this one after you list the directory contents:

~~~
00-installer-config.yaml
~~~

- Now edit this file:

~~~
sudo nano 00-installer-config.yaml
~~~

- Just copy and modify the contents with your data below, then paste it back in and save with CTRL + x:

~~~
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
     dhcp4: no
     addresses: [192.168.0.25/24]
     gateway4: 192.168.0.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]
~~~

- Once that is done restart services:

~~~
sudo netplan apply
~~~

- In case you run into some issues execute:

~~~
sudo netplan --debug apply
~~~

- Remember your new IP assignement!

## Installation of System Components

The ELK stack requires Java 8 to be installed. Some components are compatible with Java 9, but not Logstash

- Before we install our ELK-SIEM, we will need to update our Ubuntu Distro first.
- Open a Terminal or SSH connection and login and type this commands.

~~~
sudo apt-get update && sudo apt-get upgrade -y
~~~ 

- Once that is done perform a Distro upgrade to finish last few upgrades.

~~~
sudo apt-get dist-upgrade -y
~~~

- Now lets reboot the system and finish our deployment:

~~~
sudo reboot
~~~

Note: Lets check your Java version to see if it is installed, enter the following:

~~~
java -version
~~~

* If Java is installed and it's the right version, skip this step!

- Install java Version 8:

~~~
sudo apt-get install openjdk-8-jdk -y
java -version
~~~

Now lets add our Elastcisearch Repos into our system:

- Elastic repositories enable access to all the open-source software in the ELK stack. To add them, start by importing the GPG key.

+ Add Elastic Repository
~~~

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
~~~

- The system should respond with OK.

- Next, install the apt-transport-https package:

~~~
sudo apt-get install apt-transport-https -y
~~~

- Now add the Elastic repository to your system’s repository list:
~~~
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
~~~

- Prior to installing Elasticsearch, update the repositories by entering:

~~~
sudo apt-get update -y
~~~

- Install Elasticsearch with the following command:

~~~
sudo apt-get install elasticsearch -y
~~~

- Do a systemctl daemon reload.
~~~
sudo systemctl daemon-reload
~~~

## Configure Elasticsearch

Elasticsearch uses a configuration files called **.yml** to control how it behaves. Open the configuration file in a text editor of your choice.Either nano or vi.
 **We will be using nano**:

~~~
sudo nano /etc/elasticsearch/elasticsearch.yml
~~~

- You should see a configuration file with several different entries and descriptions. Scroll down to find the following entries:

~~~
network.host: localhost
http.port: 9200
~~~

- Uncomment the lines by deleting the hash (#) sign at the beginning of both lines and replace **localhost** with you ip address.

~~~
network.host: 192.168.0.25
http.port: 9200
~~~

- Just below, find the Discovery section. We are adding one more line, as we are configuring a single node cluster then save with CTRL + x:

~~~
discovery.type: single-node
~~~

- Start the Elasticsearch service by running a systemctl command:


~~~
sudo systemctl start elasticsearch.service
~~~

- Now verify that the service is running:
~~~
sudo service elasticsearch status
~~~

This may take some time for the serivce to run and verify, but once it does we will need to check that it is installed.

- Type this command in your browser:
~~~
http://my_ip_address_here:9200
~~~

Once you hit enter you shoud see something like this:

```json
{
  "name" : "elk-siem",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "LDr0LpmnbQyuk09lqgeVof",
  "version" : {
    "number" : "7.10.0",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "90s9d6f23348d0374a0f3f5c6e8f3a7997850f89",
    "build_date" : "2020-11-09T21:30:33.964949Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

This means that the system in running Elasticsearch and it has been installed. Now lets enable it to run on boot.

- Enable Elasticsearch to start on boot:

~~~
sudo systemctl enable elasticsearch.service
~~~

- You can also test Elasticsearch by running this command:

~~~
curl –X GET "192.168.0.25:9200"
~~~

- Now that we have Elasticsearch install and running, lets install Kibana.

## Configure Kibana 

Kibana is a graphical user interface for parsing and interpreting collected log files.

- lets run the following command to install Kibana:

~~~
sudo apt-get install kibana -y
~~~

- Now lets configure Kibana.

~~~
sudo nano /etc/kibana/kibana.yml
~~~

- Delete the (#) sign at the beginning of the following lines to activate them and replace the values with your IP:

~~~
server.port: 5601
server.host: "192.168.0.25"
elasticsearch.hosts: ["http://192.168.0.25:9200"]
~~~

- The above-mentioned lines should look as follows:

~~~
server.port: 5601
server.host: “192.168.0.25”
elasticsearch.hosts: [“http://192.168.0.25:9200”]
~~~

- Save the file (Ctrl+o) and exit (Ctrl+ x).

Note: This configuration allows traffic from the same system Elasticstack is configured on. You can set the server.host value to the address of a remote server or this server. This is where it will connect to!

- Start and Enable Kibana service.

~~~
sudo systemctl start kibana
~~~

If there is no output, then the service was started correctly.

- Next, configure Kibana to launch at boot:

~~~
sudo systemctl enable kibana
~~~

- Check to see if the service is running:

~~~
sudo systemctl status kibana
~~~

Allow Traffic on Port 5601
If UFW firewall is enabled on your Ubuntu system, you need to allow traffic on port 5601 to access the Kibana dashboard.

- First check if the firewall is running:
~~~
sudo ufw status verbose
~~~
- If it is not then you can skip this line, but it won't hurt to add it. I did!

~~~
sudo ufw allow 5601/tcp
~~~

- To access Kibana, open a web browser and browse to the following address:

~~~
http://192.168.0.25:5601
~~~

Note: If you receive a **"Kibana server not ready yet"** error, check if the Elasticsearch and Kibana services are active.

~~~
sudo systemctl status kibana
sudo systemctl status elasticsearch
~~~

- If the services are not running then you will need to restart or start the services with these commands:

~~~
sudo systemctl restart kibana
sudo systemctl restart elasticsearch
~~~

- Double check again and you should be ok now!


## Configure Logstash

Logstash is a tool that collects data from different sources. That data it collected and parsed by Kibana and stored in Elasticsearch.

- Install Logstash by running the following command:

~~~
sudo apt-get install logstash -y
~~~

- Start Logstash:

~~~
sudo systemctl start logstash
~~~

- Enable the Logstash service:

~~~
sudo systemctl enable logstash
~~~

If everything goes correct and Logstash was installed, the service should be running.

- Check to see if Logstash is running:

~~~
sudo systemctl status logstash
~~~

Once that service is running then it is installed.

## Configure Filebeat

Filebeat is a lightweight plugin used to collect and ship log files. It is the most commonly used Beats module. One of Filebeat’s major advantages is that it slows down its pace if the Logstash service is overwhelmed with data.

- Install Filebeat by running the following command:

~~~
sudo apt-get install filebeat -y
~~~

Note: Make sure that the Kibana service is up and running during the installation and configuration procedure.

- Configure Filebeat:

~~~
sudo nano /etc/filebeat/filebeat.yml
~~~

- Under the Elasticsearch output section, search for the commented out lines:

~~~
output.elasticsearch:
hosts: ["localhost:9200"]
~~~

- remove the (#) sign and edit the line with your system IP address.

- It should look like this:

~~~
output.logstash
hosts: ["192.168.0.25:5044"]
~~~

- Next, enable the Filebeat system module, which will examine local system logs:

~~~
sudo filebeat modules enable system
~~~

Note: Remeber to change **localhost** with you system IP addresss!
- Next, load the index template:

~~~
sudo filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["192.168.0.25:9200"]'
~~~

The system will do some work on setting it up.

- Start and enable the Filebeat service:

~~~
sudo systemctl start filebeat
sudo systemctl enable filebeat
~~~

- Verify Elasticsearch Reception of Data:

~~~
curl -XGET http://192.168.0.25:9200/_cat/indices?v
~~~

## Conclusion

Now you have a functional ELK-SIEM stack installed on your Ubuntu system. I recommend defining your requirements and start adjusting your ELK-SIEM for your needs. This powerful monitoring tool can be customized for individual use cases.

Customize data streams with Logstash, use different Beats modules to gather various types of data, and utilize Kibana for easy browsing through log files.

Security of the devices is not setup, we will setup that process in the next Guide Named: **Security-Module**
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Security-Module/Security-Module.md


# Troubleshotting Tips
- If you get an error messgae like this for **Kibana** , **Elasticsearch** or **Logstash**:

~~~
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; vendor preset: ena   Active: failed (Result: timeout) since Tue 2020-12-15 00:44:16 UTC; 24min ago
     Docs: https://www.elastic.co
  Process: 1007 ExecStart=/usr/share/elasticsearch/bin/systemd-entrypoint -p ${PID_DIR}/elast Main PID: 1007 (code=killed, signal=TERM)
    Tasks: 0 (limit: 4631)
   CGroup: /system.slice/elasticsearch.service

Dec 15 00:45:11 test01 systemd[1]: Starting Elasticsearch...
Dec 15 00:44:16 test01 systemd[1]: elasticsearch.service: Start operation timed out. TerminatDec 15 00:44:16 test01 systemd[1]: elasticsearch.service: Failed with result 'timeout'.
Dec 15 00:44:16 test01 systemd[1]: Failed to start Elasticsearch.
~~~

Try undoing the changes you have done by, **(#)** commenting you changes then save the file and restart the services. This will confirm if your recent last changes have made the file unstable!

This happens when an entry was not taken in the **.yml** file, the file may be damaged or corrupted.

Once that is done, everything should work after that!
Thi is a test
-----