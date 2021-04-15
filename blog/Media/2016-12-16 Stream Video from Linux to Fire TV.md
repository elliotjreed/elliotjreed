# Play videos on your Linux desktop on your Fire TV / Fire TV Stick

All you need on the Fire TV is the VLC app, which is free and generally really good. Just go to 'Apps' on your Fire TV and install VLC, we'll come back to this after the Linux setup.


## ReadyMedia Setup

For this we'll use a lightweight media server, running as your normal user.

ReadyMedia, formerly known as MiniDLNA, is my personal choice here as it's simple, light on system resources, in most Linux distributions' official repositories, and is fully compliant with DLNA/UPnP clients, meaning you could stream to your smart TV, PS3, XBox, smartphone, and other computers.


### Installation

To install it on Arch Linux run:

```bash
sudo pacman -S minidlna
```

To install it on Ubuntu / Mint:

```bash
sudo apt install minidlna
```


### Configuration

The default option is to run minidlna as it's own user, this probably isn't what you want if you want to serve your own media files in your home directory (or Videos directory for example). What we'll do then is run it as your user (eg. elliot).

Run the following to copy over the configuration file and set the appropriate permissions:

```bash
install -Dm644 /etc/minidlna.conf ~/.config/minidlna/minidlna.conf
```

Next we need to edit a few lines in the `~/.config/minidlna/minidlna.conf` file.

Find the line beginning with `media_dir=`, uncomment it (remove the `#` symbol) if it is commented-out and add the following, changing the directory as required (here I have used my 'Videos' directory):

```bash
media_dir=/home/$USER/Videos
```

Tip: In `nano` you can use the keys [Ctrl]+[w] to find a string of text (eg. to find `media_dir=`).
Tip: In `vim` you can search using `/media_dir=`

Find the line beginning with `db_dir=`, uncomment it (remove the `#` symbol) if it is commented-out and add the following:

```bash
db_dir=/home/$USER/.config/minidlna/cache
```

Find the line beginning with `log_dir=`, uncomment it (remove the `#` symbol) if it is commented-out and add the following:

```bash
log_dir=/home/$USER/.config/minidlna
```


### Running

To run your ReadyMedia server, run the following (note the 'd' on the end of `minidlnad`):

```bash
minidlnad -f /home/$USER/.config/minidlna/minidlna.conf -P /home/$USER/.config/minidlna/minidlna.pid -R
```

Now go to the VLC app on your Fire TV, go to 'Local Network' and you should see your PC listed there, open it up and you should see your videos there.

You will need to run the same command as before after restarting your PC, alternatively you can add the following to your `.bash_profile` to run it at login:

```bash
nano ~/.bash_profile
```

And add in the following:

```bash
minidlnad -f /home/$USER/.config/minidlna/minidlna.conf -P /home/$USER/.config/minidlna/minidlna.pid
```


### Firewall

If you're not seeing anything show up on VLC on the Fire TV, the most likely problem is your firewall.


#### UFW

If you're running the Uncomplicated Firewall (UFW) then the following should work (change 192.168.0.0 to 192.168.1.0 if necessary):

```bash
sudo ufw allow from 192.168.0.0/24 to any port 8200/tcp
sudo ufw allow from 127.0.0.0/24 to any port 8200/tcp
sudo ufw allow from 192.168.0.0/24 to any port 1900/udp
sudo ufw allow from 127.0.0.0/24 to any port 1900/udp
```


#### IPTables

The following should work for IPTables depending on your other rules (change 192.168.0.0 to 192.168.1.0 if necessary):

```bash
sudo iptables -A INPUT -p tcp --dport 8200 -s 192.168.0.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8200 -s 127.0.0.0/8 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8200 -j DROP
sudo iptables -A INPUT -p udp --dport 1900 -s 192.168.0.0/24 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 1900 -s 127.0.0.0/8 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 1900 -j DROP
```

You will need to save your IPTables rules for this to be persistent on restart.

If it still doesn't work after adding the IPTables rules above, check it isn't IPTables causing the issue by temporarily disabling it and checking VLC again:

```bash
sudo systemctl stop iptables.service
```

To learn more about IPTables, there are several guides online. Here's a good starting point: http://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/

If it's still not working after you have established it is not a firewall issue, you can check for errors by running:

```bash
minidlnad -f /home/$USER/.config/minidlna/minidlna.conf -P /home/$USER/.config/minidlna/minidlna.pid -d
```

You might also need to run the command as root if the above attempts don't work:

```bash
sudo minidlnad -f /home/$USER/.config/minidlna/minidlna.conf -P /home/$USER/.config/minidlna/minidlna.pid
```

With the `-d` flag meaning `debug`. For example, a directory might not have the necessary permissions (potentially likely if you have listed a directory outside of your home directory), or another media server may be running on the same ports (potentially likely if you have tried other servers before ReadyMedia).
