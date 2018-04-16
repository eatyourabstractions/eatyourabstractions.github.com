---
layout: page
title: is this list sorted
from: fp in scala (the red book)
subCat: chapter 2 exercise 2
---
~~~scala
def isSorted[A](as: Array[A], gt: (A, A) => Boolean): Boolean = {
  @annotation.tailrec
  def iter(i: Int): Boolean = {
    if (i >= as.length - 1) true
    else !gt(as(i), as(i + 1)) && iter(i + 1)
  }
  iter(0)
}

def greaterThan[T <: Ordered[T]](a: T, b: T):Boolean = a > b

print(isSorted(Array(1,2,3,4,5), greaterThan ))
~~~
