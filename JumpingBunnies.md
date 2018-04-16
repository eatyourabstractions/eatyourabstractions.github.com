---
layout: page
title: Jumping Bunnies
from: HackerRank
subCat: "Functional programing: ad-hoc"
---

[problem origin & description](https://www.hackerrank.com/challenges/jumping-bunnies/problem)


~~~scala
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
        def parse(s: String):List[Long] = s.split(" ").map(_.toLong).toList
        val stdIn = io.Source.stdin.getLines().toList.tail.map(parse).flatten
        def gcd(a: Long, b: Long): Long = if (b == 0) a.abs else gcd(b, a % b)
        def lcm(a: Long, b: Long): Long = (a * b).abs / gcd(a,b)
        println(stdIn.sorted.reduce(lcm))
    }
}
~~~
