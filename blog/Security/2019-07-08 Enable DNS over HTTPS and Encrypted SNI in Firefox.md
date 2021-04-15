# Enable DNS over HTTPS (DoH) and Encrypted SNI (ESNI) on Firefox

You can check your current browser security settings by visiting: [www.cloudflare.com/ssl/encrypted-sni/](https://www.cloudflare.com/ssl/encrypted-sni/).

You will likely see a warning for at least "Secure DNS" and "Encrypted SNI". Here's how to solve those on Firefox.

## DNS over HTTPS

DNS (Domain Name Server) lookups are what translates a user-friendly domain name (eg. www.elliotjreed.com) into an IP address (eg. 178.62.70.70).

Normally DNS lookups are sent over plaintext, meaning that your internet service provider and anyone listening on the Internet can find out what sites you're visiting (eg. who you are banking with, or what medical issues you might be researching).

Firefox now has encrypted DNS over HTTPS support, but this isn't always enabled by default (eg. the UK Government has requested that it is not enabled, as it restricts their ability to block websites of their choosing).

You can enable DNS over HTTPS in Firefox by going to `Preferences` > `General` > `Network Settings` > `Enable DNS over HTTPS`.

![Screenshot of Firefix network settings](https://res.cloudinary.com/elliotjreed/image/upload/f_auto,q_auto/v1556365578/blog/firefox-dns-over-https.png "Screenshot of Firefix network settings")

## Encrypted SNI

When your browser establishes a connection over TLS (eg. HTTPS), the Server Name Indication (SNI) exposes the domain name you are connecting to. You can enable encrypted SNI in Firefox by typing the following into your address bas:

```
about:config
```

Then accept the warning message Firefox will display. Then search for the following:

```
network.security.esni.enabled
```

Double-click on this to change the value to `true`.

This is a bit more 'experimental' than the DNS over HTTPS above, and may cause Firefox to show a security message or simply not establish a connection on some websites.
