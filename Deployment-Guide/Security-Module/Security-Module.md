# Securing our ELK-SIEM

This guide will walk you through on setting up security of your ELK-SIEM so that basic security features will be enabled.

- First Step navigate to the Elasticsearch folder location.

~~~
cd  /usr/share/elasticsearch/
~~~

- List the contents of the directory:

~~~
ls
~~~

- Now execute elasticsearch-certutil and create a --pem file:

~~~
sudo bin/elasticsearch-certutil ca --pem
~~~

This will prompt you to create the Certificate zip file. You can leave it to the defaults. We will rename the cert later!

Next: Install unzip application.

- Install unzip:

~~~
sudo apt install zip unzip -y
~~~

- Then unzip the elastic-stack-ca.zip file:

~~~
sudo unzip elastic-stack-ca.zip
~~~

- The cert folder name should be **ca**
- Then move the ca into the root folder: 

~~~
sudo mv ca /
~~~

- Then cd into the root directory:

~~~
cd /
~~~

At this point once the cert has been unzipped you can change the name by copying the file into another file.

This is just to change the name from default if you want to.

- Example:

~~~
cd ca/
sudo cp ca.crt mycertname.crt
sudo cp ca.key mycertname.key
~~~

- Lets go back and edit this file again:
~~~
sudo nano /etc/kibana/kibana.yml
~~~

Now uncomment (#) hash signs and edit the values below to match your values you created.

- Change the values to match the location path of the certs you created:

~~~
server.ssl.enabled: true
server.ssl.certificate: /ca/ca.crt
server.ssl.key: /ca/ca.key
~~~

- Now restart Kibana & Elasticsearch:

~~~
sudo systemctl restart kibana
sudo systemctl restart elasticsearch
~~~


- In your Elasticsearch.yml files and add this security feature at the end of the file:

~~~
sudo nano /etc/elasticsearch/elasticsearch.yml
~~~
- Security Feature:

~~~
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true
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

- Now to access Kibana type in your IP address like this:
> https://192.168.0.25:5601


Once this process is setup you will now have a secure setup of Elasticseach + Kibana ELK-SIEM.

- You can login with the user: **elastic** then follow this guide to create your own user account:

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

