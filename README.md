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

### Installation des plugins

Les plugins suivants sont à installer :

- Firebug
- Live HTTP Headers
- Web Developer
- Adblock Plus

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

#### Configuration de PHP

Il est nécessaire de modifier les fichiers php.ini de PHP pour activer le debug. Les paramètres suivants doivent être tels que :

    - error_reporting = E_ALL
    - display_errors = On
    - display_startup_errors = On
    - track_errors = On

#### Configuration des modules

Il est nécessaire d'activer certains modules avec les commandes ci-dessous :

    sudo apt-get install curl libcurl3 libcurl3-dev php5-curl php5-mcrypt
    sudo a2enmod rewrite deflate
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

## Postgresql

### Installation

    sudo apt-get install postgresql

### Création d'un nouvel utilisateur

    sudo -i -u postgres
    psql
    CREATE USER remy;
    ALTER ROLE remy WITH CREATEDB;
    CREATE DATABASE first_pq_db OWNER remy;
    ALTER USER remy WITH ENCRYPTED PASSWORD '****';
    \q
    exit

## SmartGit

- Non-commercial use only
- Git Executable: usr/bin/git
- Use system SSH client
- I don't use a hosting provider

## Sublime Text 2

### Settings

Remplacer le contenu du fichier Preferences > Settings - User par :

    // Settings in here override those in "Default/Preferences.sublime-settings", and
    // are overridden in turn by file type specific settings.
    {
      // Characters that are considered to separate words
      "word_separators": "./\\()\"'-:,.;<>~!@#%^&*|+=[]{}`~?",

      // The number of spaces a tab is considered equal to
        "tab_size": 2,

        // Set to true to insert spaces when tab is pressed
        "translate_tabs_to_spaces": true,

        // If enabled, will highlight any line with a caret
        "highlight_line": true,

        // By default, shift+tab will only unindent if the selection spans
        // multiple lines. When pressing shift+tab at other times, it'll insert a
        // tab character - this allows tabs to be inserted when tab_completion is
        // enabled. Set this to true to make shift+tab always unindent, instead of
        // inserting tabs.
        "shift_tab_unindent": true
    }

Remplacer le contenu du fichier Preferences > Key Binding - User par :

    [
      // Reindent file
      { "keys": ["f12"], "command": "reindent"}
    ]



### Package Control

Rentrer la commande :

    import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

Si un problème se produit, ce qui semble être fréquent depuis quelques temps, il faut [télécharger le binaire du package][1]  puis le déplacer dans le dossier "Installed Packages" du répertoire de configuration de Sublime Text (voir `$HOME/.config`). Redémarrer Sublime.

Si la connexion Internet subit un proxy, il faut configurer modifier le fichier Preferences > Package Settings > Package Control > Settings - User par :

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

### Skype

Via le site de skype.

### Filezilla

    sudo apt-get install filezilla

### VLC

    sudo apt-get install vlc

### Heroku

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

### Virtualbox

    sudo apt-get install virtualbox

## Autres

- Configurer Twitter avec Gwibber
- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".

[1]: http://sublime.wbond.net/Package%20Control.sublime-package