## Create an image of USB stick

### Why?
Useful if you have installed and configured raspberry pi, and don't want to go through that process if something happens to it.

### How?

1. Insert USB stick/memory card
2. Run `dmesg | grep -i attached` to see where is the new removable disk attached (`sda`, `sdb`, `sdc`, etc.)
3. Create image using `dd`

```sh
sudo dd if=/dev/sdc of=/home/user/name-of-the-image.img bs=4M status=progress
```

Param explanation:
 - `if`: input file, from where to create image
 - `of`: output file, where to store this image
 - `bs`: block size
 - `status`: show progress. This param requires `dd` version >=8.24
