# 教程和指南

您可以在这里找到基于项目的教程和专题指南，无论您是新手还是经验丰富的大师，这里提供的指南都是为帮助您实现目标而设计的。

## 入门

逐步学习如何使用Gradle，一般和具体的任务。

### [创建 Build Scans](https://guides.gradle.org/creating-build-scans/)

了解如何为Gradle构建启用构建扫描。 添加插件和许可协议，执行扫描并查看结果。 使用init脚本为所有构建添加构建扫描功能。

> 8分钟

### [创建一个新的 Gradle 项目](./creating-new-gradle-builds.md)

学习适用任何项目的通用任务。查看可用的任务、生成 **Gradle Wrapper**、将数据从一个位置复制到另一个位置、添加插件等等。

> 12分钟

### [创建一个多项目构建](https://guides.gradle.org/creating-multi-project-builds/)

通过开发包含库和文档的应用程序开始多项目构建。

> 27分钟

### [构建Java库](https://guides.gradle.org/building-java-libraries/)

使用Build Init插件创建Java项目，运行测试并查看测试报告，生成API文档，并自定义生成可交付的jar。

> 13分钟

### [构建Groovy库](https://guides.gradle.org/building-groovy-libraries/)

使用Build Init插件创建一个Groovy项目，构建它，运行测试并查看测试报告，并生成API文档。

> 8分钟

### [构建Kotlin JVM库](https://guides.gradle.org/building-kotlin-jvm-libraries/)

用Kotlin编写一个简单的库，并用JUnit测试，用Dokka生成API文档，当然还要使用Gradle的Kotlin DSL发布它。

> 15分钟

### [构建Scala库](https://guides.gradle.org/building-scala-libraries/)

使用Build Init插件创建一个Scala项目，构建运行测试并查看测试报告，并生成API文档。

> 8分钟

### [构建Java应用程序](https://guides.gradle.org/building-java-applications/)

使用Build Init插件创建具有依赖关系的客户端Java应用程序。使用应用程序插件配置可执行jar。构建项目，运行测试并查看测试报告，然后作为命令行应用程序执行项目。

> 9分钟

### [构建Java Web应用程序](https://guides.gradle.org/building-java-web-applications/)

创建一个使用Java、WAR和Gretty插件的简单web应用程序。使用Gretty部署应用程序。使用Mockito和Selenium进行测试，它们都由Gradle协调。

> 21分钟

### [用Gradle构建Spring Boot 2应用程序](https://guides.gradle.org/building-spring-boot-2-projects-with-gradle/)

使用Gradle内置的BOM支持构建和运行一个简单的Spring Boot 2应用程序。

> 13分钟

### [使用JVM库](https://guides.gradle.org/consuming-jvm-libraries/)

通过构建一个小型Java应用程序，了解如何使用外部JVM工件。

> 12分钟

### [构建C语言项目](https://guides.gradle.org/building-c-executables/)

创建一个C语言项目。添加一个C源文件和头文件。使用Gradle生成命令行可执行文件。运行应用程序并查看结果，以及编译器和链接器的输出。

> 6分钟

### [构建C++项目](https://guides.gradle.org/building-cpp-executables/)

创建一个C++项目。添加一个C++源文件和头文件。使用Gradle生成命令行可执行文件。运行应用程序并查看结果，以及编译器和链接器的输出。

> 6分钟

### [构建C++库](https://guides.gradle.org/building-cpp-libraries/)

为c++库创建一个项目。添加一个c++源文件和头文件。使用Gradle生成静态可链接和动态可链接的库。查看编译器和链接器的输出。

> 14分钟

### [在 Jenkins 上运行Gradle](https://guides.gradle.org/executing-gradle-builds-on-jenkins/)

了解如何在Jenkins的上配置和执行Gradle。

> 6分钟

### [在 TeamCity 上运行Gradle](https://guides.gradle.org/executing-gradle-builds-on-teamcity/)

了解如何在TeamCity的上配置和执行Gradle。

> 7分钟

### [在 Travis CI 上运行Gradle](https://guides.gradle.org/executing-gradle-builds-on-travisci/)

了解如何在 Travis CI 的上配置和执行Gradle。

> 5分钟

### [在Gradle上运行Webpack](https://guides.gradle.org/running-webpack-with-gradle/)

定义一个Gradle任务，利用Gradle的最新检查和构建缓存的方式将web资产与Webpack绑定。演示缓存行为和增量构建功能。

> 11分钟

### [自定义Gradle任务](https://guides.gradle.org/writing-gradle-tasks/)

学习如何使用内置DSL编写可重用的Gradle任务和创建自己的任务类。

> 9分钟

### [编写Gradle插件](https://guides.gradle.org/writing-gradle-plugins/)

编写一个简单的Gradle插件，实现适当的接口并创建一个自定义任务类来支持它。创建插件标识符和项目中使用新插件。

> 11分钟

### [发布插件到 Gradle 插件市场](https://guides.gradle.org/publishing-plugins-to-gradle-plugin-portal/)

了解如何将自己的插件发布到免费Gradle插件市场。注册之后，将生成的键添加到自己的Gradle属性中。使用带有必要元数据的发布插件来发布。

> 16分钟

## 专题

专家对有关特定主题的深入建议。

### [设计Gradle插件](https://guides.gradle.org/designing-gradle-plugins/)

根据既定实践正确设计Gradle插件，并将其应用于您自己的项目。 重点关注支持良好性能和易维护的技术和技术。

> 15分钟

### [实现Gradle插件](https://guides.gradle.org/implementing-gradle-plugins/)

使用自定义任务类型、增量构建支持和配置选择建议创建Gradle插件。

> 40分钟

### [测试Gradle插件](https://guides.gradle.org/testing-gradle-plugins/)

了解如何以手动和自动方式有效地测试插件。 演示复合构建，主流的基于JVM的测试框架和Gradle TestKit的使用。

> 26分钟

### [使用 Worker API](https://guides.gradle.org/using-the-worker-api/)

了解如何使用Worker API为自定义任务启用细粒度并行性和隔离。本指南将指导您完成将现有自定义任务转换为使用Worker API的过程。

> 25分钟

### [从Maven迁移到Gradle](https://guides.gradle.org/migrating-from-maven/)

学习如何将Maven项目转换为Gradle项目。讨论构建工具中的差异、可以自动化的内容、依赖范围的选择、如何测试结果等等。

> 24分钟

### [优化Gradle性能](https://guides.gradle.org/performance/)

使用构建扫描、依赖项解析策略、并行执行、增量和部分构建等改进构建的性能。包括针对Java和Android项目的具体建议。

> 50分钟

### [使用构建缓存](https://guides.gradle.org/using-build-cache/)

了解Gradle构建缓存以改进性能的细节。包括常见问题的解决方案、调试和诊断缓存丢失、与其他开发人员共享等等。

> 102分钟

### [将构构建脚本从Groovy迁移成Kotlin](https://guides.gradle.org/migrating-build-logic-from-groovy-to-kotlin/)

了解如何使用Gradle Kotlin DSL将构建逻辑从Groovy迁移到Kotlin。发现痛点并讨论迁移策略。包括常见问题的解决方案和互操作性配方。

> 31分钟

## 教程

与入门教程类似，但更有深度。

### [构建Java 9模块](https://guides.gradle.org/building-java-9-modules/)

如何修改常规Java库和应用程序插件添加的任务，以使它们与Java 9模块一起工作。

> 39分钟
