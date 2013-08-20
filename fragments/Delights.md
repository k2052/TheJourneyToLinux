# Nautilus

Nautilus is named after that 

# Rhythmbox

## Preferences

Preferences and plugins can be found under the edit menu.

## Lyrics. WTF is this artist saying? Cat in the catbox.

I haven't investigated as to why yet but the songlyrics plugin is broken. My guess is it uses a web api 
that lacks lyrics. It also is pretty much useless as it has no sidebar. 

You can improve your lyrics experience by installing [lLyrics](https://github.com/dmo60/lLyrics)

Add the ppa fossfreedom. Fossfreedom is a ppa (Public Project Awesome, just kidding. Real definition here).
It's a ppa with Rhythmbox plugins.

```sh
sudo add-apt-repository ppa:fossfreedom/rhythmbox-plugins
sudo apt-get update
```

Then install

```sh
sudo apt-get install rhythmbox-plugin-llyrics
```

Go enable the plugin. Click the lLyrics icon. You'll now have a sidebar for lyrics.

# Unity 

If you approach Unity like a dock you're going to hate it. It's a terrible dock, it's meant to keyboard 
driven not mouse driven. Unfortunately this is massive failure on Canonical's part in the delight department.

## Dealing with crashes

### WTF is happening

(screenshot example) insane overlay thingy

# Bugs

If you have certain themes or applications installed you may expreince the following.

```sh
Gtk-WARNING **: Unable to locate theme engine in module_path: "pixmap",
```

This can be fixed by installing `gtk2-engines-pixbuf`

Run

```sh
sudo apt-get install gtk2-engines-pixbuf
```

## Disks/Udisks

Clicking format will often just delay for a bit until and error dialog arrives. No fix. Just be aware.
Tread carefully and carry backups.

### Errors

```
Error synchronizing after initial wipe: Timed out waiting for object (udisks-error-quark, 0)
```

A known bug and unfortunately unfixed. https://bugs.launchpad.net/ubuntu/+source/udisks/+bug/1059872.
There are two solutions. 1. Format via terminal or 2. Install Gparted

Delight:
	If your disk uses gpt you will need to use Gparted not the terminal. This is likely to have occurred 
	if you clicked around in Disks trying to make things work.

	Run:

	```sh
	sudo fdisk -l
	```

	If you get back 
	```
		WARNING: GPT (GUID Partition Table) detected on '/dev/sdg1'! The util fdisk doesn't support GPT. Use GNU Parted.
	```

	Then you need to use gparted

#### Terminal

Formatting USB from the terminal requires using mkfs. I'll let them do the describing.

```
The mkfs (i.e., make filesystem) command is used to create a filesystem (i.e., a system for organizing a hierarchy of directories, subdirectories and files) on a formatted storage device or media, usually a partition on a hard disk drive (HDD).
```

Let's see what we have got by running the command.

```sh
mkfs
``

We will get back the following.

```sh
Usage: mkfs [options] [-t type fs-options] device [size]

Options:
 -t, --type=TYPE  file system type, when undefined ext2 is used
     fs-options   parameters to real file system builder
     device       path to a device
     size         number of blocks on the device
 -V, --verbose    explain what is done
                  defining -V more than once will cause a dry-run
 -V, --version    output version information and exit
                  -V as version must be only option
 -h, --help       display this help and exit

For more information see mkfs(8).
```

The filetypes that are allowed are;

- ext3. 
	A journaled file system that is commonly used by the Linux kernel. Use this if you intend to only
	use the stick on Linux systems.
- minix. 
	Minix file system. No reason to use, unless you are one of those weirdos installing strange 
	OSes for the fun of it. Don't worry, I love you anyway, but you are so god damn weird.
- msdos
	What the name says.
- vfat
	This is where things get a little convoluted. VFAT is technically an extension of FAT16 and FAT32 is 
	the next iteration. But mkfs seems to treat VFAT as FAT32. 
- xfs.
	A high performance file system originally developed for IRIX.

Delight:
	Types are mount to mkfs 'sub commands', e.g `mkfs.vfat` for FAT. We will use these for ease of use.

You need to figure out the /dev address of the usb stick.

You can run `df` if your disk is mounted. If it's not then head over to Disks Utility. 
In my case it's `/sdg`

Very Delightful:
	If your USB has multiple partitions then you need to delete them first. First enter into a sudo session

  ``sh
  sudo su
  ```

  1. Then we need to delete all the partitions on the drive.
    - `fdisk -l` will give you the drive letter
    - `fdisk /dev/driveletterhere` will open fdisk to the drive
    - `d` deletes a partition
    - `1` selects the 1st partition

    fdisk should automatically select the next partition so you just keep hitting d until all the partitions are gone. In any case repeat commands as necessary until all the partitions are gone.

	2. Finally we need to a create primary partition.
    - `n` makes a new partition
    - `p` makes the partition primary
    - `1` makes the partition the first one

    Accept the defaults and then type `w` to write the changes

We have to unmount a disk before formatting.

```sh
sudo umount /dev/driveletter
```

Then to format we do

```sh
mkfs.vfat -F 32 -n MediaUSB /dev/driveletter
```

### Cp

To copy files you can use a wildcard

```sh
cp filesfolder/* newfilesfolder/*
```

Now you may be wondering, "can cp take regular expressions?". The answer is no. But it does take
globs which are almost as powerful. If you use ruby or php chances are you have been exposed to them
at one time or another. To get details just type:

```sh
man glob
```

So how does this work? A basic example is the following:

```sh
cp cats*.jar /destination/directory
```

### Glob all the things

Did you know your shell has regex practically built in? It's in the form of globs. If you use ruby or php chances are you have been exposed to them at one time or another. Probably doing something file  Dir.glob('*.rb'). Or using
rakes Filesets. It works the same, sorta regex without all the capturing stuff. Basically globs don't
make a try at being a turing complete bastard, and just KISS it.

So what are globs useful for? Being build into the shell opens up a massive range of possibilities.
Here are a few:

Use it with ls:

```
ls *.gif  # List gifs
ls ?.gif  # List gifs with 1 char names (e.g 1.gif, a.gif)
```

Use it to remove things:

```sh
rm [A-Z]*.rar # Removes rar files
```

Use it to copy things:

```sh
cp *.jar /destination/directory # Copy all the jar files
```

You can enable more power with extended globbing: 

For bash this is:

```sh
shopt -s extglob
```

For zsh this is:

```sh
setopt kshglob
```

I'll let the man tell you what this does:

```sh
?(pattern-list)   Matches zero or one occurrence of the given patterns
*(pattern-list)   Matches zero or more occurrences of the given patterns
+(pattern-list)   Matches one or more occurrences of the given patterns
@(pattern-list)   Matches one of the given patterns
!(pattern-list)   Matches anything except one of the given patterns
```

We can use this to combine globs:

```sh
ls *+(.jpg|.md) # List jpg and md files
```

What else can this puppy do? Anything you set it do. The limit is your imagination. Maybe you
use it as a simple replacement for regex in grep or maybe you use it to build a House of Gold(fish).


#### Gparted

Install using apt

The best design is intuitive, intuition comes from experience. Know the experiences of your users.

```sh
sudo apt-get install gparted
```

We need to run as root so do:

```sh
sudo gparted
```

Find your USB drive and select it via the dropdown

[Image Here]

Delight:
	You may get a warning along the lines of:

	```sh
		/dev/sdg contains GPT signatures, indicating that it has a GPT table.  
		However, it does not have a valid fake msdos partition table, as it should. Perhaps it was corrupted 
		-- possibly by a program that doesn't understand GPT partition tables.  
		Or perhaps you deleted the GPT table, and are now using an msdos partition table.  
		Is this a GPT partition table?
  ```

  If you care about the data on the disk then you need to use gdisk and try to repair the broken partition table. But if not then just click no and continue with the process. You will overwrite the old partition table.


Delight:
	If you have no table. Do Device->Create Partition Table first

Choose Partition->New

[Image Here]

Select fat32 (this will be readable on most places; Linux, Windows and Mac). Make sure to give it a 
label you lazy human. One day when you aren't starting at four mounted volumes labeled Untitled you'll thank me. If the latter happens you'll learn a valuable lesson about listening to advice you find in ebooks
you got off the internet written by strangers.

[Image Here]

Click the checkmark icon

It will pend the changes. Be patient. Seriously, be patient, you need patience. 

## 

### Read Only Error

The first thing you need to do is determine whether or not it's Nautilus or the actual USB stick.

Do `sudo nautilus`. If you can copy files then you need to do some googling. It might be well known bug
in 12.10. 

The quick fix is:

```sh
sudo add-apt-repository ppa:webupd8team/experiments
sudo apt-get update
sudo apt-get dist-upgrade
nautilus -q
```

It will install a patche nautilus with various fixes.

To remove just do:

```sh
sudo apt-get install ppa-purge
sudo ppa-purge ppa:webupd8team/experiments
```

If you still cant copy files then it's likely a patrition issue, a filesystem check will usually resolve this. 

To check a partition do the following:

1. Open GParted

```sh
sudo gparted
```

2. Select it in the drop down

[Image here]

3. Right Click partition select Check

[Image Here]

## Terminal

Type `exit` to quit a `su` session.

# Flareget
 
Just don't. It's too buggy. Install uget instead.

do the following

```sh
sudo add-apt-repository ppa:plushuang-tw/uget-stable
sudo apt-get update
sudo apt-get install uget aria2
```

# Uget

## Scheduler

If the scheduler is enabled you must click `Force Start` to start a download.

## Settings

It wont remember some of your settings like location and number of connections per server;
so make sure to set them everytime.


## Packages

### Check what packages you have installed

Use the following

```sh
dpkg --get-selections
```

To search just pipe the result to grep:

```sh
dpkg --get-selections | grep packagename
```

For example: 

```sh
dpkg --get-selections | grep awk
```

TODO:

How the fuck does dpkg --list and less command work together?

You get back:

```sh
mawk						install
```

### Removing Packages

Use the following:

```sh
sudo apt-get remove packagename
```

For example remove vlc:
```sh
sudo apt-get remove vlc
```

What if you want to remove the config files to?

```sh
sudo apt-get --purge remove vlc
```

Delight:
	Remove takes multiple packages, e.g:

	```sh
	sudo apt-get remove vlc zsh
	```

## Get info on a package

~~~ sh
apt-cache show packagename
~~~

Example: 

~~~ sh 
apt-cache show gummi
~~~ 

If you get long output you can pipe into less

~~~ sh
apt-cache show  texlive-latex-extra | less
~~~ 

Scroll to scroll. Hit `q` to quit.

## Terminal

Change permissions.

Use numbers.

```sh 
chmod 644 filename
```

Get your group 
```sh
groups <username>
```

Defaults to your username so you can just use:

```sh 
groups
```

Change ownership:

```
sudo chown username:usergroup somefile
```

List file permissions:

```sh
ls -l
``` 

What the fuck does that `-l` do? Th answer lies in the man:

```sh
-l     use a long listing format
```

Pipe long terminal things to less:

If you get a lot of output from a command you can pipe it less and then page through it. For example:

~~~ sh
apt-cache show  texlive-latex-extra | less
~~~ 

Scroll to scroll. Hit `q` to quit.

## Man tips for your man (or woman)

Lets say you have a loved one and they have questions about cat. It's the middle of the night
and you really don't have time for that google shit. You want answers now. He is keeping you
up and god damnit cant a man research tech stuff on his own? Do they have to be so helpless when 
it comes to technology. So you pull up man and you're like "What do you want to know honey?" and
he is like "If you don't want to help thats fine I'll just google tomorrow" and you're like "Come on. Just
spit it out so we can go to bed." and he is like "what does" `cat -u` "do?"

So you go to pull up `man cat` but you're like you know what I bet I can search through this.

The first thing you try is:

```sh
man cat | grep -u
```

But you get back:

```sh
Usage: grep [OPTION]... PATTERN [FILE]...
Try 'grep --help' for more information.
```

And now you want to take your man and shove him out the window. But you push on.
What if you pass it as a string:

```sh
man cat | grep '-u'
```

Same thing. Maybe you can escape this fucker? So you try:

```sh
man cat | grep \-u
```

But no cigar. Now you have really had it so you decide to man up and man your grep for info:

```sh
man grep
```

After some scrolling you discover that you can pass a real regex to grep via `-e` so you do the following:

```sh
man cat | grep -e '-u'
```

And get back:

```sh
-u     (ignored)
```

Finally you can go to bed. You turn over to your man and he IS SLEEPING. You consider waking him up 
and teaching him lesson about tech questions in the middle of night. But you realize he cant help himself,
you smile at his adorkable ignorance and curiosity and drift off to sleep. 

Delight:
	You can scroll through man pages (actually anything paged by less). Use your mousewheel man.

## Firefox

Need to drag and drop an image to Nautilus? Hold Crtl and drag. Drag without holding crtl and you'll
save a link.

## Desktop

Ever see a strang lttl box appear in the right rand bototm corner? It looks like this

IMAGE HERE

It's Nautlius' search function. You can filter your desktop icons with it. Once you have any app 
open it will go away and you will have to focus on the desktop to get it back. (Shorcut for cfocusing on desktop here)

