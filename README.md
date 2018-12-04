# groovy-parser(Parrot)
[![CircleCI](https://circleci.com/gh/danielsun1106/groovy-parser.svg?style=svg)](https://circleci.com/gh/danielsun1106/groovy-parser)
[![Java 8+](https://img.shields.io/badge/java-8+-4c7e9f.svg)](http://www.oracle.com/technetwork/java/javase/downloads)
[![Apache License 2](https://img.shields.io/badge/license-APL2-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.txt)
[![Join the chat at https://gitter.im/groovy-parser/Lobby](https://badges.gitter.im/groovy-parser/Lobby.svg)](https://gitter.im/groovy-parser/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![follow on Twitter](https://img.shields.io/twitter/follow/daniel_sun.svg?style=social)](https://twitter.com/intent/follow?screen_name=daniel_sun)

The new parser(Parrot) can parse Groovy source code and construct the related AST, which is almost identical to the one generated by the old parser(except the corrected node position, e.g. line, column of node). Currently all features of Groovy are available. In addition, **the following new features have been added:**

* do-while loops; enhanced (now supporting commas) classic for loops, e.g. `for(int i = 0, j = 10; i < j; i++, j--) {..}`)
* lambda expressions, e.g. `stream.map(e -> e + 1)`
* method references and constructor references
* try-with-resources, AKA ARM
* code blocks, i.e. `{..}`
* Java style array initializers, e.g. `new int[] {1, 2, 3}`
* default methods within interfaces
* additional places for type annotations
* new operators: identity operators(`===`, `!==`), elvis assignment(`?=`), `!in`, `!instanceof`
* safe index, e.g. `nullableVar?[1, 2]`
* non-static inner class instantiation, e.g. `outer.new Inner()`
* runtime groovydoc, i.e. groovydoc starting with `/**@`; groovydoc attached to AST node as metadata

**JVM system properties to control parsing:**

* `groovy.antlr4.cache.threshold`: how frequently to clear DFA cache(default: 64). **Notice:** The more frequently the DFA cache is cleared, the poorer parsing performance will be(you can not set the value that is less than the default value). But the DFA cache has to be cleared to avoid OutOfMemoryError's occurring.
* `groovy.clear.lexer.dfa.cache`: whether to clear the dfa cache of lexer(default: false)
* `groovy.attach.groovydoc`: whether to attach groovydoc to node as metadata while parsing groovy source code(default: false)
* `groovy.attach.runtime.groovydoc`: whether to attach `@Groovydoc` annotation to all members which have groovydoc(i.e. `/**@ ... */`)
* `groovy.extract.doc.comment`: whether to collect groovydoc while parsing groovy source code(default: false). **DEPRECATED, USE `groovy.attach.groovydoc` INSTEAD** 

**Parrot is based on the highly optimized version of antlr4(com.tunnelvisionlabs:antlr4), which is licensed under BSD. On 20161103 Parrot was contributed to Apache Groovy, but the project will be maintained as a lab to experiment new features for Groovy. You can find it at [apache/groovy](https://github.com/apache/groovy/tree/master/subprojects/parser-antlr4).**

### Sample Code
* [Sample code from Apache Groovy 3.0 release note](http://groovy-lang.org/releasenotes/groovy-3.0.html)

### FAQ

##### Question(from Slack):
```
Can someone explain to me the importance of the Parrot compiler? Basically explain like I am 5?
```
##### Answer(by Guillaume Laforge, Project Lead of Apache Groovy):
```
the syntax of Groovy hasn’t evolved in a long time
the current / old parser is a bit complicated to evolve
and is using a very old version of the parsing library
so any change we’d want to make to the language (a new operator, for example) becomes very complicated
So we’ve been wanting to upgrade the underlying parser library for a while, but since the library evolved a lot, that also required a rewrite of the grammar of the language
But there’s another thing to consider
Groovy’s always been adopted by Java developers easily because of how close to the Java syntax it’s always been
so most Java programs are also valid Groovy programs
it’s been important to Groovy’s success to have this source compatibility
Java 8's been out for a while already
and we’ve been asked countless times if we’d support this or that particular syntax enhancement from Java 8
for “copy’n paste compatibility”, if you will
We decided to upgrade to a newer version of our parsing library (from v2 to v4 of Antlr)
to allow Groovy’s syntax to continue to evolve
to also support new operators and things like that
but to also support Java 8 constructs, for continued compatibility
And that’s about it
```
