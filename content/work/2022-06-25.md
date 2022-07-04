---
title: Advanced R
author: ''
date: '2022-06-22'
slug: Advanced R
categories: []
tags: [R]
---

## Names and values

We’ll use the lobstr package to dig into the internal representation of R objects.
```r
library(lobstr)
```

```r 
x <- c(1, 2, 3)
```

- It’s creating an object, a vector of values, c(1, 2, 3).
- And it’s binding that object to a name, x.
- the name is a reference to the value

another binding to the existing object:
```r
y <- x
```

object’s memory “address” changes every time and can use `lobstr::obj_addr` to access its identifier.
These identifiers are long, and change every time you restart R.

Non-syntactic names:

A syntactic name must consist of letters10, digits, . and _ but can’t begin with _ or a digit. Additionally, you can’t use any of the reserved words like TRUE, NULL, if, and function (see the complete list in ?Reserved)ding it with backticks:

using single or double quotes (e.g. "_abc" <- 1) instead of backticks, but you shouldn’t, because you’ll have to use a different syntax to retrieve the values

*Copy on modify*
when x and y refers to a same object, then modifying y ——it means that R created a new object, 0xcd2, a copy of 0x74b with  value changed, then rebound y to that object.

You can see when an object gets copied with the help of `base::tracemem()`.


untracemem() is the opposite of tracemem(); it turns tracing off.
