![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/0.jpg)

# 前言

&emsp;&emsp;根据UE4[官方文档](https://docs.unrealengine.com/zh-CN/Gameplay/Networking/Overview/index.html)的介绍，UE4引擎在架构时就已经考虑到了多人游戏的情景，多人游戏基于客户端-服务器模式（CS模式）。也就是说，会有一个服务器担当游戏状态的主控者，而连接的客户端将保持近似的副本。服务器是UE4多人游戏的一个重要部分，其作用是：做出所有重要决定，包含所有的主控状态、处理客户端连接、转移到新的地图以及处理比赛开始/结束时的总体游戏流程。

&emsp;&emsp;服务器的功能开发在UE4中也非常简单，UE4本身是支持客户端/服务器逻辑混合开发的，在蓝图中创建好自定义事件（Custom Event）之后，只需要选择该事件在服务器上运行（Run on Server）即可。而在C++中，也只需要用宏UFunction(Server)来修饰某个函数，然后就可以在客户端调用服务器RPC。本文在此不再具体赘述服务器游戏功能的实现，而是着重介绍如何构建出独立的UE4服务器应用。

# 服务器构建

&emsp;&emsp;在UE4编辑器中，我们只需要在播放（Play）选项中勾选运行独立服务器（Run Dedicated Server）即可在编辑器中运行独立服务器，并且客户端会自动连接到该服务器。那么如何构建出能独立运行的服务器呢？官网给出了一篇[指南](https://wiki.unrealengine.com/index.php?title=Dedicated_Server_Guide_(Windows_%26_Linux))，于是我根据这篇指南和我自己的理解开始了UE4服务器的构建。

## 编译虚幻引擎源码

&emsp;&emsp;在这第一步我就产生了疑问，我们最初从官网下载UE4启动器后，安装UE4引擎时，不就已经下载了虚幻引擎源码了吗，还有必要重新下载吗？

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/1.png)

&emsp;&emsp;于是我就尝试跳过这一步，直接从[添加构建服务器目标](#为项目添加构建服务器目标)开始操作，当选择构建目标为"Development Server"后，得到了如下的报错：

```
UnrealBuildTool : error : Couldn't find target rules file for target 'UE4Server' in rules assembly 'UE4Rules, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null'.
Location: C:\Program Files\Epic Games\UE_4.22\Engine\Intermediate\Build\BuildRules\UE4Rules.dll
Target rules found:
```

&emsp;&emsp;该问题我在[Reddit](https://www.reddit.com/r/unrealengine/comments/ae55r0/problem_building_development_server_couldnt_find/)（国外知名论坛）上找到了详细的阐述，原因就是因为没有按照指南的步骤下载与编译UE4源码。至于为何启动器中下载的UE4引擎源码会出现找不到构建目标“UE4Server”的问题，我推测是因为其源码（构建）不完整，精简了编辑器以外的其他内容（例如独立服务器模块等等），所以，还是老老实实下载源码吧。

### 下载UE4源码

&emsp;&emsp;UE4源码存放在Github仓库，并且是私有仓库（private reposity）。如果你想获得访问权限，需要加入Epic Games开发组。在Epic官网登陆帐号后，关联Github帐号，就会收到Epic Games开发组的邀请，接受后即可加入，[官网](https://www.unrealengine.com/zh-CN/ue4-on-github?lang=zh-CN)做了详细的操作教程。

&emsp;&emsp;我下载的版本是4.22.3，大小是333M，如果你访问Github速度过慢，可以从我的[百度网盘](https://pan.baidu.com/s/1-TvcvtVf8ahC_o_lElo_8Q)中下载（提取码：fv4e）。

### 安装源码

&emsp;&emsp;解压上一步下载的源码压缩包后，双击Setup.bat文件即可。

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/2.png)

&emsp;&emsp;可以看到，该一步是在检查和更新UE4引擎的依赖组件，需要更新9122M的文件（约9G），国内访问速度特别慢，我家100M电信宽带，下载速度只有0.1M/S（100KB/S），连上香港的VPN也是差不多的速度（因为VPN限速1M），按照这个下载速度，下载完大约需要**25个小时**。

&emsp;&emsp;25个小时有点太长了，查看了一下Setup.bat这个文件，它调用了“.\Engine\Binaries\DotNET\GitDependencies.exe”来执行下载，查看该进程连接的远程地址为“13.225.102.98:80”，这个ip地址在美国，所以访问缓慢。我找了一下源码目录，希望可以找到配置类似下载镜像的地方，但最终并没有找到。（绝望~）

&emsp;&emsp;难道真的要为此默默等待25小时吗？好在生活中处处有惊喜，第二天我试了一下在公司网络进行下载，速度意外地飚到了1.5M/S，只需要等待大约1个半小时，非常开心。下载完毕后，该窗口会自动消失，继续进行下一步即可。如果你也有下载依赖缓慢的问题，可以从我的[百度网盘](https://pan.baidu.com/s/1hL83W311C5iWk0cKI6Gcyw)中下载（提取码：qa8a）。

### 生成Visual Studio项目文件

&emsp;&emsp;同样在解压后的路径，双击GenerateProjectFiles.bat，会得到如下的报错：

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/2.5.png)

&emsp;&emsp;根据提示来看，是因为需要安装.net框架的SDK。打开微软.NET[官网](https://dotnet.microsoft.com/download)，下载.NET Framework Dev Pack（注意不是Runtime）。

&emsp;&emsp;（Warning！）此处有巨坑。根据上面的提示，是需要安装4.6.2版本的.NET Framework Dev Pack，如果你跟我一样直接下载了目前最新的版本（4.8），同样会有该错误提示，因为这个版本要求已经写在了如下的文件中：

```
Engine\Source\Programs\UnrealBuildTool\UnrealBuildTool.csproj
Engine\Source\Programs\DotNETCommon\DotNETUtilities\DotNETUtilities.csproj
```

&emsp;&emsp;“机智”的我立刻找到了文件中的TargetFrameworkVersion字段，将默认的v4.6.2修改成了我安装的版本v4.8，再次运行GenerateProjectFiles.bat后，果然，没有报错，并且成功生成了UE4.sln。其实，如上的步骤并不是指跳过了这个巨坑，因为，这只是噩梦的开始……

&emsp;&emsp;继续进行下面的编译源码步骤后（耗时约8小时），会得到如下的报错：

```
error MSB3075: The command "..\..\Build\BatchFiles\Build.bat exited with code 5. 
Please verify that you have sufficient rights to run this command.
```

&emsp;&emsp;根据报错提示，是"..\..\Build\BatchFiles\Build.bat"异常退出，因为没有权限。我推测可能是由于文件占用导致的，先是重启了VS再编译，没有效果。然后又重启了电脑，仍然没有效果。然后又尝试了“使用管理员权限”重新编译，还是没有变化。百度和Google上的解决方法也是五花八门，各种情况都有，无法解决。

&emsp;&emsp;最后正在我绝望的时候，突然想到了.NET版本的问题，卸载.NET Dev Pack 4.8然后重新安装.NET Dev Pack 4.6.2之后。终于，报错消失了，所以，这里只能使用.NET Dev Pack 4.6.2。

&emsp;&emsp;这个大坑主要是因为.NET Dev Pack版本安装错误后，没有明确的提示，不太容易联想到是.NET Dev Pack版本的问题，而且，编译一次UE4源码真的太太太太太耗时间了。

### 编译UE4源码

&emsp;&emsp;双击UE4.sln在Visual Studio中打开UE4源码，在左侧解决方案浏览器（Solution Explorer）中，在“UE4”项目点击右键，然后点击Build。考验电脑性能的时候到了，我在公司的i3处理器的电脑上编译了大概8个小时，并且在编译期间，CPU占用率一直100%，几乎干不了其他事，所以建议在下班的时候进行，第二天早上一上班就可以看到，编译已经完成。

##  为项目添加构建服务器目标

&emsp;&emsp;由于官网的指南是针对引擎版本为“4.14, 4.15, 4.16, 4.17, 4.18”编写的，而我使用的UE4引擎版本为4.22.3，因此使用的是指南中针对4.18版本的内容，经过测试，是可以使用的。

&emsp;&emsp;进入项目的"~/Source"路径，新建一个文件命名为“项目名Server.Target.cs”，将项目名替换为实际项目名，例如我的项目名为“Shooting_Game”，则文件名命名为“Shooting_GameServer.Target.cs”。（如果你的项目没有Source路径，则说明是纯蓝图的UE4项目，则在编辑器中添加任意一个C++类即可。）

&emsp;&emsp;编辑新建的文件，加入如下内容，注意替换项目名。

```
// Copyright 1998-2017 Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;
using System.Collections.Generic;

[SupportedPlatforms(UnrealPlatformClass.Server)]
public class Shooting_GameServerTarget : TargetRules   // 修改项目名
{
    public Shooting_GameServerTarget(TargetInfo Target) : base(Target)  // 修改项目名
    {
        Type = TargetType.Server;
        ExtraModuleNames.Add("Shooting_Game");    // 修改项目名
    }
}
```

## 切换项目的UE4引擎版本

&emsp;&emsp;右键项目根目录的“项目名.uproject”文件，选择"Switch Unreal Engine version..."

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/3.png)

&emsp;&emsp;选择刚刚安装的UE4引擎版本，点击OK即可，切换引擎后，会自动生成项目文件（Project files）。

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/4.png)

## 构建服务器

&emsp;&emsp;在Visual Studio的解决方案配置（Solution Configuration）中选择"Development Server"，然后在解决方案浏览器（Solution Explorer）中，在"Shooting_Game"（我的项目名称）点击右键，点击Build。这里只有481个构建步骤，比编译UE4源码要少 一些，大约需要2小时。

&emsp;&emsp;构建完成之后，可以看到输出日志：

```
[480/481] Shooting_GameServer.exe
Creating library F:\workspace\ue4\Shooting_Game2\Binaries\Win64\Shooting_GameServer.lib and object F:\workspace\ue4\Shooting_Game2\Binaries\Win64\Shooting_GameServer.exp
[481/481] Shooting_GameServer.target
```

&emsp;&emsp;已经生成了Shooting_GameServer.exe，路径在“~\Binaries\Win64\”，如果我们尝试直接运行（需要添加运行参数-log，否则进程会在后台运行，什么效果也看不到），会看到如下报错：

```
LogLinker: Warning: Unable to load package(...). Package contains EditorOnly data which is not supported by the current build.
```

&emsp;&emsp;意思是包含了仅支持编辑器内容的数据，“~\Binaries\Win64\”是编辑器构建文件夹，所以我们还需要进行客户端打包，然后再将服务器程序拷贝过去。

# 客户端打包

&emsp;&emsp;客户端打包需要在编辑器中进行，所以需要在解决方案配置（Solution Configuration）中选择"Development Editor"，启动项目（Startup Projects）中选择“Shooting_Game”，然后点击运行按钮。第一次运行编辑器会编译Shader，所以耗时较久（我大约需要20分钟），并且会长时间卡在45%，等待即可。

## 创建入口关卡

&emsp;&emsp;在左下方内容浏览器（Content Browser）中选择Content目录，选择存放关卡的文件夹（我的是Levels），右边窗口右键创建关卡（Level），命名为EntryMap。

## 在入口关卡中连接服务器

&emsp;&emsp;双击选择EntryMap，然后在上方工具栏选择蓝图（BluePrints），打开关卡蓝图（Open Level BluePrint）。引出BeginPlay引脚，选择OpenLevel，在LevelName中填写服务器IP地址，保存编译。

## 创建过渡关卡

&emsp;&emsp;用同样的方式创建关卡，命名为TransitionMap，无需给该关卡添加内容，保持空白即可。

## 指定游戏地图

&emsp;&emsp;菜单栏选择编辑（Edit）- 项目设置（Project Settings）- 项目（Project）- 地图和模式（Maps & Modes）- 默认地图（Default Maps），依次将编辑器初始地图（Editor Startup Map）设置为GameMap，游戏默认地图（Game Default Map）设置为EntryMap，过渡地图（Transition Map）设置为TransitionMap，服务器默认地图（Server Default Map）设置为GameMap。

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/5.png)

## 打包地图设置

&emsp;&emsp;菜单栏选择编辑（Edit）- 项目设置（Project Settings）- 项目（Project）- 打包（Packaging）- 打包地图列表（LIst of maps to include in a packaged build），分别添加EntryMap，TransitionMap，GameMap。

## 开始打包

&emsp;&emsp;菜单栏选择文件（File）- 打包项目（Package Project）- 窗口（Windows）- 64位窗口（Windows(64 bit)），然后选择打包目录，即可开始打包。

```
UATHelper: Packaging (Windows (64-bit)): BUILD SUCCESSFUL
UATHelper: Packaging (Windows (64-bit)): AutomationTool exiting with ExitCode=0 (Success)
```

&emsp;&emsp;根据提示，可以看到，打包成功。

# 运行服务器

&emsp;&emsp;之前已经提到，构建出的服务器目标文件需要在客户端资源目录中运行，因此需要拷贝过去。

## 拷贝服务器目标文件

&emsp;&emsp;将之前构建出的“~\Binaries\Win64\Shooting_GameServer.exe”拷贝到客户端的打包目录"WindowsNoEditor\Shooting_Game\Binaries\Win64"。

## 在命令行中运行服务器

&emsp;&emsp;由于服务器没有图形界面（GUI），因此需要在命令行中运行，否则什么输出都看不到。给拷贝过来的Shooting_GameServer.exe创建快捷方式，并在“目标”后加入“ -log”。

![](https://gitee.com/juzii/res/raw/master/pic/2019/09/ue4-dedicated-server-build/6.png)

&emsp;&emsp;双击运行该快捷方式，可以看到一下日志：

```
LogInit: WinSock: I am PC-20190329KLKV (192.168.52.160:0)
LogNet: GameNetDriver IpNetDriver_0 IpNetDriver listening on port 7777
LogWorld: Bringing World /Game/Levels/GameMap.GameMap up for play (max tick rate 30) at 2019.10.09-11.20.56
LogWorld: Bringing up level for play took: 0.002008
LogLoad: Took 0.129134 seconds to LoadMap(/Game/Levels/GameMap)
LogLoad: (Engine Initialization) Total time: 1.03 seconds
```

&emsp;&emsp;可以看到，服务器运行在192.168.52.160，监听端口为7777。打开cmd命令行，输入netstat -an|FIND "7777"

```
UDP    0.0.0.0:7777           *:*
```

&emsp;&emsp;由此可见，UE4服务器与客户端连接使用的是UDP协议，默认端口为7777。

&emsp;&emsp;至此，UE4项目的服务器构建完成，可以使用打出的客户端包来进行连接测试。

# 效果演示

<iframe src="//player.bilibili.com/player.html?aid=70657689&cid=122419943&page=1&high_quality=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="550"> </iframe>