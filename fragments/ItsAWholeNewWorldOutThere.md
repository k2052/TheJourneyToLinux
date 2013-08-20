Run That software updater.

ls -as is absolutely crucial. (explain waht the arugment s do)


# Shortcuts

Command becomes Crtl. Command (super) is a widow into the unity command bar. Press and hold command to get a quikc tps window with shortcuts.

Commands for an app.

Tap on Alt and you'll get the awesomness that is the HUD.

CRTL + ATL + Arrow Key to switch between spaces.

Shift is the modifier for plain text. CRTTL + Shift + V


# WTF Are these things

`~/.local/share/applications/` like application support?

CMD + W zoom on out

xdg-open . to open file browser in current directory


CRTL+H show hidden files.

CRTL + L clears terminal

Clipboard pickings.

`sudo apt-get install xclip`

Just like pboard a ppaste

# It's gonna be sublime

Sublime stores its settings in ~/.config/sublime-text-2

Now the user settings are actually Package in /Packages/User. Like a pertpetual mover Sublime needs to work on many place so it treats all it's stuff as Packages. Packages are just a place for its stuff. 

```sh
cd ~/.config/sublime-text-2/Packages/User
```

You want your your Preferences.sublime-settings file.

Lets do a cp command to get that into our dotfiles repo. 

`cp ~/.config/sublime-text-2/Packages/User/Preferences.sublime-settings ~/creations/dotfiles/Preferences.sublime-settings`

Quicktip:

You can leave off the filename at it will just put the file in the directory for you

If you don't have a repo for your dotfiles then shame on you. shame shame shame

## Now lets get in Package Control

Hit CRTL + ```


```python
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print('Please restart Sublime Text to finish installation')
```

Relaunch Sublime.

# Sublime hiccups

## Columns

|      Command             |         Shortuct           |
| -----------------------  | -------------------------- |
| Draw Selection           | Right Mouse Button + Shift | 
| Add to selection:        | Ctrl                       |
| Subtract from selection: | Alt                        |


# Lots of things are .config

Flareget dotfiles

# Window Manager

Drag to the top to get windows to go full screen (shortcut here)

Alt + Middle Click to resize windows. Delight: Only works when not full screen


CRTL + ALT + 0. should minimize windows but it might not. if this is the case then go to 
System Settings -> Keyboard -> Shortcuts -> Windows and set it. I don't know why it didn't work for me off the bat
I would guess something to do with my Apple keyboard.

I rebounded my keys to:

Actually I have no fucking idea what I bounded my keys. Let's find out together.

After some quick googling. I have determined that Ubuntu stores configs in dconf.

Dconifg is a simple key value data store that looks like this 

[path]
key = value

I'm sure you've seen the config format before it's very common. 

dconfig itself comes with a few different commands to work with the data.

[output from dconfig help here]

Unfortunately they aren't very good.
They're minimal and to read a key you need to know the whole path and it's dumping has to be fro the directory of the
conf. All very annoying stuff. Thanfully though there is gsettings which is much more powerful and somehow aware 
of all our configs. Will come back to figuring out how it is aware of the config in a moment. But judging by the Dconfig documentation. You mount dconfig up and then it can communicate with things through a dbus. So somewhere someplace in the system dconfig is told how to do it's stuff and then everything else just goes through it. I think.

Anyways back to this `gsettings` thing.

Running `$ gsettings` gives us a good idea how to work with it.

```sh
Usage:
  gsettings [--schemadir SCHEMADIR] COMMAND [ARGS...]

Commands:
  help                      Show this information
  list-schemas              List installed schemas
  list-relocatable-schemas  List relocatable schemas
  list-keys                 List keys in a schema
  list-children             List children of a schema
  list-recursively          List keys and values, recursively
  range                     Queries the range of a key
  get                       Get the value of a key
  set                       Set the value of a key
  reset                     Reset the value of a key
  reset-recursively         Reset all values in a given schema
  writable                  Check if a key is writable
  monitor                   Watch for changes

Use 'gsettings help COMMAND' to get detailed help.
```

Let's run `gsettings list-schemas`

Hmm holy shit. I think will need to grep this. Let's try something like `window`

```sh
gsettings list-schemas | grep window
```

Nope. Let's try `key`

```sh
gsettings list-schemas | grep key
```

Voila! Behold the power of gsettings! Before we get to excite though let's check that 
`org.gnome.desktop.wm.keybindings` actually holds our keys

```sh
gsettings list-keys org.gnome.desktop.wm.keybindings
```

We can hopefully get a friendly print using `list-recursively`

```sh
gsettings list-recursively org.gnome.desktop.wm.keybindings
```

And it does.

So my keybindings for minimizing + maximing are:

|  Command    |        Shortcut         |
| ----------- | ----------------------- | 
| maximize    | ['<Primary>Up']         |
| minimize    | ['<Primary><Alt>Down']  |
| unmaximize  | ['<Primary>Down']       |

There you go.

Now what about this dconfig thing? How the hell does it know what it knows? And how the hell does gsettings know?

Let's try to find out. 

Before we move on there is one final thing we should do. Dump our config files into dotfiles.
One we want our shortcuts again, that day will come. It might not be today, it might not be tomorrow but it will come.
Like an approaching winter, it will arrive with warning and signs, but you'll ignore them until it's too late and the 
walkers are upon your sons and daughters, your houses burn and the wall is all but broken. Heed the warnings.

# Better windows

On mac I love Divvy. What can I use on Linux to replicate it?
