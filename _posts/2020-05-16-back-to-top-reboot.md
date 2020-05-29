---
title: Back to Top reboot
layout: post
excerpt: The "Back To Top" button has been a holy grail along with the hamburger menu.
  Historically, jQuery was used to animate the jump from the bottom of the page to
  the top of the page. Is there a modern day equivalent?
---

This is the previous implementation,

```
<a role="button" 
   onClick="window.scrollTo(0,0)" 
   href="javascript:void(0)">Back to Top</a>
```

Recently I [learned ](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView)about the `Element.scrollIntoView` method that scrolls the elements parent container such that the element is visible to user in the viewport. Optionally, we can also define behavior to scroll smoothly.

Hence, here is my [updated version ](https://github.com/pankajparashar/pankajparashar.com/commit/f4b50e3d71b42ec83cdb7d259c46035196b1af19)of the famous Back to Top button,

```
<a role="button" 
   onClick="document.body.scrollIntoView({behavior:'smooth'})" 
   href="javascript:void(0)">Back to Top</a>
```

[CanIUse](https://caniuse.com/#feat=scrollintoview) suggests that all modern browsers support the `scrollIntoView` API, which is cool too.