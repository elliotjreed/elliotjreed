# Fix for Apt update failing on Debian Buster / Raspberry Pi's Raspbian

There are two main reasons why you might experience the following error when running `sudo apt-get update` or `sudo apt update`.

```bash
Get:1 http://archive.raspberrypi.org/debian buster InRelease [25.1 kB]
Get:2 http://raspbian.raspberrypi.org/raspbian buster InRelease [15.0 kB]
Reading package lists... Done
E: Release file for http://archive.raspberrypi.org/debian/dists/buster/InRelease is not valid yet (invalid for another 1d 1h 1min 1s). Updates for this repository will not be applied.
E: Release file for http://raspbian.raspberrypi.org/raspbian/dists/buster/InRelease is not valid yet (invalid for another 1d 1h 1min 1s). Updates for this repository will not be applied.
```

The first is that the release information for the repository has changed, in which case you can simply run the following to accept the changes:

```bash
sudo apt-get update --allow-releaseinfo-change
```

The second potential issue is that your system clock may be wrong. To check the system time, just run:

```bash
date
```

If it is incorrect I would recommend ensuring your timezone is set correctly by changing the option via `raspi-config`:

```bash
sudo raspi-config
```

Then setting the date manually if it's still incorrect after running `date` again:

```bash
sudo date -s "Tue Jul 16 08:22:11 BST 2019"
```

Try updating again (`sudo apt-get update`). If it works I would recommend installing the Network Time Protocol (NTP) so manage the time automatically over the Internet:

```bash
sudo apt-get install ntp
```
