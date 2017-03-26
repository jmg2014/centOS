# ELK-STACK

This is a small guide to install ELK stack (5.2.2 )in CentOS 7:
* Elasticsearch
* Logstash
* Kibana

## Update system
First of all, update the system.
```sh
sudo yum install epel-release -y
sudo yum install net-tools -y
sudo yum -y --exclude=kernel\* update
```

## Install java
```sh
sudo yum install java-1.8.0-openjdk -y
```

## Elasticsearch
```sh
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.2.rpm
sudo rpm -i elasticsearch-5.2.2.rpm
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
```

See information about the installation
```sh
rpm -qc elasticsearch
```
Update firewall
```sh
sudo firewall-cmd --zone=public  --permanent --add-port={9200/tcp,9300/tcp}
sudo firewall-cmd --reload
```
## Kibana
```sh
curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.2-linux-x86_64.tar.gz
tar xzvf kibana-5.2.2-linux-x86_64.tar.gz -C /opt/
mv kibana-5.2.2-linux-x86_64 kibana
```

Run Kibana as a service
```sh
cd /etc/systemd/system
vi kibana.service
chmod +x kibana.service
```
Write down in the file the following:

```sh

[Unit]
Description=Kibana
After=network.target

[Service]
ExecStart=/opt/kibana/bin/kibana
Type=simple
PIDFile=/var/run/kibana.pid
Restart=always

[Install]
WantedBy=default.target
```

Configure 'server.host' in kibana configuration file
```sh
cd /opt/kibana/config
vi kibana.yml
```


Update firewall
```sh
firewall-cmd --zone=public --permanent --add-port=5601/tcp
sudo firewall-cmd --reload
```

Redirect port 80 to 5601
```sh
firewall-cmd --zone=public  --permanent  --add-forward-port=port=80:proto=tcp:toport=5601
firewall-cmd --reload
```
Enable the service
```sh
sudo systemctl daemon-reload
sudo systemctl enable kibana.service
sudo systemctl start kibana.service
```
## Logstash
```sh
curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.2.rpm
sudo rpm -i logstash-5.2.2.rpm
```
We need to create a certificate in order to use Logstash
```sh
cd /etc/pki/tls
openssl req -subj '/CN=elk-stack/' -x509 -days -3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt
```
This logstash-forwarder.crt should be copied to all client servers those who send logs to logstash server.

### Configue Logstash
Create /etc/logstash/conf.d/logstash.conf file, using 5044 port
```sh
input {
 beats {
   port => 5044
   ssl => true
   ssl_certificate => "/etc/pki/tls/certs/logstash-forwarder.crt"
   ssl_key => "/etc/pki/tls/private/logstash-forwarder.key"
   congestion_threshold => "40"
  }
}
filter {
if [type] == "syslog" {
    grok {
      match => { "message" => "%{SYSLOGLINE}" }
    }

    date {
match => [ "timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
}
  }

}
output {
 elasticsearch {
  hosts => localhost
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
       }
stdout {
    codec => rubydebug
       }
}
```
Update firewall
```sh
firewall-cmd --zone=public --permanent --add-port=5044/tcp
sudo firewall-cmd --reload
```
Enable the service
```sh
sudo systemctl daemon-reload
sudo systemctl enable logstash.service
sudo systemctl start logstash.service
```
Log file:
```sh
cat /var/log/logstash/logstash-plain.log
```
