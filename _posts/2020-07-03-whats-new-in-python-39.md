---
title: What's new in Python 3.9
layout: post
excerpt: Python 3.9 will release today. This article serves as a preview for the new features
  that will be introduced in the final release.
---

> Bear in mind at the time of writing, the official specification is still in the draft mode. Although it is unlikely to change in the last moment, it could still happen. Hence, it is worthwhile to revisit this article for future updates.

## Dict union operator

Two dictionaries can now be combined with the pipe (`|`) operator. It also supports augmented assignment (`|=`) expression.

```
>>> d = {'spam': 1, 'eggs': 2, 'cheese': 3}
>>> e = {'cheese': 'cheddar', 'aardvark': 'Ethel'}
>>> d | e
{'spam': 1, 'eggs': 2, 'cheese': 'cheddar', 'aardvark': 'Ethel'}
>>> e | d
{'aardvark': 'Ethel', 'spam': 1, 'eggs': 2, 'cheese': 3}
>>> d |= e
>>> d
{'spam': 1, 'eggs': 2, 'cheese': 'cheddar', 'aardvark': 'Ethel'}
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