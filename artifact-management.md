# 打包和发布

## 介绍

这一章是关于如何声明项目的输出 artifacts （工件），以及如何使用它们(例如，上传)。我们将项目的 artifacts 定义为项目提供给外部世界的文件。这可能是一个库或ZIP发行版或任何其他文件。项目可以发布任意数量的artifacts。

## 配置

和 dependencies （依赖）一样，artifacts 页是任意配置的。其实一个配置可以同时包含 *依赖* 和 *工件*。

Gradle只要添加了 [base plugin](https://docs.gradle.org/5.0/userguide/base_plugin.html#base_plugin) 插件，对于项目种的每个配置项，都提供了 `upload*ConfigurationName*` 和 `build*ConfigurationName*` 两个任务。 执行这些任务将会构建或上传属于各自配置的工件。

[这个列表](https://docs.gradle.org/5.0/userguide/java_plugin.html#tab:configurations) 种列出了 java plugin 添加的各项配置。其中两个配置与工件的使用相关。`arcgives` （归档）配置是分配工件的标准配置。java plugin 自动将默认 jar 分配给这个配置。我们将在后面进一步讨论 `runtime` 配置。与依赖项一样，你可以给工件分配声明任意多的自定义配置。

## 声明工件

### 归档任务（archive task）

你可以使用归档任务（archive task）来定义工件:

*例1. 使用归档任务定义工件*

**build.gradle Groovy 版本**
```groovy
task myJar(type: Jar)

artifacts {
    archives myJar
}
```
**build.gradle.kts Kotlin 版本**
```groovy
val myJar = task<Jar>("myJar")

artifacts {
    add("archives", myJar)
}
```

需要注意的是，自定义存档不会自动分配给任何配置。你必须明确地做这个指定。

### 文件工件

你还可以使用文件来定义工件:

*例2. 使用文件定义工件*

**build.gradle Groovy 版本**
```groovy
def someFile = file("$buildDir/somefile.txt")

artifacts {
    archives someFile
}
```
**build.gradle.kts Kotlin 版本**
```groovy
val someFile = file("$buildDir/somefile.txt")

artifacts {
    add("archives", someFile)
}
```

Gradle将根据文件的名称确定工件的属性。你可以自定义这些属性:

*例3. 自定义一个工件*

**build.gradle Groovy 版本**
```groovy
task myTask(type:  MyTaskType) {
    destFile = file("$buildDir/somefile.txt")
}

artifacts {
    archives(myTask.destFile) {
        name 'my-artifact'
        type 'text'
        builtBy myTask
    }
}
```
**build.gradle.kts Kotlin 版本**
```groovy
val myTask = task<MyTaskType>("myTask") {
    destFile = file("$buildDir/somefile.txt")
}

artifacts {
    add("archives", myTask.destFile!!) {
        name = "my-artifact"
        type = "text"
        builtBy(myTask)
    }
}
```

有一个基于 map 的语法来定义一个工件。map 必须包含一个键 `file` 并对应一个文件实体。map 还可以包含其他的工件属性。


*例4. map语法定义工件*

**build.gradle Groovy 版本**
```groovy
task generate(type:  MyTaskType) {
    destFile = file("$buildDir/somefile.txt")
}

artifacts {
    archives file: generate.destFile, name: 'my-artifact', type: 'text', builtBy: generate
}
```
**build.gradle.kts Kotlin 版本**
```groovy
val generate = task<MyTaskType>("generate") {
    destFile = file("$buildDir/somefile.txt")
}

artifacts {
    add("archives",
        mapOf("file" to generate.destFile, "name" to "my-artifact", "type" to "text", "builtBy" to generate))
}
```

## 发布工件

我们已经说过，每个配置都有一个特定的上传任务。在进行上传之前，必须配置上传任务并定义将工件发布到何处。你定义的存储库(如 [声明存储库](https://docs.gradle.org/5.0/userguide/declaring_repositories.html#declaring_repositories) 中所述)不会自动用于上传。事实上，有些存储库只允许下载工件，而不允许上传。下面是一个如何配置配置的上传任务的例子:

*例5. 配置上传任务*

**build.gradle Groovy 版本**
```groovy
repositories {
    flatDir {
        name "fileRepo"
        dirs "repo"
    }
}

uploadArchives {
    repositories {
        add project.repositories.fileRepo
        ivy {
            credentials {
                username "username"
                password "pw"
            }
            url "http://repo.mycompany.com"
        }
    }
}
```
**build.gradle.kts Kotlin 版本**
```groovy
repositories {
    flatDir {
        name = "fileRepo"
        dirs("repo")
    }
}

tasks.getByName<Upload>("uploadArchives") {
    repositories {
        add(project.repositories["fileRepo"])
        ivy {
            credentials {
                username = "username"
                password = "pw"
            }
            url = uri("http://repo.mycompany.com")
        }
    }
}
```

正如你所看到的，你可以使用现有存储库，也可以创建一个新的存储库。

如果上传存储库定义了多个模式，Gradle为每个文件选择上传的模式。默认情况下，Gradle将上传到 `url` 参数定义的模式，并与可选的 `layout` （布局）参数相结合。如果没有提供 `url` 参数，那么Gradle将使用第一个定义的 `artifactPattern` 进行上传，或者使用第一个定义的 `ivyPattern` 上传Ivy文件。

如果需要上传到 Maven 请看 [这里]（https://docs.gradle.org/5.0/userguide/maven_plugin.html#uploading_to_maven_repositories）。

## 更多关于项目 libraries 的资料

如果您的项目被假定为一个库，那么您需要定义这个库的工件是什么，以及这些工件的依赖关系是什么。Java插件为此目的添加了一个运行时配置，隐含的假设是运行时依赖项是要发布的工件的依赖项。当然，这是完全可定制的。您可以添加自己的自定义配置，或者让现有配置继承于其他配置。您可能拥有不同的工件组，这些工件组具有不同的依赖项集。这种机制非常强大和灵活。

如果有人想将您的项目用作库，她只需要声明依赖配置即可。Gradle提供了声明该依赖项的配置属性。如果没有指定，则使用默认配置(请参见 [管理依赖项配置](https://docs.gradle.org/5.0/userguide/managing_dependency_configurations.html#managing_dependency_configurations))。将项目用作库可以从多项目构建中进行，也可以从存储库中检索。在后一种情况下，需要使用 `ivy.xml` 来提供必要的信息。如果您使用Maven存储库，您就没有上面描述的灵活性。有关如何向Maven存储库发布，请参见 [向Maven存储库](https://docs.gradle.org/5.0/userguide/maven_plugin.html#uploading_to_maven_repositories) 上传一节。