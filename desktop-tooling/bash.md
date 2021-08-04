# bash
_(presumes MacOSX desktop or laptop)_

### Version
* check your Version
* augment or replace it with bash v5.x

##### Check existing version:
* run 'bash -version'

```bash
# check your version
$ bash -version

GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin20)
Copyright (C) 2007 Free Software Foundation, Inc.
```

##### Augment with bash v5
* use [brew](brew.md)
* follow [this doc](https://medium.com/@thechiefalone/how-to-install-bash-5-0-mac-os-ae570be6c687)

```Bash
brew install bash
ll /usr/local/bin/bash
sudo vim /etc/shells
sudo echo /usr/local/Cellar/bash/5.1.8/bin/bash >> /etc/shells
cat /etc/shells
chsh -s /usr/local/Cellar/bash/5.1.8/bin/bash $USER
sudo reboot
```

### Configure bash to customize your profile runtime environment
* ${HOME}/.bash_profile - runs every time a new bash shell is created
* ${HOME}/.bashrc - environment variables that get sourced by your .bash_profile
* both files should be writable by author and readable by all others 'chmod 644'

```bash
vim ~/.bash_profile
chmod 644 ~/.bash_profile
```

```bash
cat ~/.bash_profile
```

```bash
#!/bin/bash
# If .bashrc exists, then source it in your runtime environment variables
if [ -f '/Users/johnsexton/.bashrc' ]; then source '/Users/johnsexton/.bashrc'; fi
```

```bash
vim ~/.bashrc
chmod 644 ~/.bashrc
cat ~/.bashrc
```

```bash
#!/bin/bash
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
