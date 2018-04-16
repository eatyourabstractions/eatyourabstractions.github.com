---
layout: page
title: Pass Cracker FP
from: HackerRank 
subCat: DP
---



[problem origin & description](https://www.hackerrank.com/challenges/password-cracker-fp/problem)

~~~scala
object Solution {
type Data = Map[Char,List[String]]

    def main(args: Array[String]) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution
*/
    def str2Map: String => Data = { str => str.split(" ").toList.groupBy(x => x(0)) }

    def getWord(ls: List[String], st: String):Option[(String,String)] = ls match {
      case x::t =>{
        if(x.size <= st.size){
          val (w,mwords) = st.splitAt(x.size)
          if(x == w) Some((w,mwords))
          else getWord(t,st)
        } else {getWord(t,st)}
      }
      case _ => None
      }

    def func(a: String, out: String = "" )(data: Data):Option[String] = {
      if(a.size != 0){
        for{
          ls <- data.get(a(0))
          (w,other) <- getWord(ls,a)
          res <- func(other, out + " " + w)(data)
        } yield res
      }else Some(out)
    }

    val stdIn = io.Source.stdin.getLines().toList.tail.sliding(3,3)
        stdIn.foreach{_ match{
            case List(_,pass, attempt) =>
            println((str2Map andThen func(attempt))(pass).getOrElse("WRONG PASSWORD"))} }

    }
}

~~~