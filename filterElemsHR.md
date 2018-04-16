---
layout: page
title: filter elements
from: HackerRank 
subCat: Recursion
---
[problem origin & description](https://www.hackerrank.com/challenges/super-digit/problem)
~~~scala
object Solution {

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/

import scala.collection.mutable.{Map => MMap}

val stdin = io.Source.stdin.getLines().toList.tail

type Info = (Int,Int) // (index, times)
type Data = MMap[Int,Info]

/* potential: if 'h' it's in, then retain it's index and update it's 'times'
              if 'h' it's not, assign the current index and times = 1
*/

def filterElems(input: List[Int], atLeast: Int) = {
  def go(in: List[Int], amap: Data, index: Int): Data = in match {
    case h :: t => {
      val potential = if(amap.contains(h)) (amap(h)._1, amap(h)._2 + 1) else (index, 1)
      go(t, amap += (h -> potential),  index + 1)
    }
    case _ => amap
  }

  val answer = go(input, MMap[Int,Info](),0)
  .filter({case (value,(idx,times)) => times >= atLeast})
  .toList
  .sortBy(_._2._1)
  .map(_._1)
  .mkString(" ")

println(if(answer.isEmpty)"-1" else answer)

}
def numerize(str: String):List[Int] = str.split(" ").map(_.toInt).toList

def recurse(strIn: List[String]): Unit = strIn match{
    case h :: t => {
        val (now,next) = t.span(line => numerize(line).size > 2)
        filterElems(now.map(numerize).flatten,numerize(h)(1))
        recurse(next)
    }
    case _ => ()
}
        recurse(stdin)

    }
}
~~~
