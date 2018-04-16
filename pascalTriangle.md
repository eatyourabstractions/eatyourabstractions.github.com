---
layout: page
title: pascal's triangle
from: HackerRank
subCat: Recursion
---

[problem origin & description](https://www.hackerrank.com/challenges/pascals-triangle/problem)
~~~scala
def pascal(xs: List[Int] = List(1,1), height: Int): List[Int] = {
  println("1")
  println("1 1")

  def loop(xs: List[Int], n: List[Int]): List[Int] = n match {
    case Nil => Nil
    case _ =>
    var lst = for(it <- 0 until xs.size - 1 )  yield {
      xs(it) + xs(it + 1)
      }
      val ls = 1 +: lst.toList :+ 1
      println(ls.mkString(" "))
      loop(ls, n.drop(1))
  }

  val h = height - 2
  loop(xs, (0 until h).toList)

}
~~~
