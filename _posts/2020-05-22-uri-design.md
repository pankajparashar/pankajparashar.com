---
title: URI Design
layout: link
excerpt_separator: <!--more-->
---

Its been almost 17 years to the date when [Anne van Kesteren](https://annevankesteren.nl/) wrote about
URI design and every single point made in that article about designing a URL is still relevant. Its
surprising that this is not something that is talked about often, and hence this article serves as a refresher for the web dev folks.

<!--more-->

## Notes

- Don't use file extensions for scripting languages like PHP, Perl, Java and Coldfusion.
- Don't use arguments, but always make it look like there is a directory structure. Think about that - directory structure and your client's content before you start. (Also try to think ahead.)
- Keep everything lowercase.
- Don't use spaces, but separate words with a hyphen or underscore. (I prefer a hyphen.)
- Make sure they are permanent. Even if you need to use 10 permanent redirects, do it. Always keep your URIs working. (It doesn't matter if you are building a site for the local bookstore or for SonyEricsson.)
Dont' change the content completely. Having the projects page on the about page just because you changed something in the scripts is no excuse.

[Read more](https://annevankesteren.nl/2004/08/uri-design)