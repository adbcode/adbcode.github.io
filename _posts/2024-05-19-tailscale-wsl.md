---
layout: post
title: Enable MagicDNS in WSL2 when using Tailscale on Windows 11
---

[Tailscale](https://tailscale.com/) is a great service for setting up a private network.
It makes it easy to access other devices you own using custom hostnames with [MagicDNS](https://tailscale.com/kb/1081/magicdns).
Moreoever, if you use SSH to access your devices, you can also use [Tailscale SSH](https://tailscale.com/kb/1193/tailscale-ssh) to login without remembering passwords.

All of this is made possible with some networking "magic", which also makes it challenging when dealing with niche situations.
One being what to do to make use of the above services inside a WSL instance when using Tailscale on a Windows machine.

It is possible to install Tailscale client inside the WSL instance. However, it is less than ideal for performance and other reasons.
Luckily, the Windows client should be able to route WSL's network traffic through itself. Sadly though, the DNS configuration needed to make MagicDNS does not work out of the box.

To fix this, we need to configure the DNS configuration inside the WSL instance. Specifically:
- Disable WSL's automatic DNS configuration.
- Set up DNS name server and search domain manually.

Here are the instructions for Ubuntu instances, which should also work for other Debian-based variants.

To disable WSL from autoconfigure DNS:
- Open the WSL configuration file using a text editor: `sudo nano /etc/wsl.conf` and add the following to the bottom of the file:
```
[network]
generateResolvConf=false
```
- Now edit the default DNS configuration file (`sudo nano /etc/resolv.conf`) and replace its contents with the following:
```
nameserver <your-magicdns-ip/100.100.100.100>
search <your-tailnet-name>
```

To find the search domain, open the [DNS page](https://login.tailscale.com/admin/dns) and find the value under `Tailnet name`:
![image](/images/331882888-e337eddb-2281-4f4b-8e0b-235abf15c81e.png)

You can also confirm the MagicDNS server address in the same page (default being `100.100.100.100`). With these changes, you should now be able to use MagicDNS as usual.
