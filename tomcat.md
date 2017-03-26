# Tomcat installation

## 1. Update system
First of all, update the system.
```sh
sudo yum install epel-release -y
sudo yum install net-tools -y
sudo yum install wget -y
sudo yum -y --exclude=kernel\* update
```
## 2. Install java
```sh
sudo yum install java-1.8.0-openjdk -y
```
## 3.	Create a dedicated user for Apache Tomcat
```sh
sudo groupadd tomcat
sudo mkdir /opt/tomcat
sudo useradd –s /bin/nologin –g tomcat –d /opt/tomcat tomcat
```
We have just created the ‘tomcat’ user who belongs to the group ‘tomcat’ and whose home directory is /opt/tomcat.

## 4.	Download and install Apache Tomcat.

```sh
sudo wget http://www-us.apache.org/dist/tomcat/tomcat-8/v8.5.12/bin/apache-tomcat-8.5.12.tar.gz
sudo tar -zxvf apache-tomcat-8.5.12.tar.gz -C /opt/tomcat --strip-components=1
```
## 5.	Configure proper permissions.
```sh
sudo chown -R tomcat:tomcat /opt/tomcat
```
## 6.	Setup a Systemd  unit file for Apache Tomcat.

We need to setup a Systemd unit file for Apache Tomcat.
```sh
sudo vi /etc/systemd/system/tomcat.service
```
Write the file with the following configuration:
```sh
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/etc/alternatives/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat

[Install]
WantedBy=multi-user.target
```
Give permissions to tomcat.service
```sh
sudo chmod +x /etc/systemd/system/tomcat.service
```
## 7.	Install ‘haveged’.

For security purposes, we will install haveged :
```sh
sudo yum install haveged -y
sudo systemctl enable haveged.service
sudo systemctl daemon-reload
sudo systemctl start haveged.service
```
## 8.	Start and test Apache Tomcat

Now, start the Apache Tomcat service and set it run on system boot:
```sh
sudo systemctl enable tomcat.service
sudo systemctl daemon-reload
sudo systemctl start tomcat.service
```

In order to test Apache Tomcat in a web browser, you need to modify the firewall rules:
```sh
sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
sudo firewall-cmd --zone=public --permanent --add-port=8443/tcp
sudo service firewalld restart
```
