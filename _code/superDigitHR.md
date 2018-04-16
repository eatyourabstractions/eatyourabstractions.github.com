---
layout: page
title: super digits
from: HackerRank
subCat: Recursion
---

[problem origin & description](https://www.hackerrank.com/challenges/super-digit/problem)


~~~scala
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
    io.Source.stdin.getLines() foreach { _.split(" ") match{
        case Array(n, k) => println(go(superDigit(n),k.toInt,""))
        case _ => ()
        }
     }

    @annotation.tailrec
    def go(str: String , k: Int, digitSum: String):String = k match {
        case a if a == 0 => digitSum
        case _ => go(str, k - 1, superDigit(str + digitSum))
    }


    @annotation.tailrec
    def superDigit(str: String):String = str.size match{
        case a if a == 1 => str
        case _ => superDigit(str.map(_.toInt - 48).sum.toString)
    }

    }
}
~~~
