---
title: Look Ma! No Media Queries
layout: post
excerpt: If you look under the hood of this website, you will not find any Media
  queries. That doesn't mean this website isn't responsive. It means it is practically
  possible to design modern websites with no media queries. Continue reading this article
  to know how.
---

I'll focus on two basic elements that changes as screen size changes which typically warrants
using media queries in CSS.

## Font-size

I use the `clamp()` function to scale the font-size between a minimum and a maximum value
as the screen size changes with respect to the viewport. More on CSS clamps [here](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp).

```
body {
    font-size: clamp(18px, 1.25vmax, 1.5em);
}
```

## Layout

The site layout is designed using CSS Grids wherein we can use the `repeat()` function to specify the column template for the grid. `repeat` accepts `auto-fit` as one of the keywords to define how the grid should behave to fill the cells in the grid container.

For example,
```
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
}
```

[Click here](https://www.w3.org/TR/css-grid-1/#valdef-repeat-auto-fit) to know more about the `auto-fit` and its sibling `auto-fill` keywords.