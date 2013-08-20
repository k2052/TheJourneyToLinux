Just like with any move before we can move onto to something new we need to organize the things we have accumulated in boxes over the years. File systesmare junk colelcotrs. We leave things trewn across folders.

Moving encourages us to throw out the ld and re-organize by necessity. We have to simplify the chaos or we cant move. To move youhave to cut yourself free of weights that hold you down and slow you down.

What do we do withse remants? There is a inclination to just throw it all onto an external HD and start over. But this just pushes the problem for later. We want a fresh start, an experience as free of old crap as possible. Anything we take with us needs to organized or it must go.

# Sturcting files

I can break down what is on my HDs into three categories; 

1. My creations.
2. Things others have created.
3. The tools I use to create and consume other creations.

Keeping all my apps, configurations, libraries etc organized is simple, I just follow the standards. Dot files for configruation, apps go in Applications, binaries in a bin directory /etc for more config etc. Where it gets complicated is with the data. The creations of others are hard enough. It's no wonder we just shove things into iTunes and let iTunes handle it. I'm tired of letting iTunes just handle it. Every go into your music folder? It's a mess. Without iTunes DB you'd never be able to manage it. 

I need some rules and standards to adopt. First let's set some further constraints, where does my data go? I think it's silly that we put data from other humans in our home directories. So no more iTunes music in /k2052/Music. That folder should be for music I make. The emptiness of said folder will be a constant reminder of my lack of music creations. 

Where to put it then? How about /media? There is a problem with this though, Ubuntu likes to mount external media to that directory. But wait, that is actually a good thing, the entertainment I consume is consumed on many devices. Even if I don't store my entertainment true external drive I want it to feel like one. Media locally stored in /media/local. 

1. `/media/local/music`  for music
2. `/media/local/books`  for books
3. `/media/local/videos` for videos

Any management apps we use we can just point to `/media/local` and voila workingness.

Actually lets go with .local in our home directory. Which is ~/.local.

a single directory where user data is stored, defaulting to ~/.local/share;
a single directory where configuration is stored, defaulting to ~/.config;
a single directory which holds non-essiential data files, defaulting to ~/.cache.

We can just directly mirror this to a external HD.

Do we organzie things by type? By the apps we use to create them?  Apps are sily the apps you use change. Don't keep your mail in a mail app. Keep them in a folder in /creations/emails. Finish one? Move to /archived/creations/emails.

So what do we do with the things we create? With our user directory free of others creations I think it becomes clear. We place everything we do immediately in our home directory in /creations it's own separate folder for each creation. Most of us know how to keep our creations organized. 

Organizing by type is silly we end up with documents for multiple projects in the same folder. Our hit lists touch our emails.

The only thing we need to worry about then is state. I think it's silly to have a separate dumping spot for stuff. We have /Downloads and /tmp. Use them god damnit. Stop filling up your desktop. It's meant for icons.
Will add one folder /archived. We store anything that is done here.

## On organizing those creations.

Most things we need for our creations are resources don't spread this stuff across Downloads and Desktop put it in the folder for the project in /resources. Got inspiration? Put it /inspirations.

# Get thos dotfiles to github.

Come on man all the cool kids are doing it.

# Get those .ssh keys

You're going to need these baby.
  I keep this is Library/Application Support/privatedotfiles

  xclip -sel clip < ~/.ssh/id_rsa.pub
