# 入门指南

每个人都必须从某个地方开始，如果你是新手，这就是起点。

## 在开始之前

为了有效地使用Gradle，您需要知道它是什么，并理解它的一些基本概念。所以在你认真使用Gradle之前，我们强烈推荐你阅读 [什么是Gradle?](./what_is_gradle.md)

即使你有使用Gradle的经验，我们建议你阅读[关于Gradle你需要知道的5件事](./what_is_gradle.md#关于Gradle你需要知道的5件事)，因为它澄清了一些常见的误解。

## 安装

如果您只想运行现有的Gradle项目，如果项目中带有 [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#gradle_wrapper)，可以通过项目根目录中的文件 *gradlew*  或 *gradlew* 识别，那么您就不需要安装Gradle。您只需要确保您的系统满足 [Gradle的先决条件](https://docs.gradle.org/current/userguide/installation.html#sec:prerequisites)。

**Android Studio** 自己附带了一个 Gradle，所以在这种情况下也不需要单独安装Gradle。

为了创建新的项目或向现有构建添加 Wrapper(包装器)，您需要根据这些 [说明](./installation.md) 安装Gradle。请注意，除了页面上描述的方法之外，可能还有其他方法来安装Gradle，因为不可能列举所有的包管理器。


## 尝试使用Gradle

积极使用Gradle是一个很好的学习它的方法，所以一旦你安装了Gradle，尝试下列介绍性的实践教程:

- [创建基础Gradle项目](https://guides.gradle.org/creating-new-gradle-builds/)
- [构建Android应用程序](https://guides.gradle.org/building-android-apps/)
- [构建Java库](https://guides.gradle.org/building-java-libraries/)
- [构建Kotlin JVM库](https://guides.gradle.org/building-kotlin-jvm-libraries/)
- [构建c++库](https://guides.gradle.org/building-cpp-libraries/)
- [创建 build scans](https://guides.gradle.org/creating-build-scans/) (构建扫描)

还有许多其他 [教程和指南](https://guides.gradle.org/) 可供选择，您可以根据类别进行筛选——例如 [基础知识](https://guides.gradle.org/?q=Fundamentals)。

# 命令行 和 IDE（集成开发环境）

有些人是命令行的铁杆用户，而另一些人则宁愿永远不离开IDE。许多人很高兴地同时使用这两种方法，而也Gradle不去歧视任一种方法。多个 [主流的IDE](https://docs.gradle.org/current/userguide/third_party_integration.html#ides) 都支持 Gradle，所有可以从命令行完成的工作都可以通过 [工具API](https://docs.gradle.org/current/userguide/embedding.html#embedding) 提供给IDE。

**Android Studio** 和 **IntelliJ IDEA** 的用户可以考虑在编辑时使用  [Kotlin DSL 脚本](https://docs.gradle.org/current/userguide/kotlin_dsl.html#kotlin_dsl) 来获得高级IDE的功能支持。

## 执行 Gradle 构建
如果您遵循 [上面链接](#尝试使用Gradle) 的任何教程，您将执行Gradle构建。但是，如果你得到了一个没有任何说明的Gradle项目，你会怎么做呢?

以下是一些有用的步骤:

1.  确定项目是否有Gradle Wrapper（包装器），如果有就 [使用它](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:using_wrapper)——当 Wrapper（包装器）可用时，主IDE默认会使用 Wrapper（包装器）。

2. 检查项目结构。  
要么用IDE导入构建，要么从命令行运行gradle项目。如果只列出根项目，则为单个项目。否则，它是一个 [多项目结构](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html#intro_multi_project_builds)。

3. 找出可以运行哪些任务。  
如果您已经将项目导入IDE，那么您能够访问一个显示所有可用任务的视图。从命令行运行 `gradle <taskname>`。

4. 通过了解更多关于任务的信息可以通过命令 `gradle help --task <taskname>`。  
gradle help 可以显示关于任务的额外信息，包括哪些项目包含该任务以及该任务支持哪些选项。

5. 运行您感兴趣的任务。  
许多遵守标准约定的Gradle项目都集成了一套生命周期任务，所以当您没有更具体的想要在构建中做的事情时，可以直接使用这些构建。例如，大多数项目构建都有清理、检查、组装和构建等任务。  
在命令行中，只需运行 `gradle <taskname>` 命令就可以执行指定的任务。有关命令行执行的更多信息，请参阅相应的 [用户手册一章](https://docs.gradle.org/current/userguide/command_line_interface.html#command_line_interface)。如果您正在使用IDE，请查看它的文档，了解如何运行任务。

Gradle项目都遵守标准的项目结构和约定的任务，如果你熟悉其他相同类型的项目，如 Java、Android 或者 系统原生项目，那么文件和目录结构、任务和项目属性都相差无几。

对于更专业化的项目或需要高度定制的项目，理想情况下您应该能够访问关于如何运行项目构建以及可以配置哪些 [构建属性](https://docs.gradle.org/current/userguide/build_environment.html#build_environment) 的文档.

## 维护Gradle项目

学习创建和维护Gradle项目是一个过程，需要一点时间。我们建议您从适合您的项目的核心插件及其约定开始，然后随着您对该工具的了解越来越多，逐步合并定制。

下面是一些在你第一步掌握Gradle旅程中非常有用的要点：

1. 尝试一到两个 [基本教程](#尝试使用Gradle)，看看Gradle构建是什么样子的，特别是那些与您使用的项目类型(Java、native、Android等等)匹配的类型。

2. 确保你已经阅读了 [关于Gradle你需要知道的5件事](./what_is_gradle.md#关于Gradle你需要知道的5件事)!

3. 了解Gradle项目的基本元素：[项目](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:projects_and_tasks)、[任务](https://docs.gradle.org/current/userguide/more_about_tasks.html#more_about_tasks) 和 [文件API](https://docs.gradle.org/current/userguide/working_with_files.html#working_with_files)。

4. 如果您正在为JVM构建软件，请务必阅读在 [构建Java & JVM项目](https://docs.gradle.org/current/userguide/building_java_projects.html#building_java_projects) 和在 [Java & JVM项目中测试](https://docs.gradle.org/current/userguide/java_testing.html#java_testing) 这些类型的项目的细节。

5. 熟悉Gradle附带的 [核心插件](https://docs.gradle.org/current/userguide/plugin_reference.html#plugin_reference) ，因为它们提供了许多开箱即用的有用功能。

6. 学习如何 [编写可维护的构建脚本](https://docs.gradle.org/current/userguide/authoring_maintainable_build_scripts.html#authoring_maintainable_build_scripts)，并 [更好地组织Gradle项目](https://docs.gradle.org/current/userguide/organizing_gradle_projects.html#organizing_gradle_projects)。

用户手册包含了许多其他有用的信息，你可以在 [Gradle指南](https://guides.gradle.org/) 中找到更多关于各种Gradle特性的教程。

## 集成第三方工具

Gradle的灵活性意味着它可以很容易地与其他工具一起工作，例如我们的 [第三方工具](https://docs.gradle.org/current/userguide/third_party_integration.html#third_party_integration) 页面上列出的那些工具。

集成方式主要有两种：

- 工具通过 [工具API](Tooling API) 驱动Gradle —— 使用它提取关于项目构建的信息并运行它。

- Gradle通过第三方工具的API的调用或生成工具的信息 —— 这通常是通过插件和自定义任务类型完成的。

现有的 java 开放 api 的工具集成起来很方便。您可以在Gradle的 [插件市场](https://plugins.gradle.org/) 上找到许多这样的集成工具。