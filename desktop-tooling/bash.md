# bash
_(presumes MacOSX desktop or laptop)_

#### Version
* check your Version
* If bash version >=5.x you are good on version
* If below <5.x then augment or replace it with bash v5.x

##### Check existing version:
* run 'bash -version'

```bash
# check your version
$ bash -version

GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin20)
Copyright (C) 2007 Free Software Foundation, Inc.
```

##### Upgrade and/or Augment with bash v5
* use [brew](brew.md)
* follow [this doc](https://medium.com/@thechiefalone/how-to-install-bash-5-0-mac-os-ae570be6c687)

```bash
# install bash v5 with brew package manager
brew install bash

# find true path by listing the 'bash' symlink in /usr/local/bin
ll /usr/local/bin/bash

# sudo to edit the /etc/shells official shell listing
sudo vim /etc/shells

# use that VALUE as the true full path to the bash v5 shell in /etc/shells like
sudo echo /usr/local/Cellar/bash/5.1.8/bin/bash >> /etc/shells

# verify your /etc/shells change took effect
cat /etc/shells

# modify your $PATH in ~/.bashrc
$ vim ~/.bashrc
```

##### sample ~/.bashrc contents
```bash
#!/bin/bash
# example snip of PATH line in .bashrc
# append 'usr/local/bin' to the FRONT of your $PATH statement in .bashrc
export PATH=/usr/local/bin:${PATH}
```
##### set bash v5 as default shell & verify
```bash
# reboot your machine
$ sudo reboot

# officially change shell to bash5
$ chsh -s /usr/local/Cellar/bash/5.1.8/bin/bash $USER

# test to see if bash v5 precedes bash v3
$ which bash

/usr/local/bin/bash

$ bash -version

GNU bash, version 5.1.8(1)-release (x86_64-apple-darwin20.3.0)
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

#### Configure bash to customize your profile runtime environment
* ${HOME}/.bash_profile - runs every time a new bash shell is created
* ${HOME}/.bashrc - environment variables that get sourced by your .bash_profile
* both files should be writable by author and readable by all others 'chmod 644'
* conventions: each APP should define a HOME variable, and $HOME/bin should be added to $PATH so all binary executables are available from any directory in bash shell. Also sometimes $HOME/lib directories are required for the proper functioning of some APPs
* structure: define shell '#!/bin/bash', define variables 'export APPHOME=/opt/app', define $PATH, define SSH private keys to add to keychain, define aliases, define command prompt

##### Use .bash_profile to check for existence of .bashrc and source it when present

```bash
# edit the file and set permissions
vim ~/.bash_profile
chmod 644 ~/.bash_profile

# display file contents
cat ~/.bash_profile
```

##### .bash_profile example file

```bash
#!/bin/bash
# sample .bash_profile contents
# If .bashrc exists, then source it in your runtime environment variables
if [ -f '/Users/${USER}/.bashrc' ]; then source '/Users/${USER}/.bashrc'; fi
```

##### Use .bashrc to setup all ENV variables, PATH & aliases

```bash
# edit the file and set permissions
vim ~/.bashrc
chmod 644 ~/.bashrc

# display file contents
cat ~/.bashrc
```

##### .bashrc example file

```bash
#!/bin/bash
# sample .bashrc contents
# export $JAVA_HOME=/usr/libexec/java_home
export M2_HOME=/users/johnsexton/apache-maven-3.5.4
export ISTIO_HOME=/Users/johnsexton/workspace/istio-1.0.2
export CONDA_PATH=/Users/johnsexton/anaconda3
export CHEF_HOME=/opt/chefdk
export PATH=~/scripts:$CHEF_HOME/bin:$CHEF_HOME/embedded/bin:$M2_HOME/bin$ISTIO_HOME/bin:$CONDA_PATH/bin:$PATH
eval `ssh-agent -s`
ssh-add ~/.ssh/id_macair
alias ll='ls -la'
alias k='kubectl'
alias g='git'
alias kx='kubectx'
```
