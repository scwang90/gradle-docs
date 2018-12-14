# 什么是Gradle

## 概述
Gradle是一个开源的自动化构建工具，设计得非常灵活，几乎可以构建任何类型的项目。以下是一些最重要功能的高级概述:
 - 高性能  
Gradle通过只运行由输入到输出之间发生变化而需要运行的任务来避免不必要的工作。您还可以使用构建缓存来重用以前运行甚至不同机器(使用共享的构建缓存)的任务输出。  
Gradle还实现了许多其他优化，开发团队不断地改进Gradle的性能。

 - 基于JVM  
Gradle运行在JVM之上，您必须安装Java开发工具包(JDK)才能使用它。对于熟悉Java平台的用户来说，这是一个额外的好处，因为您可以在项目构建逻辑中使用标准的Java api，例如自定义任务类型和插件。这样Gradle也容易在不同的平台上运行。  
请注意，Gradle不仅限于构建JVM项目，它甚至附带了对构建本地原生（c/c++）项目的支持。  

 - 约定  
Gradle借鉴Maven的做法，通过约定使一些常见的项目(例如Java项目)更加易于构建。应用适当的插件，您就可以轻松地为许多项目生成苗条的构建脚本。但是这些约定并没有限制您：Gradle允许您覆盖它们，添加您自己的任务，并对基于约定的构建进行许多其他定制。

 - 可扩展性  
您可以很容易地扩展Gradle来完成您自己的任务类型和构建模型。请参阅Android构建支持的一个例子:它添加了许多新的构建概念，例如风格和构建类型。

 - IDE支持  
多个主流的IDE（如：**Android Studio、IntelliJ IDEA、Eclipse 和 NetBeans**）都支持导入Gradle项目并建立互交。Gradle还支持生成将项目加载到Visual Studio的解决方案文件。

 - 洞察力  
[Build scans（构建扫描）](https://scans.gradle.com/) 上有大量关于构建运行的信息，您可以使用这些信息来识别和解决构建中遇到的问题。它们可以有效的帮助您识别项目构建性能中的问题。您还可以与他人共享构建扫描，如果您需要在修复构建问题时征求建议，这一点尤其有用。

## 关于Gradle你需要知道的5件事

Gradle是一个灵活、强大的构建工具，当您刚开始使用它的时候，您可能会感到不知所措。然而，理解以下核心原则将使Gradle更加平易近人，您将在了解它之前熟练地使用该工具。

1. Gradle是一个通用的构建工具  
Gradle允许您构建任何项目，因为它很少假设您要构建什么或者应该如何构建。其中唯一的限制可能是依赖关系管理目前只支持Maven和ivy兼容的存储库和文件系统。  
但这并不意味着您将要做大量的工作来创建构建项目。Gradle通过 [插件](https://docs.gradle.org/current/userguide/plugins.html#plugins) 添加一层约定和预先构建的功能，使得一些常见的项目(比如Java库项目)很容易构建。您甚至可以创建和发布自定义的插件来封装自己的约定和构建功能。

2. 基于任务的核心模型  
Gradle的构建模型是一个基于任务（工作单元）的向无环图(DAGs)。这表示构建实际上是配置了一组任务，并根据他们的依赖关系集成到了一起。创建任务图之后，Gradle将确定需要以何种顺序运行哪些任务，然后继续执行它们。

```flow
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes 
or No?|approved:>http://www.baidu.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
```
This diagram shows two example task graphs, one abstract and the other concrete, with the dependencies between the tasks represented as arrows