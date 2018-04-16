---
layout: page
title: Computing gcd
from: HackerRank
subCat: recursion
---
[problem origin & description](https://www.hackerrank.com/challenges/functional-programming-warmups-in-recursion---gcd/problem)


~~~scala
def gcd(x: Int, y: Int): Int =
  {
    val k = x max y
    val m = x min y
    @annotation.tailrec
    def go(k: Int, m: Int): Int =
      {
        if (m != 0) go(m, k % m)
        else k

      }
      go(k,m)
  }
  ~~~
