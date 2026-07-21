# Overview

## This guide walks through the manual installation of the ELK Stack (Elasticsearch, Logstash, Kibana) and Cortex on Ubuntu 24.04.4 LTS. Cortex is an open-source Observable Analysis and Active Response engine, widely used in SOC environments alongside TheHive for automating IOC (Indicator of Compromise) analysis. The ELK Stack provides centralized log collection, parsing, storage, and visualization — forming the backbone of many SIEM setups.

## Cortex & ELK Stack (Elasticsearch, Logstash, Kibana) Setup on Ubuntu 24.04.4
- Last Update Date: April 23, 2026
- Author: MOHAMMAD ZISADUL ISLAM

## Condition
- Ubuntu 24.04.4 LTS server (fresh install recommended)
- Minimum 4 GB RAM (8 GB+ recommended for production)
- sudo/root access
- Java (OpenJDK 17 or 11)

## Install Linux Packages

```
sudo apt update && sudo apt upgrade -y
```
#### Install GPG key
```
sudo apt install -y openjdk-17-jdk wget curl gnupg apt-transport-https
```
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```


#### Install JAVA
```
java -version
```

---
#### Install Linux Packages 
```
sudo apt install wget curl gnupg coreutils apt-transport-https git ca-certificates ca-certificates-java software-properties-common python3-pip lsb-release unzip
```

# *GPG*
```
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
 ```

### Updating package index and Install Java-11-JDK
```
sudo apt update sudo apt install java-common java-11-amazon-corretto-jdk
 ```
```
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"
 ```

### Chack java 
```
java -version
```

---


# Install Elasticsearch
```
bash
sudo apt update
sudo apt install -y elasticsearch 
```


*⚠️ During installation, Elasticsearch will generate a default password for the elastic user — save this securely.*

---
# Configure Elasticsearch

- *Edit the config file*

```
sudo apt-get install apt-transport-https wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic8.x.list
``` 

- Configuration File Path
```
sudo nano /etc/elasticsearch/elasticsearch.yml
```
- network.host: **0.0.0.0**
- discovery.type: **single-node**


### Configuration and create a file 
```
sudo nano /etc/elasticsearch/jvm.options.d/jvm.options
```

*Create a file path and this value past the file parh* 
- 4GB Ram consume in cortex 
```
 -Dlog4j2.formatMsgNoLookups=true
 -Xms512m
 -Xmx512m 
```

---

#### Open file path and modify 
```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

## Uncommend/Delete in tihs (Hash-#) Path

*This list is commented, with #, you need to uncomment it and put your own IP and fix the port.*

```
path.logs: "/var/log/elasticsearch"
path.data: "/var/lib/elasticsearch"
http.port: 9200
cluster.name: hive
xpack.security.enabled: false
http.host: 192.168.3.4 
transport.host: 192.168.3.4
Write it below addet
script.allowed_types: "inline,stored"
thread_pool.search.queue_size: 100000
```



*Than Save and exit. Ctrl+x, enter y, enter*

### Cortex enable,start and status all is ok 
```
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
sudo systemctl status elasticsearch.service
sudo systemctl daemon-reexec
sudo systemctl restart elasticsearch
```

## Verify installation Chack status 
```
curl -k -u elastic:192.168.4.5 https://localhost:9200
```
Or
```
curl http://192.168.3.1:9200

```
- Status
```
{
  "name" : "test",
  "cluster_name" : "Hive",
  "cluster_uuid" : "uCKRUkcCRP6QskjnwZnApQ",
  "version" : {
    "number" : "8.19.14",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "f9adf4c29021dbda28cae7d9c11924471798723d",
    "build_date" : "2026-04-02T15:09:12.561504739Z",
    "build_snapshot" : false,
    "lucene_version" : "9.12.2",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```


















