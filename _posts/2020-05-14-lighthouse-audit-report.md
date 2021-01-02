---
title: Lighthouse Audit Report
date: 2020-05-14 18:30:00 Z
layout: post
excerpt: I deployed the latest version of my website (v9.0) and was quite keen to
  run the Lighthouse Audit report. Admittedly this is such a small website that getting
  scores close to 100% is quite easy and not as daunting as a modern web app.
---

Scored 100! in pretty much all the sections, except the Performance index. I pretty much have a good grip on what needs to be done to get it as close as to 100. Watch out the permalink at the bottom of this page, which links to the latest audit report from Lighthouse.

![](https://res.cloudinary.com/dw9fem4ki/image/upload/v1589702606/Capture_xplsdy.png)

## Improvements

* Replace Google fonts with a Base 64 locally hosted version of the font stack.
* Inline critical path CSS by eliminating render blocking resources.
* Ultimately reduce the overall requests count.
* Find an alternative to Google Analytics.

> Update: Since I wrote this artice, I have managed to score 100% on this blog and I intent to maintain that.

![](https://res.cloudinary.com/dw9fem4ki/image/upload/v1590140275/Capture_dmahpr.png)