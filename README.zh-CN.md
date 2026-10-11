# BallDarkEcho — 回声信息局域网竞技游戏

[English](README.md) · [简体中文](README.zh-CN.md)

一款 Windows 局域网竞技游戏：移动、扫描和开火会通过回声波与空间声音暴露玩家。项目使用 Unreal Engine 5.7、C++ 和 Wwise 2025.1.8，把游戏状态转化为低光竞技场中可看见、可听见的信息。

![从现有演示视频提取的双玩家回声玩法画面](Docs/images/echo-gameplay.png)

[演示视频](1_Demo_Video/BallDarkEcho_Final_Demo.mp4) · [Windows 版本](2_Final_Build/BallDarkEcho_Windows_Development.zip) · [游戏说明书](3_UE_Project_and_Game_Manual/BallDarkEcho_Game_Manual.docx) · [音频设计](AUDIO_DESIGN.zh.md)

## 玩家体验

- 创建或加入局域网房间，选择地图与技能，准备后进入死亡竞赛。
- 用青／蓝色波读取移动、扫描和导航信息，用红色波读取开火暴露。
- 在普通移动、疾跑和安静移动之间选择，权衡行动与暴露。
- 切换三种武器模式，并使用选定的角色技能。
- 查看生命值、击杀、死亡和比赛目标；比赛期间被击败后重生。

## 我的实现与项目背景

这是我的 WM9M6 Games Engineering 游戏项目。实现内容连接了回声信息规则、组件式玩法、大厅与比赛流程，以及 Unreal 与 Wwise 的集成。音频部分包含 Blueprint 可调用的 C++ 事件组件、按位置播放、按 playing ID 停止／淡出，以及移动和武器的事件接入。

Unreal 提供引擎与网络基础，Wwise 提供音频编辑与播放。这些基础系统和第三方素材与我实现的玩法及集成代码分开说明。项目的技术重点是把玩法事件、视觉暴露与音频路由连接起来。

## 技术实现

| 技术／系统 | 用途 | 代码入口 |
| --- | --- | --- |
| Unreal C++ 与 Blueprint | 组件、可编辑参数与游戏状态交互 | [Source/BallDarkEcho](Source/BallDarkEcho) |
| LAN 会话与同步玩法 | 大厅流程、服务器动作和共享比赛状态 | [EchoGameInstance.cpp](Source/BallDarkEcho/EchoGameInstance.cpp)、[EchoDeathmatchGameMode.cpp](Source/BallDarkEcho/EchoDeathmatchGameMode.cpp) |
| 视觉回声与开火暴露 | 从玩家动作生成可读波形 | [EchoMovementPulseComponent.cpp](Source/BallDarkEcho/EchoMovementPulseComponent.cpp)、[EchoWeaponComponent.cpp](Source/BallDarkEcho/EchoWeaponComponent.cpp) |
| Wwise 事件桥接 | 解析已分配事件，按位置播放和停止 | [EchoAudioEventComponent.cpp](Source/BallDarkEcho/EchoAudioEventComponent.cpp) |
| Wwise 编辑与 SoundBank | 配置事件，提供 Windows 播放数据 | [Wwise 工程](BallDarkEcho_WwiseProject)、[音频设计](AUDIO_DESIGN.zh.md) |

## 体验版本与打开源码

现有版本为 Windows 64 位 Development 构建。下载 ZIP 并解压，运行 `Windows/BallDarkEcho.exe`。主机与客户端需要处于同一局域网。主机创建房间，客户端加入并选择 **READY UP**，主机再开始比赛。[快速说明](3_UE_Project_and_Game_Manual/README.md) 介绍了比赛流程。

视频与 ZIP 使用 Git LFS。克隆后需要安装 Git LFS 并运行 `git lfs pull`；小型指针文件不是实际视频或游戏包。

源码开发需要 Unreal Engine 5.7、Visual Studio C++ 工具链和 Wwise Unreal Integration 2025.1.8。将本地安装的 Wwise 插件放入 `Plugins/`，打开 `BallDarkEcho.uproject`，构建 `BallDarkEchoEditor Win64 Development`。SDK 安装内容未上传；请保持 Unreal 与 Wwise 的相对路径。

操作：WASD 移动，鼠标瞄准，Space 跳跃，左键开火，Shift 疾跑，Ctrl 安静移动，Q/1/2/3 切换武器，R 技能，按住 Tab 查看地图。

## 状态与限制

仓库提供已有演示视频和 Windows 构建。本次文档整理核对了源码与链接，没有重新进行完整 Unreal 或打包多人测试。

共享音频桥接和移动声音的播放／停止已有实现。部分事件映射、混音和空间音频方案仍需要逐项编辑与播放验证。事件桥接中的 `Loudness` 参数目前未使用，不能写成已经完成音量 RTPC。[AUDIO_DESIGN.zh.md](AUDIO_DESIGN.zh.md) 包含六月的开发状态与未来设计，不代表当前全部音频功能通过测试。

## 来源与署名

Unreal Engine 和 Wwise 遵循各自条款。声音来源记录保留在 [AudioSource/_licenses/sources.md](AudioSource/_licenses/sources.md)，其中仍有待核实条目。没有为整个游戏或素材新增许可证。
