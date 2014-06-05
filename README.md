[![Build Status](https://travis-ci.org/pathikrit/ScalaVerbalExpressions.png?branch=master)](http://travis-ci.org/pathikrit/ScalaVerbalExpressions) [![Coverage Status](https://coveralls.io/repos/pathikrit/ScalaVerbalExpressions/badge.png)](https://coveralls.io/r/pathikrit/ScalaVerbalExpressions)

ScalaVerbalExpressions
=====================

```scala
import com.github.verbalexpressions.VerbalExpression
import VerbalExpression._

val validUrl = $.startOfLine()
                .andThen("http")
                .maybe("s")
                .andThen("://")
                .maybe("www.")
                .anythingBut(" ")
                .endOfLine()

assert("https://www.google.com" is validUrl)
assert("ftp://home.comcast.net" isNot validUrl)
```

For more methods, checkout the [wiki](https://github.com/VerbalExpressions/JSVerbalExpressions/wiki) and the [source](src/main/scala/com/github/verbalexpressions/VerbalExpression.scala)

The intention of literate programming is to replace code comments with self evident code. For example:

BAD:

```scala
val numberRegexp = """(\Q-\E)?\d+((\Q.\E)\d+)?""" // negative sign followed by digits, followed by optional fraction part
```

GOOD:

```scala
val fraction = $.andThen(".").digits()
val number = $.maybe("-").digits().maybe(fraction)
val pattern = number.compile    // the compiled pattern
assert(number.regexp == """(\Q-\E)?\d+((\Q.\E)\d+)?""") // verify the regex

assert(Seq("3", "-4", "-0.458") forall number.check)
assert(Seq("0.", "hello", "4.3.2") forall number.notMatch)
```

sbt
===
Add the following to your `build.sbt`:
```scala
resolvers += "Sonatype releases" at "http://oss.sonatype.org/content/repositories/releases/"

libraryDependency += "com.github.verbalexpressions" %% "ScalaVerbalExpressions" % "1.0.0"
```
