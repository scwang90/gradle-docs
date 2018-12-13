# 安装Gradle

您可以在Linux、macOS或Windows上安装Gradle构建工具。本文档介绍如何使用像SDKMAN、Homebrew、或Scoop这样的包管理器进行安装，还有手动安装。

我们推荐使用 [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:upgrading_wrapper) 来更新Gradle。

您可以在 [发布页面](https://gradle.org/releases) 上找到所有版本及其校验值。

## 准备

Gradle 可以在所有主流的操作系统上运行，只要安装了 [Java Development Kit](http://jdk.java.net/)(Jdk) 并且版本为 1.8 或者更高的版本。要查看安装的 Java 版本，可以在哎命令行中输入 `java -version`， 你应该会看到如下信息：

```
❯ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

Gradle自己附带了Groovy库，因此不需要再安装Groovy。任何外部安装的Groovy安装都会被Gradle忽略。

Gradle会使用您环境变量 `path` 中配置的任何 Jdk 版本。所以您可以配置环境变量 `JAVA_HOME` 只想所需JDK的安装路径。

## 使用包管理器进行安装

[SDKMAN](http://sdkman.io/) 是在大多数基于unix的系统上管理多个软件开发工具包并行版本的工具。

```
❯ sdk install gradle
```

[Homebrew](http://brew.sh/) 是 macOS 不可或缺的报管理工具。
