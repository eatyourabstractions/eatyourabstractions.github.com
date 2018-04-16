---
layout: page
title: fibonacci numbers
from: fp in scala (the red book)
subCat: chapter 2 exercise 1
---

~~~scala
def fib(n: Int): Int ={

  @annotation.tailrec
  def go(n: Int, a: Int, b: Int): Int = {
      if (n <= 1) b
      else go(n-1, b, a + b)
  }
  go(n, 0 , 1)
}
// fp in scala, optional ex.:1 chapter 2
print(fib(10))
~~~
