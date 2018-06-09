---
layout: post
title:  "Python Basics: Shallow Copy and Deep Copy"
date:   2018-01-24 
categories: ["PythonBasics"]
author: "Ziyu Zhou"
---

> Python basics for beginners: Understand the differences between shallow copy and deep copy, as well as when and how should we use them. 



# How Assignments Work in Python?

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
y  ↗︎
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

In a word:

> Assignment statements in Python **do not copy objects**, they create **bindings** between a target and an object.



