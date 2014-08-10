# Introduction

Bootstrap is an [Ansible](http://ansibleworks.com) playbook that you can use to set up and immediately secure a brand new server, such as a fresh [Linode](https://www.linode.com/?r=1b44d47e6692b615dca06f2233fa741744a8ccb5). This playbook is inspired and largely based off of [Bryan Kennedy](http://plusbryan.com)'s excellent post [My First 5 Minutes On A Server; Or, Essential Security for Linux Servers](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers). There's also some stuff from Linode's own [Getting Started](https://library.linode.com/getting-started) and [Securing Your Server](https://library.linode.com/securing-your-server) guides.

# Usage

## Assumptions

* You're running this on a fresh VPS.
* [Debian 7](http://www.debian.org/News/2013/20130504) or an equivalent Linux distribution. (You can use whatever distro you want, but deviating from Debian will require more tweaks to the playbooks. See Ansible's different [packaging](http://www.ansibleworks.com/docs/modules.html#packaging) modules.)
* I can confirm that is also works as-is for [Ubuntu 14.04 x64](http://releases.ubuntu.com/14.04/).

## Preparations

### Install Ansible

First, make sure you've [got Ansible installed](http://www.ansibleworks.com/docs/intro_installation.html).

### Prep the server

When creating a fresh linode (or whatever) I make sure to set a good root password right from the Linode (or web) Manager. You can also change the root password from the command line:

    passwd

### Install sshpass

The playbook will create a 'deploy' user so that you aren't using root anymore. It will also disable sshing into your server as root once the playbook has finished. Since this is a brand new server, the playbook will need to run as root which means you should install sshpass using [Homebrew](http://brew.sh).

Read [Install sshpass Using Homebrew](http://lalyos.github.io/blog/2013/09/30/install-sshpass-on-mac/) for more on why this is necessary.

    brew install https://raw.github.com/eugeneoden/homebrew/eca9de1/Library/Formula/sshpass.rb

### Generate an SSH Key

On Mac/Linux run the command:

    ssh-keygen

Follow the onscreen instructions to generate your SSH key pairs on your desktop computer. By default, the ssh-keygen utility will save your private key to '~/.ssh/id_rsa' and the public key to '~/.ssh/id_rsa.pub'. If you don't want to use a passphrase simply press 'Enter' when prompted.

Copy the entire contents of the public key file (~/.ssh/id_rsa.pub) and paste it in /roles/bootstrap/files/id_rsa.pub, replacing the TODO line. Even if you saved your key to a different name than id-rsa, you do not need to change this filename.

Don't forget to add the newly created key or you will experience a `Permission denied (publickey)` error later on. If you entered a passphrase in the previous step you will need to enter that when prompted also.

    ssh-add ~/.ssh/id_rsa

### Configure Ansible Variables

Modify the settings in /vars/user.yml to your liking. If you want to see how they're used in context, just search for the corresponding string.

For the 'deploy' user you can generate a password hash using the openssl passwd command:

    openssl passwd -salt <salt> -1 <plaintext>

### Run the Ansible Playbook

From the directory where you've cloned this project:

    ansible-playbook -i ./hosts bootstrap.yml --user root --ask-pass

# Next Steps

I would recommend you check out [Alex Payne's Sovereign project](https://github.com/al3x/sovereign) on Github if you want to set up your own mail server and personal cloud. 

# Contributing

If you improve upon this playbook or add an exciting new one, send a pull request. Everyone benefits.

## License

Original content is "GPLv3":http://gplv3.fsf.org, same as Ansible. All files and templates based on third-party software should be considered under their respective licenses.