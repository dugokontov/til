## Stripes using only CSS
When I had to display stripes on html page I usually created a small image and then used [background-repeat](https://developer.mozilla.org/en-US/docs/Web/CSS/background-repeat) to get that effect. I was very glad to find out that CSS now supports this out of the box using CSS function [repeating-linear-gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/repeating-linear-gradient).

### Why?
To achieve something like this:

![image](https://user-images.githubusercontent.com/1465340/89706012-70295200-d962-11ea-9306-2afc01626095.png)

or

![image](https://user-images.githubusercontent.com/1465340/89706193-a5826f80-d963-11ea-8164-151289e01e4a.png)


### How?
This can be done by setting background of HTML element, like this ([jsFiddle](https://jsfiddle.net/5ya3kmuc/)):

```css
element {
    background: repeating-linear-gradient(
        -45deg,  /* angle by which stripes are rotated */
        #fedcba, /* color1 at position 0px */
        #fedcba 40px, /* color2 at position 40px. If color1 === color2, we get one stripe with the width of 40 - 0 = 40px */
        #abcdef 40px, /* color3 at position 40px. If position of color2 is same as position of color3 we will not have gradient change */
        #abcdef 80px /* color4 at position 80px. if color 4 === color3 we get second stripe with the width of 80 - 40 = 40px*/
  )
}
```

In this case we get two stripes (`#fedcba` and `#abcdef`). Both are 40px wide. If we want to set `#abcdef` to be 20px wide, we would need to replace `80px` with `60px`.

### References
 * [css-tricks.com](https://css-tricks.com/stripes-css/)
 * [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/repeating-linear-gradient)