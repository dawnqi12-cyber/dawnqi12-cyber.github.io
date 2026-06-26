---
title: QuPath从源头构建实例
date: 2024-03-26 10:11:20
updated: 2024-03-27 17:23:05
tags:
- QuPath
- Java
- Gradle
- IDEA
categories:
- 技术记录
---

> 本文原发布于 CSDN：<https://blog.csdn.net/Xi1yang/article/details/137035477>

进行 QuPath 开发时，在官方的另一份技术文档中，推荐使用的是 Eclipse。但是我结合上面的文档使用的是 IDEA。

注意，先按 debug 之后，从弹出来的框中找到 `Edit Configurations`。

具体的构建有多种方式。这里使用 IDEA 运行，逻辑和从命令行运行是一样的，具体可以参考官方文档。

### 一、下载源码

先从 QuPath 官方仓库下载源码：

```bash
git clone https://github.com/qupath/qupath.git
```

进入源码目录：

```bash
cd qupath
```

### 二、导入 IDEA

打开 IntelliJ IDEA，选择打开 QuPath 源码目录。

等待 IDEA 自动识别 Gradle 项目并下载依赖。如果依赖下载比较慢，可以检查网络或 Gradle 配置。

### 三、配置 JDK

QuPath 是 Java 项目，需要配置合适版本的 JDK。

在 IDEA 中打开：

```text
File -> Project Structure -> Project
```

然后选择对应版本的 JDK。

如果 IDEA 提示 Gradle JVM 不匹配，也需要在 Gradle 设置中同步调整：

```text
Settings -> Build, Execution, Deployment -> Build Tools -> Gradle
```

### 四、运行项目

项目导入完成后，可以先尝试运行 Gradle 任务。

在 IDEA 右侧 Gradle 面板中找到相关任务，或者直接在终端中执行：

```bash
./gradlew build
```

Windows 下可以使用：

```bash
gradlew.bat build
```

如果只是想启动 QuPath，可以运行对应的启动任务。

### 五、Debug 配置

在 IDEA 中点击 Debug 后，如果弹出配置窗口，可以进入：

```text
Edit Configurations
```

然后根据启动类、工作目录和模块信息进行配置。

主要关注：

- Main class 是否正确。
- JDK 是否正确。
- Working directory 是否指向项目根目录。
- Classpath/module 是否选择正确。

配置完成后再次运行 Debug。

### 六、常见问题

#### 依赖下载失败

可以检查：

- 网络连接是否正常。
- Gradle 配置是否正确。
- 是否需要代理。
- 是否使用了正确的 JDK。

#### IDEA 没有识别 Gradle 项目

可以尝试：

```text
Reload All Gradle Projects
```

或者重新导入项目。

#### 运行配置找不到类

通常是模块、JDK 或工作目录配置不正确。重新检查 `Edit Configurations` 中的配置即可。

### 小结

QuPath 从源码构建时，核心步骤是：

1. 下载源码。
2. 用 IDEA 导入 Gradle 项目。
3. 配置合适的 JDK。
4. 等待依赖同步完成。
5. 配置运行或 Debug。
6. 根据报错调整 Gradle、JDK 和运行配置。

整体思路和命令行构建是一致的，只是 IDEA 会把 Gradle 任务和运行配置图形化，调试时会更方便。
