# Revised: EDRSIEM Encryption SSL/TLS Setup

This process we will setup our certificates for the encrypted communications between Kibana & Elasticsearch and Fleet Agents.

- First step navigate to the Elasticsearch folder location.

~~~
cd  /usr/share/elasticsearch/
~~~

Note: This section is part of the **EDR Fleet Agent!**
- Now create this file: **instances.yml**

~~~
sudo nano instances.yml
~~~

- Then add these contents into the file.

Note: Don't forget to change the IP address:

~~~
instances:
    - name: "elasticsearch"
      ip:
        - "192.168.0.25"
    - name: "kibana"
      ip:
        - "192.168.0.25"
~~~

- Now execute elasticsearch-certutil and create a root CA --pem zip file:

~~~
sudo bin/elasticsearch-certutil ca --pem
~~~
- This CA --pem we will need later for Deployment at scale via AD.

Next: Install unzip application.

- Install unzip:

~~~
sudo apt install zip unzip -y
~~~

- Then unzip the elastic-stack-ca.zip file:

~~~
sudo unzip elastic-stack-ca.zip
~~~

### Note: This section is the re-write of the **CA Authority**.

### This will redo the SSL/TLS encryption Certificates to create the EDR SIEM function called: **Fleet Agents!**

- Then run this command to create the Elasticsearch & Kibana Certificates:

~~~
/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca-cert /ca/ca.crt --ca-key /ca/ca.key --pem --in instances.yml --out certs.zip

~~~

- Then unzip certs.zip file:
  
~~~
unzip certs.zip
~~~

- Now we need to move the Elasticsearch certificates to certs:

~~~
mv /usr/share/elasticsearch/elasticsearch/* certs/
~~~

- Now we need to move the Kibana certificates to certs:

~~~
mv /usr/share/elasticsearch/kibana/* certs/
~~~

- Make these directories for the certificate move:
~~~
mkdir /etc/kibana/certs
mkdir /etc/kibana/certs/ca
mkdir /etc/elasticsearch/certs/ca
~~~


- Now copy the certificates to them directories:
~~~
cp ca/ca.* /etc/kibana/certs/
cp ca/ca.* /etc/elasticsearch/certs/ca/
cp certs/elasticsearch.* /etc/elasticsearch/certs/
cp certs/kibana.* /etc/kibana/certs/
~~~



- Now lets clean up a bit:

~~~
sudo rm -r elasticsearch/ kibana/
~~~

## "Warning"

Note: 

This is only for the rebuild only and not for the initial build.
This will only work if you created this build from the last deployment only.

~~~
cd /

sudo rm -r /ca/ 
~~~


- Now let change some permissions:

~~~
cd /user/share
~~~

- Change folder permissions:

~~~
chown -R elasticsearch:elasticsearch elasticsearch/
chown -R elasticsearch:elasticsearch /etc/elasticsearch/
cd elasticsearch
chown -R elasticsearch:elasticsearch certs/
~~~

- Now lets change the permissions for CA/:

~~~
chown -R elasticsearch:elasticsearch ca/
~~~

Note: Since we redid our certs, we now have to change everything.

- Now lets move these CA's to the correct place now.


- Lets go back and edit this file again:
~~~
sudo nano /etc/kibana/kibana.yml
~~~

Now uncomment (#) hash signs and edit the values below to match your values you created.

- Change the values to match the location path of the certs you created:

~~~

server.ssl.enabled: true
server.ssl.certificate: /etc/kibana/certs/ca/ca.crt
server.ssl.key: /etc/kibana/certs/ca/ca.key
~~~

Note: You can copy and replace these settings if you like.

- Now make the changes to Kibana to match thses settings:

~~~
elasticsearch.hosts: ["https://192.168.0.25:9200"]
elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/certs/ca/ca.crt"]
elasticsearch.ssl.certificate: "/etc/kibana/certs/kibana.crt"
elasticsearch.ssl.key: "/etc/kibana/certs/kibana.key"
~~~

~~~

server.ssl.enabled: true
server.ssl.certificate: "/etc/kibana/certs/kibana.crt"
server.ssl.key: "/etc/kibana/certs/kibana.key"

~~~

- Now restart Kibana :

~~~
sudo systemctl restart kibana
~~~


- In your Elasticsearch.yml files and add this security feature at the end of the file:

~~~
sudo nano /etc/elasticsearch/elasticsearch.yml
~~~

- Security Feature:

~~~
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true

xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.transport.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.transport.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]

xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.verification_mode: certificate
xpack.security.http.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.http.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.http.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]
~~~

- In your Kibana.yml files and add this security feature at the end of the file:

~~~
sudo nano /etc/kibana/kibana.yml
~~~
- Security Feature:

~~~
xpack.security.enabled: true
xpack.encryptedSavedObjects.encryptionKey: "something_at_least_32_characters"
~~~

- Now lets restart Kibana & Elasticsearch:

~~~
sudo systemctl restart kibana
sudo systemctl restart elasticsearch
~~~



Once that is done we must create the system default users to be able to login and create our own username for access. Copy these logins to a secure place.

- Run these command to create system users:

~~~
cd /usr/share/elasticsearch/bin
sudo ./elasticsearch-setup-passwords auto
~~~

Note: Copy the user names and document them for safe keeping, because you will need these names later in your setup.

- Now we must go back again and add our elastic user login creds:

~~~
sudo nano /etc/kibana/kibana.yml
~~~

- Now look for this field and un-comment the values and add your password for this account:

~~~
elasticsearch.username: "kibana_system"
elasticsearch.password: "oZdTyaMMxCLaNmaW9udf"
~~~

This will create the system users for the login.

- Now restart Kibana service:

~~~
sudo systemctl restart kibana
~~~


#Verify Your Certificates

We must Verify our certificates are correct just make sure we got it right.

Note: You Root CA should be the: **ca.crt**

~~~
openssl x509 -in /etc/elasticsearch/certs/elasticsearch.crt -text -noout
~~~

- Look for: Subject: CN = elasticsearch | Subject Alternative Name: IP Address: e.g 192.168.0.25

~~~
openssl x509 -in /etc/kibana/certs/kibana.crt -text -noout
~~~

- Look for: Subject: Subject: CN = kibana | Subject Alternative Name: IP Address: e.g 192.168.0.25

~~~
openssl x509 -in /etc/kibana/certs/ca/ca.crt -text -noout
~~~

- Look for:  Basic Constraints: critical CA:TRUE

Note: This is your Root CA!

- Now to access Kibana type in your IP address like this:

> https://192.168.0.25:5601


Once this process is setup you will now have a secure setup of Elasticseach + Kibana ELKSIEM EDR.

- You can login with the username: **elastic** & **Password** that was generated for you.

Then follow this guide to create your own user account:

> https://www.elastic.co/guide/en/elasticsearch/reference/current/get-started-users.html

- Also, you can change passwords for the built in accounts if need be for security reasons.

## All set!

# Conclusion

We now have our security features enabled and working. We can now setup some beats modules for more security data!

- Packetbeats Module:

> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Beats-Setup/packetbeats.Guide.md

- Optional Setup Step!

So at this point you can navigate to the: **Wazuh Setup Guide** If you want to setup this machine as well: 
> https://github.com/watsoninfosec/ELK-SIEM/blob/main/Deployment-Guide/Wazuh-Guide/Setup-Guide.md