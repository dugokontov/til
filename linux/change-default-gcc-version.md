## Change default gcc version on Ubuntu

### Why?
Some code requires different version of gcc to be run. And sometimes installation of Linux packages require default version (gcc 9) in order to be installed on Ubuntu 20.04. One that example is [virtualbox](https://askubuntu.com/a/844547/384301).

### How?

Use [`update-alternatives`](https://github.com/tldr-pages/tldr/blob/main/pages/linux/update-alternatives.md). It is a tool used to create symlinks. Use it to create a symlink for a `gcc` to point to the particular gcc version.

```sh
# remove current symlink
sudo update-alternatives --remove-all gcc
# add symlink to the version 9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-9
```
