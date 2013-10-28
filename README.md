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

- `browser.backspace_action` to `0`
- `browser.ctrlTab.previews` to `true`

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

**Tip**: to select Software Center for apt files from Firefox, select the following script: `/usr/bin/software-center`.

### Use Aurora

To use Aurora instead of the stable release:

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

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

Add the following instructions: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

### Check it

```
cd ~/workspace && mkdir github bitbucket
cd github
git clone git@github.com:rhannequin/upgrade-ubuntu.git
```

## Chromium

    sudo apt-get install chromium-browser
    # TODO: fix missing backspace key and ctrl+tab

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
    # Only for rbenv
    rbenv rehash

Try

    rails -v

If it doesn't work with RVM installed (Rails is not currently installed on this system...) add following line to ~/.bashrc at the end

    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.


## Node

### Install

```
cd ~/workspace
git clone https://github.com/joyent/node.git
cd node
git checkout v0.10.21
mkdir ~/opt && mkdir ~/opt/node
./configure --prefix=~/opt/node
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

Replace config file *Preferences > Settings - User* with: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

Replace config file *Preferences > Key Binding - User* with: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).



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

Edit settings from *Settings - User de Sublime Linter* with: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

#### Git

Edit settings from *Settings - User from Git* with: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).


## .bashrc

### Aliases

Add the following aliases: see [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

### Prompt

    git clone https://github.com/magicmonty/bash-git-prompt.git ~/.bash-git-prompt
    cp ~/.bash-git-prompt/gitprompt.sh ~/.bash-git-prompt/gitprompt.custom.sh
    nano ~/.bash-git-prompt/gitprompt.custom.sh

At the end of `PROMPT_START` definition, add the following: `PROMPT_START="\[\e[32m\]\u@\h: ${PROMPT_START}"`

    nano ~/.bashrc

Add the following:

    # Add git-bash-prompt
    if [ -f ~/.bash-git-prompt/gitprompt.custom.sh ]; then
        . ~/.bash-git-prompt/gitprompt.custom.sh
    fi

## VIM

    sudo apt-get install vim
    mkdir ~/.vim && mkdir ~/.vim/bundle
    git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
    vim .vimrc

See [https://github.com/rhannequin/dotfiles](https://github.com/rhannequin/dotfiles).

Then run `:BundleInstall`.


## Disable some apps from launching

    sudo aptitude install sysv-rc-conf
    sudo sysv-rc-conf

## Some other softwares

`sudo apt-get install indicator-multiload vlc flashplugin-installer rar gimp filezilla openvpn virtualbox alacarte`

### Skype

From Skype website.

### Heroku

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

### Friends

    sudo apt-get install friends-app dconf-tools
    dconf-editor
    # > com > canonical > friends
    # Set interval to 1 and notifications to all

TODO: Find a way to lauch it in background in startup.

## Other

- Configure Twitter with Gwibber (install it if not already)
- System Parameters > Privacy : remove bottom inputs.

### Nautilus from 13.04

Enable Backspace:

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels

### Move windows from 13.10

    sudo apt-get install compizconfig-settings-manager
    ccsm

Go to Desktop Wall > Assignations > Move with window within the wall
