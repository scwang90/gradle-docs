# 安装Gradle

您可以在Linux、macOS或Windows上安装Gradle构建工具。本文档介绍如何使用像SDKMAN、Homebrew、或Scoop这样的包管理器进行安装，还有手动安装。

我们推荐使用 [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:upgrading_wrapper) 来更新Gradle。

您可以在 [发布页面](https://gradle.org/releases) 上找到所有版本及其校验值。

## 准备

Gradle 可以在所有主流的操作系统上运行，只要安装了 [Java Development Kit](http://jdk.java.net/)(Jdk) 并且版本为 1.8 或者更高的版本。要查看安装的 Java 版本，可以在命令行中输入 `java -version`， 你应该会看到如下信息：

```
❯ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

Gradle自己附带了Groovy库，因此不需要再安装Groovy。任何外部安装的Groovy安装都会被Gradle忽略。

Gradle会使用您环境变量 `path` 中配置的任何 Jdk 版本。所以您可以配置环境变量 `JAVA_HOME` 指向所需JDK的安装路径。

## 包管理器安装

[SDKMAN](http://sdkman.io/) 是在大多数基于unix的系统上管理多个软件开发工具包并行版本的工具。

```
❯ sdk install gradle
```

[Homebrew](http://brew.sh/) 是 macOS 不可或缺的报管理工具。

```
❯ brew install gradle
```

[Scoop](http://scoop.sh/) 是受 Homebrew 启发的一个 Windows 命令行安装工具。

```
❯ scoop  install gradle
```

[Chocolatey](https://chocolatey.org/) 是 Windows 的包管理器。

```
❯ choco install gradle
```

[MacPorts](https://www.macports.org/) MacPorts是一个管理macOS工具的系统。

```
❯ sudo port install gradle
```

[↓Proceed to next steps](#下一步)

## 手动安装

### 步骤1、下载最新的Gradle发行版

**Gradle Zip文件** 有两种风格:
 - 只包含可执行的二进制文件
 - 附带文档和源代码的完整版

如果需要使用旧版本，可以参阅 [发布页面](https://gradle.org/releases)。

### 步骤2、解压缩

#### Linux 和 MacOS 用户

在您选择的目录下解压发行版zip文件，例如:

```bat
❯ mkdir /opt/gradle
❯ unzip -d /opt/gradle gradle-5.0-bin.zip
❯ ls /opt/gradle/gradle-5.0
LICENSE  NOTICE  bin  getting-started.html  init.d  lib  media
```

#### Windows 用户
用**文件浏览器**创建一个新目录 `C:\Gradle`。

打开另一个**文件浏览器**窗口并进入 Gradle 的下载目录。鼠标双击 zip 浏览内容，拖动内容文件夹 `gradle-5.0` 到新创建的 `C:\Gradle` 文件夹。

或者，您可以使用自己喜欢的压缩软件把 **Gradle Zip 文件** 解压到 `C:\Gradle` 文件夹中。

### 步骤3、配置系统环境

要运行Gradle，首先要添加环境变量 `GRADLE_HOME` 指向Gradle安装的文件夹 `C:\Gradle`。然后，将 `GRADLE_HOME/bin` 添加到 `path` 环境变量中。这样就可以运行Gradle了。

#### Linux和MacOS用户
配置您的PATH环境变量，以包含未压缩分发版的bin目录，例如:

```
❯ export PATH=$PATH:/opt/gradle/gradle-5.0/bin
```

#### 微软Windows用户
文件资源管理器中右键单击 **这台电脑**(或**计算机**) 图标，然后单击 → `属性` → `高级` → `环境变量`。

在 `系统变量` 下选择 `Path` ，然后单击 `编辑` 为 `C:\Gradle\Gradle-5.0\bin`添加一个条目。单击`确定`保存。

[↓Proceed to next steps](#下一步)

## 验证安装

打开控制台(或Windows命令提示符)，运行 `gradle -v` ，运行gradle并显示版本，例如:

```
❯ gradle -v

------------------------------------------------------------
Gradle 5.0
------------------------------------------------------------

Build time:   2018-02-21 15:28:42 UTC
Revision:     819e0059da49f469d3e9b2896dc4e72537c4847d

Groovy:       2.4.15
Ant:          Apache Ant(TM) version 1.9.9 compiled on February 2 2017
JVM:          1.8.0_151 (Oracle Corporation 25.151-b12)
OS:           Mac OS X 10.13.3 x86_64
```
如果遇到任何问题，请参阅有关故 [障排除安装](https://docs.gradle.org/current/userguide/troubleshooting.html#sec:troubleshooting_installation) 的部分。
您可以通过下载SHA-256文件（可从 [发布页面](https://gradle.org/releases) 获得）并遵循这些 [验证说明](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:verification) 来验证Gradle发行版的完整性。

## 下一步

现在您已经安装了Gradle，可以使用下列资源开始学习:
- 按照 [创建新的Gradle项目](./creating-new-gradle-builds.md/) 教程创建您的第一个Gradle项目。
- 注册一个与核心工程师一起的 [入门级培训](https://gradle.org/training/intro-to-gradle/)。
- 了解如何通过 [命令行界面](https://docs.gradle.org/current/userguide/command_line_interface.html#command_line_interface) 完成常见任务。
- [配置Gradle](https://docs.gradle.org/current/userguide/build_environment.html#build_environment) ，例如使用HTTP代理下载依赖项。
- 订阅 [Gradle最新资讯](https://newsletter.gradle.com/?_ga=2.166159100.33602168.1544699840-373492736.1525957809) 每月发布和社区更新。