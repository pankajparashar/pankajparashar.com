---
title: The Walrus operator
layout: post
excerpt: The walrus operator (:=) is perhaps the most controversial operator in the history of Python. Guido was so unhappy with the proposal that he even stepped down permanently from his BDFL role after he accepted this operator as part of PEP 572. 
---

As the name suggests, the walrus operator inherits its name from the two eyes and the teeth represented by the colon (`:`) and the equal (`=`) operators in conjunction. It is a higher order form of assignment expression designed to do two things at once,

1. Assign the variable
2. Return the value of the variable

Lets consider an example,

When you match a pattern for a regular expression, you save the result in a variable `res` which may or may not be `None`. You have to check for NoneTypes before retreiving the groups from the matched pattern.

```
>>> res = re.match(r'\s')
>>> if res:
...    print(res.group(0))
```

With the introduction of the walrus operator, you can now write the same code as,

```
>>> if(res := re.match(r'\s')):
...    print(res.group(0))
```

## Rules

The operator is used as part of an expression, so writing `y:=f(x)` is invalid. It has to be surrounded by parenthesis like, `(y:=f(x))`. This might be a bit wierd because generally Python doesnt enforce the usage of parenthesis as part of its design language. Beyond this caveat, there are few other criticisms with the introduction of this operator,

1. It is not so obvious to a beginner level programmer despite the existence of this operator in other languages.
2. It goes against the [Zen of Python](https://en.wikipedia.org/wiki/Zen_of_Python) which describes, "There should be one—and preferably only one—obvious way to do it".
3. Apart from #2, it also disobeys the principle, "Explicit is better than implicit", 
"Simple is better than complex" by adding complexity in the name of saving whitespaces.
4. There's not enough real world usage data of how developers intend to use it.

> Hate it or like it, the walrus operator is officially part of Python 3.8!