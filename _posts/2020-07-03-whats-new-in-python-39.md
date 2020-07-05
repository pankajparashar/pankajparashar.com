---
title: What's new in Python 3.9
layout: post
excerpt: Python 3.9 will release today. This article serves as a preview for the new features
  that will be introduced in the final release. Bear in mind at the time of writing, the official specification is still in the draft mode. Although it is unlikely to change in the last moment, it could still happen. Hence, it is worthwhile to revisit this article for future updates.
---

## Dict union operator

Two dictionaries can now be combined with the pipe (`|`) operator. It also supports augmented assignment (`|=`) expression.

```
>>> d = { 'a':0, 'b':1 }
>>> e = { 'c':2 }

>>> d | e
{ 'a':0, 'b':1, 'c':2 }
>>> e | d
{ 'c':2, 'a':0, 'b':1 }
>>> d |= e
>>> d
{ 'a':0, 'b':1, 'c':2 }
```

## Improved support for built-ins

In type annotations, you can now use built-in collection types like `list`, `dict` as generic types.

```
def greet_all(names: list[str]) -> None:
    for name in names:
        print("Hello", name)
```

## New string methods for prefix/suffix

The builtin `str` class now adds two new methods `str.removeprefix( prefix )`, `str.removesuffix( suffix )` to remove prefixes and suffixes respectively from the string object.

```
>>> text = 'Mr. John Doe Jr'
>>> text.removeprefix( 'Mr. ' ).removesuffix( ' Jr' )
'John Doe'
```

## Str replace behavior

Serhiy Storchaka first reported [about this bug](https://bugs.python.org/issue28029). In the latest release, `"".replace("", s, n)` now returns `s` instead of an empty string for all non-zero `n`. It is now consistent with `"".replace("", s)`.

```
>>> "".replace( "", "|")
"|"
>>> "".replace( "", "|",0)
""
>>> "".replace( "", "|",1)
"|"
>>> "".replace( "", "|",2)
"|"
```

## New module: zoneinfo

The `zoneinfo` module brings support for the IANA time zone database to the standard library. It adds `zoneinfo.ZoneInfo`, a concrete datetime.tzinfo implementation backed by the system’s time zone data.

```
>>> from zoneinfo import ZoneInfo
>>> from datetime import datetime, timedelta

# Daylight saving time
>>> dt = datetime(2020, 10, 31, 12, 
                  tzinfo=ZoneInfo("America/Los_Angeles"))
>>> dt.tzname()
'PDT'

# Standard time
>>> dt += timedelta(days=7)
>>> print(dt)
2020-11-07 12:00:00-08:00
>>> print(dt.tzname())
PST
```

## Miscellaneous

1. The isocalendar() of datetime.date and isocalendar() of datetime.datetime methods now returns a namedtuple() instead of a tuple. (Contributed by Dong-hee Na in [bpo-24416](https://bugs.python.org/issue24416).)
2. `__import__()` now raises ImportError instead of ValueError, which used to occur when a relative import went past its top-level package. (Contributed by Ngalim Siregar in [bpo-37444](https://bugs.python.org/issue37444).)