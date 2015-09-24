---
layout: post
title: "Scala反射"
description: ""
category: 
tags: [scala, reflection]
---
{% include JB/setup %}
本文翻译自[Scala Reflection](http://docs.scala-lang.org/overviews/reflection/overview.html)

#概述

###Heather Miller, Eugene Burmako, Philipp Haller
反射是程序去检查、甚至改变自己的能力。它的历史贯穿面向对象、函数式编程和逻辑编程范式。一些语言把反射作为指导原则，并不断改进其反射的能力。

反射涉及到将程序中的隐式元素具体化的能力。这些元素包含静态程序元素如类、方法、表达式，也包含动态元素，如当前的协程（Continuation)或者执行事件如方法调用、属性访问等。通常区别编译时（compile-time)反射和运行时（runtime）反射是依据反射执行的时间。编译时反射是开发代码转换和生产程序的强大方式，而运行时反射典型使用是在语义适应或者模块间的延迟绑定。

直到2.10，Scala没有自身的反射能力。而通过Java反射API获得动态检测类和对象，并访问它们的成员的能力。