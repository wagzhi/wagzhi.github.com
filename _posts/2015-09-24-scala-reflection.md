---
layout: post
title: "Scala反射"
description: ""
category: 
tags: [scala, reflection]
---
{% include JB/setup %}
本文翻译自[Scala Reflection](http://docs.scala-lang.org/overviews/reflection/overview.html)

##概述

###Heather Miller, Eugene Burmako, Philipp Haller
反射是程序去检查、甚至改变自己的能力。它的历史贯穿面向对象、函数式编程和逻辑编程范式。一些语言把反射作为指导原则，并不断改进其反射的能力。

反射涉及到将程序中的隐式元素具体化的能力。这些元素包含静态程序元素如类、方法、表达式，也包含动态元素，如当前的协程（Continuation)或者执行事件如方法调用、属性访问等。通常区别编译时（compile-time)反射和运行时（runtime）反射是依据反射执行的时间。编译时反射是开发代码转换和生产程序的强大方式，而运行时反射典型使用是在语义适应或者模块间的延迟绑定。

直到2.10，Scala没有自身的反射能力。而通过Java反射API获得动态检测类和对象，并访问它们的成员的能力。但是，许多Scala特有的元素仅通过Java反射是无法发现的，Java反射只能发现Java元素（没有functions和traits）和类型（没有existential，higher-kinded，path-dependent和abtract types）。另外，Java反射也不能发现编译时泛型的Java类型的运行时运行信息；这是对于Scala泛型反射的限制。

在Scala 2.10，新引入的反射类库不仅Java运行时反射对Scala特有类型和泛型的缺陷，而且还为Scala提供了拥有强大的通用反射能力的工具包。不仅为Scala类型和泛型提供了功能齐全的运行时反射，Scala 2.10还通过[marcros](http://docs.scala-lang.org/overviews/macros/overview.html)的形式提供了编译时反射能力，以及实例化Scala表达式为抽象语法树的能力。

###运行时反射

什么是运行时反射？ 在运行时给定一个类型或者一个对象实例，反射可以：

* 检测对象的类型，包括泛型。
* 实例化一个新对象。
* 访问或者调用对象的成员。

让我们通过一些例子来看看以上功能是如何实现的。

####例子

#####检测运行时类型（包括运行时泛型）

和其他JVM语言一样，Scala类型会在编译时被清除。也就是说，如果你在运行时去检测一个实例的类型，你可能无法访问所以Scala的所有编译时类型信息。

TypeTags是一种承载所以类型信息的对象，在编译时和运行时都可用。需要指出的事TypeTags总是在编译时产生的。当隐式参数或者上下文绑定需要TypeTags被使用时会触发TypeTags生成。

下面例子使用上下文绑定：

```

scala> import scala.reflect.runtime.{universe => ru}
import scala.reflect.runtime.{universe=>ru}

scala> val l = List(1,2,3)
l: List[Int] = List(1, 2, 3)

scala> def getTypeTag[T: ru.TypeTag](obj: T) = ru.typeTag[T]
getTypeTag: [T](obj: T)(implicit evidence$1: ru.TypeTag[T])ru.TypeTag[T]

scala> val theType = getTypeTag(l).tpe
theType: ru.Type = List[Int]

```

首先import `scala.reflect.runtime.universe`（必须先导入以便使用TypeTag），然后定义一个类型为`List[Int]`的`l`。然后我们定义一个方法




