---
layout: page
title: Remove Duplicates
from: HackerRank
subCat: "Functional programing: ad-hoc"
---

[problem origin & description](https://www.hackerrank.com/challenges/remove-duplicates/problem)

~~~scala
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
    val stdin = io.Source.stdin.getLines().toList.mkString("")
      println(stdin.foldLeft(""){(acc, s) => if(!acc.contains(s)) acc + s else acc})
    }
}

~~~
