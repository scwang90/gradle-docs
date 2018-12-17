# 自定义Gradle任务

任务在Gradle中是完成构建的基石，它们表示构建中的单个原子工作片段，例如：创建JAR或链接可执行文件。本文档将指导您使用小型定制任务来定制构建项目。

## 你将要创建什么

您将从创建一个临时 Gradle 任务开始，该任务将 *Hello, World* 打印到控制台。然后，您将使其可配置以打印任何消息。在这个过程中，您将了解一些特殊任务和自定义任务类型。

## 你需要什么

- 大约8分钟
- 文本编辑器或者 IDE （集成开发环境）
- Java运行时环境（JRE）或Java Development Kit（JDK），版本1.8或更高版本（仅运行Gradle时需要）
- Gradle分发版，版本 5.0 或 更高版本

## 创建一个临时任务

在一个新文件夹中创建一个 `build.gradle` 文件(如果您喜欢使用Groovy DSL)或 `build.gradle.kts` 文件(如果您喜欢使用Kotlin DSL)并输入以下命令:

**build.gradle**
```groovy
tasks.register("hello") { //注册一个名为hello的新临时任务。
    doLast { 
        println 'Hello, World!'//添加要打印到控制台的任务操作。
    }
}
```
**build.gradle.kts**
```groovy
tasks.register("hello") { //注册一个名为hello的新临时任务。
    doLast { 
        println("Hello, World!")//添加要打印到控制台的任务操作。
    }
}
```

保存文件并在命令行上执行 `gradle tasks --all`。您的新任务将出现在 `Other tasks` 下。

**验证您的任务已经创建**

```
$ gradle tasks --all

Other tasks
-----------
hello
```

> 如果您使用的是Gradle 4.0或更高版本，那么控制台的输出可能比本指南中的输出要少。这是因为在本指南中，输出显示为当 `--console-plain` 也作为命令行传递给Gradle时。这样做是为了显示Gradle正在执行的任务。

运行新的任务

**执行临时任务的输出**
```java
$ gradle hello

:hello //这表示您的hello任务已经执行
Hello, World //这是您的任务的输出。
```

恭喜你!您已经添加了第一个任务。

## 添加任务描述

尽管您已经测试了新的任务，并且知道它是有效的，但是最好告诉将使用您的构建脚本的其他人您的新任务的目的是什么。对任务进行分类也很有用。

在你前面运行 `gradle tasks --all` 时，你看到清单中其他的任务，这些任务带上了描述信息，并且根据功能分组了。
当您在前面运行时，您将看到清单中的其他任务，这些任务包含描述，并根据函数进行分组。而且(自从 Grade 3.3后)如果一个任务没有组，它将不会被列出，除非指定了 `--all`。

**标注完整的任务**
```
Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.
```

这是通过在任务上设置 `group` 和 `description` 属性来实现的。编辑您的特别任务，并添加以下内容:

**build.gradle**
```groovy
tasks.register("hello") {
    group = 'Welcome'
    description = 'Produces a greeting'

    doLast {
        println 'Hello, World'
    }
}
```
**build.gradle.kts**
```groovy
tasks.register("hello") {
    group = "Welcome"
    description = "Produces a greeting"

    doLast {
        println("Hello, World")
    }
}
```

再次运行 `gradle tasks`。

**输出结果**
```
$ gradle tasks

Welcome tasks
-------------
hello - Produces a greeting
```

做的好！

## 使消息可配置

此时，您需要将临时任务转换为自定义任务类型，这可以通过在构建脚本中创建类来实现。

**build.gradle**
```groovy
/**
 * build.gradle 是基于Groovy语言的DSL脚本，可以直接在这里定义一个 Groovy类，但是很少这做。
 * 尽管Gradle API中的其他任务类可以在特定的环境中使用，但是继承 DefaultTask 是最常见。
 */
class Greeting extends DefaultTask { 
    /**
     * 给任务添加可配置的属性
     */ 
    String message 
    String recipient
    
    @TaskAction //注释默认任务操作。
    void sayGreeting() {
        println "${message}, ${recipient}!" //使用标准的Groovy插值字符串打印消息。
    }
}

tasks.register("hello", Greeting) { //通过引用您在上面添加的类来指定任务类型。
    group = 'Welcome'
    description = 'Produces a world greeting'
    message = 'Hello'   //配置消息和收件人。
    recipient = 'World' //配置消息和收件人。
}

```
**build.gradle.kts**
```groovy
open class Greeting: DefaultTask() {  
    lateinit var message: String   
    lateinit var recipient: String

    @TaskAction 
    fun sayGreeting() {
        println("$message, $recipient!") 
    }
}

tasks.register<Greeting>("hello") { 
    group = "Welcome"
    description = "Produces a world greeting"
    message = "Hello" 
    recipient = "World"
}
```

测试你的修改。您应该看到相同的输出。

**转换为自定义任务类型后的输出**
```bat
$ gradle hello

:hello
Hello, World!
```

现在您已经拥有了自定义任务类型，您通过创建一个额外的任务并添加德语版本的问候语。

**build.gradle**
```groovy
tasks.register("gutenTag", Greeting) {
    group = 'Welcome'
    description = 'Produces a German greeting'
    message = 'Guten Tag'
    recipient = 'Welt'
}
```
**build.gradle.kts**
```groovy
tasks.register<Greeting>("gutenTag") {
    group = "Welcome"
    description = "Produces a German greeting"
    message = "Guten Tag"
    recipient = "Welt"
}
```

再次运行 `gradle task` 以验证添加了新任务。

**添加第二个任务后，`gradle tasks` 的输出结果**
```bat
$ gradle tasks

Welcome tasks
-------------
gutenTag - Produces a German greeting
hello - Produces a world greeting
```

最后，通过执行 `gradle gutenTag` 来运行新任务

**输出结果**
```bat
$ gradle gutenTag

:gutenTag
Guten Tag, Welt!
```

## 总结

就是这样!您已经完成了创建自定义Gradle任务所需的步骤。你现在应该学会了：

- 注册一个临时的任务并使用 `doLast` 添加一个操作。
- 给任务添加说明。
- 将特定任务转换为自定义Gradle任务类型并注册任务实例。
- 使用 `@TaskAction` 为任务类型设置默认操作。

## 下一步

- 在构建脚本中直接写类将很快导致混乱且可能变成不可维护的脚本。学习如何 [组织构建逻辑](https://docs.gradle.org/5.0/userguide/organizing_build_logic.html)。
- 阅读更多关于 [使用任务](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)、[预定义任务和任务类型](https://docs.gradle.org/5.0/dsl/org.gradle.api.Task.html) 的信息。