# Git installation

## 1. Update system
First of all, update the system.
```sh
sudo yum install epel-release -y
sudo yum install wget -y
sudo yum -y --exclude=kernel\* update
```
## 2. Install tools
```sh
sudo yum groupinstall "Development Tools" -y
sudo yum install gettext-devel openssl-devel perl-CPAN perl-devel zlib-devel curl-devel -y
```
## 3.	Download Git

To Check latest release
https://github.com/git/git/releases
```sh
wget https://github.com/git/git/archive/v2.12.2.tar.gz -O git.tar.gz
```
## 4.	Install

```sh
tar -zxf git.tar.gz
cd git-*
make configure
./configure --prefix=/usr/local
sudo make
sudo make install
```
## 5.	Check installation
```sh
git --version
```
