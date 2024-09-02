### Downlod the CT in Proxmox
```
centos-9-stream-default_20221109_amd64.tar.xz
```

### Update the system
```
yum -y install epel-release
yum -y update
yum clean all
```

### Install minimal X Windows system and XFCE Desktop
```
yum -y groupinstall 'X Window System'
yum -y groupinstall xfce
```

### Run the following to start XFCE GUI at boot-time
```
systemctl set-default graphical.target
```

### Update the system and clean the cache
```
yum -y install epel-release
sudo yum -y update
sudo yum clean all
```

### Install XRDP
```
sudo yum -y install xrdp tigervnc-server
```

### Make necessary changes to SELinux (requires more understanding)
```
sudo chcon -h system_u:object_r:bin_t:s0 /usr/sbin/xrdp
sudo chcon -h system_u:object_r:bin_t:s0 /usr/sbin/xrdp-sesman
```

### If necessary - adjust firewall rules to allow port 3389 (RDP server runs on this port)
```
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=3389/tcp
sudo firewall-cmd --reload
```

### Enable XRDP
`sudo systemctl enable xrdp`

### Set password for your default user on the machine
`passwd`

#### Create a new user
```
sudo adduser sukanta
passwd sukanta
```
#### If necessary - add new user to the sudoers list 
```
sudo usermod -aG wheel sukanta
```
By default, on CentOS, members of the `wheel` group have sudo privileges.

### Start RDP client and use the VM's public address to connect.

If XRDP session immediately closes after loggin in:
```
echo xfce4-session > $HOME/.xsession
chmod +x .xsession
```

### Install Microsoft Edge 
```
sudo dnf clean all
sudo dnf update

```
### Let’s add the Microsoft Edge repository
```
sudo dnf config-manager --add-repo https://packages.microsoft.com/yumrepos/edge
```
###Next, import the GPG key:
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```
### install Microsoft Edge
```
sudo dnf update
sudo dnf install microsoft-edge-stable
```
### Verify the build and version of Edge:
```
microsoft-edge -version
```

```
```
```
```
```
```
```


