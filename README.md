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

The intention of [self-documenting](http://en.wikipedia.org/wiki/Self-documenting) code is to replace comments with self-evident code. For example:

*BAD* (have to mental-parse regex and/or trust the comment):

```scala
val numberRegex = """(\Q-\E)?\d+((\Q.\E)\d+)?""" // negative sign followed by digits, followed by optional fraction part
```

*GOOD* (code describes what it is and assert in end to make sure no magic was done by the library):

```scala
val fraction = $.andThen(".").digits()
val number = $.maybe("-").digits().maybe(fraction)
val pattern: java.util.regex.Pattern = number.compile    // the compiled pattern
assert(number.regexp == numberRegex)

assert(Seq("3", "-4", "-0.458") forall number.check)
assert(Seq("0.", "hello", "4.3.2") forall number.notMatch)
```

Performance: The library is essentially [a builder](http://stackoverflow.com/questions/328496/) around a regex string. So, it is as fast as using vanilla regex strings.

sbt
===
Add the following to your `build.sbt`:
```scala
resolvers += "Sonatype releases" at "http://oss.sonatype.org/content/repositories/releases/"

libraryDependency += "com.github.verbalexpressions" %% "ScalaVerbalExpressions" % "1.0.0"
```
