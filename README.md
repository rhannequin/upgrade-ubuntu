# Upgrade Ubuntu

## Pré-requis

Créer un dossier workspace dans le home.

```
mkdir ~/workspace
sudo ln -s ~/workspace/ /workspace
```

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
- Pocket
- JSONView
- feedly
- Firefox OS Simulator

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

    - error_reporting = E_ALL
    - display_errors = On
    - display_startup_errors = On
    - track_errors = On

#### Configuration des modules

Il est nécessaire d'activer certains modules avec les commandes ci-dessous :

```
sudo apt-get install curl libcurl3 libcurl3-dev php5-curl php5-mcrypt
sudo a2enmod rewrite deflate
sudo service apache2 restart
```

## SSH

```
cd
ssh-keygen -t rsa
```

Se connecter à Github et Bitbucket et ajouter la clé publique SSH dans l'administration de compte.

## GIT

### Installation

    sudo apt-get install git

### Configuration

    gedit ~/.gitconfig

Ajouter le texte suivant :

```
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
  user = rhannequin
```

### Vérification

```
mkdir workspace github bitbucket
cd github
git clone git@github.com:rhannequin/upgrade-ubuntu.git
```

## Chrome

```
/* 32 bits */
$ wget -c www.mirrorservice.org/sites/archive.ubuntu.com/ubuntu//pool/main/u/udev/libudev0_175-0ubuntu13_i386.deb

/* 64 bits */
$ wget -c www.mirrorservice.org/sites/archive.ubuntu.com/ubuntu//pool/main/u/udev/libudev0_175-0ubuntu13_amd64.deb
```

[Télécharger](https://www.google.com/intl/fr/chrome/browser/) le .deb 32 ou 64 bit et l'exécuter.

TODO: ctrl+tab, backspace

## Java

[Télécharger](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u9 JDK.

```
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
```

## Adobe Flash

## Dropbox

Pour 32 bits

    cd ~ && wget -0 - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

Pour 64 bits

    cd ~ && wget -0 - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Puis lancer

    ~/.dropbox-dist/dropboxd

## Ruby avec rbenv

```
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 1.9.3-p392
rbenv global 1.9.3-p392
```

## Ruby avec RVM

Installation de aptitude, samba et curl

    sudo apt-get install aptitude samba curl

Installation des pré-requis pour rails

    sudo apt-get install libyaml-dev zlib1g-dev libcurl4-openssl-dev libsqlite3-dev g++ gcc

Installation de RVM avec Ruby

    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    /* Installer les paquets requis */

Si Ruby non installé

    rvm install 1.9.3 && rvm install 2.0.0

## Quelques gem à installer

    gem install therubyracer execjs compass rails sinatra

## Node

### Installation

```
cd ~/workspace
git clone https://github.com/joyent/node.git
cd node
git checkout v0.10.1
./configure
make
sudo make install
```

### Ajout de modules

    sudo npm install -g express coffee-script bower grunt-cli jslint nodemon

## Postgresql

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

## Mongo

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
sudo nano /etc/apt/sources.list.d/10gen.list
/* Ecrire : */
deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen
/* Fin */
sudo apt-get update
sudo apt-get install mongodb-10gen
```

## SmartGit

- Non-commercial use only
- Git Executable: usr/bin/git
- Use system SSH client
- I don't use a hosting provider

## Sublime Text 2

### Settings

Remplacer le contenu du fichier Preferences > Settings - User par :

```javascript
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
  "shift_tab_unindent": true,

  // Columns in which to display vertical rulers
  "rulers": [80]
}
```

Remplacer le contenu du fichier Preferences > Key Binding - User par :

```javascript
[
  // Reindent file
  { "keys": ["f12"], "command": "reindent"}
]
```



### Package Control

Rentrer la commande :

    import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

Si un problème se produit, ce qui semble être fréquent depuis quelques temps, il faut [télécharger le binaire du package](http://sublime.wbond.net/Package%20Control.sublime-package)  puis le déplacer dans le dossier "Installed Packages" du répertoire de configuration de Sublime Text (voir `$HOME/.config`). Redémarrer Sublime.

Si la connexion Internet subit un proxy, il faut configurer modifier le fichier Preferences > Package Settings > Package Control > Settings - User par :

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
- CoffeeScript
- INI
- Markdown Preview
- Open Recent Files
- Trailing Spaces
- DocBlockr
- Prefixr
- Emmet
- Git
- Sublime Linter
- SideBarEnhancements
- Alignment
- AdvancedNewFile

#### Sublime Linter

Éditer les Settings - User de Sublime Linter :
```javascript
{
  "jshint_options":
  {
    "indent": 2,
    "evil": true,
    "regexdash": true,
    "browser": true,
    "wsh": true,
    "trailing": true,
    "sub": true,
    "asi": true,
    "lastsemic":true
  }
}
```

#### Git

Éditer les Settings - User de Sublime Linter :

```javascript
{
  "git_command": "/usr/bin/git"
}
```


## .bashrc

Ajouter les alias suivants :

```
alias ssh-portfolio='ssh ****@****'
alias sbl='~/Logi/Sublime\ Text\ 2/sublime_text'
alias dropbox='~/.dropbox-dist/dropboxd'
alias sudo='sudo '
alias free-memory='echo 3 | sudo tee /proc/sys/vm/drop_caches'

# git log with more info and colors
alias gl="git log --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Recursive directory listing
alias lr='ls -R | grep ":$" | sed -e '\''s/:$//'\'' -e '\''s/[^-][^\/]*\//--/g'\'' -e '\''s/^/   /'\'' -e '\''s/-/|/'\'''

# Quick apt update-upgrade-clean
alias upgrade='sudo apt-get update && sudo apt-get upgrade && sudo apt-get clean'

# Create python server to this directory
alias servethis="python -c 'import SimpleHTTPServer; SimpleHTTPServer.test()'"

# Get external ip address
alias ifconfig-ext='curl ifconfig.me'

# Quick back directory
alias ..1='cd ..'
alias ..2='cd ../../../'
alias ..3='cd ../../../../'
alias ..4='cd ../../../../'
alias ..5='cd ../../../../../'

# Check is alias exists
alias al="alias | grep"

# Ask it politely
alias please="sudo"

# If needed, create command ll
alias ll='ls -l --color=auto'

# Do lastest command with sudo
alias sulast='sudo $(history -p !-1)'

# Quick apt install
alias install='sudo apt-get install'

# Quci push
alias gpm="git push origin master"

# Warn if rm recursive or deleting more than 3 items
alias rm='rm -I'


### Functions ###

# Create directory and go to it
function mcd() {
  mkdir -p "$1" && cd "$1";
}

# Find string in a file and replace it with another string
findreplace(){
    printf "Search: ${1}\n"
    printf "Replace: ${2}\n"
    printf "In: ${3}\n\n"

    find . -name "*${3}" -type f | xargs perl -pi -e 's/${1}/${2}/g'
}

# Quick exctract
extract () {
    if [ -f $1 ] ; then
      case $1 in
        *.tar.bz2)   tar xjf $1     ;;
        *.tar.gz)    tar xzf $1     ;;
        *.bz2)       bunzip2 $1     ;;
        *.rar)       unrar e $1     ;;
        *.gz)        gunzip $1      ;;
        *.tar)       tar xf $1      ;;
        *.tbz2)      tar xjf $1     ;;
        *.tgz)       tar xzf $1     ;;
        *.zip)       unzip $1       ;;
        *.Z)         uncompress $1  ;;
        *.7z)        7z x $1        ;;
        *)     echo "'$1' cannot be extracted via extract()" ;;
         esac
     else
         echo "'$1' is not a valid file"
     fi
}

# Tail with search highlight
t() {
    tail -f $1 | perl -pe "s/$2/\e[1;31;43m$&\e[0m/g"
}
```

## VIM

    sudo apt-get install vim
    gedit .vimrc
    /* Remplir */
    syntax on
    set number
    au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif

## OhMyZSH

    sudo apt-get install zsh
    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
    chsh -s $(which zsh) # Set ZSH default shell

Ajouter ces éléments au ~/.zshrc

```
# Choisir le theme `candy`
# Ajouter la version 1.9.3 de ruby au PATH :
# (remplacer `392` si besoin)
/home/remy/.rvm/gems/ruby-1.9.3-p392/bin:/home/remy/.rvm/gems/ruby-1.9.3-p392@global/bin:/home/remy/.rvm/rubies/ruby-1.9.3-p392/bin
# Ajouter les path RVM :
PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Loads RVM
# Ajouter tous les alias de ~/.bashrc
```


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

### OpenVPN

    sudo apt-get install openvpn

### Virtualbox

    sudo apt-get install virtualbox

### Raccourcis Unity

    sudo apt-get install alacarte

## Autres

- Configurer Twitter avec Gwibber (l'installer si pas déjà fait)
- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".

### Nautilus à partir de 13.04

Activer le Backspace pour revenir en arrière :

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels


## Points à éclaircir :

- Skype marche malgré pas de version pour 13.04 ?
- Pas de notification pour Friends-app
- Vérifier que install oh-my-zsh marche tout le temps
