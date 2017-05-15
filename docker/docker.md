# Docker installation

### 1. Update system
```
sudo yum install epel-release -y
sudo yum install wget -y
sudo yum -y --exclude=kernel\* update
```

### 2. Docker
```
curl -fsSL https://get.docker.com/ | sh
sudo systemctl start docker
sudo systemctl status docker
sudo systemctl enable docker
```

### 3. Docker compose
```
sudo yum install -y python-pip
pip install --upgrade pip
sudo pip install docker-compose
```

### 4. Uninstall docker
```
 sudo yum remove docker-engine.x86_64
 rm -rf /var/lib/docker
```
