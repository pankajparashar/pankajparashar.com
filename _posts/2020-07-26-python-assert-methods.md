---
title: Python Assert Methods
date: 2020-07-26 00:00:00 Z
layout: post
excerpt: This post is about using the different assert methods available in the unittest
  framework. It is however, has no resemblance with the python assert statements.
---

Python's `unittest` framework hosts a bunch of useful assert methods that we can use to validate the behaviour of our tests. Before, we begin, it is essential to add the unittest framework to our code and setup the test class.

```
>>> import unittest
>>> tc = unittest.TestCase()
>>> [_ for _ in dir(tc) if _.startswith('assert')]
['assertAlmostEqual', 'assertAlmostEquals', 'assertCountEqual', 'assertDictContainsSubset', 'assertDictEqual', 'assertEqual', 'assertEquals', 'assertFalse', 'assertGreater', 'assertGreaterEqual', 'assertIn', 'assertIs', 'assertIsInstance', 'assertIsNone', 'assertIsNot', 'assertIsNotNone', 'assertLess', 'assertLessEqual', 'assertListEqual', 'assertLogs', 'assertMultiLineEqual', 'assertNotAlmostEqual', 'assertNotAlmostEquals', 'assertNotEqual', 'assertNotEquals', 'assertNotIn', 'assertNotIsInstance', 'assertNotRegex', 'assertNotRegexpMatches', 'assertRaises', 'assertRaisesRegex', 'assertRaisesRegexp', 'assertRegex', 'assertRegexpMatches', 'assertSequenceEqual', 'assertSetEqual', 'assertTrue', 'assertTupleEqual', 'assertWarns', 'assertWarnsRegex', 'assert_']
```

## assertEqual / assertNotEqual

It accepts two elements to check for equality with an optional `msg` parameter which will print when the assertion fails.

```
>>> a, b = 1, 1
>>> tc.assertEqual(a, b)
```
```
>>> a, b = 1, 2
>>> tc.assertEqual(a, b, msg=f'a={a} and b={b} are not equal')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 912, in assertEqual
    assertion_func(first, second, msg=msg)
  File "/usr/lib/python3.8/unittest/case.py", line 905, in _baseAssertEqual
    raise self.failureException(msg)
AssertionError: 1 != 2 : a=1 and b=2 are not equal
```

Similarly, we have `assertNotEqual` to assert for inequality. `assertEquals` has been deprecated since.

## assertTrue / assertFalse

It accepts an expressions that evaluates to a boolean value with an optional `msg` parameter which will print when the assertion fails.

```
>>> tc.assertTrue(bool(1))
>>> tc.assertTrue(bool(0), msg='0 is not a truthy expression')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 765, in assertTrue
    raise self.failureException(msg)
AssertionError: False is not true : 0 is not a truthy expression
```

## assertIs / assertIsNot

It checks for the identity of the two objects if they are same or not.

```
>>> a, b = 1, 1
>>> id(a), id(b)
(140188520280384, 140188520280384)
>>> tc.assertIs(a, b)
```

```
>>> b = 'c'
>>> id(a), id(b)
(140188520280384, 140188521613808)
>>> tc.assertIs(a, b, msg=f'a={a} and b={b} are not the same objects')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 1193, in assertIs
    self.fail(self._formatMessage(msg, standardMsg))
  File "/usr/lib/python3.8/unittest/case.py", line 753, in fail
    raise self.failureException(msg)
AssertionError: 1 is not 'c' : a=1 and b=c are not the same objects
```

Similarly, we have methods like `assertIsNone`, `assertIsNotNone`, `assertIn`, `assertNotIn`, `assertIsInstance`, `assertIsNotInstance`.

## assertRaises / assertRaisesRegex

Test that an exception is raised when callable is called with any positional or keyword arguments that are also passed to assertRaises(). The test passes if exception is raised, is an error if another exception is raised, or fails if no exception is raised.

```
>>> tc.assertRaises(ZeroDivisionError, lambda x: x/(x-1), 1)
>>> 
>>> tc.assertRaises(ZeroDivisionError, lambda x: x/(x-1), 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 816, in assertRaises
    return context.handle('assertRaises', args, kwargs)
  File "/usr/lib/python3.8/unittest/case.py", line 202, in handle
    callable_obj(*args, **kwargs)
  File "/usr/lib/python3.8/unittest/case.py", line 224, in __exit__
    self._raiseFailure("{} not raised by {}".format(exc_name,
  File "/usr/lib/python3.8/unittest/case.py", line 164, in _raiseFailure
    raise self.test_case.failureException(msg)
AssertionError: ZeroDivisionError not raised by <lambda>
```

If you want to check for the exact error message, you can use the following method,

```
>>> tc.assertRaisesRegex(ValueError, "invalid literal for.*XYZ'$", int, 'XYZ')
>>> 
>>> tc.assertRaisesRegex(ValueError, "invalid literal for.*XYZ'$", int, 'ABC')
ValueError: invalid literal for int() with base 10: 'ABC'
During handling of the above exception, another exception occurred:
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 1357, in assertRaisesRegex
    return context.handle('assertRaisesRegex', args, kwargs)
  File "/usr/lib/python3.8/unittest/case.py", line 202, in handle
    callable_obj(*args, **kwargs)
  File "/usr/lib/python3.8/unittest/case.py", line 240, in __exit__
    self._raiseFailure('"{}" does not match "{}"'.format(
  File "/usr/lib/python3.8/unittest/case.py", line 164, in _raiseFailure
    raise self.test_case.failureException(msg)
AssertionError: "invalid literal for.*XYZ'$" does not match "invalid literal for int() with base 10: 'ABC'"
```

Similarly, we have `assertWarns` and `assertWarnsRegex` to handle test validations for warnings.

## assertLogs

A context manager to test that at least one message is logged on the logger or one of its children, with at least the given level. The object returned by the context manager is a recording helper which keeps tracks of the matching log messages. It has two attributes: records and output.

```
with self.assertLogs('foo', level='INFO') as cm:
   logging.getLogger('foo').info('first message')
   logging.getLogger('foo.bar').error('second message')
self.assertEqual(cm.output, ['INFO:foo:first message',
                             'ERROR:foo.bar:second message'])
```

## assertAlmostEqual / assertNotAlmostEqual

It is useful for asserting floating point numbers when you need to compare two values approximately. By default the rounding is done upto 7 decimal places.

```
>>> tc.assertAlmostEqual(0.1234567,0.1234568)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 966, in assertAlmostEqual
    raise self.failureException(msg)
AssertionError: 0.1234567 != 0.1234568 within 7 places (1.0000000000287557e-07 difference)
>>> tc.assertAlmostEqual(0.1234567,0.12345678)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.8/unittest/case.py", line 966, in assertAlmostEqual
    raise self.failureException(msg)
AssertionError: 0.1234567 != 0.12345678 within 7 places (7.99999999995249e-08 difference)
>>> tc.assertAlmostEqual(0.1234568,0.12345678)
```

Similarly, we have assert methods like, `assertGreater`, `assertGreaterEqual`, `assertLess`, `assertLessEqual`, `assertRegex`, `assertNotRegex`, `assertCountEqual`.

> `assertCountEqual` tests that sequence first contains the same elements as second, regardless of their order. When they don’t, an error message listing the differences between the sequences will be generated.

## New assert methods in 3.1

Python 3.1 introduces new assert methods to compare native object types directly. The name of the methods are self-explanatory.

|**Method**   	              |&nbsp;&nbsp;&nbsp; |**Used to compare**   	|
|:--	                        |---                |---	                  |
|`assertMultiLineEqual(a, b)` |                   |strings                |
|`assertSequenceEqual(a, b)`  |                   |sequences              |
|`assertListEqual(a, b)`      |                   |lists                  |
|`assertTupleEqual(a, b)`     |                   |tuples                 |
|`assertSetEqual(a, b)`       |                   |sets or frozensets     |
|`assertDictEqual(a, b)`      |                   |dicts                  |

This by no means is a comprehensive overview of all the available assert methods, but however, serves as a catalogue for the most commonly used test assertions in a python unit test script.