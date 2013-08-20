The first thing we are going to do is cd into and create the folder

```sh
cd ~/.local/share
```

```sh
mkdir media
```

Now open a file browser

```sh
xdg-open . 
```

Delight:
	Add a shortcut to your folder by hitting CRTL + D in the folder. A bookmark will be added

Copy in your music....

Now if you're like and have allowed iTunes to manage your music some things may have gone horribly wrong.
iTunes is like fuck this I don't care about folders, organiation is what databses are for. 

There is any easy way to correct this. You might think you need a crazy ass music application which a bunch of bells 
and reindeer (who wants whistsles, give me reindeer or give me horse power). Like most things in Linux the app for processing mp3 tags is scriptable. First let's grab it.

```sh
sudo apt-get install easytag
```
 
Open EasyTag. Scan the music dir by selecting it.

Wait...


CRTL + A select all the files.

Then go to the file menu Hit Rename files and Directory.

Now click the little + folder icon

`~/.local/share/media/music/%a/%b/%n-%t`

This will rename the file to /album/artist/track_num-track.ext.

Hit the icon to scan the tags. Delight: the icon will dash out like this [image]() and you'll see the 
indicators in the file log. Don't click anything until the indicator on the icon changes and it says "All files have scanned" in the log; or it will try to re-scan. 

Then go back to the file view and CRTL + S. It will prompt once about tags and then about the renames.
Select repeat and close both times.

Delight:
  You may some files that failed to be renamed.

You're done!

Now let us go back to our media player, Rhythm. And change media settings. We want this to match the rename structure we just used.

Delight:
If you have an enormouse collection click away from music and onto Podcasts ASAP. The music app will handle the size
just fine but GTK list views cant handle appending that many items that quickly and it will cause Ubuntu error dialogs.

Let us seek out the config settings for our Rhythm.

Let us now take that file and put it into our dotfiles

# Maid

Have you ever used Hazel? Not that the popular nut spread that makes people nuts, or that hot redhead
that burned all your stuff in the front lawn after you slept with her sister and ate all the nutella, no that popular 
Mac scripting app. Hazel is known for helping you keep your downloads organized not her insane antics on the lawn.

Chances are you have downloaded some media already and had to mnually move it to the correct folders. Maid is
the open source developer friendly version of hazel. Lets get it setup. (Follow the ruby dev guide first)

First the gem

```sh
gem install maid
```

Delight:
	Set your rbenv to 1.9.3. Most gems are still developed against 1.9.3 and anything you run locally 
	is just best ran against 1.9.3. Use 2.0 when you are ready to mess around. Otherwise just 1.9.3, you don't need
	to worry about any performance improvements of 2.0 when you are just using things locally.


Delight:
	Don't run this with sudo as the repo docs suggest. It'll break with systems gems. If you'd like to 
	uinstall via system ruby you can first do `sudo apt-get install ruby-dev`. Then `sudo gem install maid` should run.

Lets generate a sample so we can get an idea how it works.

```sh
maid sample
```

Delight:
  You might see something like this (I did). 

	```sh
		zsh: correct 'maid' to 'amidi' [nyae]? n
		zsh: command not found: maid
	```

	It cant find the command. Which translates to it cant find the binary. Let's echo our path to see 
	if we have the sheell pointing to the bin for rubygems.

	```sh
	echo $PATH
	```

	```sh
	/usr/local/heroku/bin:/home/k2052/.rbenv/shims:/home/k2052/.rbenv/bin:/usr/lib/lightdm/lightdm:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
	```

  hmm nothing that leaves me to think we have our gems. Let's see where rubygems installs gems.

  ```sh
  gem env
  ```

  I got something like this back.
  ```sh
  - EXECUTABLE DIRECTORY: /home/k2052/.rbenv/versions/1.9.3-p392/bin
  ```

  That is not in our path. We could just `subl .zshrc` and manually add it but we wouldn't learn anything 
  about why it's not in our path already. Let's eliminate future fustration and learn why. 

  First off we need to know a little about how bins work in rubygems. The binaries in a gem are never called directly.
  (This is why you don't see whole bunch of gem bin folders in your paths). They're wrapped in what are ccllaed binstubs. Binstubs load the env (usually just rubygems) and then pass of the ahndling to the relevant gem.

  Everytime you install a gem it gets a binstub added to the folder. It looks a little this
   
  (edit this to be unique example)
  ```ruby
  #!/usr/bin/env ruby
	require 'rubygems'

	# Prepares the $LOAD_PATH by adding to it lib directories of the gem and
	# its dependencies:
	gem 'rspec-core'

	# Loads the original executable
	load Gem.bin_path('rspec-core', 'rspec')
  ```

  That means only the path to your rubygems binstubs folder ends up on your path. So then why isn't our 
  rubygems for bin path on our path? The reason is that rbenv does it's own stubbing/shimming on top of rubygems.
  These stubs are placed in ~/.rbenv/shims/ which is on our path (or it should be). THe purpose of these
  shims is to make sure the correct ruby version is ran.

	An example of a shim is:

	```sh
	#!/usr/bin/env bash
	export RBENV_ROOT="$HOME/.rbenv"
	exec rbenv exec "$(basename "$0")" "$@"
	```

	This routes everything through `rbenv exec .` which will dtermine the correct version of ruby to load.
	This btw is the same purpose of `bundle exec` to make sure the correct environment is loaded.
  
  Well then why aren't things working? The answer is the shim for maid has not been made. Pun made by accident, 
  the power of serendipity. Serendipity, the force that maketh puns.

  Run 
  ```sh
  rbenv rehash
  ```

  it will generate and add your shims. Enjoy.

	Further Delight: 
	  ### Automated shimming

	  There is a defacto gem  available that will automate the process of shimming for you.
	  [rbenv-rehash](https://github.com/sstephenson/rbenv-gem-rehash). No more rehash! Now you'll have more time 
	  for your hash.

	  ### Bundler-generated binstubs

	  Ever get tired of running stuff like `bundle exec rspec`? I'm sure you have. You probably have
	  a zillions aliases to make your life easier. Or maybe you have installed a gem to detect bundler and execute 
	  `bundle exec` automatically. I bet you didn't know bundler can remove the need itself.

	  Run the following and you'll get generate bin stups
    
    ```sh
		bundle install --binstubs
		```

		This creates, for example, ./bin/rspec.	You can run the gems directly by doing bin/gemname, e.g 
		`bin/rspec`. Enjoy the delight!

So lets take a look at what we have now. Maid will have a made a file in `~/.maid/rules.sample.rb`.

It should have actually told you this already. 

```sh
Sample rules created at "/home/k2052/.maid/rules.sample.rb"
```

Pay attention young pandawan! (A pandawan is a panda in training to become a Jedi, pandas can become 
jedi to).


Lets open it up.

```sh
subl ~/.maid/rules.sample.rb
```

Note the comments about the need to only use this as a template. If you use something other than cron
to run the default tasks or if you cron is too frequent they could delete things you don't want deleted.

Lets modify the first one:

```ruby
rule 'Linux ISOs, etc' do
  trash(dir('~/Downloads/*.iso'))
end
```

to be a bit more reasonable. First, instead of passing an array of files to trash lets loop through each file and check if it is old.

```ruby
rule 'Linux ISOs, etc' do
  dir('~/Downloads/*.iso').each do |path|
    trash(path) if 1.week.since?(accessed_at(path))
  end
end
```
Simple huh? What we really need is a rule that moves are mp3s into the right folder so let's create one:

```ruby
rule 'MP3s likely to be music' do
  dir('~/Downloads/*.mp3').each do |path|
    move(path, '~/.local/share/media/music/')
  end
end
```

Delight:
	Make sure you put your rules inside:

	```ruby
	Maid.rules do
	end
	```

Delight:  
	Run your crontabs spread out enough that you don't copy broken mp3 files. Depending on your download manager you can also prevent this by configuring the `fragement` file extension. Using Flareget should 
	prevent any issues.

Rename the file rules.rb and then run:

```sh
maid clean --dry-run
```

This will tell if your rules are ready to go. If you get errors stop and check your file. 
Beyond this there is not much to constructing rules. Read the docs! 

So how do we schedule this stuff? We could use cron but cron is absolutely the ugliest thing 
ever invented for scheduling. Lets use something more human to write our scheduled tasks.

Luckily theres a gem for that [https://github.com/javan/whenever](https://github.com/javan/whenever)
Install it `gem install whenever`.


Lets construct a schedule file.

```ruby 
require 'whenever'
every 12.hours do
  command "maid clean --silent"
end
```

Save as ~/schedule.rb. Now run:

```sh
whenever -f schedule.rb
```

Delight:
	Don't forget to run `rbenv rehash`

You'll get some cron stuff back that will look like:

```sh
0 0,12 * * * /bin/bash -l -c 'maid clean --silent'

## [message] Above is your schedule file converted to cron syntax; your crontab file was not updated.
## [message] Run `whenever --help' for more options.
```


So how do we get this into cron then? Cron actually isn't all that
complicate. It's essentially some files with commands that run at scheduled times a daemon (crond) that
is always on.

First lets see if we have a daemon running:

```sh
ps aux | grep crond
```

You should get one line back that looks like:

```sh
k2052    22687  0.0  0.0  13580   936 pts/2    S+   00:34   0:00 grep crond
```

Cron stores its instructions in a crontab file. Lets take a look.

```sh
cat /etc/crontab
```

Output:

```sh
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
0 0,12 * * * /bin/bash -l -c 'maid clean --silent'
#
```
We can add our commands from running `whenever -f schedule.rb` to the `# m h dom mon dow user	command`. So it looks like this:

```sh
# other stuff

# m h dom mon dow user	command
## Other commands
0 0,12 * * * /bin/bash -l -c 'maid clean --silent'
#
```

That should be it.

