# `What is F[_] in Scala?`

Scala type system is one of the most sophisticated type systems I have ever come across and sometimes it can quickly get complicated for people trying to learn this beautiful language to understand what these types are. 

One of such types is the `F[_]`. I remember being confused and asking myself what it was and why I needed it so in this blog, I will try my best to explain what it is and why you even need it.

Before we can understand what `F[_]` is, we need to understand types in Scala. 

Scala has normal types or what I call level zero types, these types are `Int`, `Float`, `Double`, `String` and even `case classes`. Level zero types or normal types are types that can be attached to a value by themselves that is why it is possible to say

```
val company : String = "47 Degree"

case class Person(name: String, age: Int)

val personOne: Person = Person("Fede", 1000)
```

We made mention of `Float`, `Double` and `String` as `normal` or `zero level` types in scala, what about `List`, `Option`, `Either` and `Map`? what are these?

Well, these are what we call first-order types or level one types because they can't be attached to a value by themselves. If we put the expression below in the Scala compiler we would get this error message 

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

This means that first-order types or level one types like `List`, `Option`, `Map` and `Either` are `generic types` with a type constructor `[_]` that takes a normal type or level zero type to produce other level zero type.


We made mentions of two new words `Type Constructor` and `Generic Types` that need to be understood.


## What is a generic type?


Assuming we had a class called `MyStack`

```
class MyStack[A] {

    //some code here
}
```

We say that `MyStack` is generic because whatever code we write inside of of the `MyStack` class, inside the `{}`, will work for any type `A`. This is what we mean by a type being generic.


## What is a Type Constructor `[_]` or a higher kinded type?.

A type constructor is something like a function which take a type as an argument and returns a type.

```
(Int) => Int  

or

String => String
```

so a type constructor `List[_]` is just a function of type

`(A) => List[A]`

or 

`String => List[String]`

