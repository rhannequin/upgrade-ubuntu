# Upgrade Ubuntu

## Pré-requis

Créer un dossier workspace dans le home.

    mkdir ~/workspace
    sudo ln -s ~/workspace/ /workspace

## Firefox

- changer l'adresse Home
- configurer Sync

### Configuration des touches

Aller dans about:config et mettre les paramètres suivants :

- browser.backspace_action à 0
- browser.ctrlTab.previews à true

## LAMP

### Installation

    sudo apt-get update
    sudo apt-get install lamp-server^
    sudo ln -s ~/workspace/ /var/www/workspace
    sudo apt-get install phpmyadmin
    /* sélectionner "apache2" pour reconfigurer automatiquement */
    /* sélectionner "oui" pour configurer phpmyadmin avec dbconfig-common */
    sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
    sudo service apache2 restart

## SSH

    cd
    ssh-keygen -t rsa

Se connecter à Github et Bitbucket et ajouter la clé publique SSH dans l'administration de compte.

## GIT

### Installation

    sudo apt-get install git

### Configuration

    gedit ~/.gitconfig

Ajouter le texte suivant :

    [color]
      diff = auto
      status = auto
      branch = auto
    [user]
      name = Rémy Hannequin
      email = ***@***
    [alias]
      ci = commit
      co = checkout
      st = status
      br = branch
    [github]
      user = RemyHannequin

### Vérification

    mkdir workspace github bitbucket
    cd github
    git clone git@github.com:RemyHannequin/upgrade-ubuntu.git

## Chrome

## Java

Télécharger JAVA SE 7u9 JDK sur http://www.oracle.com/technetwork/java/javase/downloads/index.html

    sudo mkdir /usr/lib/jvm
    cd /usr/lib/jvm
    sudo tar xvzf ~/jre-7u4-linux-*.tar.gz
    sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_04/bin/java 1
    sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_04/bin/javac 1
    sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_04/bin/javaws 1
    /* Pour 64 bits */
    /* sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_04/jre/lib/amd64/libjavaplugin_jni.so 1 */
    sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_04/jre/lib/i586/libjavaplugin_jni.so 1
    sudo update-alternatives --config java
    sudo update-alternatives --config javac
    sudo update-alternatives --config javaws
    sudo update-alternatives --config mozilla-javaplugin.so

## Adobe Flash

## Dropbox

Pour 32 bits

    cd ~ && wget -0 - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

Pour 64 bits

    cd ~ && wget -0 - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Puis lancer

    ~/.dropbox-dist/dropboxd

## RVM

Installation de aptitude, samba et curl

    sudo apt-get install aptitude samba curl

Installation des pré-requis pour rails

    sudo apt-get install libyaml-dev zlibb1g-dev libcurl4-openssl-dev libsqlite3-dev g++ gcc

Installation de RVM avec Ruby

    \curl -L https://get.rvm.io | bash -s stable --ruby

## Rails

GEM pré-requis

    gem install therubyracer execjs

## Node

### Installation

    cd ~/workspace
    git clone https://github.com/joyent/node.git
    cd node
    git checkout v0.8.14
    ./configure
    make
    sudo make install

### Ajout de modules

    sudo npm install -g express
    sudo npm install -g coffeescript

## SmartGit

- Non-commercial use only
- Git Executable: usr/bin/git
- Use system SSH client
- I don't use a hosting provider

## Sublime Text 2

### Settings

- retirer "$" de word_separators
- tab_size: 2
- translate_tabs_to_space: true
- highlight_line: true
- shift_tab_unindent: true

### Package Control

Rentrer la commande : import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())

Installer les packages

- BracketHighlither
- CoffeeScript
- INI
- Markdown Preview
- Open Recent Files
- Trailing Spaces

## .bashrc

Ajouter les alias suivants :

- alias ssh-portfolio='ssh ****@****'
- alias sbl='/home/remy/Logi/Sublime\ Text\ 2/sublime_text'
- alias dropbox='~/.dropbox-dist/dropboxd'
- alias sudo='sudo '
- alias free-memory='echo 3 | sudo tee /proc/sys/vm/drop_caches'

## VIM

    sudo apt-get install vim
    gedit .vimrc
    /* Remplir */
    syntax on
    set number
    au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif

## Moniteur système

    sudo apt-get install indicator-multiload

## Désactiver les lancements au démarrage

    sudo aptitude install sysv-rc-conf
    sudo sysv-rc-conf

## Installer d'autres logiciels

- Skype : via le site de skype
- Filezilla : sudo apt-get install filezilla
- VLC : via la Logithèque