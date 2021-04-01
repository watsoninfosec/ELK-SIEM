First create:

nano /usr/share/elasticsearch/instances.yml

instances:
    - name: "elasticsearch"
      ip:
        - "172.16.100.126"
    - name: "kibana"
      ip:
        - "172.16.100.126"




.
Second generate certificate:

/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca-cert /ca/ca.crt --ca-key /ca/ca.key --pem --in instances.yml --out certs.zip




Third unzip certs:

unzip certs.zip




Fourth:

Troubleshoot: cat /etc/systemd/system/multi-user.target.wants/elasticsearch.service

chown -R elasticsearch:elasticsearch elasticsearch/
chown -R elasticsearch:elasticsearch /etc/elasticsearch/
mkdir certs
cp /usr/share/elasticsearch/elasticsearch/* certs/
chown -R elasticsearch:elasticsearch certs/
cp /ca/* certs/
cd certs/
mkdir ca
mv ca.* ca/
chown -R elasticsearch:elasticsearch ca/


nano /etc/elasticsearch/elasticsearch.yml 

xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true
# Transport layer
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.transport.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.transport.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]
# HTTP layer
xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.verification_mode: certificate
xpack.security.http.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.http.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.http.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]


systemctl restart elasticsearch
systemctl status elasticsearch


Fifth:

cd /etc/kibana/
mkdir certs
cp /usr/share/elasticsearch/kibana/* certs/
cd certs/
mkdir ca
cp /etc/elasticsearch/certs/ca/* ca/
chown -R kibana:kibana /etc/kibana/
nano /etc/kibana/kibana.yml 

# The Kibana server's name.  This is used for display purposes.
server.name: "edrsiem"

# The URLs of the Elasticsearch instances to use for all your queries.
elasticsearch.hosts: ["https://172.16.100.126:9200"]
elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/certs/ca/ca.crt"]
elasticsearch.ssl.certificate: "/etc/kibana/certs/kibana.crt"
elasticsearch.ssl.key: "/etc/kibana/certs/kibana.key"

# Kibana uses an index in Elasticsearch to store saved searches, visualizations and
# dashboards. Kibana creates a new index if the index doesn't already exist.
#kibana.index: ".kibana"

# The default application to load.
#kibana.defaultAppId: "home"

# If your Elasticsearch is protected with basic authentication, these settings provide
# the username and password that the Kibana server uses to perform maintenance on the Kibana
# index at startup. Your Kibana users still need to authenticate with Elasticsearch, which
# is proxied through the Kibana server.
elasticsearch.username: "kibana_system"
elasticsearch.password: "GECFnY0CmKyEBCwD2hHq"


 Enables SSL and paths to the PEM-format SSL certificate and SSL key files, respectively.
# These settings enable SSL for outgoing requests from the Kibana server to the browser.
server.ssl.enabled: true
server.ssl.certificate: "/etc/kibana/certs/kibana.crt"
server.ssl.key: "/etc/kibana/certs/kibana.key"

systemctl restart kibana


Six:
cat kibana.crt


It works:

PS C:\Program Files\elastic-agent-7.12.0-windows-x86_64> .\elastic-agent.exe install -f --kibana-url=https://172.16.100.
134:5601 --enrollment-token=eFlpWWdYZ0JWbEZHYVBYTHNWTjk6d1FmbFN4UEdTekdoR1ZWalp0eHFrQQ==
The Elastic Agent is currently in BETA and should not be used in production

2021-03-30T00:34:11.066-0500    INFO    application/enroll_cmd.go:191   Successfully triggered restart on running Elasti
c Agent.
Successfully enrolled the Elastic Agent.
PS C:\Program Files\elastic-agent-7.12.0-windows-x86_64>





