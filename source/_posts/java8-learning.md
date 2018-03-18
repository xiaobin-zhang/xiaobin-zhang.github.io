---
title: java8-learning
date: 2018-03-18 15:41:38
comments: true
tags: 
- java8
categories:
- 技术分享
---
{% asset_img java8.jpg logo %}
自认为：如果想要真正的掌握一门技术，仅仅是通过片段的学习是不够的，一定要形成自己知识体系。当然，这里不包括一些“天才”。像吾等屌丝，没有过目不忘的本领，还是乖乖的埋头苦耕吧。
前段时间在网上看到java9新特性一篇文章，纳尼？？java9？？相信现在有很多人还停留在java7阶段，包括我，突然脑海回想起一首歌：“不是我不明白，是世界变化快”。
虽然做不了时代的弄潮儿，但是，也不能成为站在岸上的吃瓜群众吧！既然这样，那还等什么呢，撸起袖子搞起来。获取完整代码： https://github.com/xiaobin-zhang/java8learning.git

## Table of Contents
* 函数式接口

## 1)函数式接口：FunctionalInterface
### 函数式接口的定义

* 注解：@FunctionalInterface
* 函数式接口不允许是空接口,也不允许只有默认方法和静态方法
* 如果有抽象方法，则只能有一个抽象方法
* 抽象方法和普通方法在函数式接口中不允许共存
* 可以有多个默认方法，默认方法必须要有方法体
* 可以有多个静态方法，静态方法必须要有方法体,静态方法只能通过接口调用

	{% codeblock FunctInterf lang:java https://github.com/xiaobin-zhang/java8learning/tree/master/src/com/java8/features/feature1/topic1 FunctInterf.java %}
		@FunctionalInterface
		public interface FunctInterf {
		
			//函数式接口中，有且只能有一个抽象方法
			abstract void abstractMet();
			
			//默认方法
			default void defaultMethod1() {
				System.out.println("this is FunctionalInterface's default method 1");
			}
			
			//默认方法
			default void defaultMethod2() {
				System.out.println("this is FunctionalInterface's default method 2");
			}
			
			//静态方法
			static void staticMethod1() {
				System.out.println("this is FunctionalInterface's static method 1");
			}
			
			//静态方法
			static void staticMethod2() {
				System.out.println("this is FunctionalInterface's static method 2");
			}
		}
	{% endcodeblock %}

### 函数式接口的使用

* 实现类必须重写接口的抽象方法
* 如果实现类实现了多个函数式接口，也必须重写全部接口的抽象方法
* 如果多个函数式接口中存在相同的默认方法，则必须重写该默认方法，在重写默认方法是，可以指定使用哪个接口中的方法，也可以全部不用，完全重写
