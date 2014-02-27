# Upgrade Ubuntu

## Sommaire

* [Pré-requis](#pre-requis)
* [Firefox](#firefox)
* [LAMP](#lamp)
* [SSH](#ssh)
* [GIT](#git)
* [Chromium](#chromium)
* [Java](#java)
* [Adobe Flash](#adobe-flash)
* [Dropbox](#dropbox)
* [Ruby](#ruby)
* [Node](#node)
* [PostgreSQL](#potsgresql)
* [MongoDB](#mongodb)
* [Sublime Text 3](#sublime-text-3)
* [.bashrc](#bashrc)
* [VIM](#vim)
* [Désactiver les lancements au démarrage](#desactiver-les-lancements-au-demarrage)
* [Créer des lanceurs personnalisés sur Unity](#creer-des-lanceurs-personnalises-sur-unity)
* [Installer d'autres logiciels](#installer-d-autres-logiciels)
* [Autres](#autres)


## Pré-requis

Créer un dossier `workspace` dans le home.

```
mkdir ~/workspace
sudo ln -s ~/workspace/ /workspace
```

## Firefox

- changer l'adresse Home
- configurer Sync

### Configuration des touches

Aller dans *about:config* et mettre les paramètres suivants :

- `browser.backspace_action` à `0`
- `browser.ctrlTab.previews` à `true`

### Installation des plugins

- Firebug
- Live HTTP Headers
- Web Developer
- Adblock Plus
- Pocket
- JSONView
- feedly
- Firefox OS Simulator
- ColorZilla

**Tip** : pour sélectionner la Logitech pour les fichiers apt depuis Firefox, sélectionner le script suivant : `/usr/bin/software-center`.

### Utiliser Aurora

Pour utiliser Firefox Aurora au lieu de la version stable officielle :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

### Utiliser Nightly

Pour installer Firefox Nightly (n'écrase pas la version Stable/Aurora) :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa
    sudo apt-get install firefox-trunk


## LAMP

### Installation

```
sudo apt-get update
sudo apt-get install lamp-server^
sudo ln -s ~/workspace/ /var/www/workspace
sudo apt-get install phpmyadmin
/* sélectionner "apache2" pour reconfigurer automatiquement */
/* sélectionner "oui" pour configurer phpmyadmin avec dbconfig-common */
sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
sudo service apache2 restart
```

#### Configuration de PHP

Il est nécessaire de modifier les fichiers php.ini de PHP pour activer le debug. Les paramètres suivants doivent être tels que :

```
- error_reporting = E_ALL
- display_errors = On
- display_startup_errors = On
- track_errors = On
```

#### Configuration des modules

Il est nécessaire d'activer certains modules avec les commandes ci-dessous :

```
sudo apt-get install curl libcurl3 libcurl3-dev php5-curl php5-mcrypt
sudo a2enmod rewrite deflate
sudo service apache2 restart
```

## SSH

```
cd ~
ssh-keygen -t rsa
```

Il peut être intéressant d'installer openssh-server : `sudo apt-get install openssh-server`

/!\ Se connecter à Github et Bitbucket et ajouter la clé publique SSH dans l'administration de compte.

## GIT

### Installation

```
sudo apt-get install git
```

### Configuration

```
gedit ~/.gitconfig
```

Ajouter le texte suivant : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

### Vérification

```
cd ~/workspace && mkdir github bitbucket
cd github
git clone git@github.com:rhannequin/upgrade-ubuntu.git
```

## Chromium

    sudo apt-get install chromium-browser
    # TODO: fix missing backspace key and ctrl+tab

## Java

[Télécharger](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u45 JDK.

```
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
sudo tar xvzf ~/jre-7u45-linux-*.tar.gz
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_45/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_45/bin/javac 1
sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_45/bin/javaws 1
# Pour 64 bits
# sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_04/jre/lib/amd64/libjavaplugin_jni.so 1
sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_04/jre/lib/i586/libjavaplugin_jni.so 1
sudo update-alternatives --config java
sudo update-alternatives --config javac
sudo update-alternatives --config javaws
sudo update-alternatives --config mozilla-javaplugin.so
```

## Adobe Flash

## Dropbox

32 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

64 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Puis lancer

    ~/.dropbox-dist/dropboxd

## Ruby avec rbenv

### Installation des pré-requis pour rails

Afin d'installer Ruby On Rails, il est nécessaire d'installer les packages suivants **avant** d'installer Ruby.

Il semble y avoir quelque problème pour installer Rails sur Ubuntu 13.10. Ce [post stackoverflow](http://stackoverflow.com/questions/5720484/how-to-solve-certificate-verify-failed-on-windows) est une solution temporaire.

```

Maintenant que les dépendances sont installées, vous pouvez installer rbenv et Ruby.

sudo apt-get install libyaml-dev zlib1g-dev libcurl4-openssl-dev libsqlite3-dev g++ gcc
```

```
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone git@github.com:sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.1.0
rbenv global 2.1.0
```

## Ruby avec RVM

Installation de aptitude, samba et curl

    sudo apt-get install aptitude samba curl

Installation de RVM avec Ruby

    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    /* Installer les paquets requis */

Si Ruby non installé

    rvm install 2.0.0 && rvm install 2.1.0

## Quelques gem à installer

    gem install therubyracer execjs compass rails sinatra
    # Seulement pour rbenv
    rbenv rehash

Try

    rails -v

Si rails ne semble pas installé avec RVM d'installé (Rails is not currently installed on this system...) ajouter la ligne suivante à la fin du fichier ~/.bashrcd

    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.


## Node

### Installation

```
cd ~/workspace
git clone https://github.com/joyent/node.git
cd node
git checkout v0.10.21
mkdir ~/opt && mkdir ~/opt/node
./configure --prefix=~/opt/node
make
make install

/* make peut potentiellement remonter des erreurs, si c'est le cas : */
sudo apt-get install g++
/* puis relancer make et make install */
```

### Ajout de modules

    sudo npm install -g express coffee-script bower grunt-cli jslint nodemon

## PostgreSQL

### Installation

    sudo apt-get install postgresql

### Création d'un nouvel utilisateur

```
sudo -i -u postgres
psql
CREATE USER rhannequin;
ALTER ROLE rhannequin WITH CREATEDB;
CREATE DATABASE first_pq_db OWNER rhannequin;
ALTER USER rhannequin WITH ENCRYPTED PASSWORD '****';
\q
exit
```

### Pgadmin

`sudo apt-get install pgadmin3`

## MongoDB

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
sudo nano /etc/apt/sources.list.d/10gen.list
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install mongodb-10gen
```

## Sublime Text 3

### Installation

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt-get update
    sudo apt-get install sublime-text-installer

### Paramètres

Remplacer le contenu du fichier *Preferences > Settings - User* par : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

Remplacer le contenu du fichier *Preferences > Key Binding - User* par : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).



### Package Control

Rentrer la commande :

    import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

Si un problème se produit, ce qui semble être fréquent depuis quelques temps, il faut [télécharger le binaire du package](http://sublime.wbond.net/Package%20Control.sublime-package)  puis le déplacer dans le dossier "Installed Packages" du répertoire de configuration de Sublime Text (voir `$HOME/.config`). Redémarrer Sublime.

Si la connexion Internet subit un proxy, il faut configurer modifier le fichier *Preferences > Package Settings > Package Control > Settings - User* par :

```javascript
{
  // An HTTP proxy server to use for requests
  "http_proxy": "xxx.xxx:xx",

  // An HTTPS proxy server to use for requests - this will inherit from
  // http_proxy if it is set to "" or null and http_proxy has a value. You
  // can set this to false to prevent inheriting from http_proxy.
  "https_proxy": "xxx.xxx:xx",

  // Username and password for both http_proxy and https_proxy
  "proxy_username": "xxx",
  "proxy_password": "xxx"
}
```

### Installer les packages

- BracketHighlither
- Better CoffeeScript
- INI
- JSONLint
- SCSS
- Markdown Preview
- Trailing Spaces
- DocBlockr
- Emmet
- Git
- Sublime Linter
- SideBarEnhancements
- Alignment
- AdvancedNewFile
- Theme - Flatland

#### Sublime Linter

Éditer les *Settings - User de Sublime Linter* : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

#### Git

Éditer les Settings - User de Git : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).


## .bashrc

### Alias

Ajouter les alias suivants : voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

### Prompt

    git clone https://github.com/magicmonty/bash-git-prompt.git ~/.bash-git-prompt
    cp ~/.bash-git-prompt/gitprompt.sh ~/.bash-git-prompt/gitprompt.custom.sh
    nano ~/.bash-git-prompt/gitprompt.custom.sh

Après la définition de `PROMPT_START`, ajouter la ligne suivante : `PROMPT_START="\[\e[32m\]\u@\h: ${PROMPT_START}"`

    nano ~/.bashrc

Ajouter les lignes suivantes :

    # Add git-bash-prompt
    if [ -f ~/.bash-git-prompt/gitprompt.custom.sh ]; then
        . ~/.bash-git-prompt/gitprompt.custom.sh
    fi

## VIM

    sudo apt-get install vim
    mkdir ~/.vim && mkdir ~/.vim/bundle
    git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
    vim .vimrc

Voir [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

Lancer `:BundleInstall`.


## Désactiver les lancements au démarrage

    sudo aptitude install sysv-rc-conf
    sudo sysv-rc-conf

## Créer des lanceurs personnalisés sur Unity

```
sudo apt-get install gnome-panel
sudo gnome-desktop-item-edit /usr/share/applications/ --create-new
```

## Installer d'autres logiciels

`sudo apt-get install indicator-multiload vlc flashplugin-installer rar gimp filezilla openvpn virtualbox alacarte`

### JetBrains

Installer [IntteliJ IDEA](http://www.jetbrains.com/idea/download), [RubyMine](http://www.jetbrains.com/ruby/download) et [WebStorm](http://www.jetbrains.com/webstorm/download) depuis le site de JetBrains.

### Skype

Via le site de skype.

### Heroku

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

### Friends

    sudo apt-get install friends-app dconf-tools
    dconf-editor
    # > com > canonical > friends
    # Mettre interval à 1 et notifications à all

## Autres

- Configurer Twitter
- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".

### Nautilus à partir de 13.04

Activer le Backspace pour revenir en arrière :

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels

### Déplacer les fenêtres à partir de 13.10

    sudo apt-get install compizconfig-settings-manager
    ccsm

Aller dans Desktop Wall > Assignations > Move with window within the wall
