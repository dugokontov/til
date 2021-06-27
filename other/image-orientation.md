## Image orientation

When photos are taken/scanned, device taking the photo usually stores additional metadata about that image. One format for that metadata is [Exif](https://en.wikipedia.org/wiki/Exif) (Exchangeable image file format). That format defines one property that is called "orientation". It is a number value (with value between 1-8), telling at what position device took the photo.

For convenience, here is what the letter F would look like if it were tagged correctly and displayed by a program that ignores the orientation tag (thus showing the stored image): ([source](http://sylvana.net/jpegcrop/exif_orientation.html))
```
      1        2       3      4         5            6           7          8

    888888  888888      88  88      8888888888  88                  88  8888888888
    88          88      88  88      88  88      88  88          88  88      88  88
    8888      8888    8888  8888    88          8888888888  8888888888          88
    88          88      88  88
    88          88  888888  888888
```

![images](https://www.impulseadventure.com/photo/images/orient_flag2.gif)

### References

 * https://www.impulseadventure.com/photo/exif-orientation.html
 * http://sylvana.net/jpegcrop/exif_orientation.html
 * https://en.wikipedia.org/wiki/Exif
