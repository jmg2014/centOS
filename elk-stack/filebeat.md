# Filebeat

Download Filebeat
```sh
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.2.2-x86_64.rpm
sudo rpm -vi filebeat-5.2.2-x86_64.rpm
```
Filebeat (beats) uses SSL certificate for validating logstash server identity, so copy the logstash-forwarder.crt from the logstash server to the client.

```sh
scp <localfile>username@tohostname:<remotefile>

```
Enable the service
```sh
sudo systemctl daemon-reload
sudo systemctl enable filebeat.service
sudo systemctl start filebeat.service
```


Configure filebeat
```sh
output.logstash:
	 # The Logstash hosts
	 hosts: ["elk-stack:5044"]
	 tls:
	    # List of root certificates for HTTPS server verifications
	    certificate_authorities: ["/etc/pki/tls/certs/logstash-forwarder.crt"]
```


## Kibana dashboard
Download beats-dashboards
```sh
 curl -L -O https://download.elastic.co/beats/dashboards/beats-dashboards-1.3.1.zip
 sudo yum -y install unzip
 unzip beats-dashboards-*.zip
 cd beats-dashboards-*
```
Load the sample dashboards, visualizations and Beats index  patterns into Elasticsearch
```sh
 ./load.sh
```
Create the file : filebeat-index-template.json
```json
{
  "mappings": {
    "_default_": {
      "_all": {
        "enabled": true,
        "norms": {
          "enabled": false
        }
      },
      "dynamic_templates": [
        {
          "template1": {
            "mapping": {
              "doc_values": true,
              "ignore_above": 1024,
              "index": "not_analyzed",
              "type": "{dynamic_type}"
            },
            "match": "*"
          }
        }
      ],
      "properties": {
        "@timestamp": {
          "type": "date"
        },
        "message": {
          "type": "string",
          "index": "analyzed"
        },
        "offset": {
          "type": "long",
          "doc_values": "true"
        },
        "geoip"  : {
          "type" : "object",
          "dynamic": true,
          "properties" : {
            "location" : { "type" : "geo_point" }
          }
        }
      }
    }
  },
  "settings": {
    "index.refresh_interval": "5s"
  },
  "template": "filebeat-*"
}
```
load the template
```sh
curl -XPUT 'http://localhost:9200/_template/filebeat?pretty' -d@filebeat-index-template.json
```

load dummy data to create index in kibana
```sh
curl -XPUT http://localhost:9200/filebeat-2015.05.19 -d '
{
  "mappings": {
    "log": {
      "properties": {
        "geo": {
          "properties": {
            "coordinates": {
              "type": "geo_point"
            }
          }
        }
      }
    }
  }
}
';
```
To check values
```sh
curl 'localhost:9200/_cat/indices?v'
```
Test Filebeat Installation
```sh
curl -XGET 'http://localhost:9200/filebeat-*/_search?pretty'
```
