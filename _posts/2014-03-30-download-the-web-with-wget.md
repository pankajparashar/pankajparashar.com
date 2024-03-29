---
title: Download the web with WGET
date: 2014-03-30 00:00:00 Z
layout: post
excerpt: Having recently discovered the power of wget command, I have written this
  article to remind myself the various ways we could use this command to download
  the world wide web.
---

The name `wget` is derived from the amalgamation of `World Wide Web` and `Get`. The UNIX [manual page](http://unixhelp.ed.ac.uk/CGI/man-cgi?wget) describes it as,

```
$ man wget
GNU Wget is a free utility for non-interactive download of files from
the Web. It supports HTTP, HTTPS, and FTP protocols, as well as retrieval
through HTTP proxies.
```

Wget can follow links in HTML and XHTML pages and create local versions
of remote web sites, fully recreating the directory structure of the
original site. The best part is that Wget respects the Robot Exclusion Standard [(/robots.txt)](/robots.txt)

The help page is good enough to explain all the available options that comes bundled with the `wget` package,

```
pankajparashar@macbook-pro: ~
$ wget -h
GNU Wget 1.14, a non-interactive network retriever.
Usage: wget [OPTION]... [URL]...
```

We'll go through the various use-cases and how to use wget commmand to accomplish basic tasks. For all the examples, we'll use the long format to specify the options, because they are verbose and self-explanatory.

## Download the index page of a website

You can download the file located at the root of the url by simply specifying the website address.

```
$ wget http://google.com
```

However, you would have no control over the name of the file downloaded in your local system.

## Download and save using a different filename

Fortunately, wget allows to explicitly specify the name of the downloaded file, as shown below,

```
$ wget --output-document=index.html http://google.com
```

## Download the entire website

If you want to clone the entire website and restrict the pages only to the specified domain for offline viewing, then wget has got you covered.

```
$ wget --mirror --recursive --no-clobber --page-requisites --adjust-extension --convert-links --domains pankajparashar.com --no-parent pankajparashar.com
```

You could also specify the file extensions that you may/may not want to download by specifying the --accept=LIST or --reject=LIST appropriately.

## User-agent masking

You can also simulate the download by explicitly specifying the user agent. Might be useful for websites that block download for few UAs

```
$ wget --user-agent=Mozilla http://google.com
```

## Download file via FTP url  

```
# Anonymous FTP
$ wget ftp://cdn.pankajparashar.com/file.txt

# FTP download using wget with username and password authentication
$ wget --ftp-user=USERNAME --ftp-password=PASSWORD ftp://cdn.pankajparashar.com/file.txt
```