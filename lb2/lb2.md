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
#### 🤔 Was passiert hier? 🤔
> Hier wird ein neues Vagrant-Projekt auf Basis von CentOS 7 erstellt.
> Das initiale Vagrantfile wird durch ein echo-to-file erstellt
> Danach wird die Vagrant-Maschine gestartet und wir verbinden uns per SSH.

### 📦 Pakete aufsetzen 📦
```sh
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum -y install xrdp tigervnc-server
yum -y install lightdm 
yum -y install openbox 
yum -y install terminator   
yum -y install nautilus 
yum -y install firefox
yum -y install google-droid-sans-mono-fonts
yum -y install wget
```
#### 🤔 Was passiert hier? 🤔
> In diesem Inline-Block werden die benötigten Pakte für die Maschine heruntergeladen.
> Zuerst laden wir das EPEL-Repo von Fedora herunter.
> Danach werden die RDP-Pakete und ein VNC-Server heruntergeladen.
>
> Nächstens werden die grafischen Pakete heruntergeladen;
> * Erst ein Display-Manager, der den Display selbst managed
> * Weiter geht es mit unserem Window-Manager, also dem Paket, das das Fenster-Rendering erlaubt
> * Zusätzlich werden einige Usability-Pakete installiert, ein Filemanager, ein Terminal-Emulator und ein Webbrowser
> * Zulezt wird ein Monospace-Font installiert, der die Darstellung der Fenster verbessert
> * Ebenfalls wird wget heruntergeladen, damit wir anschliessend den Tor-Client downloaden können.


### 🛠️ Systemd & Firewall konfigurieren 🛠️
```sh
systemctl enable xrdp
firewall-cmd --permanent --add-port=3389/tcp
firewall-cmd --reload
```
#### 🤔 Was passiert hier? 🤔
> Wir setzen hier den XRDP-Daemon auf autostart, damit der Daemon gestartet wird sobald sich ein Nutzer einloggt
> Ebenfalls erlauben wir Zugriff auf den RDP-Port durch den Firewall-Dienst von CentOS

### 👤 User aufsetzen und GUI-Session konfigurieren 👤
```sh
useradd user; echo -e "al-Lad71\nal-Lad71" | passwd user
echo "user ALL=(ALL) ALL" >> /etc/sudoers
cd /home/user
echo "openbox-session" > .xsession
chmod +x .xsession
```
#### 🤔 Was passiert hier? 🤔
> Unser Endnutzer-Account wird hier automatisch erstellt und er bekommt ein Standard-Passwort
> Nach der Erstellung wird der Nutzer den Admins hinzugefügt (hier manuell über das sudoers-file mit den Rechten überall alles zu machen)
>
> Wir gehen nun in das Home-Directory des Nutzers und legen nun die Session-Art (das zu startende GUI) für X fest.
> Das Xsession-File wird nun auch noch ausführbar gemacht, sodass X das File einlesen kann.

### 🔧 Bugfixing 🔧
```sh
echo -e "al-Lad71" | su -l user
echo -e "al-Lad71" | sudo mkdir ~/.config
echo -e "al-Lad71" | sudo mkdir ~/.config/nautilus
```
#### 🤔 Was passiert hier? 🤔
> Wir loggen uns nun als Nutzer ein und erstellen einige benötigte Ordner für unseren Filemanager


### 📢 Finale Schritte inline 📢
```sh
echo -e "al-Lad71" | sudo systemctl start xrdp
echo -e "al-Lad71" | sudo systemctl isolate graphical.target
```
#### 🤔 Was passiert hier? 🤔
> Als letztes werden nun noch XRDP gestartet
> Ebenfalls wird Systemd hier aufgefordert die GUI-Treiber usw. bereitzustellen



