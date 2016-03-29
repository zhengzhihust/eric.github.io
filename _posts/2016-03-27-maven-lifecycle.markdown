---
layout: post
title:  "Maven生命周期"
date:   2016-03-27 22:39:00 +0800
categories: jekyll update
group: navigation
author: Eric Zheng
tags: Java
---

Maven有三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”，初学者容易将Maven的生命周期看成一个整体，其实不然。这三套生命周期分别是：

- Clean Lifecycle 在进行真正的构建之前进行一些清理工作。

- Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。

- Site Lifecycle 生成项目报告，站点，发布站点。

我再次强调一下它们是相互独立的，你可以仅仅调用clean来清理工作目录，仅仅调用site来生成站点。当然你也可以直接运行 mvn clean install site 运行所有这三套生命周期。

知道了每套生命周期的大概用途和相互关系以后，来逐个详细看一下每套生命周期，Clean和Site相对比较简单，先解释一下。

每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行mvn clean ，这个的clean是Clean生命周期的一个阶段。有点绕？要知道有Clean生命周期，也有clean阶段。Clean生命周期一共包含了三个阶段：

- pre-clean  执行一些需要在clean之前完成的工作

- clean  移除所有上一次构建生成的文件

- post-clean  执行一些需要在clean之后立刻完成的工作

mvn clean 中的clean就是上面的clean，在一个生命周期中，运行某个阶段的时候，它之前的所有阶段都会被运行，也就是说，mvn clean 等同于 mvn pre-clean clean ，如果我们运行 mvn post-clean ，那么 pre-clean，clean 都会被运行。这是Maven很重要的一个规则，可以大大简化命令行的输入。

下面看一下Site生命周期的各个阶段：

- pre-site     执行一些需要在生成站点文档之前完成的工作

- site    生成项目的站点文档

- post-site     执行一些需要在生成站点文档之后完成的工作，并且为部署做准备

- site-deploy     将生成的站点文档部署到特定的服务器上

这里经常用到的是site阶段和site-deploy阶段，用以生成和发布Maven站点，这可是Maven相当强大的功能，Manager比较喜欢，文档及统计数据自动生成，很好看。

最后，来看一下Maven的最重要的Default生命周期，绝大部分工作都发生在这个生命周期中，这里，我只解释一些比较重要和常用的阶段：

- validate

- generate-sources

- process-sources

- generate-resources

- process-resources     复制并处理资源文件，至目标目录，准备打包。

- compile     编译项目的源代码。

- process-classes

- generate-test-sources

- process-test-sources

- generate-test-resources

- process-test-resources     复制并处理资源文件，至目标测试目录。

- test-compile     编译测试源代码。

- process-test-classes

- test     使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。

- prepare-package

- package     接受编译好的代码，打包成可发布的格式，如 JAR 。

- pre-integration-test

- integration-test

- post-integration-test

- verify

- install     将包安装至本地仓库，以让其它项目依赖。

- deploy     将最终的包复制到远程的仓库，以让其它开发人员与项目共享。

基本上，根据名称我们就能猜出每个阶段的用途，关于其它阶段的解释，请参考:[Apache Maven Introduction to the Build Lifecycle](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

命令行：

```maven
mvn clean
mvn test
mvn clean
mvn clean deploy site-deploy
mvn clean package -P deploy -Dmaven.test.skip=true  //本地打包
mvn clean compile -U
```
