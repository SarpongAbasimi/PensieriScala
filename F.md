# `What is F[_] in Scala?`

Scala type system is one of the most sophisticated type systems I have ever come across and sometimes it can quickly get complicated for people trying to learn this beautiful language to understand what these types are. 

One of such types is the `F[_]`. I remember being confused and asking myself what it was and why I needed it, so in this blog, I will try my best to explain what it is and why you even need it.

Before we can understand what `F[_]` is, we need to first understand types in Scala. 

Scala has `normal` types or what I call `level zero` types, these types are `Int`, `Float`, `Double`, `String` and even `case classes`. Level zero types or normal types are types that can be attached to a value by themselves. This is why it is possible to say

```
val company : String = "47 Degree"

case class Person(name: String, age: Int)

val personOne: Person = Person("Fede", 1000)
```

We made mention of `Float`, `Double` and `String` as `normal` or `level zero` types in scala, what about `List`, `Option`, `Either` and `Map`? what are they?

Well, these are what we call `first-order` types or `level one` types because they can't be attached to a value by themselves. If we put the expression below in the Scala compiler we would get this error message 

```
val countries: List = List("🇬🇭 ", "🇪🇸 ", "🇺🇸", "🇬🇧", "🇯🇵" )

1 |val countries: List = List("🇬🇭 ", "🇪🇸 ", "🇺🇸", "🇬🇧", "🇯🇵" )
 |       ^^^^
 |       Missing type parameter for List
 ```

This is because the compile won't allow as to say a type 
`List` it wants us to say a list of something `List[_]`.

In order for this to compile, we need to pass the `List` a level zero type or a normal type. For this reason the example above would be

```
val countries: List[String] = List("🇬🇭 ", "🇪🇸 ", "🇺🇸", "🇬🇧", "🇯🇵" )
```

This means that first-order types or level one types like `List`, `Option`, `Map` and `Either` are `generic types` with a type constructor `[_]` that takes a normal type or level zero type, `Int`, `String`, `Float` etc, to produce other level zero types.

```
List // This is a level one type
```
and

```
List[Int] // This is a level zero type
```

We made mentions of two new words `Type Constructor` and `Generic Types` that need to be understood.


## What is a generic type?


Assuming we had a class called `MyStack`

```
class MyStack[A] {

    //some code here
}
```

We say that `MyStack` is generic because whatever code we write inside of the `MyStack` class, inside the `{}`, will work for any type `A`. This is what we mean by a type being generic.


## What is a Type Constructor `[_]` or a higher kinded type?.

A type constructor is something like a function which takes a type as an argument and returns a type.

```
(Int) => Int  

or

String => String
```

so a type constructor `List[_]` is just a function of type

`(A) => List[A]`

or 

`String => List[String]`

Now that we an understanding of what 

- a `normal` or `level zero` type in Scala is, aka `String`, `Int`, `Float` etc 
- a `first order` or `level one` type in Scala is, aka 
`List`, `Option`, `Map`, `Either`.
- a type constructor(`[_]`) is, aka a function that takes a type and returns a type `String => String`

we can finally review what `F[_]` is in Scala. `F[_]` simple means that a `type` with a `type constructor` or a type with `one slot` and us we stated ealier, a type with one slot can be a `List`,`Option` or even a scala `Future`. Using `F[_]` is a way to abstract over level one types so that they can all share common functionalities in order for us not to repeat these same  functionalities for `List`, `Option`, `Future` etc. 

Life would be pretty boring we had to do  repeat ourselves.

Lets say we have a `My47Degree` class which takes a level one type, we could write something like this

```
class My47Degree[F[_]] {
    def map[A, B](fa: F[A])(f:A => B): F[B]
}
```

What this means is that we could replace `F[_]` with any level one type and have them use a common method called `map` so we could say

`new My47Degree[List]` or `new My47Degree[Option]`

without the `F[_]` we would have done something like

```
class My47DegreeForList[List] {
    def map[A, B](fa: List[A])(f:A => B): List[B]
}

class My47DegreeForOption[Option] {
    def map[A, B](fa: Option[A])(f:A => B): Option[B]
}
```

Isn't this beautiful?

By using `F[_]`, we just abstracted over all `first-oder` types with one hole to share a common functionality called `map` and this is why we need `F[_]`.

That is basically it. Easy right?

Try and think about the benefits this will provide and let us know if you have any questions.