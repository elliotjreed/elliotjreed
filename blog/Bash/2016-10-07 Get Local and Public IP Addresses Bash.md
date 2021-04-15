# Find public IP address and local IP address on Linux via the command line

Here's a useful Bash function to find your local and public IP addresses:

```bash
function myip() {
    publicip=$(curl -s http://ipecho.net/plain)
    localip=$(ip -o -4 addr show | awk -F '[ /]+' '/global/ {print $4}')
    echo "Public IP: " $publicip
    echo "Local IP:  " $localip
}
```

Then you just need to type `myip` into your terminal / Bash shell and it'll show you your public IP address and your local IP address.

You could add this to your `.bashrc` file (eg. `nano ~/.bashrc`) so you always have it to use. Remember after adding it to the `.bashrc` file to source it to load in your changes:

```bash
. ~/.bashrc
```
or

```bash
source ~/.bashrc
```
