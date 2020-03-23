
About
-----

This repository contains my personal dotfiles, which I use on unix systems of
varying flavors, and on my Macs. They are published here because I
occasionally want to share them or use them as examples, and this is an easy
way to do so. Feel free to use anything you find here.

Note however, that I do not make any particular effort to make this easy for
others to use. I randomly move things around, add and remove Vim bundles,
according to my needs. If you want to use these dotfiles, I would definitely
suggest forking them to your own repo so you can selectively merge changes as
you see fit. YMMV.

Compatibility
-------------

I have used this setup with macOS, Solaris, FreeBSD, and Linux systems.
Today I primarily use macOS and Linux (Debian and Redhat families).

Some parts of the Bash, Zsh, and Vim configs may assume you have 256 color
support in your terminal. If you are using Apple Terminal before OS X Lion,
you don't. Try [iTerm2][] instead.

[iterm2]: http://sites.google.com/site/iterm2home/


Installation
------------

The Ansible setup requires a `hosts` inventory file. That is not present in this
repository as it's specific to me. See Ansible's documentation for info about the
format of the inventory file. (There is more than one available format.)

    umask 0022
    git clone https://github.com/zanewestover/dotfiles.git ~/.dotfiles

To install my dotfiles as *your* dotfiles, you can create symlinks with
`./install.pl all`. If you do *not* run that command, everything will be
isolated inside of the `~/.dotfiles` directory (or wherever you cloned it)
and will not interfere with your existing environment.

    cd ~/.dotfiles && ./install.pl all

`install.pl` will ask you before overwriting any files that already exist.

You can also install subsets of the environment using one of the following.

    ./install.pl bash
    ./install.pl zsh
    ./install.pl vim
    ./install.pl git

Updating
--------

I periodically change the Vim bundles I use. There are two update tasks for
Vim. The first updates the bundles from their repositories.

    ./install.pl update:vim

The second cleans up any bundles which are no longer known. (Note that
`update:vim` will run the cleanup before doing the update step.)

    ./install.pl cleanup:vim

You can refresh the zsh environment with

    ./install.pl update:zsh

