# 👀 LB02 M300 👀 
## 💡 Projektidee 💡
### 🔐 Eine Security-Box erbauen 🔐
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

```