# Gradle 手册目录【自译版】

## 手册分类

 - [用户手册](./userguide.md)
 - [教程和指南](https://guides.gradle.org/)
 - [版本发布](https://docs.gradle.org/current/release-notes.html)
 - Gradle Api
 > 1. [Java 文档](https://docs.gradle.org/current/javadoc/overview-summary.html)
 > 2. [Groovy DSL 参考](https://docs.gradle.org/current/dsl/)
 > 3. [Groovy DSL 初识](https://docs.gradle.org/current/userguide/groovy_build_script_primer.html)
 > 4. [Kotlin DSL Api](https://gradle.github.io/kotlin-dsl-docs/api/)
 > 5. [Kotlin DSL 参考](https://docs.gradle.org/current/userguide/kotlin_dsl.html)


## 用户手册

 - [入门指南](./getting_started.md)
 - [Gradle安装](./installation.md)
 - [Gradle升级](https://docs.gradle.org/current/userguide/upgrading_version_4.html)
 - 迁移到Gradle
 > 1. [从Maven迁移](https://guides.gradle.org/migrating-from-maven/)
 > 2. [从Ant迁移](https://docs.gradle.org/current/userguide/migrating_from_ant.html)
  - [故障排除](https://docs.gradle.org/current/userguide/troubleshooting.html)

## 运行Gradle

 - 自定义执行
 > 1. [配置构建环境](https://docs.gradle.org/current/userguide/build_environment.html)
 > 2. [配置后台进程](https://docs.gradle.org/current/userguide/gradle_daemon.html)
 > 3. [使用初始化脚本](https://docs.gradle.org/current/userguide/init_scripts.html)

 - [多项目构建](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html)
 - [创建构建扫描](https://guides.gradle.org/creating-build-scans/)
 - 优化构建时间
 > 1. [构建性能指南](https://guides.gradle.org/performance/)
 > 2. [启用和配置构建缓存](https://docs.gradle.org/current/userguide/build_cache.html)
 - [集成单独的Gradle构建(复合构建)](https://docs.gradle.org/current/userguide/composite_builds.html)

## 使用Gradle

 - 基础知识
> 1. [脚本基本](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)
> 2. [处理任务](https://docs.gradle.org/current/userguide/more_about_tasks.html)
> 3. [更多知识](https://docs.gradle.org/current/userguide/writing_build_scripts.html)
> 4. [文件处理](https://docs.gradle.org/current/userguide/working_with_files.html)
> 5. [自定义任务](https://docs.gradle.org/current/userguide/custom_tasks.html)
> 6. [使用Gradle插件](https://docs.gradle.org/current/userguide/plugins.html)
> 7. [生命周期](https://docs.gradle.org/current/userguide/build_lifecycle.html)
> 8. [日志处理](https://docs.gradle.org/current/userguide/logging.html)
> 9. [多项目配置](https://docs.gradle.org/current/userguide/multi_project_builds.html)

 - [目录结构](https://docs.gradle.org/current/userguide/organizing_gradle_projects.html)
 - [可维护性](https://docs.gradle.org/current/userguide/authoring_maintainable_build_scripts.html)
 - 依赖管理
 > 1. [依赖简介](https://docs.gradle.org/current/userguide/introduction_dependency_management.html)
 > 2. [依赖关系术语](https://docs.gradle.org/current/userguide/dependency_management_terminology.html)
 > 3. [依赖类型](https://docs.gradle.org/current/userguide/dependency_types.html)
 > 4. [库类型](https://docs.gradle.org/current/userguide/repository_types.html)
 > 5. [依赖声明](https://docs.gradle.org/current/userguide/declaring_dependencies.html)
 > 6. [库声明](https://docs.gradle.org/current/userguide/declaring_repositories.html)
 > 7. [检查依赖](https://docs.gradle.org/current/userguide/inspecting_dependencies.html)
 > 8. [管理依赖](https://docs.gradle.org/current/userguide/managing_dependency_configurations.html)
 > 9. [过渡依赖](https://docs.gradle.org/current/userguide/managing_transitive_dependencies.html)
 > 10. [依赖锁定](https://docs.gradle.org/current/userguide/dependency_locking.html)
 > 11. [依赖故障排查](https://docs.gradle.org/current/userguide/troubleshooting_dependency_resolution.html)
 > 12. [自定义依赖解决方案](https://docs.gradle.org/current/userguide/customizing_dependency_resolution_behavior.html)
 > 13. [依赖缓存](https://docs.gradle.org/current/userguide/dependency_cache.html)
 > 14. [依赖处理](https://docs.gradle.org/current/userguide/working_with_dependencies.html)
 > 15. [属性的匹配](https://docs.gradle.org/current/userguide/dependency_management_attribute_based_matching.html)

 - [发布](https://docs.gradle.org/current/userguide/publishing_overview.html)
 - Java 项目
 > 1. [构建 Java & Jvm 项目](https://docs.gradle.org/current/userguide/building_java_projects.html)
 > 2. [测试 Java & Jvm 项目](https://docs.gradle.org/current/userguide/java_testing.html)
 - C++ 项目
 > 1. [构建原生软件](https://docs.gradle.org/current/userguide/native_software.html)
 > 2. [软件模型概念](https://docs.gradle.org/current/userguide/software_model_concepts.html)
 > 3. [基于规则的模型配置](https://docs.gradle.org/current/userguide/software_model.html)
 > 4. [在插件中实现模型规则](https://docs.gradle.org/current/userguide/rule_source.html)
 > 5. [扩展软件模型](https://docs.gradle.org/current/userguide/software_model_extend.html)
 - 高级技术
 > 1. [任务延迟](https://docs.gradle.org/current/userguide/lazy_configuration.html)
 > 2. [并行任务](https://guides.gradle.org/using-the-worker-api/)
 > 3. [使用TestKit测试构建](https://docs.gradle.org/current/userguide/test_kit.html)
 > 4. [在Gradle中使用Ant](https://docs.gradle.org/current/userguide/ant.html)
 - Gradle示例项目
 > 1. [Groovy DSL 示例](https://github.com/gradle/gradle/tree/master/subprojects/docs/src/samples)
 > 2. [Kotlin DSL 示例](https://github.com/gradle/kotlin-dsl/tree/master/samples)
 - 参考
 1. [核心插件](https://docs.gradle.org/current/userguide/plugin_reference.html)
 2. [命令行界面](https://docs.gradle.org/current/userguide/command_line_interface.html)
 3. [第三方工具](https://docs.gradle.org/current/userguide/third_party_integration.html)
 4. [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)
 5. [Gradle 文件目录](https://docs.gradle.org/current/userguide/directory_layout.html)
