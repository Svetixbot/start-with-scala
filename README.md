# Funcitonal Programming

## What is funcitonal programming

it's programming with functions!

### Let's create some functions!

```scala
def findSymbol(symbol: String, line: String): Boolean = line.contains(symbol)
def findSEverywhere(line: String): Boolean = line.contains("S")
```
#### Currying
```scala
def findSymbol(symbol: String)(line: String): Boolean = line.contains(symbol)
def findSEverywhere: String => Boolean = findSymbol("S")
```

#### Dependency injection with currying
```scala
trait DB {
  def getConnection(): String
}

case class RealDB() extends DB {
  override def getConnection(): String = "real"
}

case class Noop() extends DB {
  override def getConnection(): String = "noop"
}

type Customer = String

def getCustomerById(db: DB)(id: Int): Customer = {
  "such customer"
}

val noopStuff = getCustomerById(Noop()) _ 

```

#### Pattern matching

```
trait Command
case class Translate(from: String, to: String) extends Command
case class Run(executable: String) extends Command

val command = ???

command match {
  case Translate(from, to) => ???
  case Run(execute) => ???
}
```

#### Immutability
```scala

val list = List(1,2,3,4,5)

def foldMuch[T](list: List[Int])(seed: T)(f: (T, Int) => T): T = {
  list match {
    case head::tail => foldMuch(tail)(f(seed, head))(f)
    case _ => seed
  }
}

val result = foldMuch[String](list)("")((s, v) => s + "a")


```

#### Partial functions
```scala
val one: PartialFunction[Int, String] = { case 1 => "one" }

val two: PartialFunction[Int, String] = { case 2 => "two" }

one(1)
one(2)

val onetwo = one orElse two

onetwo(2)
```

#### Intro to monads
```scala

trait Either[L,R]
case class Right(right: R) extends Either
case class Left(left: L) extends Either

type Error = String
def parseInt(value: String): Either[Error, Int] = 
  try {
    Right(value.toInt)
  } catch {
    case e: Exception => Left(e.toString)
  }

val result = parseInt("1")

result.map(println)

```

#### Monadic composition

```scala
val input = "Svetlana Marina"

case class Name(first: String, last: String)

def parseFirstName(input: String): Option[String] = {
  input.split(" ").toList match {
    case first::_ => Some(first)
    case _ => None
  }
}
def parseLastName(input: String): Option[String] = {
  input.split(" ").toList match {
    case first::last ::_ => Some(last)
    case _ => None
  }
}

val output: Option[Name] = for {
  first <- parseFirstName(input)
  last <- parseLastName(input)
} yield Name(first, last)

```