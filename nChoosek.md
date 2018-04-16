---
layout: page
title: Count number of ways (nCk or n choose k)
from: HackerRank 
subCat: DP
---


[problem origin & description](https://www.hackerrank.com/challenges/different-ways-fp/problem)

~~~scala
// first solution
object Solution {

    def main(args: Array[String]) {
           import scala.collection.mutable.{Map => MMap}
   val store = MMap[BigInt,BigInt](BigInt(0) -> BigInt(1), BigInt(1) -> BigInt(1))
    def fac(n: BigInt): BigInt = {
        if(store.contains(n)) store(n)
        else (store += (n -> n * fac(n - 1))); store(n)
    }
    def count(n: BigInt, k: BigInt): BigInt = {
        ( fac(n)/(fac(n - k) * fac(k)) ) % (100000000 + 7)
    }

    io.Source.stdin.getLines().toList.tail.map(_.split(" ").map(_.toLong))
        .foreach(x => println(x match {case Array(n,k) => count(n,k)}) )
    }
}

// second solution
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
  import scala.collection.mutable.{Map => MMap}
    def nCk(from: Long, take: Long): Long = {
     type Store = MMap[(Long,Long),Long]
     val myStore = MMap[(Long,Long),Long]()
          def loop(n: Long, k: Long, store: Store): Long ={
            if(!store.contains((n,k))){
             (n,k) match {
            case (n,0) => store.updated((n,k), 1); 1
            case (0,k) => store.updated((n,k), 0); 0
            case _ => store.updated((n,k), loop(n-1,k,store)+loop(n-1,k-1,store))((n,k))}
            } else store((n,k))
          }
          loop(from,take,myStore)
        }

    io.Source.stdin.getLines().toList.tail.map(_.split(" ").map(_.toLong))
        .foreach(x => println(x match {case Array(n,k) => nCk(n,k)}) )
    }
}


~~~