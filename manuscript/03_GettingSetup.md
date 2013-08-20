So you wanna install Ubuntu too? Well good for you!

The Ubuntu wiki is so detailed that it includes specific instructions for each interation of the Macbook. It's a good idea to look over the tables to see what will and won't work, but really devs use Maxbooks so chances are 99% of things work correctly. The proces basically boils to three steps.

1. Install a boot manager.
2. Make a partition for your Ubuntu install.
3. Make a bootable USB thing a ma bob.
3. Boot up and install.

I wont document this here but will direct you to the wikis (add wiki links)

I will hoeverw go over some of the common hiccups. As what is missing from that wikipedia page is a common run down off issues.

# Hiccups

explain noemodeset and acpi.

explain reboots fixing things.

mention AskUbuntu

## The things we need.

The cool thing about linux is the package managers. Like app stores but on steroids.
No anvigating around Nightly channels for Firefox release we just do.


Update them repos sonny.

```sh
sudo apt-get update && sudo apt-get upgrade
```

Add the basics.

```sh
sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool  ncurses-term exuberant-ctags libnotify-bin curl autoconf make automake ssh git-core git-doc
```

OH My Zsh.

```sh
sudo apt-get install zsh
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

Add your .ssh files.

## Rbenv
 
https://github.com/sstephenson/rbenv/#basic-github-checkout

## A text editor

### Sublime

```sh
sudo add-apt-repository ppa:webupd8team/sublime-text-2
sudo apt-get update
sudo apt-get install sublime-text-2-beta
```

### Emacs

```
sudo apt-get install emacs-snapshot-gtk
```

If you use vim, then join the modern times man. If your programming is primarily refactoring of code go with Vim. for anything else command + package editors blow modal out of the water.

A browser

```sh
  sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa
  sudo apt-get update
  sudo apt-get install firefox-trunk 
```

Setup it as the default browser.

```sh
subl /usr/share/applications/defaults.list
```

You'll see a few things. Don't be overwhelmed by these things. They're just things. You're their master. You can will them into submission.

Find the firefox entry. We need to figure out the nightly desktop so try. `ls | grep firefox` Voila 
firefox-trunk.desktop. Do a find and replace in your defaults.list. Hit save. Uh oh no permissions.

Better sudo it.

Now if you're using 1Password you'll need a modern WebKit browser to use 1Password anywhere.

```sh
sudo apt-get install chromium-browser
```

Then run it like this `chromium-browser --allow-file-access-from-files`

Now you can drag and drop your keychain file. But wait that is really fucking annoying to do everytime right? Well, you can edit your CLI arguments and when you start the app via Unity it will run them. Heres how. 


```sh
cd /usr/share/applications/ 
```

```sh 
ls | grep chromium
```

You should see something like `chromium-browser.desktop`

```sh
mkdir ~/.local/share/applications
cp /usr/share/applications/chromium-browser.desktop ~/.local/share/applications
```

```sh
cd ~/.local/share/applications
```

```sh
subl chromium-browser.desktop
```
 
CRTL + F and look for `exec` then add the argument `--allow-file-access-from-files`

It will have a few different entries. These correspond to the way the appilcation can initlized. Via the desktop, new window, and icognito mode.

explain: desktop file

Source: http://blog.thewebsitepeople.org/2010/09/working-with-freedesktop-menus/

```
- The menu “layout”, specified in .menu files (XML format).
- The menu “content”, specified in .directory (sub-category) and .desktop (item or “launcher”) files. .directory and .desktop files are both in config file format, and similar in content.

In GNOME shell, the menu is accessible under Activities.  There is only 1 menu (called Applications).  Menu sub-categories are listed on the right and menu items on the left.
```

explain: config file format
explain: config file format parsing

Lets make these edits and then test it. All working?

Lets go ahead add this to our dotfiles

```sh
cp ~/.local/share/applications/chromium-browser.desktop ~/creations/dotfiles
```

## Creating cusotm shortcut with arguments.

And done.

Download Manager

```sh
  sudo add-apt-repository ppa:upubuntu-com/flareget-amd64
  sudo apt-get update 
  sudo apt-get install flareget
```

### Skype

Skype is annoying so just visit the download page (add skype download here) and grab it. It will launcht eh software center when you open it. Install and then launch.

## Managing Passwords

I use 1Password on my iOS devises and mac. So the best option for e is to build an keychain reeder and a browser extension. But for now we utilize 1PasswordAnywhere. A bit inconvenient but an agilekeychain reader cant be too hard right?

## Apps motherfucker

Apps on Linux suck so we are going to make heavy use of the web. The Linux equivalent of Fluid is Fogger. Which I will forever accidently mispronounce as Flogger. You should to, it will make for fun discussion at those Linux parties everyone gets together at.

```sh
  sudo add-apt-repository ppa:loneowais/fogger
  sudo apt-get update 
  sudo apt-get install fogger
```

## Free as in who gives a shit about copyight

Due to assholes, dicks and washinton fatcats Ubunut cannot dsitrbute the essentials to you. Copyright isa pain and they workaround it by allowing you to install manually.

```sh
sudo apt-get update
sudo apt-get install ubuntu-restricted-extras
```

Lets get flash setup

```sh
sudo apt-get update
sudo apt-get install flashplugin-installer
```

Lets get a real media player installed. VLC stands for Very Likely Cool.

```sh
sudo apt-get update
sudo apt-get install vlc vlc-plugin-pulse mozilla-plugin-vlc
```

## Photo Editor

``sh
sudo apt-get install gimp
```

## Vector editor

Install Inkscape

``sh
sudo apt-get install inkscape
```

## 3D editor

Install Blender

```sh
sudo add-apt-repository ppa:irie/blender
sudo apt-get update
sudo apt-get install blender
```

## Smuxi

```sh
sudo add-apt-repository ppa:meebey/ppa
sudo apt-get update 
sudo apt-get install smuxi
```

## ChessApp

```sh
sudo apt-get install stockfish
sudo apt-get install pychess
```

## Privacy

Ubuntu includes shit that pushers your web search to it's servers.

https://www.eff.org/deeplinks/2012/10/privacy-ubuntu-1210-amazon-ads-and-data-leaks

```sh
sudo apt-get remove unity-lens-shopping
```

`cmd + a` then search for Privacy and disable internet searches

## Password Manager

A brief explanation on the differences between KeePass and KeePassX. KeePass is built using .net and
and KeePassX on QT. In the early days this kept KeePass Windows only but with release of Mono it can 
now be run pretty much anywhere. However, mono just isn't as integrated into the Linux ecosystem as
QT is. Don't forget QT is used by KDE. This inevitably means KeePassX is likely to be more stable
and might be a better choice if you need less features.

Unfortunately we absolutely need Firefox plugins and these only work through KeePass, because they rely on
KeePass' plugin API. 

First up we have to install mono.

```sh 
sudo apt-get install mono-runtime
sudo apt-get install mono-utils
sudo apt-get install mono-devel
sudo apt-get install mono-complete
```

Then KeePass2

```sh
sudo apt-get install keepass2
```

Now we need KeeFox after installing it will present you with a screen informing you how to setup on 
Mac + Linux. Copy the file KeePassRPC.plgx to your KeePass plugins folder:

```sh
/usr/lib/keepass2 && sudo mkdir plugins
sudo cp /home/k2052/.mozilla/firefox-trunk/yg828kme.default/extensions/keefox@chris.tomlinson/deps/KeePassRPC.plgx /usr/lib/keepass2/plugins
```
Load up keepass and make sure you see the plugin.

Change the keepass installation location to /use/lib/keepass2 in the KeeFox options. When you restart
Firefox you'll be presented with a create database screen. Follow the instructions.

Now there is one final problem to resolve. Keeping KeePass open all the time is annoying, when switching apps it's always there. Go into Options -> Interface -> and check Minimize to tray [Image Here]

To relaunch KeePass when it disappears just use unity `CMD + A` then KeePass2.

## Rar that shit

The archive manager operates on cli apps which makes it easy as enything to add extra extrators.
TO add rar support do the following:

```sh
sudo apt-get install unrar
```

And done!

## VirtualBox

Visit link here to install. Or even just CMD + A search for VitualBox and install. If you prefer terminal 
do the following:

```sh
sudo apt-get install virtualbox
```

Now for the most part you can just follow the dialgos to get setup. Once you have a vm machine we need to
get file sharing working. Boot up the machine and then got to Devices -> Install Guest Addditions.
Follow the dialogs. Now add your user to the to the vbxo group

```
sudo usermod -a -G vboxusers k2052
```

Delight:
	Use whoiam to get your username
  It may be necessary to restart Ubuntu after changing your group. Try first just restarting terminal.

## Dropbox

Run the following:

```sh
sudo apt-get install nautilus-dropbox
```

This is will install and integrate your dropbox with nautilus.
