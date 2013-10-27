# Upgrade Ubuntu

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

Pour utiliser Aurora au lieu de la version stable officielle :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

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
source ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.0.0-p247
rbenv global 2.0.0-p247
```

## Ruby avec RVM

Installation de aptitude, samba et curl

    sudo apt-get install aptitude samba curl

Installation de RVM avec Ruby

    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    /* Installer les paquets requis */

Si Ruby non installé

    rvm install 1.9.3 && rvm install 2.0.0

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

### Pgadmin

`sudo apt-get install pgadmin3`

## Mongo

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
sudo nano /etc/apt/sources.list.d/10gen.list
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
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

Remplacer le contenu du fichier *Preferences > Settings - User* par :

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

Remplacer le contenu du fichier *Preferences > Key Binding - User* par :

```javascript
[
  {
    "keys": ["f12"],
    "command": "reindent"
  },
  {
    "keys": ["super+alt+&"],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 1.0],
      "rows": [0.0, 1.0],
      "cells": [[0, 0, 1, 1]]
    }
  },
  {
    "keys": ["super+alt+é"],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 0.5, 1.0],
      "rows": [0.0, 1.0],
      "cells": [[0, 0, 1, 1], [1, 0, 2, 1]]
    }
  },
  {
    "keys": ["super+alt+\""],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 0.33, 0.66, 1.0],
      "rows": [0.0, 1.0],
      "cells": [[0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1]]
    }
  },
  {
    "keys": ["super+alt+'"],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 0.25, 0.5, 0.75, 1.0],
      "rows": [0.0, 1.0],
      "cells": [[0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1], [3, 0, 4, 1]]
    }
  },
  {
    "keys": ["super+alt+2"],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 1.0],
      "rows": [0.0, 0.5, 1.0],
      "cells": [[0, 0, 1, 1], [0, 1, 1, 2]]
    }
  },
  {
    "keys": ["super+alt+3"],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 1.0],
      "rows": [0.0, 0.33, 0.66, 1.0],
      "cells": [[0, 0, 1, 1], [0, 1, 1, 2], [0, 2, 1, 3]]
    }
  },
  {
    "keys": ["super+alt+("],
    "command": "set_layout",
    "args":
    {
      "cols": [0.0, 0.5, 1.0],
      "rows": [0.0, 0.5, 1.0],
      "cells":
      [
        [0, 0, 1, 1], [1, 0, 2, 1],
        [0, 1, 1, 2], [1, 1, 2, 2]
      ]
    }
  }
]
```



### Package Control

Rentrer la commande :

    import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

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

Éditer les *Settings - User de Sublime Linter* :
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
    "lastsemic":true,
    "laxcomma":true,
    "eqeqeq":false,
    "boss":true,
    "curly":false,
    "bitwise":false,
    "forin":false,
    "eqnull":true
  }
}
```

#### Git

Éditer les Settings - User de Git :

```javascript
{
  "git_command": "/usr/bin/git"
}
```


## .bashrc

### Alias

Ajouter les alias suivants :

```
# Add ~/opt bins (for Node.js)
export PATH=~/opt/node/bin:${PATH}
export PATH="./node_modules/bin:${PATH}"

# Add ./node_modules to access local node modules
export PATH=${PATH}:./node_modules/.bin:

alias sbl='~/opt/Sublime\ Text\ 2/sublime_text'
alias dropbox='~/.dropbox-dist/dropboxd'

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
alias ..='cd ..'
alias ...='cd ../../'
alias ....='cd ../../../'
alias .....='cd ../../../../'
alias ......='cd ../../../../../'

# Check is alias exists
alias al="alias | grep"

# Ask it politely
alias please="sudo"

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

Remplir

    " Vundle {{{
    set nocompatible
    filetype off

    set rtp+=~/.vim/bundle/vundle/
    call vundle#rc()

    " Bundles:
    " let Vundle manage Vundle
    Bundle 'gmarik/vundle'

    Bundle 'maksimr/vim-jsbeautify'
    Bundle 'jelera/vim-javascript-syntax'
    Bundle 'scrooloose/nerdcommenter'

    " Colorschemes
    Bundle 'nanotech/jellybeans.vim'

    filetype plugin indent on
    " }}}


    " JavaScript syntax for JSON files
    autocmd BufNewFile,BufRead *.json set ft=javascript
    " UTF-8 as default encoding
    scriptencoding utf-8
    set encoding=utf-8
    set fileencoding=utf-8

    " Display useless characters
    set listchars=nbsp:¤,tab:>-,extends:>,precedes:<,trail:·
    " See the difference between tabs and spaces and for trailing blanks
    set list

    " Use the appropriate number of spaces to insert a tab
    set expandtab
    " Number of spaces that a tab in the file counts for
    set tabstop=2
    " Number of spaces to use for each step of (auto)indent
    set shiftwidth=2
    " Number of spaces that a <Tab> counts for while performing editing operations
    set softtabstop=2
    " When a bracket is inserted, briefly jump to the matching one
    set showmatch

    " Backup
    set backup
    set backupdir=/tmp
    set directory=/tmp

    " Ignore search case
    set ic

    " The syntax with this name is loaded
    syntax on
    " Use 256 colors in Console mode if we think the terminal supports it
    if &term =~? 'mlterm\|xterm'
      set t_Co=256
    endif

    " Define color scheme
    colorscheme jellybeans

    " Show the line number relative to the line with the cursor in front of each line
    set relativenumber
    autocmd InsertEnter * :set number
    autocmd InsertLeave * :set relativenumber

    " Redefine <Leader> to ","
    let mapleader= ","
    " Set 300ms to fire keystroke
    set tm=300

    " Override the 'ignorecase' option if the search pattern contains upper case characters
    set smartcase

    " Remove white spaces with _s
    nmap _s :%s/\s\+$//<CR>

    " check php syntax with Ctrl + L
    autocmd FileType php noremap <C-L> :!/usr/bin/env php -l %<CR>
    autocmd FileType phtml noremap &lt;

    " Use Q for formatting the current paragraph (or selection)
    vmap Q gq
    nmap Q gqap

    if has("autocmd")
      filetype plugin indent on
      autocmd FileType text setlocal textwidth=78

    " always jump to last edit position when opening a file
      autocmd BufReadPost *
      \ if line("'\"") > 0 && line("'\"") <= line("$") |
      \   exe "normal g`\"" |
      \ endif
    endif

    " Allow saving of files as sudo when I forgot to start vim using sudo.
    cmap w!! w !sudo tee > /dev/null %

    " Reload .vimrc when we edit it
    au! BufWritePost .vimrc source %


Lancer `:BundleInstall`.


## Désactiver les lancements au démarrage

    sudo aptitude install sysv-rc-conf
    sudo sysv-rc-conf

## Installer d'autres logiciels

`sudo apt-get install indicator-multiload vlc flashplugin-installer rar gimp filezilla openvpn virtualbox alacarte`

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

- Configurer Twitter avec Gwibber (l'installer si pas déjà fait)
- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".

### Nautilus à partir de 13.04

Activer le Backspace pour revenir en arrière :

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels

### Déplacer les fenêtres à partir de 13.10

    sudo apt-get install compizconfig-settings-manager
    ccsm

Aller dans Desktop Wall > Assignations > Move with window within the wall
