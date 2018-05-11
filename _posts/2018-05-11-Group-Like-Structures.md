---
published: true
tags:
  - Algebraic
  - Monoid
  - SemiGroup
date: 2018-05-11T18:00:00.000Z
---
#Group-Like Structures
today we're going to explain 2 structures that are more or less like groups, monoids and semigroups.


let's start with what's a group and in short we'll explain how it relates to the new guys: a group is a Set **G** with an operation[link to operation] **( * )** that is associative, it has an element identity **e** such that e*a = a*e = a and every element a has an element -a called the inverse of a, such that a*-a = -a*a = e.

still if you wanna dig a little deeper check this[link to group post]

## Monoids


Now, the way that our yet to be defined hero, relates to groups is that it can be regarded as group who lost their inverses, formally, a monoid is a set **M** with a binary associative operation with identity e, that's why the title, they generalize (in another post we're goint to explain what is meant with generelize more in depth) the notion of a group and  it's easier to be a monoid than a group, you know, cause we have one less condition.

Let's see some examples from math:

* the natural numbers **N** with addition and e = 0 or (not and) with multiplication and e = 1
* Given a set A, all subsets of A form a commutative monoid under intersection operation (identity element is A itself).
* Given a set A, all subsets of A form a commutative monoid under union operation (identity element is the empty set).
* The set of all finite strings over some fixed alphabet A forms a monoid with string concatenation as the operation. The empty string serves as the identity element. This monoid is denoted (A,∗) and is called the free monoid over A.

probably that sensitve eye of yours noticed that there are 2  set **N** with 1 operation and 1 neutral element each, when we talk about structures the number of operations matters, here we're talking about the same set but different operation in each case, there are structures like **Rings** which have 2 operations at the same time but this is a talk for anoter post.

Anyways, now the more programming-interesting examples

http://blog.leifbattermann.de/2016/11/02/hands-on-monoids-in-scala/

~~~scala
trait Monoid[T] {
	  // here our symbol (*) is called plus
	  def plus(that: T, andThat: T): T 
	  def zero: T
	}
~~~

and a few implementation examples:

~~~scala
object Monoid {
implicit val stringMonoid = new Monoid[String] {
    def plus(a1: String, a2: String) = a1 + a2
    val zero = ""
  }
  
implicit def listMonoid[A] = new Monoid[List[A]] {
  def plus(a1: List[A], a2: List[A]) = a1 ++ a2
  val zero = Nil
  }
  
  implicit def optionMonoid[A](implicit mon: Monoid[A]): Monoid[Option[A]] = new Monoid[Option[A]] {
    def plus(f1: Option[A], f2: Option[A]) = (f1, f2) match {
      case (Some(a1), Some(a2)) => Some(mon.plus(a1, a2))
      case (Some(a1), None)     => f1
      case (None, Some(a2))     => f2
      case (None, None)         => None
    }

    def zero: Option[A] = None
  }
}
~~~

I can almost hear you say, humm but mike the string and the list monoids aren't doing anything special they're just passing their respective arguments to their underlying types, and that's absolute right!!! but this is because they are already monoids (among other abstractions), and that's a beautiful thing, Martin(the scala creator) just decided to left out the jargon from category theory, due to the scary baggage they carry, but ultimately these concept are simple when you get to know and use them, you don't even need to know that you're using monoids, functors or, drums here please!!!, **monads**, here we're just using them explicitly through their "interface"

Which it give us a property for free: If we have an Algebraic DataType **alg** which uses a monoid under the hood then **alg** is a monoid too and all the properties are "inherited"


~~~scala
implicitly[Monoid[String]].plus("galileo", "")
===> "galileo"

implicitly[Monoid[List[Int]]].plus(List(1,2,3),List(4,5,6))
===> List(1,2,3,4,5,6)

implicitly[Monoid[List[String]]].plus(List("appl","amzn"),List("msft","gogl"))
===> List("appl","amzn","msft","gogl")

~~~
 the first example demonstrate the effect of the identity or Zero element, the second operator just with old plain numbers, while the the third give evidence of our new free property which in this instance our **alg** is a List[String]

I particularly find monoids a very elegant abstraction and i encounter myself using it all the time, is can be applied in a very wide range of situations as it complement perfectly with one of the 
quintessential functions in FP, **The foldLeft** (and foldRight).

It is left as an exercise to imagine an example where the different parts of the monoid match the arguments for the foldLeft (or foldRight).

## SemiGroups

Semigroups are even "waeker" than monoids in the sense that it doesn't even have an identity element in the base set, so why they have the word "group" in the name if it's less related than monoids to almighty Group? I Don't really know, naming in math is one of the big obstacles for begginers to really get the field(which is another structure, you see!!)

~~~scala
trait SemiGroup[T]{
	def combine(that: T, andThat: T): T

}

// associativity
combine(x, combine(y, z)) = combine(combine(x, y), z)

implicit val intAdditionSemigroup: Semigroup[Int] = new Semigroup[Int] {
  def combine(x: Int, y: Int): Int = x + y
}

List(1,2,3,4).reduce(intAdditionSemigroup.combine)
===> 15

~~~

just as semigroups are a weaker version of the monoids the **Reduce** function is an specialized version of the foldLeft, so a rule of thumb to know when to use a monoid or semigroupd combined with their respective operators is to know in advance if the inpute set should bahave differently for a particular element in the set.
