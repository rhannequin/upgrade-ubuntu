# Upgrade Ubuntu

Hey frogs, there is a French version of this project: [README.fr.md](https://github.com/rhannequin/upgrade-ubuntu/blob/master/README.fr.md)

## Index

* [Requirements](#requirements)
* [Firefox](#firefox)
* [SSH](#ssh)
* [GIT](#git)
* [ZSH](#zsh)
* [Vim](#vim)
* [Chromium](#chromium)
* [Java](#java)
* [Ruby](#ruby)
* [NodeJS](#nodejs)
* [PostgreSQL](#postgresql)
* [Disable some apps from launching](#disable-some-apps-from-launching)
* [Unity custom launchers](#unity-custom-launchers)
* [Some other softwares](#some-other-softwares)


## Requirements

Create a `code` folder into `$HOME`.

```
mkdir ~/code
```


## Firefox

- Change homepage
- Configure Sync

### Keyboard configuration

Enable most recent tab switch in Settings

Go to *about:config* and update the followings :

- `browser.backspace_action` to `0`


### Use Aurora

To install Firefox Aurora instead of the stable release:

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

### Use Nightly

To install Firefox Nightly (doesn't override Stable/Aurora):

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


## Chromium

    sudo apt-get install chromium-browser

### Backspace for back

Install extension `Backspace to go Back`.


## Java

### OpenJDK

```
sudo apt-get install default-jre
sudo apt-get install icedtea-plugin
```


## Ruby

### Requirements

Requirements for Ruby (and Rails) are:

```
sudo apt-get install build-essential libssl-dev g++ libsqlite3-dev libyaml-dev libreadline6-dev \
zlib1g-dev libncurses5-dev
```

### With rbenv

```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone git@github.com:sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.6.1
rbenv global 2.6.1
```

### With RVM

    sudo apt-get install curl
    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    rvm install 3.0.3

Make sure you have the following line in your `~/.bashrc`:

    export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.

### Installing Rails

```
gem install rails
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
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### Install NodeJS

```
nvm ls-remote # Find the latest stable version
nvm install 10.15.0
```

### Some modules

Those modules may be useful:

```
npm install -g express create-react-app
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


#### Workspaces

```
gnome-tweaks
```

Go to "Workspaces" and enable 4 workspaces.

Install "Workspace Matrix" Gnome extension and configure it to use 2 rows and 2 columns.


## Disable some apps from launching

    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf


## Some other softwares

```
sudo apt-get install indicator-multiload vlc meld rar gimp virtualbox
```

### Ubuntu 21.04

`indicator-multiload` seems not to work since 20.04, instead install the Gnome Extension [system-monitor](https://extensions.gnome.org/extension/120/system-monitor/)

### Heroku

    sudo snap install --classic heroku

