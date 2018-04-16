---
layout: page
title: Function or not
from: HackerRank 
subCat: Intro
---


[problem origin & description](https://www.hackerrank.com/challenges/functions-or-not/problem)

~~~scala

object Solution {
import scala.collection.mutable.ListBuffer

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
        val stdIn = io.Source.stdin.getLines.toList
        def isFunc(pairList: List[(String,String)]):String = {
            val uniquePoints = pairList.distinct
            val xs = uniquePoints.map(_._1)
            val uniqueXs = xs.distinct
            if(xs == uniqueXs) "YES" else "NO"
        }
        def makePairs(s: String): (String,String) = {
            val newSt = s.split(" ")
            (newSt(0),newSt(1))
        }
        @annotation.tailrec
        def loop(acc: ListBuffer[String], next: List[String]):List[String] = {
            if(next.isEmpty) acc.toList
            else {
                val (now, later) = next.tail.splitAt(next.head.toInt)
                val res = acc += isFunc(now.map(makePairs))
                loop(res,later)

            }
        }
        loop(ListBuffer[String](),stdIn.tail).foreach(println)
    }
}


~~~
