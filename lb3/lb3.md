# 👀 LB03 M300 👀 
## 💡 Projektidee 💡
### 🔐 Eine Wordpress Host mit Docker erstellen 🔐
> * Vagrant VM hosted eine Docker-Instanz, die eine Wordpress installation bereitstellt 
> * Die Wordpress installation sollte durch die VM weitergeleitet werden
> * Dafür werden folgende Container benötigt
>   * MySQL
>   * PHPMyAdmin
>   * Wordpress
### 🧱 Setup der Vagrant VM 🧱
```sh
vagrant init gusztavvargadr/docker-linux-community-ubuntu-server
echo 'config.vm.provision "shell", inline: <<-SHELL
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
  SHELL'
  > Vagrantfile

vagrant up
vagrant ssh
```
#### 🤔 Was passiert hier? 🤔
> Hier wird ein neues Vagrant-Projekt auf Basis einer spezialisierten Ubuntu version erstellt
> Das initiale Vagrantfile wird durch ein echo-to-file erstellt
> Danach wird die Vagrant-Maschine gestartet und wir verbinden uns per SSH.

### 📦 Container definieren 📦
#### Ⓜ️ MySQL definieren Ⓜ️
```sh
db:
    image: mysql:latest
    volumes:
        - db_data:/var/lib/mysql
    restart: always
    environment:
        MYSQL_ROOT_PASSWORD: al-Lad71
        MYSQL_DATABASE: mybase
        MYSQL_USER: dockersql
        MYSQL_PASSWORD: al-Lad71
    networks:
    - wpsite
```

#### ⛵ PHPMyAdmin definieren ⛵
```sh
phpmyadmin:
    depends_on:
    - db
    image: phpmyadmin:latest
    restart: always
    ports:
        - '8080:80'
    environment:
    PMA_HOST: db
    MYSQL_ROOT_PASSWORD: al-Lad71
    networks:
    - wpsite
```

#### 📃 Wordpress definieren 📃
```sh
wordpress:
    depends_on:
        - db
    image: wordpress:latest
    ports:
        - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
        WORDPRESS_DB_HOST: db:3306
        WORDPRESS_DB_USER: dockersql
        WORDPRESS_DB_PASSWORD: al-Lad71
    networks:
    - wpsite
```
#### 🤔 Was erkennt man hier? 🤔
> * Alle Container befinden sich im Netzwerk "wpsite"¨
> * Die Passwörter und Accounts für die DB

## ⚙️ Reflexion ⚙️
### 🔄 Schwierigkeiten 🔄
> Dieses Projekt war etwas schwierig zu realisieren, nicht nur verpasste ich einen grossen Teil der Arbeitszeit durch eine OP sondern auch durch einige Provider Schwierigkeiten.
>
> Erstens bin ich mittlerweile auf VMware Workstation 16 umgestiegen.
> Vagrant sagt zwar aus, VMware zu unterstützen, jedoch benötige ich für die Workstation-Edition eine [$79 Lizenz]("https://www.vagrantup.com/vmware").
> Da nun aber Hyper-V und eine seperate VirtualBox instanz nicht funktionierten, hatte ich keine Möglichkeit das Projekt zu testen.

### 💾 Quellenangabe 💾
[Vagrant]("https://www.vagrantup.com/")
[Docker]("https://hub.docker.com/")