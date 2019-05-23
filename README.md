<img src="./HTML/EN/plugins/GoogleMusic/html/images/icon.png" align="left" width="144px" height="144px"/>

# squeezebox-googlemusic

[![](https://img.shields.io/travis/squeezebox-googlemusic/squeezebox-googlemusic.svg)](https://travis-ci.org/squeezebox-googlemusic/squeezebox-googlemusic)
![](https://img.shields.io/github/release/squeezebox-googlemusic/squeezebox-googlemusic.svg)
![](https://img.shields.io/github/issues/squeezebox-googlemusic/squeezebox-googlemusic.svg)
![](https://img.shields.io/github/license/squeezebox-googlemusic/squeezebox-googlemusic.svg)

This is a [Squeezebox](http://www.mysqueezebox.com/) (Logitech Media
Server) Plug-in for playing music from your [Google Play
Music](https://play.google.com/music/) library and All Access. It is
based on the Python [gmusicapi](http://unofficial-google-music-api.readthedocs.org/)
library and the ability of inlining Python in Perl programs.

## Table of Contents

1. [Overview](#overview)
1. [Installation Requirements](#installation-requirements)
1. [Installation](#installation)
    1. [Installation on macOS](#installation-on-macos)
1. [Usage](#usage)
1. [Dev Resources](#dev-resources)
    1. [Development setup (Deb-based Linux)](#development-setup-deb-based-linux)
1. [Donate for this Plugin](#donate-for-this-plugin)
1. [Todo](#todo)
1. [Credits](#credits)

## Overview

This is an fork of [hechtus'](https://github.com/hechtus/squeezebox-googlemusic)
googlemusic plugin for squeezebox which has ceased development.

## Installation Requirements

* Logitech Media Server 7.8.0 or 7.9.1 or greater (some versions of 7.9.0
  might work)
  * Highly recommended to upgrade to one of the latest [nightly builds](http://downloads.slimdevices.com/nightly/) if possible
* Python 2.7.x
* A Google Account
  * An App Password if you have 2-Step Verification enabled on your account
    (see the [Usage](#usage) section below for more details)

## Installation

This installation procedure will only work on Linux based systems. At
the moment it is unknown if this works on Windows (contributions to the wiki
will be greatly appreciated). If you want to install this plugin on macOS have
a look at the [Installation on macOS](#installation-on-macos) section below.
Please let us know if you found a way to get this plugin running on non-Linux
systems to extend this howto.

1. You will need a Google account and some music and/or playlists in
   your library. If you want to use Google Music All Access features
   you will need a subscription to this service.

1. Install Python and [Python pip](http://www.pip-installer.org).

1. Install [gmusicapi](https://github.com/simon-weber/gmusicapi)
   by running:

    ```
    sudo pip install gmusicapi==12.1.0
    ```

1. The Google Music plugin requires the perl modules Inline::Python and IO::Socket::SSL
   to be installed. The following are instructions for installation on Debian
   and Redhat-based distributions.
1. If you're on a new enough version of Debian or Ubuntu, install the following
   packages:

    ```
    sudo apt-get install libio-socket-ssl-perl libinline-python-perl
    ```

   If they aren't available, install `python-dev` then continue on to installing
   the modules from cpan in the next step.

    ```
    sudo apt-get install python-dev
    ```

   On **redhat** systems do:

   If you're on a new enough Redhat based system, install the following
   packages:

    ```
    sudo yum install perl-IO-Socket-SSL perl-Inline-Python
    ```

   If they aren't available, install `python-devel` then continue on to installing
   the modules from cpan in the next step.

    ```
    sudo yum install python-devel
    ```

1. Install the Perl CPAN package
   [Inline](http://search.cpan.org/~ingy/Inline/) and
   [Inline::Python](http://search.cpan.org/~nine/Inline-Python/) by
   running:

    ```
    sudo cpan App::cpanminus
    sudo cpanm --notest Inline
    sudo cpanm --notest Inline::Python
    sudo cpanm --notest IO::Socket::SSL
    ```

1. To install the plugin, add the repository URL
   [https://squeezebox-googlemusic.github.io/squeezebox-googlemusic/repository/repo.xml](https://squeezebox-googlemusic.github.io/squeezebox-googlemusic/repository/repo.xml)
   to your squeezebox plugin settings page.
1. Save, restart the Logitech Media Server and continue on to the [usage](#usage)
   section.

### Installation on macOS

The installation on macOS is quite similar to Linux. According to the
reports for issue #28 you will have to do the following:

1. Open Terminal by typing `Terminal` in Spotlight. A command line
   interface should open.
1. Install pip. To do that, type

   ```
   sudo easy_install pip
   ```

   Note: for this to work you need to have XCode (the Mac developer
   tools) installed, you will also need the Xcode command line tools,
   but XCode will prompt you to install them if you don't.
   Then go to step 5 below.

1. Alternatively, if you don't have and don't want to install XCode
   you can also download the `get-pip.py` script from [here](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)
   But please be aware that you are doing this at your own risk. You
   are installing a binary through a script downloaded from the internet
   without any verification...

1. Change to the `Downloads` directory

   ```
   cd Downloads
   ```

1. Install [gmusicapi](https://github.com/simon-weber/gmusicapi)
   by running:

   ```
   sudo pip install gmusicapi==12.1.0
   ```
1. Install the Perl CPAN package
   [Inline](http://search.cpan.org/~ingy/Inline/) and
   [Inline::Python](http://search.cpan.org/~nine/Inline-Python/) by
   running:

   ```
   sudo cpan App::cpanminus
   sudo ARCHFLAGS="-arch i386 -arch x86_64" cpanm --notest Inline
   sudo ARCHFLAGS="-arch i386 -arch x86_64" cpanm --notest Inline::Python
   ```

1. Go to the Logitech Media Server with your browser. Visit the
   Plugins page and insert the repository URL for this Plugin at the
   bottom of the page:
   [http://squeezebox-googlemusic.github.io/squeezebox-googlemusic/repository/repo.xml](http://squeezebox-googlemusic.github.io/squeezebox-googlemusic/repository/repo.xml)
1. Save, restart the Logitech Media Server and continue on to the [usage](#usage)
   section.

## Usage

1. Go to the plug-in settings page and set your Google username and
   password for the Google Music plug-in.

   **Note**: If you have 2-Step Vertification enabled on your account, you'll
   need to generate and use an application-specific password instead of your
   regular password. Follow the instructions in this [support page](https://support.google.com/accounts/answer/185833)
   to generate an App Password for the Google Music plug-in.

1. The mobile device ID is a 16-digit hexadecimal string (without a
   '0x' prefix) identifying an Android device or a string of the form
   `ios:01234567-0123-0123-0123-0123456789AB` (including the `ios:`
   prefix) identifying an iOS device you must already have registered
   for Google Play Music. On Android you can obtain this ID by dialing
   `*#*#8255#*#*` on your phone (see the aid) or using this
   [App](https://play.google.com/store/apps/details?id=com.evozi.deviceid)
   (see the Google Service Framework ID Key). You may also use the
   script `mobile_devices.py` to list all registered devices. If your
   Android or iOS device is already registered, you may leave the
   field `Mobile Device ID` empty. It will be filled in automatically
   after setting the username and password.

   **Note**: A registered PC MAC address will not work as a mobile
     device ID.

1. Enable All Access if you have an All Access subscription.

1. You will find the plug-in in the 'My Apps' section of the
   squeezebox menu.

## Dev Resources

* Current forum thread: [Announce: Google Music plugin](http://forums.slimdevices.com/showthread.php?105800-Announce-Google-Music-plugin)
* Previous forum thread (Hechtus's version): [Google Music Plugin](http://forums.slimdevices.com/showthread.php?98526-Google-Music-Plugin)
* [https://github.com/squeezebox-googlemusic/squeezebox-googlemusic/issues](https://github.com/squeezebox-googlemusic/squeezebox-googlemusic/issues)
* [https://github.com/hechtus/squeezebox-googlemusic/issues](https://github.com/hechtus/squeezebox-googlemusic/issues)

### Development setup (Deb-based Linux)

1. Install the required libraries*
   depending on your installed python version, you might have to use pip

    ```
    sudo pip2 install gmusicapi==12.1.0
    sudo apt-get install python-dev
    sudo apt-get install libio-socket-ssl-perl
    sudo cpan App::cpanminus
    sudo cpanm --notest Inline
    sudo cpanm --notest Inline::Python
    sudo cpanm --notest IO::Socket::SSL
    ```

1. configure permissions and clone the repo

```
sudo addgroup squeezeboxserver
sudo adduser YOURUSERNAME squeezeboxserver
```

1. Now log out and log back in again, or do something like `sudo su YOURUSERNAME`

```
cd /var/lib/squeezeboxserver/Plugins
sudo mkdir GoogleMusic
sudo chgrp squeezeboxserver GoogleMusic
sudo chmod g+wx GoogleMusic
cd GoogleMusic
# don't forget the dot at the end
git clone https://github.com/squeezebox-googlemusic/squeezebox-googlemusic.git .
sudo service logitechmediaserver restart
```

## Donate for this Plugin

This plugin was originally written by [hechtus](https://github.com/hechtus).
If you are enjoying this plugin feel free to [donate](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Z2KE8W5HW9F8W)
to him.

## Todo

I'm looking forward to your help. Feel free to
[contribute](https://help.github.com/articles/fork-a-repo) or to
[report
bugs](https://github.com/squeezebox-googlemusic/squeezebox-googlemusic/issues). Here
are some things you may help on:

* Get this plugin running on non-Linux systems
* Add or improve
  [translations](https://github.com/hechtus/squeezebox-googlemusic/blob/master/GoogleMusic/strings.txt)
  to other languages
* Test the plugin with various Android or iOS Apps. Is it working with
  iPeng?
* Support for creating and deleting radio stations
* Improve Track and Album Info

## Credits

* Hechtus: original plugin author
* [Huub Bouma](https://github.com/huubbouma): All Access Support
* [icons8](https://icons8.com): Icon design
* @jamesray2: [macOS installation instructions](https://github.com/hechtus/squeezebox-googlemusic/issues/28#issuecomment-43212109)
