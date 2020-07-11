## Pi Hole
Enables local network DNS that blacklists trackind, ad and malware domains.

### Install
After doing ssh on raspberry pi, run

``` bash
$ curl -sSL https://install.pi-hole.net | sudo bash
```

Additional instructions can be found here: https://github.com/pi-hole/pi-hole/#one-step-automated-install

### Setting
After installation, configure local router. Make sure raspberry always gets same IP address. When configuring DHCP, raspberry's pi IP as a DNS location.
