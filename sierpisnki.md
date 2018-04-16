---
layout: page
title: sierpinski triangle
from: HackerRank 
subCat: Recursion
---

[problem origin & description](https://www.hackerrank.com/challenges/functions-and-fractals-sierpinski-triangles/problem)

~~~scala


case class Triangle(x: Int, y: Int ,height: Int){

  def getNextPoints: List[Triangle] = {
    val up = Triangle(x, y, height / 2)
    val left = Triangle(x - height / 2, y + height / 2, height / 2)
    val right = Triangle(x + height / 2, y + height / 2, height / 2)
    List(up,left,right)
  }

  def p2Flip: List[(Int,Int)] = {

    val triangle = (1 to (height * 2) ).filter(_ % 2 != 0)

    def points(row: Int, mid: Int, sliceSize: Int):List[(Int,Int)] = {
      val startingIndex = mid - ((sliceSize - 1) / 2)
      (startingIndex until (startingIndex + sliceSize)).map(col => (row,col)).toList

    }

  (y until (y + triangle.size)).zip(triangle)
  .map({case (row,sliceSize) => points(row,x,sliceSize)}).toList.flatten

  }

}


  def sierpinski(iterN: Int): Unit = {

    def go(p: List[Triangle], iterN: Int): List[(Int,Int)] = {
      if(iterN == 0) p.map(_.p2Flip).flatten
      else{
        go(p.map(_.getNextPoints).flatten, iterN - 1)
      }

    }

    def draw(p2flip: List[(Int,Int)],ma: Array[Array[String]]):Array[Array[String]] ={
      p2flip foreach {p => ma(p._1)(p._2) = "1"}
      ma
    }

    def printMatrix(arr: Array[Array[String]]):Unit = {
      arr.map(_.mkString("")).foreach(println)
    }

    val board = Array.fill(32)(Array.fill(63)("_"))
    val p1 = List(Triangle(31,0,32))
    val p2Draw = go(p1,iterN)
    printMatrix(draw(p2Draw,board))


  }

~~~