# GAD作业：Unreal Engine 与 Wwise 音频集成开发指南

## 一、开发指南

### 1.1 项目简介

本项目用于 GAD（Game Audio Design）课程作业，实现 Unreal Engine 游戏引擎与 Wwise 音频中间件的集成开发。

本次开发内容涵盖：

- 完成 Unreal Engine 和 Wwise 的安装和整合，跑通资产创建、打包和调用的过程
- 创建 GitHub 账户和 Repo 仓库，开发过程中确保有正常的上传记录

- 创建一个环境声基底资产，包括可循环播放设置，以及相应的 3D 化 Positioning 设置和 Attenuation 衰减曲线
- 创建一个篝火物件的声音资产，包括 Random 和 Blender Container 层级结构的具体设置，以及相应的 3D 化 Positioning 设置和 Attenuation 衰减曲线
- 创建一个角色跑步的声音资产，包括 Game Sync Switch 两种鞋子类型和四种材质类型和相应的 Switch Container 嵌套层级结构的具体设置
- 创建一条用于模拟空间混响的 Aux Bus，包括 Effect 插件和相应的 3D 化 Positioning 设置，以及声音传送到 Aux Bus 的设置
- 所有资产的结构和命名合理，并创建相应的 Event 资产用于引擎中调用
- 在地图场景中使用 Room 和 Portal 对象创建室内外空间和联通，以及组件内各个 Component 相对应功能的具体设置
- 室外空间需要播放环境声基底，以及实现室内外空间联通处的衰减过渡效果
- 室内空间需要设置混响效果
- 使用蓝图对象的方式创建一个篝火物件，摆放至地图中的室内空间，正确播放且带有混响效果
- 角色跑步声资产以 Notify 的形式配置在角色动画中，并指定到对应的 Socket 上
- 创建四种 Material 材质 Dirt、Rock、Wood 和 Grass，以及对应的 Physical Material 和 Surface Type，并将材质正确配置到场景中
- 对 Notify 的功能进行改造，实现 Raycast 射线检测材质的功能

------

### 1.2 环境准备

#### 安装 Unreal Engine

在官网上下载 Epic Games Launcher，正常安装完之后打开界面，登录账号后进入 Library 页面。

界面中的几个选项卡：

- **News**：新闻消息
- **Sample**：官方提供的案例，包含各个模块的教学案例，可下载使用
- **Fab**：Epic 公司推出的资产平台，包含 2D、3D、特效、美术、场景、声音等资产，很多素材免费

在 Library 下方点击加号，选择版本 5.4 或 5.5，点击安装，选择安装路径，等待完成。

本次开发使用5.5.4版本

#### 安装 Wwise

在官网下载 Audiokinetic Launcher，注册登录账号后进入 Wwise 界面。

本次课程使用指定版本 **2024.1.2**（1.3 版本也可用）。

安装步骤：

1. 点击 Install，选择版本 2024.1.2
2. 点击下一步
3. 默认一定要安装 Wwise 软件本体
4. 建议把 SDK 也装上（后续整合需要）
5. 选择部署平台：Windows 开发选 Microsoft，Mac 电脑可选 Apple
6. 选择安装路径
7. 插件页面选择 Select None（简化起见，不选付费插件）
8. 点击下一步，等待安装完成

------

### 1.3 创建工程

#### 创建 Wwise 工程

1. 打开 Wwise
2. 点击 New Project，输入工程名称
3. 选择保存目录
4. 平台保留 Windows（如需部署其他平台可添加）
5. 不勾选插件预设
6. 弹出许可证提示时点击 Close（免费版无需 License）

**Wwise 界面布局说明**：

- **Designer Layout (F5)**：默认布局，可查看 Event、Game Sync 等
- **SoundBank Layout (F7)**：打包 SoundBank 文件，供引擎识别使用

#### 创建 UE 工程

1. 打开 UE，第一次打开可能有预加载，需等待
2. 在弹出的窗口中，左侧 Games 下选择 **Third Person** 模板
3. 项目类型选择 **Blueprint**（本课程不涉及 C++）
4. 按需勾选 Starter Content
5. 选择项目路径
6. 点击 Create
7. 首次打开工程会编译着色器，等待完成后工程打开，出现默认地图，点击 Play 即可控制角色

------

### 1.4 集成 Wwise 到 UE

#### 一键整合

1. 打开 Audiokinetic Launcher
2. 在 Unreal Engine下可以看到创建好的 UE 工程
3. 点击**Integrate Wwise In Project**按钮
4. 选择Integration Version,即需要集成的Wwise版本
5. 选择创建好的Wwise的项目路径
6. 点击Update the Launcher to proceed
7. 完成

#### 整合后的设置

**Wwise 工程设置**：

1. 打开 Wwise 工程 → Project → Project Settings → SoundBank 选项卡
2. 勾选 **Enable auto SoundBank definition**（自动定义 SoundBank）
3. 检查 Root Output Path，默认为 Wwise 工程下的 GeneratedSoundBanks 路径

**UE 工程设置**：

1. 打开 UE 编辑器 → Edit → Project Settings → 找到 Wwise
2. Installation 下选择 Wwise Project Path，指向 Wwise 工程下的 GeneratedSoundBanks 路径,此处路径为../GAD_Wwise/GAD_Wwise.wproj
3. 选择Root Output Path，为 Wwise 工程下的 GeneratedSoundBanks 路径
4. 检查User Setting中的Wwise安装地址
5. 检查 IP 地址：Wwise 的 User Preferences 中启用 Wwise Authoring API，IP 为 127.0.0.1，端口 8080；UE的Wwise-User Setting-WAAPI 中的Waapi IP Address和Waapi port保持一致

#### 验证整合

1. 在 Wwise 中拖入一个声音资产，创建对应的 Event
2. 在 SoundBank Layout 中点击 Generate 打包
3. 在 UE 中打开 **Wwise Browser**
4. 新 Event 显示 Missing，点击 **Reconcile**，资产即被导入
5. 双击Event进入Content Browser按空格播放测试

之后所有新增的 Event 都按此流程创建。

## 2. 制作篝火声音

### 2.1 导入声音资源

将提供好的声音资源直接拖入 Wwise 的 **Actor-Mixer Hierarchy** 中，默认放到 SFX 目录下，也可以创建子文件夹归类。

### 2.2 创建 Work Unit

在大项目中，不同的 Work Unit 可以在版本控制软件中被不同的人 Check Out，互不影响。

右键创建 Work Unit：

- 创建 `Ambience` Work Unit，将环境类声音拖入
- 创建 `Campfire` Work Unit，将篝火相关声音放入

保存。

### 2.3 创建三层声音结构

篝火声音由三层组成：Loop、Crack、Sizzle。

**第一步：创建 Random Container**

选中 7 个 Crack 声音文件 → 按住 Shift → 创建 **Random Container**，命名为 `Campfire_Crack`

对 Sizzle 声音重复上述操作

**第二步：创建 Blend Container**

选中上面三个 Random Container → 按住 Shift → 创建 **Blend Container**，命名为 `OBJ_Campfire_Burn`

### 2.4 配置各层参数

#### Loop 层设置

在OBJ_Campfire_Burn_Loop 中 ，找到 **Loop** 选项，勾选 Enable，使其无限循环。

#### Crack 层设置

在 Crack 中：

- **Mode**：Play Type改为 Continuous（连续播放）
- **Loop**：Enable,并设为 Infinite（无限循环）
- **Transitions**:Enable,Transition Type设置为Trigger Rate，Duration设置为5s

**设置随机间隔**：双击 Duration旁的小圆点，Enable随机偏移（如 Min -2, Max 3），则实际间隔在 3~8 秒之间随机。

#### Sizzle 层设置

同 Crack 层设置。

#### 调整响度

在 General 选项卡中，通过 **Gain** 调整各层响度：

- Loop 可适当降低（如 -3dB）,Sizzle-5
- 其他层根据听感调整

### 2.5 创建 Event

1. 选中 `OBJ_Campfire_Burn 结构`
2. 右键 → **New Event** → **Play**
3. Event 会自动出现在 Events 目录下

在 Event 中按空格即可预览播放效果。

### 2.6 导入 UE

**第一步：生成 SoundBank**

切换到 SoundBank Layout (F7) → 点击 **Generate** 打包

**第二步：同步到 UE**

1. 打开 UE 的 **Wwise Browser**
2. 新 Event 显示 Missing → 点击 **Reconcile**
3. 在 Content Browser 的 `Wwise > Events` 目录下找到篝火 Event，按空格播放测试

## 3. 在 UE 中播放篝火声音

### 3.1 方式一：直接拖入场景

1. 从 Content Browser 直接将 Event 资产拖入场景
2. 自动生成带 **AkAmbientSound** 的对象
3. 选中该对象，在 AkAmbientSound 面板中：
   - 勾选 **Auto Post**（运行时自动播放）
   - 将 Ak Component 下的**Obstruction Occlusion** 的Refresh Interval设为 0（让 Wwise 处理遮挡效果，而非 UE 处理）

运行游戏即可听到篝火声音。

### 3.2 方式二：通过蓝图播放

**第一步：创建蓝图类**

1. 在 Content Browser 中右键 → **Blueprint Class** → 选择 **Actor**
2. 命名为 `BP_Campfire`

**第二步：添加 Ak Component**

打开蓝图，在 Components 面板中添加 **Ak Component**（Wwise 提供的组件）,命名为AkComp

**第三步：添加播放逻辑**

在 Event Graph 中：

1. 拖出 Ak Component 组件
2. 搜索并添加 **Post Ak Event** 节点
3. 将 Event 设为篝火的 Play Event-Play_OBJ_Campfire_Burn
4. 把Event Begin Play连接到Post Ak Event
5. 编译保存

**第四步：添加到场景**

将 `BP_Campfire` 拖入场景，运行游戏即可听到声音。如需视觉显示，可额外添加静态网格体（如 Cube）并调整位置。

## 4. 空间音频：房间（Room）与门户（Portal）

### 4.1 核心组件

在 Place Actors 中搜索 `ak`，可以看到 Wwise 提供的工具：

| 组件                        | 说明                           |
| :-------------------------- | :----------------------------- |
| **Ak Spatial Audio Volume** | 定义房间（Room），界定空间范围 |
| **Ak Portal**               | 连接两个房间的门户（门窗）     |

### 4.2 创建室外房间

1. 拖入一个 **Ak Spatial Audio Volume** 到场景
2. 按空格键切换视图模式，调整盒子大小，**包裹整个场景**
3. 命名为 `Room_Base`（室外基础房间）

**组件配置**（选中Room_Base后在SurfaceReflectorSet 面板中）：

Surface Properties的Enable Surface取消，Transmission Loss设置为0

Enable Component的Enable Surface Reflector Set取消



Room的面板中的Ak Event的Ak Audio Event中选择Play_AMB_2D_EXT_GEN,勾选Auto Post

Room下的Room的Transmission Loss设置为0

### 4.3 创建室内房间

1. 再拖入一个 **Ak Spatial Audio Volume**
2. 调整大小，包裹室内区域
3. 命名为 `Room_Int`

**组件配置**：

- **Surface Reflector**：保持默认启用（室内需要明显声学边界）
- **Ak Late Reverb**：暂留空

总结就是都不调整

**优先级设置**：

- 室内房间的Room 中的 Room的一项的 **Priority** 需要高于室外
- 室外默认 Priority 为 0，室内设为 1 或更高
- 这样角色进入室内时，会使用室内房间的音频设置
- 此外LateReverb的LateReverb的Priority设置为3

### 4.4 创建门户（Portal）

1. 拖入一个 **Ak Portal** 到场景
2. 旋转 Portal（红色 X 轴指向两个房间的连接方向）
3. 将 Portal 拖动到室内与室外房间的**交界面上**
4. 当 Portal 变为**蓝色**时，表示已正确连接两个房间
5. 调整 Portal 大小，使其覆盖门口区域

**验证连接**：选中 Portal，Details 面板中会显示连接到的两个房间名称（如 `Room_Base` 和 `Room_Interior`）

**Portal 属性**：

- **Ak Portal Component的Initial State:Enabled**：是否启用（可用于动态开关门）
- 默认 Enable，表示门是打开的

------

## 5. 声音衰减设置（Attenuation）

### 5.1 Wwise 端设置

在 Wwise 中对声音资产进行 3D 空间化设置。

**选中篝火的 Blend Container -OBJ_Campfire_Burn**：

1. 进入 **Positioning** 选项卡
2. 选择 **3D Spatialization-Position**
3. Attenuation中的Enable下一栏，选择New Attenuation Default
4. 3D Position的3D Position选择Emitter,勾选Diffraction and Transmission
5. max distance设置为1000
6. 曲线调整
7. Soundbank Generate All

完成后进入UE中调整效果

### 5.2 环境声的特殊设置

对于AMB_2D_EXT_GEN，

- Position下的Listener Relative Routing的3D Spatialization，选择Position+Orientation

- Attenuation选择New Attenuation Default

- Max Distance设置为5000

- 曲线调整
- 勾选3D Position中的Diffraction and Transmission

### 5.3 验证效果

1. 每次修改后，在 SoundBank Layout 中点击 **Generate**
2. 回到 UE 运行游戏
3. 靠近篝火：声音变大
4. 远离篝火：声音逐渐衰减至消失
5. 走进 Portal 区域：环境声逐渐衰减过渡

## 6. 混响效果（Aux Bus）

### 6.1 创建混响辅助总线

创建混响效果不在 Actor-Mixer Hierarchy 层级下，而是在 **Master Audio Bus** 层级下。

**两种 Bus 类型**：

| 类型          | 说明                         |
| :------------ | :--------------------------- |
| Audio Bus     | 可理解为目录（Folder）       |
| Auxiliary Bus | 辅助通道，效果器放在此通道上 |

**操作步骤**：

1. 在 Master Audio Bus 下创建一个 Audio Bus 作为目录（如命名为 `Reverb`）
2. 在 Audio Bus 下面创建 **Auxiliary Bus**（注意图标不同），命名为 `Reverb_Int`（室内混响）
3. 选中 Auxiliary Bus，在属性面板中的Listener Relative Routing勾选 **Enable**（Positioning 下）
4. 右键 → **Add Effect**，添加混响效果器
5. 选择 **RoomVerb**，找一个夸张的预设（Cathedrals-Hloy）
6. 可根据需要调整效果器参数
7. 保存

### 6.2 将 Aux Bus 同步到 UE

1. 在 Wwise 中生成 SoundBank
2. 打开 UE 的 **Wwise Browser**
3. 在 Bus 目录下找到新创建的 Aux Bus
4. 点击 **Reconcile** 导入资产

此时 Aux Bus 已成为 UE 中可用的 Wwise 资产。

### 6.3 将 Aux Bus 挂载到 Room

1. 选中 UE 场景中的室内 Room（如 `Room_Int`）
2. 找到 **Late Reverb** 组件
3. 取消勾选 **Auto Assign Aux Bus**（手动指定）
4. 将 Aux Bus 设为 `Reverb_Int`

### 6.4 启用声音的 Game-Defined Aux Sends

此时播放声音还没有混响效果，因为声音本身没有将干声发送到辅助通道。

**在 Wwise 中设置**：

1. 选中篝火声音（如 `OBJ_Campfire_Burn`）
2. 进入 **Routing** 选项卡
3. 找到 **Game-Defined Aux Sends**
4. 勾选 **Enable**
5. 发送量（Send Volume）可先设为 0
6. 保存并重新生成 SoundBank

### 6.5 测试混响效果

回到 UE 运行游戏：

- 在室外听篝火：无混响
- 走进室内：篝火声音带有明显混响效果（尤其是 Crack 爆裂声）

------

## 7. 音频监听器位置调整（Audio Listener）

### 7.1 问题背景

- **第一人称游戏**：Listener 位置和朝向默认与 Camera 一致，无需调整
- **第三人称游戏**：默认 Listener 也在 Camera 上，但更合理的设置是：
  - **位置**：角色头部（跟随角色移动）
  - **朝向**：Camera 方向（跟随镜头转动，便于听声辨位）

### 7.2 找到当前游戏模式

1. 进入 `Project Settings` → `Maps & Modes`
2. 查看当前使用的 **Game Mode**
3. 找到 **BP_DefaultCharacter**（运行游戏时生成的角色）
4. 双击打开该角色蓝图

### 7.3 添加监听器覆写逻辑

在角色蓝图的 Event Graph 中添加以下逻辑：

**节点**：

- `Event Tick`（游戏运行时每帧执行）
- `Set Audio Listener Override`（覆写默认监听器设置）

**参数设置**：

| 参数     | 来源                              | 说明               |
| :------- | :-------------------------------- | :----------------- |
| Location | 从角色 Mesh 的 `head` Socket 获取 | 监听器放在角色头部 |
| Rotation | 从当前 Camera 获取                | 监听器朝向跟随镜头 |

**具体操作**：

1. 从 `Event Tick` 拖出
2. 添加 `Set Audio Listener Override` 节点
3. Location：使用 `Get Socket Location` 从 Mesh 获取 `head` 骨骼位置
4. Rotation：从 Player Camera Manager 获取当前 Camera 朝向
5. 将两个值输入到 `Set Audio Listener Override`

### 7.4 效果

- 监听器位置在角色头上（符合人耳听音位置）
- 监听器朝向跟随摄像机（玩家转动镜头时，声音方向随之变化，便于听声辨位）

这种设置在第三人称射击游戏中尤为常用。

## 8. 脚步声：Wwise 端数据准备

### 8.1 创建目录结构

创建一个新的 Work Unit 命名为 `Player`，在其下创建 `Player_Foley` Actor-Mixer用于存放脚步声相关资产。在下方再新建Foley_Cloth,Foley_FS

### 8.2 导入音频资源

以跑步（Run）动作和两种材质为例：

| 动作 | 材质          | 说明          |
| :--- | :------------ | :------------ |
| Run  | Dirt（泥土）  | 跑步声 - 泥土 |
| Run  | Grass（草地） | 跑步声 - 草地 |

将对应的音频文件拖入 Wwise 的Foley_FS Actor-Mixer Hierarchy 中。

### 8.3 创建 Switch Group

脚步声涉及两个维度的切换：

- **鞋型**（ShoeType）：运动鞋、靴子等
- **地面材质**（Material）：泥土、草地、木头、石头等

**操作步骤**：

1. Game Sync中的Switches下的Default Work Unity下，右键 → `New Child` → `Switch Group`，命名为 `ShoeType`
2. 同样创建 `Material` 

**添加 Switch 值**：

在 `ShoeType` 下添加：

- `Sneaker`（运动鞋）
- `Highheel`（靴子）

在 `Material` 下添加：

- `Dirt`（泥土）
- `Grass`（草地）
- `Wood`（木头）
- `Rock`（石头）

> 本示例仅使用 `Boot` 和 `Dirt`、`Grass` 作为演示，其他值可按需扩展。

### 8.4 创建 Random Container

将同类型的音频文件放入 Random Container：

1. 选中 Dirt 材质的跑步音频 → 按住 `Shift` → 创建 **Random Container**，命名为 `FOL_FS_Run_Highheel_Dirt`
2. 同样创建 `FOL_FS_Run_Highheel_Grass`

### 8.5 创建 Switch Container

Switch Container 用于根据 Switch 的值播放对应的子声音。

**第一步：创建主 Switch Container**

在Foley_FS下右键 → `New Child` → `Switch Container`，命名为 `Foley_FS_Run`

**第二步：关联 Switch Group**

选中 `Foley_FS_Run`，在属性面板中将 **Switch Group** 设为 `ShoeType`，默认值设为 `Highheel`

**第三步：创建子 Switch 分支**

在 `Foley_FS_Run` 下面，为每种鞋型创建子结构：

| 分支                    | 内部结构                 |
| :---------------------- | :----------------------- |
| `Foley_FS_Run_Highheel` | 把Random Container拖进去 |
| `Foley_FS_Run_Sneaker`  | （本示例留空）           |

**第四步：给Foley_FS_Run_Highheel的属性填写映射**

Switch Group设置为Material

**第五步：设置默认值**

为 `Material` Switch Group 设置默认值（如 `Dirt`），当材质检测失败时使用。

**第六步：自动映射（快捷方式）**

Wwise 提供了自动映射功能：当 Switch 分支名称与子资产名称相似时，右键 → `Auto Assign` 即可快速完成映射。

### 8.6 测试 Switch 切换

1. 选中 `Footstep_Run`
2. 在 Transport Control 中点击播放
3. 切换 `Shoe_Type` 的值，观察播放的声音是否正确
4. 切换 `Material` 的值，观察播放的声音是否正确

### 8.7 创建 Event

右键选中 `Foley_FS_Run` → `New Event` → `Play`，命名为 `Play_Foley_FS_Run`

### 8.8 生成 SoundBank 并同步到 UE

1. 切换到 SoundBank Layout（F7）
2. 点击 **Generate** 打包 SoundBank
3. 打开 UE 的 **Wwise Picker**
4. 点击 **Reconcile** 导入所有新资产

同步完成后，在 UE 的 Content Browser → `Wwise` 目录下可以看到：

- `Events` 目录：`Play_Footstep_Run`
- `Switch Containers` 目录：`Footstep_Run`
- `Switch Groups` 目录：`Shoe_Type`、`Material`

------

## 9. 脚步声：动画通知（Anim Notify）

### 9.1 找到跑步动画

默认第三人称角色的跑步动画路径：
`/All/Game/Characters/Mannequins/Animations/Quinn/MF_Run_Fwd`

### 9.2 添加动画通知

1. 双击打开动画资源
2. 添加 **Notify Track**,添加FS Notify Track
3. 在动画时间轴上，右键添加Notify,选择 **AnimNotify_AkEvent**

### 9.3 配置 Ak Event Notify

在 Details 面板中，将 **Event** 设为 `Play_Foley_FS_Run`

> 为了让脚步声与动画脚步动作同步，建议在左右脚着地的关键帧位置各添加一个 Notify。

### 9.4 测试

点击动画预览窗口的播放按钮，应该能听到脚步声循环播放。

### 9.5 当前局限

此时脚步声只是固定地播放同一个声音，无法根据地面材质变化。材质检测功能将在下一章节实现。

## 10. 脚步声：材质检测与物理材质映射

### 10.1 定义 Surface Type

1. 打开 `Project Settings`
2. 在左侧找到 `Engine-Physics` → `Physical Surface`
3. 修改列表中的SurfaceType

本示例需要添加以下 Surface Type：

| Surface Type 名称 | 说明 |
| :---------------- | :--- |
| `Dirt`            | 泥土 |
| `Grass`           | 草地 |
| `Wood`            | 木头 |
| `Rock`            | 石头 |

添加后点击保存。

### 10.2 创建物理材质（Physical Material）

1. 在 Content Browser 中，在/All/Game/BP路径下，右键 → `Physics→`Physical Material`
2. 命名为 `PM_Dirt`（Physical Material 缩写为 PM）
3. 双击打开，将 **Surface Type** 设置为 `Dirt`
4. 同样创建 `PM_Grass`、`PM_Wood`、`PM_Rock`，并分别关联对应的 Surface Type

### 10.3 创建渲染材质并关联物理材质

1. 在 Content Browser 中，在/All/Game/BP路径下，右键 → `Material`，命名为 `M_Dirt`
2. 双击打开材质编辑器
3. 在 Details 面板中找到 **Physical Material** 选项
4. 设置为 `PM_Dirt`
5. 同样创建 `M_Grass` 并关联 `PM_Grass`
6. 可按需给材质添加颜色以便区分（如 Dirt 设为棕色，Grass 设为绿色）

### 10.4 将材质应用到场景物体

将材质直接拖拽到场景中的物体上：

| 物体       | 材质      |
| :--------- | :-------- |
| 地面       | `M_Dirt`  |
| 测试立方体 | `M_Grass` |

------

## 11. 脚步声：射线检测蓝图逻辑

### 11.1 打开动画通知蓝图

1. 打开Content/Characters/Mannequins/Animations/Quinn/MF_Run_Fwd.uasset，在动画序列的 Notify 轨道上
2. 右键点击 `Ak Event Notify`
3. 选择 **Open Notify Blueprint**

### 11.2 清除默认逻辑

将蓝图中原有的 Wwise 相关节点全部删除，从头构建。

### 11.3 射线检测核心逻辑

**节点一：获取脚部 Socket 位置**

- 使用 `Get Mesh Component` 获取角色模型
- 使用 `Get Socket Location` 获取指定骨骼的位置
- Socket Name 使用 `Attach Name` 变量（在 Notify 属性中填写，如 `foot_l` / `foot_r`）

**节点二：设置射线起点和终点**

| 参数  | 设置                            | 说明           |
| :---- | :------------------------------ | :------------- |
| Start | Socket 位置 + 向上偏移 10 单位  | 从脚部上方开始 |
| End   | Socket 位置 - 向下偏移 100 单位 | 向地面方向发射 |

**节点三：执行射线检测**

使用 `Multi Line Trace by Channel` 节点进行直线射线检测。

**节点四：处理检测结果（Branch）**

| 条件               | 处理         |
| :----------------- | :----------- |
| 检测成功（Hit）    | 进入后续逻辑 |
| 检测失败（No Hit） | 打印警告信息 |

**节点五：从检测结果中提取 Surface Type**

- 使用 `Break Hit Result` 展开检测结果
- 获取 `Physical Material`
- 调用 `Get Surface Type` 获取 Surface Type

**节点六：Switch on Surface Type**

根据检测到的 Surface Type 分别处理：

| Surface Type | 操作                       |
| :----------- | :------------------------- |
| `Dirt`       | 设置 Wwise Switch 为 Dirt  |
| `Grass`      | 设置 Wwise Switch 为 Grass |
| `Wood`       | 设置 Wwise Switch 为 Wood  |
| `Rock`       | 设置 Wwise Switch 为 Rock  |
| 默认         | 设置默认值（如 Dirt）      |

**节点七：设置 Wwise Switch**

- 使用 `Get or Create Ak Component` 获取或创建角色的 Ak 组件
- 使用 `Set Switch` 设置 Switch Group 和 Switch 值
  - **Switch Group**：`Material`（在 Wwise 中创建的材质分组）
  - **Switch Value**：根据检测到的 Surface Type 决定

**节点八：播放声音**

使用 `Post Ak Event` 播放脚步声 Event，选择 `Play_Footstep_Run`。

### 11.4 蓝图连接顺序

text

```
Begin → Get Socket Location → Line Trace → Branch
    ├─ (Hit) → Break Hit Result → Get Surface Type → Switch on Surface Type
    │           → Set Switch → Get or Create Ak Component → Post Ak Event
    └─ (No Hit) → Print String
```



### 11.5 填写 Socket 名称

1. 选中动画序列中的 `Ak Event Notify`
2. 在 Details 面板中找到 `Attach Name` 参数
3. 填写对应的脚部骨骼名称（如 `foot_l`、`foot_r`）

> 角色骨骼中的脚步 Socket 名称通常是 `foot_l`（左脚）和 `foot_r`（右脚）。

### 11.6 测试

运行游戏：

- 走在 Dirt 地面上 → 播放 Dirt 材质的脚步声
- 走到 Grass 立方体上 → 播放 Grass 材质的脚步声

------

## 12. 脚步声：应用混响

为了让脚步声也能受到房间混响的影响：

1. 在 Wwise 中，选中 `Foley_FS` Actor Mixer
2. 进入 **RTPC** 选项卡
3. 找到 **Game-Defined Aux Sends**
4. 勾选 **Override Parents,Enable**
5. 发送量（Send Volume）设为 `-6` 或根据需要调整

完成后重新生成 SoundBank，同步到 UE 即可生效。

