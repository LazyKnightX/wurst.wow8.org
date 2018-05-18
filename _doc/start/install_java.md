---
title: 安装Java
sections:
  - 下载JRE
  - 验证/更新Java
---

WurstScript以及其相关工具需要Java8来正常运行，因此你需要确保你的电脑上安装了Java8。

### 下载 JRE

[*&nbsp;*{: .fa .fa-download} 下载 Java](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html){: .btn .btn-orange} ![](/assets/images/setup/java_powered.png) 

下载适合你系统的Java，并跟随下方的安装步骤来安装。

一旦完成后，就可进入下一步。

### 验证/更新Java

WurstScript的许多功能依赖于电脑上配置的java环境变量。


如果你的 **JAVA_HOME** 环境变量还没有设置，或者设置错了路径。

参考 [*&nbsp;*{: .fa .fa-external-link-square} 这些教程](https://www.cnblogs.com/silence9527/p/7358239.html) 来正确设置你的 **JAVA_HOME** 环境变量。

按win+r,输入cmd,回车 来开启命令行窗口，输入 `java -version` 来验证你当前安装的JAVA版本。

如果你的Java正确安装了，就应该能够看到如下的消息：

![](/assets/images/setup/JavaVerify.png){: .img-responsive}

如果以上命令出现了错误，你可以尝试输入 `java ` 或者 `javac ` 命令来测试环境变量是否成功配置

当你正确安装完Java后，接着安装VSCode。
