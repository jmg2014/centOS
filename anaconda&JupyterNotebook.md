### Anaconda & Jupyter Notebook Installation

### 1. Update system
```
sudo yum install epel-release -y
sudo yum install wget -y
sudo yum install bzip2 -y
sudo yum -y --exclude=kernel\* update
```


### 2. Install Anaconda
```
cd ~
wget http://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh
sudo bash Anaconda3-4.3.1-Linux-x86_64.sh
```
• Check installation
```
conda list
```

• To create an environment

```
cd my_environment_folder
conda env create
```
• Activate the environment (name from environment.yml file)
```
source activate ztdl
```

• Check that your prompt changed to
```
(ztdl) $
```

• Updating Conda
```
conda update conda
```
• To remove the environment
```
source deactivate
```

delete the environment:
```
conda remove -y -n ztdl --all
```


### 2. Configure Jupyter Notebook

Generate a configuration file:
```
cd my_environment_folder
jupyter notebook --generate-config
```

This command will create a default Jupyter Notebook configuration file: /home/user/.jupyter/jupyter_notebook_config.py

For security purposes, use the following commands to setup a password for your Jupyter Notebook server:
```
python
>>> from notebook.auth import passwd
>>> passwd()
Enter password:<your-password>
Verify password:<your-password>
'sha1:<your-sha1-hash-value>'
>>> Ctrl+Z
```

Save the SHA1 hash value for later use, which will look like: 'sha1:a3fbb0de406e:52714a3ac8eadb76da329f2ea84a4284236e3461'

Create a self-signed certificate and the matched key:
```
cd ~
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout jkey.key -out jcert.pem
```
The above command will generate a certificate file jcert.pem and the matched key file jkey.key.

Open the default configuration file:

/home/user/.jupyter/jupyter_notebook_config.py

Find each line below respectively:
```
# c.NotebookApp.password = ''
# c.NotebookApp.port = 8888
# c.NotebookApp.ip = 'localhost'
# c.NotebookApp.open_browser = True
# c.NotebookApp.certfile = ''
# c.NotebookApp.keyfile = ''
```
Modify each of them as below:
```
c.NotebookApp.password = 'sha1:<your-sha1-hash-value>'
c.NotebookApp.port = 8888
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.certfile = 'path/jcert.pem'
c.NotebookApp.keyfile = 'path/jkey.key'
```


Modify firewall rules in order to allow inbound traffic on port 8888:
```
sudo firewall-cmd --zone=public --add-port=8888/tcp --permanent
sudo systemctl restart firewalld.service
```

Start the Jupyter Notebook server:
```
jupyter notebook
```
Finally, visit "https://your-server-IP:8888" from your browser and use the password that was set up earlier
