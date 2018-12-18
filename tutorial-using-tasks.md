# 脚本基本知识

本章向您介绍编写Gradle构建脚本的基本知识。要获得快速的实践介绍，请尝试 [创建新的Gradle构建指南](https://guides.gradle.org/creating-new-gradle-builds/)。

## 项目与任务

Gradle的一切都基于两个基本概念:项目和任务。

每个 Gradle 构建都由一个或多个子项目组成。项目代表什么取决于你想让Gradle做什么。例如，项目可能代表JAR库或web应用程序。它可能代表由其他项目生成的jar组装而成的发行版ZIP。项目不一定代表要构建的东西。它可能表示要做的事情，例如将应用程序部署到登台或生产环境。如果这看起来有点模糊，不要担心。Gradle的按惯例构建支持为项目添加了一个更具体的定义。

每个项目由一个或多个任务组成。任务表示构建执行的某个原子工作片段。这可能是编译一些类、创建一个JAR、生成 Javadoc 或将一些存档发布到存储库。

现在，我们将着眼于在一个项目的构建中定义一些简单的任务。后面的章节将介绍如何处理多个项目，以及如何处理项目和任务。

# Hello world （世界你好）

您可以使用 `gradle` 命令运行Gradle构建，`gradle` 命令在当前目录中查找一个名为 `build.gradle` 的文件，我们将 `build.gradle` 称为构建脚本，尽管严格地说它是一个构建配置脚本，我们稍后将看到构建脚本中定义了一个项目及其任务。

为此，可以根据下面内容创建名为 `build.gradle` 的构建脚本。

*例1.您的第一个构建脚本*

**build.gradle**
```groovy
task hello {
    doLast {
        println 'Hello world!'
    }
}
```
**build.gradle.kts**
```kotlin
task("hello") {
    doLast {
        println("Hello world!")
    }
}
```

在命令行shell中（或 cmd），进入到 `build.gradle` 的目录并使用 `gradle -q hello` 执行构建脚本:

> **-q是做什么的?**  
> 本用户指南中的大多数示例都使用 `-q` 命令行选项运行。这会抑制Gradle的日志消息，以便只显示任务的输出。这使得本用户指南中的示例输出更加清晰。如果您不想使用此选项，则不需要使用此选项。有关影响Gradle输出的命令行选项的更多细节，请参见 [日志](https://docs.gradle.org/current/userguide/logging.html#logging)。

*例2.构建脚本的执行*

**输出结果来自命令：gradle -q hello**
```
> gradle -q hello
Hello world!
```

这是怎么回事？这个构建脚本定义了一个名为 `hello` 的任务，并向其添加了一个操作。当您运行 `gradle hello` 时，gradle 执行 `hello` 任务，该任务反过来执行您提供的操作。该操作只是一个包含要执行的代码的块。

如果您认为这看起来与 Ant 的 target 相似，那么你是正确的。Gradle任务就相当于 Ant 的 target，但是却要强大得多。我们使用了与 Ant 不同的术语，因为我们认为单词 task 比单词 target 更具表现力。不幸的是，这引入了与 Ant 冲突的术语，因为在 Ant 中会把它的命令 （如javac或copy）叫做任务。所以当本文说到任务时，总是指的都是是 Gradle 任务，它相当于 Ant target 目标。如果我们提到 Ant 任务 (Ant命令)，将会明确地说 *Ant 任务*。

## 构建脚本也是可执行代码

Gradle 的构建脚本提供了 Groovy 和 Kotlin 的全部功能支持。作为开胃菜，看看这个:

*例3.在Gradle的任务中使用Groovy或Kotlin*

**build.gradle**
```groovy
task upper {
    doLast {
        String someString = 'mY_nAmE'
        println "Original: $someString"
        println "Upper case: ${someString.toUpperCase()}"
    }
}
```
**build.gradle.kts**
```
task("upper") {
    doLast {
        val someString = "mY_nAmE"
        println("Original: $someString")
        println("Upper case: ${someString.toUpperCase()}")
    }
}
```
**输出结果来自命令：gradle -q upper**
```bat
> gradle -q upper
Original: mY_nAmE
Upper case: MY_NAME
```

或者

**build.gradle**
```groovy
task count {
    doLast {
        4.times { print "$it " }
    }
}
```
**build.gradle.kts**
```
task("count") {
    doLast {
        repeat(4) { print("$it ") }
    }
}
```
**输出结果来自命令：gradle -q count**
```
> gradle -q count
0 1 2 3
```

## 任务依赖

正如您可能已经猜到的，您可以声明依赖于其他任务的任务。

**build.gradle**
```groovy
task hello {
    doLast {
        println 'Hello world!'
    }
}
task intro {
    dependsOn hello
    doLast {
        println "I'm Gradle"
    }
}
```
**build.gradle.kts**
```
task("hello") {
    doLast {
        println("Hello world!")
    }
}
task("intro") {
    dependsOn("hello")
    doLast {
        println("I'm Gradle")
    }
}
```
**输出结果来自命令：gradle -q intro**
```
> gradle -q intro
Hello world!
I'm Gradle
```

要添加依赖任务的时候，不要求当前存在的任务，也可以是后面添加的任务。

*例6.懒惰依赖 —— 另一个任务不存在*

**build.gradle**
```groovy
task taskX {
    dependsOn 'taskY'
    doLast {
        println 'taskX'
    }
}
task taskY {
    doLast {
        println 'taskY'
    }
}
```
**build.gradle.kts**
```
task("taskX") {
    dependsOn("taskY")
    doLast {
        println("taskX")
    }
}
task("taskY") {
    doLast {
        println("taskY")
    }
}
```
**输出结果来自命令：gradle -q taskX**
```bat
> gradle -q taskX
taskY
taskX
```

`taskX` 依赖 `taskY` 的时候，`taskY` 还没有声明定义。这个在多项目构建中非常重要。关于任务依赖更多详细信息请参考 [给任务添加依赖](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:adding_dependencies_to_tasks)。


不过需要注意，在引用尚未定义的任务时，不能使用 [快捷表示法](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:shortcut_notations)。

## 动态任务

Groovy或Kotlin的强大功能不仅可以用于定义任务的功能。例如，您还可以使用它来动态创建任务。

*例7.任务的动态创建*

**build.gradle**
```groovy
4.times { counter ->
    task "task$counter" {
        doLast {
            println "I'm task number $counter"
        }
    }
}
```
**build.gradle.kts**
```
repeat(4) { counter ->
    task("task$counter") {
        doLast {
            println("I'm task number $counter")
        }
    }
}
```
**输出结果来自命令：gradle -q task1**
```bat
> gradle -q task1
I'm task number 1
```

## 操作任务

一旦创建了任务，就可以通过API访问它们。例如，您可以使用它在运行时动态地向任务添加依赖项。而在 Ant 中不支持这样的操作。

*示例8.通过API访问任务——添加依赖项*

**build.gradle**
```groovy
4.times { counter ->
    task "task$counter" {
        doLast {
            println "I'm task number $counter"
        }
    }
}
task0.dependsOn task2, task3
```
**build.gradle.kts**
```
repeat(4) { counter ->
    task("task$counter") {
        doLast {
            println("I'm task number $counter")
        }
    }
}
tasks["task0"].dependsOn("task2", "task3")
```
**Output of gradle -q task0**
```bat
> gradle -q task0
I'm task number 2
I'm task number 3
I'm task number 0
```
你还可以向现有任务添加行为。

*示例9.通过API添加行为访问任务*

**build.gradle**
```groovy
task hello {
    doLast {
        println 'Hello Earth'
    }
}
hello.doFirst {
    println 'Hello Venus'
}
hello.doLast {
    println 'Hello Mars'
}
hello {
    doLast {
        println 'Hello Jupiter'
    }
}
```
**build.gradle.kts**
```
val hello = task("hello") {
    doLast {
        println("Hello Earth")
    }
}
hello.doFirst {
    println("Hello Venus")
}
hello.doLast {
    println("Hello Mars")
}
hello.apply {
    doLast {
        println("Hello Jupiter")
    }
}
```
**Output of gradle -q hello**
```bat
> gradle -q hello
Hello Venus
Hello Earth
Hello Mars
Hello Jupiter
```

`doFirst` 和 `doLast` 可以调用多次。它们将一个操作添加到任务操作列表的开头或结尾。当任务执行时，将按顺序执行操作列表中的操作。

## Groovy DSL快捷符号

访问现有任务有一种快捷的表示法。每个任务都可以作为构建脚本的属性使用:

*示例10.将任务作为构建脚本的属性访问*

**build.gradle**
```groovy
task hello {
    doLast {
        println 'Hello world!'
    }
}
hello.doLast {
    println "Greetings from the $hello.name task."
}
```
**Output of gradle -q hello**
```
> gradle -q hello
Hello world!
Greetings from the hello task.
```

这大大提高了代码的可读性，特别是在使用插件提供的任务时，比如 `compile` 任务。

## 任务额外属性

您可以将自己的属性添加到任务中。要添加名为 `myProperty` 的属性需要给 `ext.myProperty` 初始值。然后就可以像读取和设置预定义的任务属性一样读取和设置属性。

*例11.向任务添加额外属性*

**build.gradle**
```groovy
task myTask {
    ext.myProperty = "myValue"
}

task printTaskProperties {
    doLast {
        println myTask.myProperty
    }
}
```
**build.gradle.kts**
```groovy
task("myTask") {
    extra["myProperty"] = "myValue"
}

task("printTaskProperties") {
    doLast {
        println(tasks["myTask"].extra["myProperty"])
    }
}
```
**Output of gradle -q printTaskProperties**
```
> gradle -q printTaskProperties
myValue
```

额外的属性并不仅限于任务使用。您可以在 [额外属性](https://docs.gradle.org/current/userguide/writing_build_scripts.html#sec:extra_properties) 中阅读更多关于它们的信息。

## 使用Ant任务

Gradle 中内嵌了 Ant 完整的子系统 （Ant tasks are first-class citizens in Gradle）。Gradle 可以在 Groovy 的基础上通过简单地依赖配置方便的集成 Ant 任务。Groovy 附带了出色的 `AntBuilder` 。使用Gradle 中的 Ant 任务与使用 `build.xml` 文件中的 Ant 任务一样方便和强大。Kotlin也可以使用。从下面的例子中，您可以学习如何执行 Ant 任务以及如何访问 Ant 属性:

*示例12.使用 AntBuilder 执行 ant.loadfile target*
**build.gradle**
```groovy
task loadfile {
    doLast {
        def files = file('./antLoadfileResources').listFiles().sort()
        files.each { File file ->
            if (file.isFile()) {
                ant.loadfile(srcFile: file, property: file.name)
                println " *** $file.name ***"
                println "${ant.properties[file.name]}"
            }
        }
    }
}
```
**build.gradle.kts**
```groovy
task("loadfile") {
    doLast {
        val files = file("./antLoadfileResources").listFiles().sorted()
        files.forEach { file ->
            if (file.isFile) {
                ant.withGroovyBuilder {
                    "loadfile"("srcFile" to file, "property" to file.name)
                }
                println(" *** ${file.name} ***")
                println("${ant.properties[file.name]}")
            }
        }
    }
}
```
**Output of gradle -q loadfile**
```bat
> gradle -q loadfile
 *** agile.manifesto.txt ***
Individuals and interactions over processes and tools
Working software over comprehensive documentation
Customer collaboration  over contract negotiation
Responding to change over following a plan
 *** gradle.manifesto.txt ***
Make the impossible possible, make the possible easy and make the easy elegant.
(inspired by Moshe Feldenkrais)
```

在构建脚本中，您可以使用 Ant 做更多的事情。你可以在 [Ant文档](https://docs.gradle.org/current/userguide/ant.html#ant) 中找到更多信息。

## 使用方法

Gradle 在如何组织构建逻辑方面具有伸缩性。对于上述示例，组织构建逻辑的第一级是提取方法。

*示例13.使用方法组织构建逻辑*
**build.gradle**
```groovy
task checksum {
    doLast {
        fileList('./antLoadfileResources').each { File file ->
            ant.checksum(file: file, property: "cs_$file.name")
            println "$file.name Checksum: ${ant.properties["cs_$file.name"]}"
        }
    }
}

task loadfile {
    doLast {
        fileList('./antLoadfileResources').each { File file ->
            ant.loadfile(srcFile: file, property: file.name)
            println "I'm fond of $file.name"
        }
    }
}

File[] fileList(String dir) {
    file(dir).listFiles({file -> file.isFile() } as FileFilter).sort()
}
```
**build.gradle.kts**
```groovy
task("checksum") {
    doLast {
        fileList("./antLoadfileResources").forEach { file ->
            ant.withGroovyBuilder {
                "checksum"("file" to file, "property" to "cs_${file.name}")
            }
            println("$file.name Checksum: ${ant.properties["cs_${file.name}"]}")
        }
    }
}

task("loadfile") {
    doLast {
        fileList("./antLoadfileResources").forEach { file ->
            ant.withGroovyBuilder {
                "loadfile"("srcFile" to file, "property" to file.name)
            }
            println("I'm fond of ${file.name}")
        }
    }
}

fun fileList(dir: String): List<File> =
    file(dir).listFiles { file: File -> file.isFile }.sorted()
```
**Output of gradle -q loadfile**
```
> gradle -q loadfile
I'm fond of agile.manifesto.txt
I'm fond of gradle.manifesto.txt
```

稍后您将看到这些方法可以在多项目构建中的子项目之间共享。如果您的构建逻辑变得更加复杂，Gradle将为您提供其他非常方便的方式来组织它。我们用了整整一章来讨论这个问题。参见 [组织Gradle项目](https://docs.gradle.org/current/userguide/organizing_gradle_projects.html#organizing_gradle_projects) 。

## 默认任务

Gradle允许您定义一个或多个默认任务，如果没有指定其他任务，将执行这些任务。

*例14. 定义默认任务*
**build.gradle**
```groovy
defaultTasks 'clean', 'run'

task clean {
    doLast {
        println 'Default Cleaning!'
    }
}

task run {
    doLast {
        println 'Default Running!'
    }
}

task other {
    doLast {
        println "I'm not a default task!"
    }
}
```
**build.gradle.kts**
```groovy
defaultTasks("clean", "run")

task("clean") {
    doLast {
        println("Default Cleaning!")
    }
}

task("run") {
    doLast {
        println("Default Running!")
    }
}

task("other") {
    doLast {
        println("I'm not a default task!")
    }
}
```
**Output of gradle -q**
```bat
> gradle -q
Default Cleaning!
Default Running!
```

这相当于运行 `gradle clean run` 。在多项目构建中，每个子项目都可以有自己特定的默认任务。如果子项目没有指定默认任务，则使用父项目的默认任务（如果定义了）。

### 使用DAG配置

Gradle有一个配置阶段和一个执行阶段（详细查看 [构建什么周期](https://docs.gradle.org/current/userguide/build_lifecycle.html#build_lifecycle)）。在配置阶段之后，Gradle 会知道应该执行哪些任务。Gradle 提供了一个钩子来利用这些信息。用例之一是检查发布任务是否在要执行的任务中。根据这一点，您可以为某些变量分配不同的值。

在下面的示例中，`distribution ` 和 `release ` 任务的执行将导致 `版本` 变量的不同值。

*15例. 构建的不同结果取决于所选择的任务*
**build.gradle**
```groovy
task distribution {
    doLast {
        println "We build the zip with version=$version"
    }
}

task release {
    dependsOn 'distribution'
    doLast {
        println 'We release now'
    }
}

gradle.taskGraph.whenReady { taskGraph ->
    if (taskGraph.hasTask(":release")) {
        version = '1.0'
    } else {
        version = '1.0-SNAPSHOT'
    }
}
```
**build.gradle.kts**
```kotlin
task("distribution") {
    doLast {
        println("We build the zip with version=$version")
    }
}

task("release") {
    dependsOn("distribution")
    doLast {
        println("We release now")
    }
}

gradle.taskGraph.whenReady {
    version =
        if (hasTask(":release")) "1.0"
        else "1.0-SNAPSHOT"
}
```
**Output of gradle -q distribution**
```bat
> gradle -q distribution
We build the zip with version=1.0-SNAPSHOT
```
**Output of gradle -q release**
```
> gradle -q release
We build the zip with version=1.0
We release now
```
重要的是 `whenReady` 会在执行发布任务之前影响发布任务。即使发布任务不是主要任务。

## 构建脚本的外部依赖项

如果构建脚本需要使用外部库，可以将它们添加到构建脚本本身中的脚本类路径中。您可以使用 `buildscript()` 方法，传入一个声明构建脚本类路径的块。

*示例16. 为构建脚本声明外部依赖关系*
**build.gradle**
```groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 'commons-codec', name: 'commons-codec', version: '1.2'
    }
}
```
**build.gradle.kts**
```kotlin
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        "classpath"(group = "commons-codec", name = "commons-codec", version = "1.2")
    }
}
```

上面的代码块传递给 `buildscript()` 方法的块配置 [ScriptHandler](https://docs.gradle.org/current/javadoc/org/gradle/api/initialization/dsl/ScriptHandler.html) 实例。通过 `classpath` 配置添加依赖项来声明构建脚本类路径。这与声明Java编译类路径(例如)的方式相同。除了项目依赖项之外，您可以使用任何依赖项类型。

添加了构建脚本依赖之后，就可以在构建脚本中直接使用依赖中的类来完成特定的工作了。下面的示例通过给构建脚本添加 `Base64` 的依赖来，完成对内容的 Base64 加密。

**Output of gradle -q encode**
```bat
> gradle -q encode
aGVsbG8gd29ybGQK
```

对于多项目构建，用根项目的 `buildscript()` 方法声明的依赖可用于其所有子项目中的构建脚本。

构建脚本的依赖项可以是 Gradle 插件。有关Gradle插件的更多信息，请咨询 [使用Gradle插件](https://docs.gradle.org/current/userguide/plugins.html#plugins)。

每一个项目都会自带一个类型为 [BuildEnvironmentReportTask](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.BuildEnvironmentReportTask.html) 名称为 `buildEnvironment` 的任务。可以通过执行它来查看构建脚本的依赖项情况。

## 接下来去哪里？

在这一章中，我们首先看了任务。但这并不是任务的结束。如果您想了解更多细节，请查看 [更多关于任务](https://docs.gradle.org/current/userguide/more_about_tasks.html#more_about_tasks) 的信息。

或者，继续学习 [教程](https://docs.gradle.org/current/userguide/tutorial_java_projects.html#tutorial_java_projects) 和 [依赖关系管理](https://docs.gradle.org/current/userguide/dependency_management_for_java_projects.html#dependency_management_for_java_projects)。




