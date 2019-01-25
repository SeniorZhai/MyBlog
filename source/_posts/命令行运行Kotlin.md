title: 命令行运行Kotlin
date: 2018-11-28 20:36:23
categories: Kotlin
tags: [Coroutine,Kotlin,Android]
---

<!--more-->
```shell
$ brew update
$ brew install kotlin
```
运行

```kt
// Hello.kt
fun main(args : Array<String>) {
    println("Hello world!")
}
```

```
kotlinc hello.kt -include-runtime -d Hello.jar
java -jar Hello.jar
```