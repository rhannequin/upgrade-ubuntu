# Upgrade Ubuntu

## Sommaire

* [Pré-requis](#pre-requis)
* [Firefox](#firefox)
* [SSH](#ssh)
* [GIT](#git)
* [ZSH](#zsh)
* [Vim](#vim)
* [LAMP](#lamp)
* [Chromium](#chromium)
* [Java](#java)
* [Adobe Flash](#adobe-flash)
* [Dropbox](#dropbox)
* [Ruby](#ruby)
* [NodeJS](#nodejs)
* [PostgreSQL](#postgresql)
* [MongoDB](#mongodb)
* [Sublime Text 3](#sublime-text-3)
* [Désactiver les lancements au démarrage](#desactiver-les-lancements-au-demarrage)
* [Créer des lanceurs personnalisés sur Unity](#creer-des-lanceurs-personnalises-sur-unity)
* [Installer d'autres logiciels](#installer-d-autres-logiciels)
* [Autres](#autres)


## Pré-requis

Créer un dossier `workspace` dans le `$HOME`.

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

- Adblock Plus
- Ecosia

**Tip** : pour sélectionner la Logitech pour les fichiers apt depuis Firefox, sélectionner le script suivant : `/usr/bin/software-center`.

### Utiliser Aurora

Pour utiliser Firefox Aurora au lieu de la version stable officielle :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

### Utiliser Nightly

Pour installer Firefox Nightly (n'écrase pas la version Stable/Aurora) :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa
    sudo apt-get update
    sudo apt-get install firefox-trunk


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

Ajouter le texte suivant : voir [https://github.com/rhannequin/dotfiles/blob/master/gitconfig](https://github.com/rhannequin/dotfiles/blob/master/gitconfig).

### Vérification

```
cd ~/workspace && mkdir github bitbucket
cd github
git clone git@github.com:rhannequin/upgrade-ubuntu.git
```


## ZSH

```
sudo apt-get install zsh curl
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
chsh -s /bin/zsh
```

Modifier le fichier `~/.zshrc` et remplacer son contenu par : [https://github.com/rhannequin/dotfiles/blob/master/zshrc](https://github.com/rhannequin/dotfiles/blob/master/zshrc).


## Vim

    sudo apt-get install vim
    mkdir ~/.vim && mkdir ~/.vim/bundle
    git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
    vim .vimrc

Voir [https://github.com/rhannequin/dotfiles/blob/master/vimrc](https://github.com/rhannequin/dotfiles/blob/master/vimrc).

Lancer `:PluginInstall`.


## LAMP

### Installation

```
sudo apt-get install lamp-server^
sudo ln -s ~/workspace/ /var/www/workspace
sudo apt-get install phpmyadmin
/* sélectionner "apache2" pour reconfigurer automatiquement */
/* sélectionner "oui" pour configurer phpmyadmin avec dbconfig-common */
sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
/* ATTENTION : ne jamais utiliser /var/www/phpmyadmin ci dessus, replacez par exemple par /var/www/pma_xxx */
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

#### Composer

Vous aimez composer ? Pour l'installer globalement : 
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

## Chromium

    sudo apt-get install chromium-browser

### Backspace pour retourner à la page précédente

Installer l'extension `Backspace to go Back`.


## Java

[Télécharger](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u55 JDK.

```
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
sudo tar xvzf ~/jre-7u45-linux-*.tar.gz
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_55/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_55/bin/javac 1
sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_55/bin/javaws 1
# Pour 64 bits
# sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/amd64/libjavaplugin_jni.so 1
sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/i586/libjavaplugin_jni.so 1
sudo update-alternatives --config java
sudo update-alternatives --config javac
sudo update-alternatives --config javaws
sudo update-alternatives --config mozilla-javaplugin.so
```

### OpenJDK

```
sudo apt-get install default-jre
sudo apt-get install icedtea-plugin
```

## Adobe Flash


## Dropbox

32 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

64 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Puis lancer

    ~/.dropbox-dist/dropboxd


## Ruby

### Pré-requis

Pour installer Ruby (et Rails), il faut au préalable installer :

```
sudo apt-get install build-essential libssl-dev g++ libsqlite3-dev libyaml-dev libreadline6-dev \
zlib1g-dev libncurses5-dev
```

### Avec rbenv

```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone git@github.com:sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.2.3
rbenv global 2.2.3
```

### Avec RVM

    sudo apt-get install curl
    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    rvm install 2.2.3

Assurez-vous d'avoir la ligne suivante dans votre `~/.bashrc`:

    export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.

### Installation de Rails

```
gem install rails
# Only for rbenv
rbenv rehash
rails -v
```


## NodeJS

### Installation de [NVM](https://github.com/creationix/nvm)

```
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
. ~/.nvm/nvm.sh
```

Ajouter ces lignes aux `~/.bashrc`, `~/.profile`, ou `~/.zshrc` :

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### Installation de NodeJS

```
nvm ls-remote # Trouver la dernière version stable
nvm install 10.15.0
```

### Ajout de modules

Ces modules NPM peuvent être utiles :

```
npm install -g express create-react-app
```


## PostgreSQL

### Installation

    sudo apt-get install postgresql

### Création d'un nouvel utilisateur

```
sudo -i -u postgres
psql
CREATE USER rhannequin;
ALTER ROLE rhannequin WITH CREATEDB;
ALTER ROLE rhannequin WITH SUPERUSER;
CREATE DATABASE first_pq_db OWNER rhannequin;
ALTER USER rhannequin WITH ENCRYPTED PASSWORD 'root';
\q
exit
```

### Pgadmin

`sudo apt-get install pgadmin3`


## MongoDB

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```


## Sublime Text 3

### Installation

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo add-apt-repository "deb https://download.sublimetext.com/ apt/stable/"
sudo apt-get update
sudo apt-get install sublime-text
```

### Package Control

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
- SCSS
- Markdown Preview
- Trailing Spaces
- Emmet
- Sublime Linter
- SideBarEnhancements
- AdvancedNewFile
- Theme - Flatland

### Paramètres

Remplacer le contenu du fichier *Preferences > Settings - User* par : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings).

Remplacer le contenu du fichier *Preferences > Key Binding - User* par : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings).

#### Sublime Linter

Éditer les *Settings - User de Sublime Linter* : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings).


## Atom

Télécharger le `.deb` depuis le site [https://atom.io/download/deb](https://atom.io/download/deb).


## Désactiver les lancements au démarrage

    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf


## Créer des lanceurs personnalisés sur Unity

```
sudo apt-get install gnome-panel
sudo gnome-desktop-item-edit /usr/share/applications/ --create-new
```


## Installer d'autres logiciels

```
sudo apt-get install indicator-multiload vlc meld rar gimp virtualbox
```

### Redshift

```
sudo apt-get install redshift-gtk
```

Lancer l'application et vérifier qu'elle fonctionne. Eventuellement modifier les coordonnées géographiques.

### JetBrains

Installer [IntteliJ IDEA](http://www.jetbrains.com/idea/download), [RubyMine](http://www.jetbrains.com/ruby/download) et [WebStorm](http://www.jetbrains.com/webstorm/download) depuis le site de JetBrains.

### Skype

Via le [site](https://www.skype.com/fr/get-skype/) de skype.

### Heroku

    sudo snap install --classic heroku

### Steam

Télécharger le [paquet .dev de Steam](http://media.steampowered.com/client/installer/steam.deb) et l'installer.


## Autres

- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".
