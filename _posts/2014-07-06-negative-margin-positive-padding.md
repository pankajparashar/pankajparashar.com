---
title: Negative Margin, Positive Padding
date: 2014-07-06 00:00:00 Z
layout: post
excerpt: Neat little trick I learned from Joshua Hibbert's website, where he has effectively
  used this technique to design backgrounds stretching infinitely on either directions
  but the content respects the width.
---

Ever wondered, how you can stretch the background of the container beyond the viewport but the content should not exceed the defined width? Well, if you are looking closely, then the answer is all over the place on this website. As I have used this technique, to design the `blockquotes`, `pre`, and `code` blocks on my website.

## Technique

The idea stems from the [box model technique](http://css-tricks.com/the-css-box-model/), in which you can apply negative margins on block child container for them to stretch outside the parent container. We use this idea of negative margins to stretch the background of the container to the entire width of the webpage. For example, `margin-left: -999em` will stretch the left margin of the box to far left, which in this case is outside the viewport (`-999em` is the magic number).

![](https://res.cloudinary.com/dw9fem4ki/image/upload/v1404648677/https_dl_kraken_io_7e3eb546529ff3421622655117b4bd51_negative-positive_s1vuna.png)

## Problem

This however, causes one problem, which also pulls the content (or the text) outside of the parent container, which destroys the alignment and flow of the content. This can be solved by using the exact same number to push the content right where it belongs with `padding-left: 999em`. This ensures that the background covers the entire left portion of the web page, but the content still respects the width.

## Variations

Interestingly, this technique not only works for fixed-width containers but also fluid width containers and also not only works horizontally (for widths) but also vertically (for heights).

- Fixed/Fluid width container  
    ```
    a. Left aligned  
    margin-left: -999em; padding-left: 999em;
    b. Right aligned  
    margin-right: -999em; padding-right: 999em;
    c. Center aligned  
    margin: 0 -999em; padding: 0 999em;
    ```

- Fixed/Fluid height container,
    ```
    a. Top aligned  
    margin-top: -999em; padding-top: 999em;
    b. Bottom aligned  
    margin-bottom: -999em; padding-bottom: 999em;
    c. Middle aligned  
    margin: -999em 0; padding: 999em 0;
    ```

## Mixins

Without a doubt, the next thing I hopped into are handy Sass-mixins to leverage this technique for your projects.

```
@mixin align($num: 999em, $dir: 'left') {
    @if $dir == 'center' {
        margin: 0 -#{$num};
        padding: 0 #{$num};
    }
    @else if $dir == 'middle' {
        margin: -#{$num} 0;
        padding: #{$num} 0;
    }
    @else {
        margin-#{$dir}: -#{$num};
        padding-#{$dir}: #{$num};
    }
}
```

.. and the usage of this mixin, for each type of variation,

```
.left-align {
    @include align($dir: 'left');
}
```