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

【中国的网络环境可能导致超时，需要科学上网使用。】

![](/assets/images/setup/WurstSetup.png){: .img-responsive}

### 安装编译器

该设置将检查编译器是否已经安装。如果没有，则需要先安装它，然后才能创建项目。

要安装编译器，只需点击 **Install Wurst** 按钮。

在大多数情况下，首次安装编译器可能会进入一种一直读条的状态。根据你的网络情况，你需要在等待约5分钟后关闭安装程序，并重新打开来重试，如果此时**Create Project**按钮已经可用，你可以直接创建项目了。
如果你一切都安装完成后，VSCode插件却表现的十分不正常，你可以尝试删掉 `C:\Users\你的用户名\.wurst` 目录，并重新安装编译器。

当一切完成后，安装程序应该会显示你的安装是最新版的，现在 **Create Project** 按钮可以使用了：

![](/assets/images/setup/WurstInstallDone.png){: .img-responsive}

### 创建项目

编译器安装完成，你就可以继续创建你的Wurst项目了。

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

## 中国1.27版搭建教程(官方平台版)

> 作者：FBI琪露诺——750160517@qq.com
>
> Q群：934023552
>
> 版本：1.27

道道做的搭建视频https://www.bilibili.com/video/av412963739/

### 搭建开始

按照上方教程搭建wurst文件夹

按照道道的搭建视频，删除<u>_build\dependencies\</u>中的<u>WurstStdlib2</u>文件夹

打开wurst.build 添加代码

 ```
  dependencies:
  - https://github.com/fbicirno/WurstStdlib2
 ```

  ### 替换为1.27

打开【Q群934023552】的tasks.json，找到%编辑器路径%，修改为你的we编辑器路径
**【可以使用文本编辑器的替换功能】**

```
例如
"command": "%编辑器路径%\\bin\\YDWEConfig.exe",
修改为
"command": "D:\\SoftWare\\WorldEdit\\bin\\YDWEConfig.exe",
```

将%编译地图名%修改为_build文件夹下的地图名

将

打开项目文件夹\\.vscode

将本教程的tasks.json复制进去

### 测试地图

重启vscode

按F5

输入run map with YDWEConfig

运行地图即可

### 常见问题

Q 为什么会启动混乱之治，而不是冰封王座？

A可能是魔兽文件夹里混乱之治叫Warcraft III.exe，将其随意改名即可

更多问题请点击[避坑指南](https://github.com/fbicirno/WurstStdlib2/tree/master/%E9%81%BF%E5%9D%91%E6%8C%87%E5%8D%97)
