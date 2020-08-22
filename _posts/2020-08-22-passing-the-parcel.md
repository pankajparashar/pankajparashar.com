---
title: Passing the parcel
layout: post
excerpt: In many ways Python's approach to handle references is quite different to other
  programming languages but I find it remarkably intuitive if you understand the idea of how everything in Python is an object and how objects are supposed to behave when passed as arguments to a function.
---

Lets start with a simplest example,

```
>>> a = 1
>>> def f(a):
...     print("Inside func:", a)
...     a = 2
...
>>> f(a)
Inside func: 1
>>> a
1
```

In a regular programming language, we'd be quick to deduce that `a` in this case is passed by value to the function `f`, which then is overwritten inside the scope of the function which has no bearing on the variable `a` that exists outside of it. So far so good?

Well no, it'd have been nice to print the memory address alongwith the value, just to confirm this behavior.

```
>>> a = 1
>>> id(a)
1488123824
>>> def f(a):
...     print("Inside func:", a, id(a))
...     a = 2
...
>>> f(a)
Inside func: 1 1488123824
>>> a
1
```

Hang on! we thought that only the value is passed to the function, however, `id(a)` suggests that they are both indeed references to the same memory location. So is Python now doing pass by reference?

Not exactly! In order to understand how this works in Python, you'd have to get rid of all your pre-conceived notions of variable references in other programming languages and instead evaluate this behavior from a fresh perspective.

## Everything is an object

Remember when I said that how everything in Python is an object? I wasn't kidding. In this case, `a` is a reference to an object of type integer with a value `1`.

```
(ref)        (obj)
             +-----+
 [a] ------->|  1  |
             +-----+
             (loc: 1488123824)
```

So when Python calls the function, it simply passes the reference to the integer object which is much more efficient than duplicating the same value (`1`) in two different memory locations, if it were to do pass-by-value.

Now when the interpreter encounters the assignment expression `a = 2`, Python creates a new reference within the local scope of the function that points to the memory location of the Integer object holding the value `2` and we lose the reference to the value `1`. This shouldn't be surprising because Python behaves exactly like this even when you test this outside the function in a shell.

```
>>> a = 1
>>> id(a)
1488123824
>>> a = 2
>>> id(a)
1488123840
```

If you were to visualize, it would look something like this,

```
(ref)        (obj)
             +-----+
  ?  ------->|  1  |
             +-----+
             (loc: 1488123824)

             +-----+
 [a] ------->|  2  |
             +-----+
             (loc: 1488123840)
```

## What about mutable objects?

Integer objects in Python by its nature are immutable (hardly surprising). What about mutable types like list, dict?

```
>>> lst = [1]
>>> id(lst)
10844808
>>> def f(lst):
...     print("Inside func:", lst, id(lst))
...     lst = [2]
...
>>> f(lst)
Inside func: [1] 10844808
>>> lst
[1]
```

The behavior is exactly the same regardless of the type of an object, each time Python encounters an assignment expression, be it inside or outside the function.

```
>>> lst = [1]
>>> id(lst)
10704712
>>> lst = [2]
>>> id(lst)
10844808
```

Since list is a mutable data type, if we modify the list inside the scope of the function, it will continue to persist outside the scope because Python is basically working on the references to the  existing memory location. In my view this behavior is consistent with how we expect it to work.   

```
>>> lst = [1]
>>> def f(lst):
...     lst.append(2)
...
>>> f(lst)
>>> lst
[1, 2]
```

Hence, if were to condense the behavior under a single rule, it would read like this,

> Python will continue to use the same references to objects until it encounters an assignment expression irrespective of the scope it is operating under. Python only creates new reference when it needs to, which makes Python's implementation of passing the parcel several orders of magnitude efficient than other languages.

So if next time someone asks, if Python is _pass-by-value_ or _pass-by-reference_, the answer is _it depends_.