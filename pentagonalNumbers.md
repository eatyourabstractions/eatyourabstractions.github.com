---
layout: page
title: Pentagonal Numbers
from: HackerRank 
subCat: DP
---
[problem origin & description](https://www.hackerrank.com/challenges/pentagonal-numbers/problem)

~~~scala
// non memoized version
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution */

    def p(n: Long) = ((n*2) - 1) + ((n-1)*2) + edgeDots(n)
    def edgeDots(n: Long) = {
       val sum = ((n-2)*(1 + (n-2)))/2
       sum*3
    }
    val stdIn = io.Source.stdin.getLines().toList.tail
    stdIn.foreach(n => println(p(n.toLong)))

    }
}

// memoized version
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution */
    import scala.collection.mutable.Map

    val store = Map[Long,Long](1L -> 1L, 2L -> 5L)

    def loop(n: Long): Long =  {
      val res = store.getOrElse(n,4L + (n - 2L) * 3L + loop(n - 1L))
      store += (n -> res)
      store(n)
    }
    val stdIn = io.Source.stdin.getLines().toList.tail
    stdIn.foreach(n => println(loop(n.toLong)))

    }
}
~~~