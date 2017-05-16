# Nexus installation

### 1. Update system
```
sudo yum install epel-release -y
sudo yum install wget -y
sudo yum -y --exclude=kernel\* update
```


### 2.	Download Java oracle.
```
sudo wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm
```
• Check installation
```
java -version
```

### 3.	Install the RPM file
```
sudo rpm -ivh jdk-8u131-linux-x64.rpm
```

### 4. Installing Nexus
```
sudo wget http://download.sonatype.com/nexus/3/latest-unix.tar.gz
sudo tar -zxvf latest-unix.tar.gz -C /opt/
cd /opt
mv nexus-3.3.1-01 /opt/nexus
sudo groupadd nexus
sudo useradd -g nexus nexus
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
```

### 5. Systemd
Create a file called nexus.service. Add the following contents, save the file in the /etc/systemd/system/ directory.

```
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
•	Change permissions
```
chmod +x nexus.service
```
•	Enable the service
```
sudo systemctl enable nexus.service
sudo systemctl daemon-reload
sudo systemctl start nexus.service
```
•	After starting the service for any Linux-based operating systems, verify that the service started successfully.
```
tail -f /opt/sonatype-work/nexus3/log/nexus.log
```

•	Open firewall
```
sudo firewall-cmd --permanent --add-port=8081/tcp
sudo firewall-cmd --permanent --add-port=18443/tcp
sudo firewall-cmd --permanent --add-port=18444/tcp
sudo service firewalld restart
```
•	Nexus configuration
```
nexus-context-path=/nexus  in /opt/nexus/etc/nexus-default.properties”
```
