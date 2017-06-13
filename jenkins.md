# Jenkins installation

## 1. Update system
First of all, update the system.
```
sudo yum install epel-release -y
sudo yum install wget -y
sudo yum -y --exclude=kernel\* update
```
## 2. Install java
```
sudo yum install java-1.8.0-openjdk-devel -y
```
## 3.    Download Jenkins (stable)

```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo rpm --import http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key
sudo yum install jenkins -y
```
## 3.1 latest Jenkins
```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```

## 4.    Set up service

```
sudo systemctl start jenkins.service
sudo systemctl enable jenkins.service
```
## 5.    Set up firewall
```
sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
sudo firewall-cmd --zone=public --permanent --add-service=http
sudo firewall-cmd --reload
