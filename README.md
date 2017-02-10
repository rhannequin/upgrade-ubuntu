# Upgrade Ubuntu

Hey frogs, there is a French version of this project: [README.fr.md](https://github.com/rhannequin/upgrade-ubuntu/blob/master/README.fr.md)

## Index

* [Requirements](#requirements)
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
* [Disable some apps from launching](#disable-some-apps-from-launching)
* [Unity custom launchers](#unity-custom-launchers)
* [Some other softwares](#some-other-softwares)
* [Other](#other)


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

To use Firefox Aurora instead of the stable release:

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

### Use Nightly

To use install Firefox Nightly (doesn't override Stable/Aurora):

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa
    sudo apt-get update
    sudo apt-get install firefox-trunk


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

Add the following instructions: see [https://github.com/rhannequin/dotfiles/blob/master/gitconfig](https://github.com/rhannequin/dotfiles/blob/master/gitconfig).

### Check it

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

Update the file `~/.zshrc` and replace content with : [https://github.com/rhannequin/dotfiles/blob/master/zshrc](https://github.com/rhannequin/dotfiles/blob/master/zshrc).


## Vim

    sudo apt-get install vim
    mkdir ~/.vim && mkdir ~/.vim/bundle
    git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
    vim .vimrc

See [https://github.com/rhannequin/dotfiles/blob/master/vimrc](https://github.com/rhannequin/dotfiles/blob/master/vimrc).

Run `:PluginInstall`.


## LAMP

### Install

```
sudo apt-get install lamp-server^
sudo ln -s ~/workspace/ /var/www/workspace
sudo apt-get install phpmyadmin
# select "apache2" tp configure it automatically
# select "yes" to configure phpmyadmin with dbconfig-common
sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
# WARNING : never use /var/www/phpmyadmin above, replace by /var/www/pma_xxx eg
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

#### Composer

You like composer? Install it globally: 
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

## Chromium

    sudo apt-get install chromium-browser
    # TODO: fix missing backspace key and ctrl+tab


## Java

[Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u55 JDK.

```
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
sudo tar xvzf ~/jre-7u45-linux-*.tar.gz
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_55/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_55/bin/javac 1
sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_55/bin/javaws 1
# 64 bits only
# sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/amd64/libjavaplugin_jni.so 1
sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/i586/libjavaplugin_jni.so 1
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


## Ruby

### Requirements

Requirements for Ruby (and Rails) are:

```
sudo apt-get install build-essential libssl-dev g++ libsqlite3-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev
```

### With rbenv

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

### With RVM

    sudo apt-get install curl
    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    rvm install 2.2.3

Make sure you have the following line in your `~/.bashrc`:

    export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.

### Installing Rails

```
gem install rails
# Only for rbenv
rbenv rehash
rails -v
```


## NodeJS

### Install [NVM](https://github.com/creationix/nvm)

```
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
. ~/.nvm/nvm.sh
```

Add those lines to `~/.bashrc`, `~/.profile`, or `~/.zshrc`:

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

### Install NodeJS

```
nvm ls-remote # Find the latest stable version
nvm install 5.3.0
```

### Some modules

Those modules may be useful:

```
npm install -g express coffee-script nodemon gulp
```


## PostgreSQL

### Install

    sudo apt-get install postgresql

### Create new user

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

### Install

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt-get update
    sudo apt-get install sublime-text-installer

### Package Control

Run command:

    import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

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
- Better CoffeeScript
- INI
- SCSS
- Markdown Preview
- Trailing Spaces
- Emmet
- Sublime Linter
- SideBarEnhancements
- AdvancedNewFile
- Theme - Flatland

### Settings

Replace config file *Preferences > Settings - User* with: see [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings).

Replace config file *Preferences > Key Binding - User* with: see [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings).

#### Sublime Linter

Edit settings from *Settings - User de Sublime Linter* with: see [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings).


## Atom

Download `.deb` from [https://atom.io/download/deb](https://atom.io/download/deb).


## Disable some apps from launching

    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf


## Unity custom launchers

```
sudo apt-get install gnome-panel
sudo gnome-desktop-item-edit /usr/share/applications/ --create-new
```


## Some other softwares

```
sudo apt-get install indicator-multiload vlc meld rar gimp virtualbox
```

### Redshift

```
sudo apt-get install redshift-gtk
```

Start the application and check it's working. Eventually change geographical coordinates.

### JetBrains

Install [IntteliJ IDEA](http://www.jetbrains.com/idea/download), [RubyMine](http://www.jetbrains.com/ruby/download) and [WebStorm](http://www.jetbrains.com/webstorm/download) from JetBrains website.

### Skype

From Skype website.

#### Troubles with webcam since 14.04

    sudo gedit /usr/share/applications/skype.desktop
    > Exec=env PULSE_LATENCY_MSEC=60 LD_PRELOAD=/usr/lib/i386-linux-gnu/libv4l/v4l1compat.so skype %U

### Heroku

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

### Friends

    sudo apt-get install friends-app dconf-tools
    dconf-editor
    # > com > canonical > friends
    # Set interval to 1 and notifications to all

### Steam

Download the [Steam .deb package](http://media.steampowered.com/client/installer/steam.deb) and install it.


## Other

- Configure Twitter
- System Parameters > Privacy : remove bottom inputs.

### Notification bar for 14.04 (systray)

    sudo apt-add-repository ppa:gurqn/systray-trusty
    sudo apt-get update
    sudo apt-get upgrade

### Nautilus for 13.04 and 13.10 (fixed in 14.04)

Enable Backspace:

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels

### Move windows for 13.10 (fixed in 14.04)

    sudo apt-get install compizconfig-settings-manager
    ccsm

Go to Desktop Wall > Assignations > Move with window within the wall

