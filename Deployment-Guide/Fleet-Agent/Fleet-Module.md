# Fleet Agent Setup Process

This setup guide will show you how to configure and setup the fleet agent on your windows system or environment.

**Note:** Also, this will only work if you have setup your **ElasticSIEM** from my setup guide.

If not, please read the overview and follow the Install guide below: 

### ELKSIEM Build Overview:

> https://github.com/watsoninfosec/ELK-SIEM

### Install Guide Process Below:

> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Installation-Guide/Installation-Guide.md

Note: This software will need to be install on that **Windows System**! 

### Required software for this setup:

> https://winscp.net/eng/index.php

- This software will let you be able to retrieve your Root CA from the **ElasticSIEM** server from the system.

## Example Screenshots Below

### Server2016:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/server2016.png)

### ElasticSIEM:
![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/ElasticSIEM.png)

### WinSCP Windows Client:
![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/WinSCP.png)

### Now lets retrieve our Root CA from the ElasticSIEM Server.

- Open **WinSCP** and connect to your Server:

Note: You will need your username and password!

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/WinSCP-Login.png)

- Now Connect with you IP Address, username & password and login.

- You should see a screen like this below:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/WinSCP-Access.png)

Now that you have acess to your server via WinSCP, lets go back to our ElasticSIEM and go copy that certificate and change its permissions.

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/ElasticSIEM.png)

- Navigate to this directory:

~~~
sudo cd /usr/share/elasticsearch/ca
~~~

- Then we need to copy this certificate to your home drive:

Note: Your home will be different from mines, so tab after you paste: **cp ca.crt /home/**

~~~
sudo cp ca.crt /home/test
~~~

- Now will need to change the permission of this certificate to be able to download it:

~~~
sudo chmod 755 /home/test/ ca.crt
~~~

Now once we change the permissions, we can download the certificate.

Note: If the ca.crt is not showing, reload the session by pressing on your keyboard:

~~~
Ctrl + R
~~~

- Right click on the ca.crt:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/Downloaded-cert.png)

- Now download the ca.crt & choose a location to store it in.

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/Downloaded.png)


Once you have that certificate downloaded, go to that folder and double click that ca.crt file.

- Windows ca.crt on desktop:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/CA-Root.png)

Now we need to install that certificate now.

- Double click on ca.crt:


![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/Certificate-install.png)

- Now click on Install Certificate and select Local Machine:


![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/local-machine.png)

- Now select **Place all certificates in the following store**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/cert-store.png)

- Now browse for **Trusted Root Certification Authorites**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/Root-CA.png)

- Then click Ok, then Next then finish!:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/Next.png)

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/finish.png)

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/done.png)


# Certificate Has Been Installed!

#### Now we need to setup Kibana

Lets go back to our ElasticSIEM and run this command:

~~~
sudo cat /home/test/ ca.crt
~~~

Note: You home directory will be different, press Tab to auto complete the path.


### Example  Output:
 You should see something like this, only copy the random charators and not these lines:

~~~
-----BEGIN CERTIFICATE-----

Only Copy this middle part of this key and not the BEGIN and END.

-----END CERTIFICATE-----
~~~

~~~
test@server:/usr/share/elasticsearch/ca$ sudo cat /home/test/ ca.crt
cat: /home/test/: Is a directory
-----BEGIN CERTIFICATE-----
MIIDSjCCAjKgAwIBAgIVALvkv97iwHAEMTZmLBx0LTRZUwKBMA0GCSqGSIb3DQEB
CwUAMDQxMjAwBgNVBAMTKUVsYXN0aWMgQ2VydGlmaWNhdGUgVG9vbCBBdXRvZ2Vu
ZXJhdGVkIENBMB4XDTIxMDQxMDA0MTMzM1oXDTI0MDQwOTA0MTMzM1owNDEyMDAG
A1UEAxMpRWxhc3RpYyBDZXJ0aWZpY2F0ZSBUb29sIEF1dG9nZW5lcmF0ZWQgQ0Ew
ggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCY7xCQDhnE+tzyMecRMs8/
rqd95gkn27obmLxFZmItFlHiT+3N4pPP49g4RrLpNvWcg4ux+IbySv3qDda1gQAw
d1ee1QlHwhx2mC6uYwjGUpeYA0u1sG1JyO6tGRBkCjN5QHiowiQc1UToK4kWTWmc
7TPYhyXdlQ8UIx/X+ts70KXq8MBJh4m/iwDqHxUvbbpsZd8XZ4XJxDNtNPTZI759
wgs0BxIb+40SXw+m/Iq25YyApzScLWxuPwOPqnwMnztPuye4zK4b9Bu5Z37CAe9g
Uu+kUToRx4CpZGwt1z3rbdhOAsP0LaoV3sfu7xAJ9B+Al07LnplkyTe9ke3BUPqN
AgMBAAGjUzBRMB0GA1UdDgQWBBQcPoWr2DIVU1jSMG5VCSNjW8Nc4DAfBgNVHSME
GDAWgBQcPoWr2DIVU1jSMG5VCSNjW8Nc4DAPBgNVHRMBAf8EBTADAQH/MA0GCSqG
SIb3DQEBCwUAA4IBAQCAopy5as2MG6EKaXnAYVvu4nN9cC6ooq2q7MbnX8rP2nSF
32qlQEFfcnq8ScgcUJO56CAlbPW3+kqfFk6oFaRC8Wsx/AUnKDaf7ZMThcezuSns
1aBxyl7zxH2JWFg0LvhXC0dRjZK7NMAB+BTNExJBM4ULu+4ePh0E0HnLduPTn3IC
FlG/LTHYW3KzxtLYRpbNWaQpnhTIijFp4BbNa6NizK53XNlMajQW4yJozE1O5Jbi
c6c62YVjCkAiKzNCm17WhZaYPQr0ua3EK1D2rOHqG32BRasgRoPdIzKKnpmmIvmZ
5KZxC2pCdKoosiClpFfBvOP6Z11zpdnqn7Ffu9N9
-----END CERTIFICATE-----
test@server:/usr/share/elasticsearch/ca$ 
~~~


One that is done we will need to go to Kibana.

- Login to your Kibana dashboard and you should see this:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/kibana.png)

Now go to **Add Elastic Agent**!

This should be in the bottom middle page as shown above.

- But before we go futher, we will need to create a intergration and policy.

Note: Also, if it tells you to create a user and setup the Fleet. Then create to get pass that step, I did mines on the Fleet panel.

- Now we will need to add a new Intergration to our SIEM.

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/intergration.png)

- In the search bar type: **endpoint**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/endpoint2.png)

- Now click on **endpoint** and add **Endpoint Security**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/add-endpoint.png)

- Now click on **Create Agent Policy:**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy.png)

Once that is done, create your new policy and give it a name. Then click on advanced options.

- Now name your policy and click Create agent policy:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy2.png)

Now enter a Intergration name and description and click save intergration.


![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy3.png)

- Once that is done then go to **Policies**:

- Then click on your policy name under **Agent policy**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy4.png)

Then click the three dots under **Actions** and click on **Edit intergration:**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy5.png)

Now these options are up to you to define. You can change these to suit your purpose of this policy. So click on the one you want to enable.

1. Protection: Malware protections **enabled** or **disable**.

2. Protection: Level: **Detect** or **Prevent**

3. Everything else should be default you can leave or change.

4. Register as antivirus **Windows Only** Turn on or not!


![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy6.png)

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy7.png)

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy8.png)

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy9.png)

Now after that is done click on: **Show advanced settings**
# Install Root CA Certificate in kibana section.

#### This part we will need to import our Root CA into our windows TLS CA_cert section.

- Scroll down untill you see this section below:

Name: **windows.advanced.elasticsearch.tls.ca_cert**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy10.png)

Section Below: **windows.advanced.elasticsearch.tls.ca_cert**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/policy11.png)

Now we need to go back to our ElasticSIEM server and copy that certificate key.

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/cert-key.png)

- Now paste that key into the: **windows.advanced.elasticsearch.tls.ca_cert**

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/cert-key2.png)

- Now click **Save intergration**

- Now once you save intergration, click on settings in the right corner:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/settings.png)


Now we need to change localhost to the server ip address.

- Before: http://localhost:9200

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/settings2.png)

- After: https://172.16.100.139:9200

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/settings3.png)

Once that change is done, hit enter to change it and it will look like mines above.

### Almost done, promise! Lets enable the detections rules.

- So now click on the **Elastic** icon in the left hand corner:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules.png)


- Now go to **Security**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules2.png)

- Then click on **Detections** then **Manage detection rules**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules3.png)

- Now scroll all the way down to the bottom:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules4.png)

Now select 600 rows then scroll back up to top. Then select **Rule** then **Bulk actions** and then **Activate selected**:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules5.png)


- All should be activated:

![](https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/screenshots/rules6.png)


# ElasticSIEM EDR is Complete.

Now we will continue to the next setup guide, and that is the **Fleet Agent Enrollment Module**

### Fleet Agent Enrollment Module:
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Fleet-Agent/Fleet-Agent-Enrollment.md
