On the Mac I use Little Snitch and it's wonderful. Absolutely wonderful. Lets replicate that experience
on Ubuntu. Our setup is going to involve two bits, which makes a terrible cookie, but a very simple firewall.
Like many security setups there is a cli app and a Gui. The firewall on Ubuntu is called UFW, it uses iptables internally. Completely unrelated to Ipee Tablets, a brand of diuretic. The GUI for UFW is called GUFW.

Before we go installing things lets check the current status of UFW.

``sh
 sudo ufw status verbose
```

You will probably get back:

```sh
Status: inactive
```

But you might get back:

```sh
response for enabled ufw here
```

According to the documentation. By default UFW will deny most `incoming` (with a few exceptions for the critical services) and allow most
of the normal outgoing things.

Lets take a look at the rules we

Delight: 

Make sure you aren't doing anything on the interwebs before starting this process. Pause all downloads,
stop email syncing and don't post any tweets saying "Setting up UFW using the guide from this great book called JourneyToLinux".

To enable UFW we execute the following in our inputy device:

```sh
sudo ufw enable
```

We should get the following:

```sh
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing)
New profiles: skip
```

All this is quite easy.

Lets utlize ome other features of linux to check things. We can check all outgoing connections by using lsof. lsof = `list open files`, one of those vary obvious ancronyms like CATS = Complete Assholes Takes Stuff or
GOV = Got Only Vaccines. 

```sh
sudo lsof -i -P -n
```

Might want to alias to something that makes sense and add to your .zshrc file

```sh
alias checkopenconnections="lsof -i -P -n"
```

But K, this is fucking silly if I want to keep an eye on open network connections. Isn't there like 
an app indicator for this stuff? No but you can one based on

```sh
netstat -lnptu
```

Incoming blocking is pretty good and might be all you need. But I recommend this by default:

Deny by default:

```sh
sudo ufw default deny incoming
sudo ufw default deny outgoing
```

Lets check this is working by opening our browser to your favorite cats photo blog:

```sh
firefox-trunk
```

Now ufw is pretty smart kinda like that Doctor Who fellow, life the universe and everything and bow ties?
Einstein ain't got not nothing on him. UFW will utlize services listed in /etc/services.

Delight:
	Don't cat /etc/services. It's too big for a cat to handle you'll need to scroll through it in a text editor. 

	For your convenience here is a command to open it in Sublime:

	```sh
	subl /etc/services
	```

Delight:
	Rules are often stored in `/lib/ufw/user.rules`

Lets allow our browsers. Because man, browsers are cool. A browser once saved an African village from
a vicious attack of blueballs. Tru story bro.

```sh
sudo ufw allow http
sudo ufw allow https
```

Lets make sure things are working:

```sh
sudo ufw status
```

We should get the following back:

```sh

```

Delight:

I tried this

```sh
sudo ufw status numbered
```
And got back:

```sh
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 80                         ALLOW IN    Anywhere
[ 2] 443                        ALLOW IN    Anywhere
[ 3] 80                         ALLOW IN    Anywhere (v6)
[ 4] 443                        ALLOW IN    Anywhere (v6)
```

WTF!

Lesson learned

 Always always download from trusted sources. If you cant
trust the source use hashes from a source you do trust. If that is not possible then don't fucking download.


Recommend you add via GUFW.


