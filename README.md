# Upgrade Ubuntu

Hey frogs, there is a French version of this project: [README.fr.md](https://github.com/rhannequin/upgrade-ubuntu/blob/master/README.fr.md)

## Requirements

Create a `workspace` folder into $HOME.

```
mkdir ~/workspace
sudo ln -s ~/workspace/ /workspace
```

## Firefox

- Change homepage
- Configure Sync

### Keyboard configuration

Go to *about:config* and update the followings :

- browser.backspace_action to `0`
- browser.ctrlTab.previews to `true`

### Plugins

- Firebug
- Live HTTP Headers
- Web Developer
- Adblock Plus
- Pocket
- JSONView
- feedly
- Firefox OS Simulator
- ColorZilla

## LAMP

### Install

```
sudo apt-get update
sudo apt-get install lamp-server^
sudo ln -s ~/workspace/ /var/www/workspace
sudo apt-get install phpmyadmin
# select "apache2" tp configure it automatically
# select "yes" to configure phpmyadmin with dbconfig-common
sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
sudo service apache2 restart
```

#### PHP configuration

You need to edit `php.ini` files from PHP to activate debug. The following parameters must be like this:

```
- error_reporting = E_ALL
- display_errors = On
- display_startup_errors = On
- track_errors = On
```

#### Modules

You must activate the following modules:

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

You should also install openssh-server: `sudo apt-get install openssh-server`

/!\ Don't forget to add you public key to your Github and/or Bitbucket accounts.

## GIT

### Install

```
sudo apt-get install git
```

### Configuration

```
gedit ~/.gitconfig
```

Add the following instructions:

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

### Check it

```
cd ~/workspace && mkdir github bitbucket
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

[Download](https://www.google.com/intl/fr/chrome/browser/) .deb 32 or 64 bit and execute it.

TODO: ctrl+tab, backspace

## Java

[Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u45 JDK.

```
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
sudo tar xvzf ~/jre-7u45-linux-*.tar.gz
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_45/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_45/bin/javac 1
sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_45/bin/javaws 1
# 64 bits only
# sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_45/jre/lib/amd64/libjavaplugin_jni.so 1
sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_45/jre/lib/i586/libjavaplugin_jni.so 1
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

Start dropbow

    ~/.dropbox-dist/dropboxd

## Ruby with rbenv

### Requirements for Rails

In order to install Ruby On Rails, you must install those packages **before** installing Ruby.

Seems to have some trouble with installing Rails on Ubuntu 13.10. This [stackoverflow post](http://stackoverflow.com/questions/5720484/how-to-solve-certificate-verify-failed-on-windows) is a temporary solution.

```
sudo apt-get install libyaml-dev zlib1g-dev libcurl4-openssl-dev libsqlite3-dev g++ gcc
```

Now all the requirements are installed, you can install rbenv and Ruby.

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

## Ruby with RVM

aptitude, samba et curl installation

    sudo apt-get install aptitude samba curl

Install RVM with Ruby

    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    # Install required packages

Of Ruby is not installed

    rvm install 1.9.3 && rvm install 2.0.0

## Some gems to install

    gem install therubyracer execjs compass rails sinatra

## Node

### Install

```
cd ~/workspace
git clone https://github.com/joyent/node.git
cd node
git checkout v0.10.21
mkdir ~/opt
./configure --prefix=~/opt
make
make install

# make may eventually throw errors, in that case do:
sudo apt-get install g++
# then try again make and make install
```

### Some modules

    npm install -g express coffee-script bower grunt-cli jslint nodemon sails

## Postgresql

### Install

    sudo apt-get install postgresql

### Create new user

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
# Write:
deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen
# End
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

Replace config file *Preferences > Settings - User* with:

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

Replace config file *Preferences > Key Binding - User* with:

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

Run command:

    import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

If an error occurs, you need to [download the binary package](http://sublime.wbond.net/Package%20Control.sublime-package) and move it to *Installed Packages* at `$HOME/.config/sublime-text-2`. Restart Sublime Text.

If you're bihind a proxy, you need to edit the config file ùPreferences > Package Settings > Package Control > Settings - User* with:

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

### Some packages

- BracketHighlither
- CoffeeScript
- INI
- JSONLint
- SCSS
- LESS
- EJS
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

Edit settings from *Settings - User de Sublime Linter* with:
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

Edit settings from *Settings - User de Sublime Linter* with:

```javascript
{
  "git_command": "/usr/bin/git"
}
```


## .bashrc

Add the following aliases:

```
# Add ~/opt bins (for Node.js)
export PATH=~/opt/bin:${PATH}

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
alias ..1='cd ..'
alias ..2='cd ../../../'
alias ..3='cd ../../../../'
alias ..4='cd ../../../../'
alias ..5='cd ../../../../../'

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

## VIM

    sudo apt-get install vim
    vim .vimrc

Fill

    syntax enable
    autocmd BufNewFile,BufRead *.json set ft=javascript
    scriptencoding utf-8

    set listchars=nbsp:¤,tab:>-,extends:>,precedes:<,trail:·
    set list

    set expandtab
    set tabstop=2
    set shiftwidth=2
    set softtabstop=2
    set showmatch

    set backupdir=~/tmp

    " Ignore search case
    set ic

    set smartcase

    " Remove white spaces with _s
    nmap _s :%s/\s\+$//<CR>

    " check php syntax with Ctrl + L
    autocmd FileType php noremap <C-L> :!/usr/bin/env php -l %<CR>
    autocmd FileType phtml noremap &lt;

    " Use Q for formatting the current paragraph (or selection)
    vmap Q gq
    nmap Q gqap

    set encoding=utf-8
    set fileencoding=utf-8

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

Comment pluggin for wim (add it into  ~/.vim/plugin/)
http://www.vim.org/scripts/script.php?script_id=1218


## System Mnitoring

    sudo apt-get install indicator-multiload

## Disable some apps from launching

    sudo aptitude install sysv-rc-conf
    sudo sysv-rc-conf

## Some other softwares

### Skype

From Skype website.

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

### Unity shortcuts manager

    sudo apt-get install alacarte

## Other

- Configure Twitter with Gwibber (install it if not already)
- System Parameters > Privacy : remove bottom inputs.

### Nautilus from 13.04

Enable Backspace:

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels


## TODO:

- No notifications from Friends-app
