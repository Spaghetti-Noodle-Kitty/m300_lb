# 👀 LB02 M300 👀 
## 💡 Projektidee 💡
### 🔐 Eine Security-Box bauen 🔐
> * Vagrant sollte automatisch eine Security-Box erstellen
> * Diese Security-Box sollte über RDP erreichbar sein
> * Die Security-Box erlaubt dem Nutzer zugriff auf etliche Privatsphären-wahrende-Features enthalten
>   * Tor
>   * eine VPN
>   * Sicherheits tools
### 🧱 Installation der Security-Box 🧱
```sh
vagrant init centos/7
echo 'Vagrant.configure("2") do |config|
  config.vm.box = "Centos/7"
end' > Vagrantfile

vagrant up
vagrant ssh
```
### 📦 Pakete aufsetzen 📦
```sh
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm # Add epel repo #
yum -y install xrdp tigervnc-server
yum -y install lightdm 
yum -y install openbox 
yum -y install terminator   
yum -y install nautilus 
yum -y install firefox
yum -y install google-droid-sans-mono-fonts
yum -y install wget
```

### 🛠️ Systemd & Firewall konfigurieren 🛠️
```sh
systemctl enable xrdp
firewall-cmd --permanent --add-port=3389/tcp
firewall-cmd --reload
```

### 👤 User aufsetzen und GUI-Session konfigurieren 👤
```sh
useradd user; echo -e "al-Lad71\nal-Lad71" | passwd user
echo "user ALL=(ALL) ALL" >> /etc/sudoers
cd /home/user
echo "openbox-session" > .xsession
chmod +x .xsession
```

### 🔧 Bugfixing 🔧
```sh
echo -e "al-Lad71" | su -l user
echo -e "al-Lad71" | sudo mkdir ~/.config
echo -e "al-Lad71" | sudo mkdir ~/.config/nautilus
```

### 📢 Finale Schritte inline 📢
```sh
echo -e "al-Lad71" | sudo systemctl start xrdp
echo -e "al-Lad71" | sudo systemctl isolate graphical.target
```


