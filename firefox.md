
### Install GNOME
```
yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
```


### Check current version
```
firefox -V
which firefox
```

### Download Firefox and install

```
wget https://download-installer.cdn.mozilla.net/pub/firefox/releases/54.0/linux-x86_64/en-US/firefox-54.0.tar.bz2
tar xvf firefox-54.0.tar.bz2


sudo mv /usr/bin/firefox /usr/bin/firefox-old

Now move the firefox directory to /opt:
sudo mv firefox /opt/firefox54

sudo ln -s /opt/firefox54/firefox /usr/bin/firefox

firefox -V
which firefox
```
