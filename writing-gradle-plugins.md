# 自定义Gradle插件

本章将指导您在自定义Gradle插件来创建可重用的构建逻辑，然后将其应用于其他Gradle构建。Gradle核心为构建任何类型的项目提供了一个基础架构，但是插件允许构建脚本作者以最少的工作量完成任务，让他们专注于构建什么，而不是如何构建。插件可以应用约定、添加新的任务类型、与第三方工具和库集成等等。例如，`java` 插件提供了标准的源目录布局和任务，例如 `compileJava` 和 `compileTestJava`。本章主要介绍最基本的插件，只是冰山一角。

## 你将会建立什么
您将编写一个简单的插件，它将添加一个新的任务类型，并在项目中使用插件并创建该类型的任务。您还将验证插件是有效的，并通过将插件应用于构建来查看其效果。


## 你需要什么

- 大约11分钟
- 文本编辑器或者 IDE （集成开发环境）
- Java运行时环境（JRE）或Java Development Kit（JDK），版本1.8或更高版本（仅运行Gradle时需要）
- Gradle分发版，版本 5.0 或 更高版本

## 创建一个项目

你需要为这个插件项目创建一个目录并进入该目录:

```bat
$ mkdir greeting-plugin
$ cd greeting-plugin
```

接下来，您将为插件代码创建一个特殊的目录结构:

```bat
$ mkdir -p buildSrc/src/main/java/org/example/greeting
```

> 如果您想了解关于这个 `buildSrc` 目录的更多信息，您可以在 [用户手册](https://docs.gradle.org/4.10-rc-2/userguide/organizing_gradle_projects.html#sec:build_sources) 中了解它是如何允许您组织构建逻辑的。

## 创建插件

在刚刚创建的目录中创建类 `GreetingPlugin`，将其内容设置为:

**buildSrc/src/main/java/org/example/greeting/GreetingPlugin.java**
```java
package org.example.greeting;

import org.gradle.api.Plugin;
import org.gradle.api.Project;

public class GreetingPlugin implements Plugin<Project> {
    public void apply(Project project) {
        //创建一个名为hello的新任务，类型为Greeting(稍后将对此进行定义)
        project.getTasks().create("hello", Greeting.class, (task) -> { 
            task.setMessage("Hello");//为新任务设置默认值
            task.setRecipient("World");//为新任务设置默认值              
        });
    }
}
```

这是插件的本体，其中 `Project` 对象是访问 Gradle Api 的入口，Gradle Api 可以让你在构建脚本中能做的事情在这里也同样能做。在本例中，你创建了一个简单名为 `hello` 的任务。

> 使用 [Gradle DSL](https://docs.gradle.org/4.10-rc-2/dsl/) 引用和 [javadoc](https://docs.gradle.org/4.10-rc-2/javadoc/) 来学习如何使用Gradle API。可以从 [这里](https://docs.gradle.org/4.10-rc-2/dsl/org.gradle.api.Project.html) 开始。

接下来，您将要在插件中创建一个自定义类型的任务类。在与插件相同的包中添加一个新的 `Greeting` 类:

**buildSrc/src/main/java/org/example/greeting/Greeting.java**
```java
package org.example.greeting;

import org.gradle.api.DefaultTask;
import org.gradle.api.tasks.TaskAction;

public class Greeting extends DefaultTask {
    private String message;
    private String recipient;

    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }

    public String getRecipient() { return recipient; }
    public void setRecipient(String recipient) { this.recipient = recipient; }

    @TaskAction
    void sayGreeting() {
        //在任务运行时打印配置的问候语
        System.out.printf("%s, %s!\n", getMessage(), getRecipient()); 
    }
}
```
> 您可以在 [用户手册](https://docs.gradle.org/4.10-rc-2/userguide/custom_tasks.html) 中了解更多关于创建自己的任务类型的信息。

## 在主项目中使用插件

在项目根目录中创建一个构建脚本文件，内容如下:

**build.gradle**
```groovy
//将您的插件应用于当前项目，插件将会把 hello 任务添加到构建中。
apply plugin: org.example.greeting.GreetingPlugin
```
**build.gradle.kts**
```kotlin
apply<org.example.greeting.GreetingPlugin>() 
```

> 这样一句代码就够了，因为插件位于 `buildSrc` 目录中，Gradle 会自动编译并加载 `buildSrc` 中的插件。应用其他非核心 Gradle 插件可以在 [用户手册](https://docs.gradle.org/4.10-rc-2/userguide/plugins.html#sec:binary_plugins) 查看其他的写法。

现在，您可以通过在主构建中运行hello任务来验证插件是否工作:

```bat
$ gradle hello
:buildSrc:compileJava
:buildSrc:compileGroovy UP-TO-DATE
:buildSrc:processResources UP-TO-DATE
:buildSrc:classes
:buildSrc:jar
:buildSrc:assemble
:buildSrc:compileTestJava UP-TO-DATE
:buildSrc:compileTestGroovy UP-TO-DATE
:buildSrc:processTestResources UP-TO-DATE
:buildSrc:testClasses UP-TO-DATE
:buildSrc:test UP-TO-DATE
:buildSrc:check UP-TO-DATE
:buildSrc:build
:hello
Hello, World!
```
从输出的内容可以看出 `buildSrc` 中的文件被 Gradle 视为 `Java` 项目，需要先构建。然后项目中的类就可以在主构建中使用，主构建可以执行指定的任务。

您的构建当前只是使用默认的属性值作为问候语，因此它会输出 “Hello, World!”。 这不一定是这样，因为您可以在构建脚本中直接配置任务:

**build.gradle**
```groovy
hello { //配置名为hello的任务的多个属性
    message = "Hi"
    recipient = "Gradle"
}
```
**build.gradle.kts**
```kotlin
import org.example.greeting.Greeting
tasks.getByName<Greeting>("hello") { 
    message = "Hi"
    recipient = "Gradle"
}
```

> 您可以在 [用户手册](https://docs.gradle.org/4.10-rc-2/userguide/more_about_tasks.html#sec:configuring_tasks) 中了解更多配置任务的语法。

现在，当您运行 `hello` 任务时 (这次使用 `-q` 隐藏 `buildSrc` 的输出) 您将看到以下内容:

```bat
$ gradle -q hello
Hi, Gradle!
```

您的插件现在已经完成了，并且您已经在上面的构建中看到了它的运行结果。我们还想向您展示一件事，它有助于使构建脚本更加整洁，并且在发布插件时也有所帮助:*添加插件标识符*。

## 声明插件标识符

在大多数情况下，应用插件都是指定插件的ID，而不是具体的插件类名，因为它们比更长的类名更容易记住。它们还会生成更整洁的构建文件。所以确保您自己的插件也可以以同样的方式应用是非常有意义的，这就是为什么您现在要为插件声明一个标识符。

创建以下属性文件:

**buildSrc/src/main/resources/META-INF/gradle-plugins/org.example.greeting.properties**
```properties
implementation-class=org.example.greeting.GreetingPlugin
```

Gradle使用这个文件来决定哪个类实现了 `Plugin` （插件）接口。属性文件的名称 （不包括 `.properties` 扩展名）将成为**插件的标识符**。

> 您必须将属性文件放在 `META-INF/Gradle-plugins` 目录中，因为Gradle 会尝试从插件的JAR包中的指定的位置解析该文件。

以上就是定义插件标识符需要做的全部，现在你可以替换下面的构建脚本:

**build.gradle**
```groovy
//pply plugin: org.example.greeting.GreetingPlugin
plugins {//使用插件ID:
  id 'org.example.greeting'
}
```
**build.gradle.kts**
```kotlin
//apply<org.example.greeting.GreetingPlugin>()
plugins {//使用插件ID:
  id("org.example.greeting")
}
```

请注意属性文件的名称 ——`org.example.greeting.properties`—— 映射到上面的ID。

> 请始终把您的插件放在一个唯一的名字空间，时不要像本教程一样都指定为 `org.example`。这样做有助于避免插件之间的名称冲突。您可以在 [用户手册](https://docs.gradle.org/4.10-rc-2/userguide/custom_plugins.html#sec:creating_a_plugin_id) 中找到关于插件id的更多细节。

## 总结

你现在完成了！您已经成功地创建了一个插件并在构建中使用它。一路上，你学会了如何:

- 在插件中编写构建逻辑
- 在buildSrc目录中编写插件类
- 给插件一个ID并将其应用到构建脚本中

本教程主要关注插件的本质，但是大多数插件所提供的特性要丰富得多。下一节将指导您更多地了解插件可以做什么，以及应该如何实现它们。

## 下一步

现在您已经熟悉了构建 Gradle 插件的基本知识，您可能会对以下内容感兴趣:

- [使用Java Gradle plugin development plugin简化插件开发](https://docs.gradle.org/4.10-rc-2/userguide/java_gradle_plugin.html)
- [向Gradle插件市场发布插件](https://guides.gradle.org/publishing-plugins-to-gradle-plugin-portal/)
- [使用扩展对域建模](https://docs.gradle.org/4.10-rc-2/userguide/custom_plugins.html#sec:getting_input_from_the_build) 
- [测试插件](https://docs.gradle.org/4.10-rc-2/userguide/test_kit.html)
- [为新任务类型添加增量构建支持](https://docs.gradle.org/4.10-rc-2/userguide/more_about_tasks.html#sec:up_to_date_checks)


