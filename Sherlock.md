---
layout: page
title: Sherlock valid String
from: HackerRank 
subCat: String
---
[Problem origin & description](https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem)


~~~scala

def sherlock(str: String):String = {
  val count: String => Map[Char,Int] = s => {
    s.groupBy(identity).mapValues(_.size)
  }

  def hofun(data: Map[Int,Int])(idx: Int):Boolean = {
    val newM = if(data(idx) > 1) data.updated(idx, data(idx) - 1)
    else data - idx

    newM.values.forall(_ == newM.head._2)
  }

  def loop(ixes: List[Int], allSame: Int => Boolean): String = ixes match {
    case h::t => if(allSame(h)) "YES" else loop(t,allSame)
    case _ => "NO"
  }

    val aMap = count(str).values.zipWithIndex.map{case (k,v) => (v,k)}.toMap
    val func = hofun(aMap)(_)
    if(aMap.values.forall(_ == aMap.head._2)) "YES"
    else loop((0 until aMap.size).toList, func)

  }

~~~