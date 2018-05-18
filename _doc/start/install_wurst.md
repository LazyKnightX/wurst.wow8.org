---
title: 安装 Wurst
sections:
  - 安装编译器
  - 创建项目
  - 更新和导入项目
---

现在我们将配置一个Wurst编译器环境，并使用Wurst安装程序创建一个项目。

[*&nbsp;*{: .fa .fa-download} 下载 Wurst安装程序](https://peeeq.de/hudson/job/WurstSetup/lastSuccessfulBuild/artifact/downloads/WurstSetup.jar){: .btn .btn-green}

下载完成后，通过双击来运行WurstSetup.jar。

如果你在国内环境打开，这个软件会启动的比较慢，大约需要30秒。

![](/assets/images/setup/WurstSetup.png){: .img-responsive}

### 安装编译器

该设置将检查编译器是否已经安装。 如果没有，则需要先安装它，然后才能创建项目。

要安装编译器，只需点击 **Install Wurst** 按钮。

在大多数情况下，首次安装编译器可能会进入一种一直读条的状态。根据你的网络情况，你需要在等待约5分钟后关闭安装程序，并重新打开来重试，如果此时**Create Project**按钮已经可用，你可以直接创建项目了。
如果你一切都安装完成后，vscode插件缺表现的十分不正常，你可以尝试删掉 x:\user\你的用户名\.wurst目录，并重新安装编译器。

当一切完成后，安装程序应该会显示你的安装是最新的，现在 **Create Project** 按钮可以使用了：

![](/assets/images/setup/WurstInstallDone.png){: .img-responsive}

### 创建项目

编译器安装完成，你可以继续创建你的Wurst项目了。
选择你想要的项目文件夹和名称，然后单击 **Create Project** 按钮启动该过程。
安装程序现在将创建一个Wurst项目结构，并下载必要的依赖项。
如果一切安装正确，你应该会看到下方的消息：

![](/assets/images/setup/ProjectSetupDone.png){: .img-responsive}

安装完成后，你现在可以在VSCode中打开项目的根文件夹。
打开Hello.wurst文件来激活Wurst插件，代码应该会开始编译，并且完成项目搭建。

如果你只是WurstScript的新手，但并不是编程的新手，那么你可能会需要[*&nbsp;*{: .fa .fa-external-link-square} Wurst入门指南](tutorials/wurstbeginner.html)。

### 更新和导入项目

通过点击导入按钮，你可以加载一个现有的wurst.build文件。
点击“更新项目”按钮后，将更新你的项目依赖关系并根据需要生成必要的本地文件。
你应该使用此功能来更新现有项目并导入新克隆的项目。
