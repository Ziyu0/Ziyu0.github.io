---
layout: post
title:  "Python Basics: Shallow Copy and Deep Copy"
date:   2018-01-24 
categories: ["PythonBasics"]
author: "Ziyu Zhou"
---

> Python basics for beginners: Understand the differences between shallow copy and deep copy, as well as when and how should we use them. 

**Table of Contents:**
<!-- TOC -->

- [Problem that You Might Not Expect](#problem-that-you-might-not-expect)
- [How Assignments Work in Python](#how-assignments-work-in-python)
- [What Will Happen If We Copy a List](#what-will-happen-if-we-copy-a-list)
- [Shallow copy & Deep copy](#shallow-copy--deep-copy)
    - [Differences between Shallow copy and Deep copy](#differences-between-shallow-copy-and-deep-copy)
    - [How to Perform Shallow copy](#how-to-perform-shallow-copy)
        - [Using the `copy` Module](#using-the-copy-module)
        - [Calling the  Factory Functions](#calling-the- factory-functions)
        - [Using List Slicing](#using-list-slicing)
    - [How to Perform Deep copy](#how-to-perform-deep-copy)

<!-- /TOC -->


# Problem that You Might Not Expect

Guess what's the output of this program:

```python
>>> list_a = ['a', 'b']
>>> list_b = list_a
>>> list_b[0] = 'c'
>>> print(list_b)
>>> print(list_a)
```

You might expect the result to be:

```python
>>> print(list_b)
['c', 'b']
>>> print(list_a)
['a', 'b']
```

However, the actual result is:

```python
>>> print(list_b)
['c', 'b']
>>> print(list_a)
['c', 'b']
```

The value of `list_a` changes as `list_b` changes! This blog is to answer why this happens and how we can slove it.



# How Assignments Work in Python

Before we talk about copies, let's quickly go over how assignments work in Python first so that we can better understand the following sections. Assigning and copying data types like integer or list seem to be very simple in Python, like this:

```python
>>> x = 5
>>> y = x
>>> x
5
>>> y
5
```

However, what actually happens is not that simple. `y` is not a "new" variable, because Python will let it points to the memory location of `x`, which means that `y` and `x` are just two identifiers of the same variable whose value is 3. The figure below shows how it works:

```
x  ⟶ memory location where value 3 is stored
y  ↗
```

We can confirm this by checking their identities using the `id()` function. Only unique object or variable will have its own identities. Here we can see `x` and `y` have the same id, that being said, they are the same object:

```python
>>> id(x)
4297637024
>>> id(y)
4297637024
```

However, as soon as `y` is assigned to a different value, Python will give it its own memory location. Now `x` and `y` are different variables, and thus changing the value of `y` won't affect `x`:

```python
>>> y = 1
>>> y
1
>>> x
5
>>> id(y)
4297636896
>>> id(x)
4297637024
```

> Note: Assignment statements in Python **do not copy objects**, they create **bindings** between a target and an object.



# What Will Happen If We Copy a List

According to the section above, copying a list and then assigning a different value to the new one is supposed to work smoothly:

```python
>>> list_a = ['a', 'b']
>>> list_b = list_a
>>> list_b = ['e', 'f']
>>> print(list_b)
['e', 'f']
>>> print(list_a)
['a', 'b']
```

But this is problematic:

```python
>>> list_a = ['a', 'b']
>>> list_b = list_a
>>> list_b[0] = 'c'
>>> print(list_b)
['c', 'b']
>>> print(list_a)
['c', 'b']
```

This is because changing a value of a list, which is a **compound object** (objects that contain other objects, like lists that contain strings), will not give this list a new memory location. That is, aftering executing `list_b[0] = 'c'`, `list_b` still points to the same location as `list_a`.

This is the situation where we need "actual" copy, so one can **change one copy without changing the other**.



# Shallow Copy & Deep Copy

Shallow copy and Deep copy are both used for the "actual" copy purpose.

## Differences between Shallow Copy and Deep Copy

According to the Python [documentation](https://docs.python.org/2/library/copy.html), the difference between shallow and deep copying is only relevant for **compound objects**.

- A **shallow copy** constructs a new compound object and then (to the extent possible) inserts **references** into it to the objects found in the original.
- A **deep copy** constructs a new compound object and then, **recursively**, inserts **copies** into it of the objects found in the original.

## How to Perform Shallow Copy 

There are three ways to perform a shallow copy:

### Using the `copy` Module

```python
# Return a shallow copy of x.
import copy
copy.copy(x)
```

### Calling the  Factory Functions

Python’s built-in mutable collections like **lists, dicts, and sets** can be copied by calling their *factory functions* on an existing collection.

```python
new_list = list(original_list)
```

### Using List Slicing 

```Python
>>> list1 = ['a','b','c','d']
>>> list2 = list1[:]
>>> list2[1] = 'x'
>>> print(list2)
['a', 'x', 'c', 'd']
>>> print(list1)  # list1 is not affected by list2
['a', 'b', 'c', 'd']
```

Perfect. However, what happens if our list is a nested one? For example:

```python
>>> list1 = ['a','b',['ccc','ddd']]
>>> list2 = list1
>>> list2[2][0] = 'E'
>>> print(list2)
['a', 'b', ['E', 'ddd']]
>>> print(list1)  # Oops!
['a', 'b', ['E', 'ddd']]
```

For nested lists, we need to recursively copy it. In other words, we have to perform deep copy.

> Note: shallow copy is only one level deep. The copying process **does not recurse** and therefore won’t create copies of the child objects themselves.



## How to Perform Deep Copy

We'll need to use the `copy` module:

```python
# Return a deep copy of x.
import copy
copy.deepcopy(x)
```

Let's try the above code using deep copy:

```python
>>> import copy
>>> list1 = ['a','b',['ccc','ddd']]
>>> list2 = copy.deepcopy(list1)
>>> list2[2][0] = 'E'
>>> list2
['a', 'b', ['E', 'ddd']]
>>> list1
['a', 'b', ['ccc', 'ddd']]
```

Now we're good to go 😄!



# Reference

1. [https://realpython.com/copying-python-objects/](https://realpython.com/copying-python-objects/)
2. [https://www.programiz.com/python-programming/shallow-deep-copy](https://www.programiz.com/python-programming/shallow-deep-copy)
3. [https://stackoverflow.com/questions/17873384/deep-copy-a-list-in-python](https://stackoverflow.com/questions/17873384/deep-copy-a-list-in-python)