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

### 4. Configure Docker to start on boot
```
sudo systemctl enable docker
```

### 5. Manage Docker as a non-root user

To create the docker group and add your user:
```
sudo groupadd docker
```
Add your user to the docker group.
```
sudo usermod -aG docker $USER
```
Log out and log back in so that your group membership is re-evaluated.


### 4. Uninstall docker
```
 sudo yum remove docker-engine.x86_64
 rm -rf /var/lib/docker
```
