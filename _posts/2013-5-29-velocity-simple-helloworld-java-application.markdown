---
layout: post
title: Velocity入门级HelloWorld java application示例
---

由于考虑到Velocity的性能优势及使用简单，特别是Velocity-tool项目中的各种Tool，于是我今天也来试一把Velocity。

按普遍学习新技术的流程三步曲：

- 搜索并打开Velocity项目官网：[Velocity](http://velocity.apache.org/)
- 点击[Downloads](http://velocity.apache.org/download.cgi) 选择下载版本[velocity-1.7.zip](http://apache.dataguru.cn//velocity/engine/1.7/velocity-1.7.zip)和[velocity-tools-2.0.zip](http://apache.dataguru.cn//velocity/tools/2.0/velocity-tools-2.0.zip)
- 将下载好的velocity-1.7.zip解压到适应位置，并创建新的Java Project （名称随意，我的是：velocityStudy ）然后将velocity-1.7.jar和velocity-1.7-dep.jar两个jar包构建到工程的buildpath中，现在我们就可以使用Velocity中的类啦！
- 创建一个Java类，并放到自已定义的包下（io.github.qingtian.velocitystudy）

 类名为：HelloWorldVelocity.java，代码如下：


	package io.github.qingtian.velocitystudy;

	import java.io.StringWriter;
	import org.apache.velocity.Template;
	import org.apache.velocity.VelocityContext;
	import org.apache.velocity.app.VelocityEngine;

	public class HelloWorldVelocity {
		public static void main(String[] args) {
			VelocityEngine ve = new VelocityEngine();
			ve.init();

			Template t = ve.getTemplate("res/helloworld.vm");
			VelocityContext context = new VelocityContext();
			context.put("name", "qingtian");
			context.put("site", "http://qingtian.github.io");

			StringWriter writer = new StringWriter();
			t.merge(context, writer);
			System.out.println("合并后的模板输出：");
			System.out.println(writer.toString());
		}
	}


- 然后在源码包再创建一个名为res的资源包，并创建一个velocity模板（名为：helloworld.vm），内容如下：

> Hello $name , visit $site

- 最后运行HelloWorldVelocity.java，可以看到控制台打印如下内容：

> 合并后的模板输出：

> Hello qingtian , visit http://qingtian.github.io

 至此，Velocity的入门级示例已经完成了！（参考来自：[http://www.knowsky.com/349968.html](http://www.knowsky.com/349968.html)）
