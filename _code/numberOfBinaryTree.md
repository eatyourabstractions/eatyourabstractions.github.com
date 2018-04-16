---
layout: page
title: Number of Binary Trees
from: HackerRank 
subCat: DP
---


[problem origin & description](https://www.hackerrank.com/challenges/number-of-binary-search-tree/problem)

~~~scala
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
    import scala.collection.mutable.{Map => MMap}
    val store = MMap(BigInt(0) -> BigInt(1), BigInt(1) -> BigInt(1), BigInt(2) -> BigInt(2))
        def countTrees(n: BigInt): BigInt = {
          if(store.contains(n)) store(n)
          else {
          store += n -> (BigInt(1) to n).map(k => countTrees(k-1) * countTrees(n - k)).sum % (100000000 + 7)
          store(n)
          }
        }
        io.Source.stdin.getLines().toList.tail.foreach(n => println(countTrees(BigInt(n))))
    }
}

~~~
