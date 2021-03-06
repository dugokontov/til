## Pi Hole
[Pi-hole](https://pi-hole.net/) is local network DNS sinkhole that blacklists tracking, ad and malware domains.

### Install
After doing ssh on raspberry pi, run

``` bash
$ curl -sSL https://install.pi-hole.net | bash
```

Source: https://github.com/pi-hole/pi-hole/#one-step-automated-install

### Setting router
After installation, configure local network router:

 * Make sure raspberry pi always gets the same IP address
 * Configuring DHCP and set raspberry's pi IP as a DNS

### Some application on the phone still show ads
This is because ads can be cached by the application. From the phone settings -> App, select your application and clear cache/data.
