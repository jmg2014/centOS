# Docker swarm installation

•	See information
```
docker info
```

### Open ports in Leader and worker nodes

```
sudo firewall-cmd --zone=public --permanent --add-port=2377/tcp
sudo firewall-cmd --zone=public --permanent --add-port=7946/tcp
sudo firewall-cmd --zone=public --permanent --add-port=7946/udp
sudo firewall-cmd --zone=public --permanent --add-port=4789/udp
sudo service firewalld restart
```
### Create the Leader/manager
```
docker swarm init
```

### Get token for workers
```
docker swarm join-token manager
```

### Check nodes
```
docker node ls
```
