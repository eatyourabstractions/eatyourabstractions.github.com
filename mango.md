---
layout: page
title: mangoes
from: HackerRank
subCat: "Functional programing: ad-hoc"
---

[problem origin & description](https://www.hackerrank.com/challenges/mango/problem)

~~~scala
// only work for small problems

object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/


       val stdin = io.Source.stdin.getLines().toList.map(_.split(" "))
       val (n,m) = ((stdin(0)(0)).toInt,(stdin(0)(1)).toLong) // (Int,Int)
       val a = stdin(1).map(_.toInt) // array[Int]
       val h = stdin(2).map(_.toInt) // array[Int]
       val friendsColl = (1 to n).toList // list int

       //list int => int, action
       def formula(friendList: List[Int]): Int ={
          val k = friendList.size - 1
          friendList.map(f => a(f - 1) + k * h(f - 1)).sum
    }
        // int => (num =< mangoes,numOfFriends int)
        def closestToMangoeNumber(numOfFriends: Int):(Int,Int) = {
         val middleResult = friendsColl
            .combinations(numOfFriends) // iterator[ list[int] ]
            .map(x => (formula(x), numOfFriends)) // (int, int)
            .filter(_._1 <= m)
         if (middleResult.isEmpty) (0,0) else middleResult.maxBy(_._1)
    }



    println(friendsColl.map(closestToMangoeNumber).maxBy(_._2)._2)


    }
}
~~~
