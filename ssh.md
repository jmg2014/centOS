# SSH Configuration

## 1. Install policycoreutils-python
```sh
sudo yum -y install policycoreutils-python
```
## 2. Edit file sshd_config
```sh
sudo vi /etc/ssh/sshd_config
Port 2222
```
## 3.	Configure Semanage
```sh
sudo semanage port -a -t ssh_port_t -p tcp 2222
```
## 4.	Update firewall
```sh
sudo firewall-cmd --permanent --zone=public --add-port=2222/tcp
sudo firewall-cmd --reload
```
## 5.	Restart SSH
```sh
sudo systemctl restart sshd.service
```
## 6. Check installation
```sh
netstat -ntpl | grep ssh
```
