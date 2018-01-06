---
title: what is a group in abstract algebra
date: 2017-12-12 23:30:09
tags:
- Algebraic
- Group
---

# Introduction
As we're about to see, groups pop out everywhere in the imaginary world of mathematics as also in the real world, once you get to know groups you'll start to see them even in your soup. what really happens is that when you're done with the process of abstracting over a concept the result which confusedly is also called 'abstraction' serves as a new category for which one can classify a whole bunch of things.


# Operations
Before defining what a Group is there's a simple concept that you need to know first, the operation.

An operation (*) on a set A is a rule which assigns to each ordered pair (a,b)  of elements of A exactly a * b in A

there are 3 aspects that a symbol (*) need to satisfy to qualify as an operation
 
1. a * b is defined  for every pair (a,b)
2. a * b must be uniquely defined
3. a * b must be closed under * 

for example:
Let's imagine that we have two similar symbols that we what to know if they are operations, say, (--) is good old regular subtraction on the Z of integers and (\*) is subtraction of the set of positive integers +Z. It's important to note that (\*) is a symbol like any other, what's really important are the properties that it satisfies.

A quick check over the properties tell us that the first symbol (--) is indeed an operation in the sense we're aiming, whereas our second symbol is not because it violates closure requirement, 4 * 3 = 4 - 3 = -1 which is not a positive integer.

now ready to get to jump in the world of groups

# group definition

By a group we mean a set G with an operation * which satisfies the axioms:

* (G1) * is associative.
* (G2) There is an element 'e' in G such that a * e = a and e * a = a for every element a in G.
* (G3) For every element 'a' in G, there is an element -a in G such that a * (-a) = e and (-a) * a = e.

(-a here is the inverse of a)

I'm using a very useful tool called ammonite to make my workflow a little bit more dynamic ala python or javascript, **check it out**

we will use a trait to model a Group in Scala:
~~~scala
trait Group[T] {
	  // here our symbol (*) is called combine
   def combine(that: T): T 
   def zero: T
	}
~~~
in this way, we can take advantage that Scala provides us to use the methods in the trait in infix notation (a * b) instead of *(a,b)


now, we need a way to verify that our axioms are being satisfied,
in scala this look like this:

~~~scala
def associativity[G <: Group[G]](lst: List[G]): Boolean = {
    lst.combinations(3).forall{
    l => l match {
      case h :: t :: th :: _ => {
       ((h combine t) combine th) == (h combine (t combine th)) }
      case _ => false }
    }
 }
    
def getMeIdentity[G <: Group[G]](myGroup: List[G]): Option[G] = {
  def isIdentity(elem: G, mlist: List[G]):Boolean = {
	 mlist.forall{x => ((elem combine x) == x) && ((x combine elem) == x)}
		 }
	
    myGroup.filer(isIdentity(_, myGroup)).headOption
	 }

def getMeInverses[G <: Group[G]](mylist: List[G], identity: G): Map[G,G] = {
	  val combList = for {
	  elem <- mylist
	  item <- mylist } yield (elem, item, elem combine item)
	  combList.filter(_._3 == identity).map({case (x,y,z) => (x,y)}).toMap
	}
~~~	  


here we present 3 Polymorphic methods, each responsible to prove an axiom. 

the first partitions the set in subsets of 3 elements and tries to verify associativity for the whole set.

the second method finds the identity element in a finite group, notice that the element it encounters must be the same as the one we defined as **zero** (the result wrapped in an Option type, this doesn't have anything to do with whats going on, so omit that part)

the third method gives us a Map in which each value is the **inverse** of the corresponding key, the size of this map must be equal to number of elements in this group, proving that every elements has an inverse.


# Groups in the math branches

## Groups in Linear Algebra
matrices under the right perspective can be viewed as groups,
a matrix, we will use 2x2 matrices here for the sake of simplicity
   

with the dot product is modeled as follows:

~~~scala
case class MatrixGroupInt(a: Int, b: Int, c: Int, d: Int) extends Group[MatrixGroupInt] {
	   def combine(that: MatrixGroupInt): MatrixGroupInt = {
	    that match {
	        case MatrixGroupInt(ap,bp,cp,dp) => {
	          val newa = (a*ap) + (b*cp)
	          val newb = (a*bp) + (b*dp)
	          val newc = (c*ap) + (d*cp)
	          val newd = (c*bp) + (d*dp)
	          MatrixGroupInt(newa,newb,newc,newd)
	        }
	      }
	    }
	  def zero: MatrixGroupInt = MatrixGroupInt(1,0,0,1)
	}

	// stock of matrices
	val A = MatrixGroupInt(0,1,1,0)
	val B = MatrixGroupInt(0,1,-1,-1)
	val C = MatrixGroupInt(-1,-1,0,1)
	val D = MatrixGroupInt(-1,-1,1,0)
	val K = MatrixGroupInt(1,0,-1,-1)
	val I = MatrixGroupInt(1,0,0,1)
	
	val matrixList = List(A,B,C,D,K,I)
~~~


let's see if our case class satisfie our axioms


~~~scala
associativity(matrixList)
===> true // we got associativity
	
val identity = getMeIdentity(matrixList).get
===> Option[MatrixGroupInt] = MatrixGroupInt(1, 0, 0, 1)
	
getMeInverses(matrixList, identity)
===> Map[MatrixGroupInt, MatrixGroupInt] = Map(...)
~~~


## Groups in combinatorics and Geometry

we learned in high school that a permutation was just a rearrangement of a set, but for our next example we'll need a more general version, say, a permutation is a bijective or one-to-one correspondence from a set A to itself, this new version allows to talk about infinitely bigger sets.

the set of all the permutations of a set is also a group, how? a bijection under composition is also a bijection, that means, that a permutation under composition is also a permutation of our set, a bijection also means that by default we have an inverse, and an identity would be a function that given an element x, it returns that same element x.

~~~scala 
def buildSymmetries[A](lst: List[A]) =
  lst.permutations.map(x => (lst zip x).toMap).toList
~~~
the above code lets us construct finite groups of permutations( Symmetries from now on) of any type of variable size, we are going to use numbers in our examples.

~~~scala
\\ A doesn't matter at all what it really matters
\\ is how things compose

case class Symm[A](perm: Map[A,A]) extends Group[Symm[A]]{
    def combine(that: Symm[A]): Symm[A] = 
      Symm(perm.map({case (k,v) => (k,that.perm(v))}) )
      
    def zero = Symm(perm.map({case (k, _) => (k,k)})) 
  } 
  
val symList = buildSymmentries(List(1,2,3))
===> List(Map( ... ))

val id = Symm(symMaps(0)) 
===> id: Symm = Symm(Map(1 -> 1, 2 -> 2, 3 -> 3))
val alpha = Symm(symMaps(1)) 
===> alpha: Symm = Symm(Map(1 -> 1, 2 -> 3, 3 -> 2))
val beta = Symm(symMaps(2)) 
===> beta: Symm = Symm(Map(1 -> 2, 2 -> 1, 3 -> 3))
val gamma = Symm(symMaps(3)) 
===> gamma: Symm = Symm(Map(1 -> 2, 2 -> 3, 3 -> 1))
val iota = Symm(symMaps(4)) 
===> iota: Symm = Symm(Map(1 -> 3, 2 -> 1, 3 -> 2))
val kappa = Symm(symMaps(5)) 
===> kappa: Symm = Symm(Map(1 -> 3, 2 -> 2, 3 -> 1))

val symList = List(id,alpha,beta,gamma,iota,kappa)

\\ proof of 'groupness'
associativity(symList)
===> true

getMeidentity(symList).get
===> Symm(Map(1 -> 1, 2 -> 2, 3 -> 3)) \\ which is equal to id

getMeInverses(symList, id)
===> \\ map of all inverses :-)
~~~

starting with permutations and complementing with a tiny constraint, these group of permutations easily migrate to another domain, geometry, the group of symmetries of geometric figures they're called Dihedral Groups or D4. What is this constraint? well, it has to be a regular polygon and all rearrangement have to be the sum of all the possibles reflections and rotations.

if we have a square and we define a center P and apply the transformations to the figure with respect to its center, we get 4 rotations,90, 180, 270 and 360 (it's original position) and 4 reflections base on 4 axes, horizontal, vertical and 2 diagonal.

*d4 images*


the above table presents in HD all the possibles combinations, notice the remembrance with the multiplication table that you use memorize early in life, we're kind of multiplying with objects other than numbers.

# groups in real life

the next  few examples make clear why mathematicians talk about concepts as if they are something they can grab with their bare hands, something as  concrete as a doorknob or a baseball ball or other everyday object, again what it matters are the properties this object/concept have, it doesn't matter to them if they're talking about a car or a very exotic kind of monad.



## the clock

addition modulo 12, a fancy name for what in the real-world is called simply as a  **clock**, yeah you heard well a clock is a group also, but how?, let's define the operation 'addition modulo 12' as the sum of 2 numbers and getting the remainder of dividing by 12, (a + b) % 12 you'll see that there are just 12 numbers in this group, the numbers from 0 to 11, being 0 the 12th and the neutral element.

ignoring the minutes and seconds hands we can see more clearly why the clock makes an everyday group if, for example, we take a clock pointing to the 10:00 and "sum it up" (or multiply it, it's just a notational difference) with another clock pointing,let's say, to the 06:00 you'll "get" a clock pointing to the 04:00, but this is just a small part of the definition, in detail:


again modelling with case classes:

~~~scala
case class Clock(num: Int) extends Group[Clock] {
  def combine(that: Clock): Clock = Clock((that.num + num) % 12)
  def zero: Clock = Clock(0) }
	  
  val clockList = (0 to 11).map(Clock(_)).toList
	  
associativity(clockList)
===> true // and again, assoc
	
val identity = getMeIdentity(clockList).get
===> Clock(0)
	
getMeInverses(clockList, identity)
===> Map[Clock, Clock] = Map(...)
~~~

## A deck of cards

I know what are you thinking, how do you combine a queen of hearts with the King of clubs to get the jack of Spades?

it's simpler than what you think, we don't need anything else than our old reliable clock from the last example, the deck of card is just the direct product of two **addition modulo n** where n is different for the rank of the card, 13, from the type of suit, from 0 to 3, for clubs, spades, hearts, diamonds, respectively.


~~~scala
case class Rank(num: Int) extends Group[Rank]{ 
  def combine(that: Rank): Rank = Rank((num + that.num) % 13)
  def zero: Rank = Rank(0) 
	  }
	    
case class Suit(num: Int) extends Group[Suit]{
   def combine(that: Suit): Suit = Suit((num + that.num) % 4)
   def zero: Suit = Suit(0) 
     }
	    
case class Card(rank: Rank, suit: Suit) extends Group[Card] {
   def combine(that: Card): Card = 
    Card(rank combine that.rank, suit combine that.suit)
 
   def zero: Card = Card(rank.zero, suit.zero) 
	    }
	// load the deck
	val deck = for {
	  r <- (0 to 12).toList
	  s <- (0 to 3).toList } yield Card(Rank(r),Suit(s))
	  
associativity(deck)
===> true 
	    
val identity = getMeIdentity(deck).get
===> Card(Rank(0),Suit(0))    
	
getMeInverses(deck, identity)
===> Map[Card, Card] = Map( ... ) // size 52  
~~~

# Conclusion

I stated earlier when you get the notion of a group you just can't unsee them, they are everywhere, I just presented examples in a few simple domains that you may have stumbled upon before (bet you didn't notice them when learning elementary algebra back in school),  there are many many more, what I personally like about Groups and abstract algebra in general is that you make knowledge transferable from one branch of math to the other, it makes easier to get new math concepts.

now, for the sake of learning is always recommendable to put this knowledge to work to make it sink in your mind, as a homework try to proof the "groupness" of another interesting but ordinary everyday object, the Rubik's cube. [answer](https://cornellmath.wordpress.com/2008/01/27/puzzles-groups-and-groupoids/)