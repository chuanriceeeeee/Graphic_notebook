# 🛠️ Unity 编辑器与环境配置速查 (CheatSheet)

> **Tags:** #Unity #快捷键 #环境搭建 #URP配置 #IDE

## 一、 快捷键与视口操作 (Shortcuts)

### 1. 基础工具单键 (QWERTY)
- `Q`：扒手拖动（Scene 漫游）
- `W`：移动（GameObject）
- `E`：旋转
- `R`：三维缩放（GameObject 整体缩放，不会变形）
- `T`：单维缩放（GameObject 局部拉伸，会变形）
- `Y`：平移、旋转、缩放综合工具
- `Z`：切换 坐标系中心点（Pivot / Center）
- `X`：切换 坐标系方向（Local / Global）
- `V`：顶点吸附（按住 V 拖动，用于模型边缘精准对齐）

### 2. 窗口切换与视图控制
- `Ctrl + 1~6`：分别切换到 Scene(1), Game(2), Inspector(3), Hierarchy(4), Project/Assets(5), Animation(6)
- **Scene 视角控制：**
  - `F` 或 `双击物体`：将选中的对象放在屏幕中心（快速聚焦）
  - `Shift + F`：镜头跟随物品移动
  - `Alt + 鼠标左键拖拽`：围绕目标旋转观察
  - `鼠标右键拖拽`：原地转头观察（配合 WASD 可漫游）
  - `Ctrl + Shift + F`：**对齐视图！**将选中的相机瞬间移到你当前 Scene 窗口的观察点
- **Game 窗口控制：**
  - `Shift + 空格`：全屏 / 恢复当前聚焦的窗口（如 Game 或 Scene 窗口）
  - `Ctrl + P`：播放 (Play)
  - `Ctrl + Shift + P`：暂停 (Pause)
  - `Shift + 空格`：切换透视 / 等距视图（正交）

### 3. Hierarchy 节点操作
- `Ctrl + D`：原地复制并粘贴当前选中对象
- `Alt + →` 或 `Ctrl + →`：展开选中对象的全部层级
- `Alt + ←` 或 `Ctrl + ←`：收缩选中对象的全部层级

---

## 二、 URP 环境搭建标准流 (Pre-Environment)

1. **下载核心包：**
   - `Window` -> `Package Manager` -> 切换到 `Packages: Unity Registry`
   - 搜索 `Universal RP` -> `Install`
2. **创建渲染管线资产：**
   - 在 Project 面板新建一个文件夹 (如 URP_Settings)
   - 右键 `Create` -> `Rendering` -> `URP Asset (with Universal Renderer)`
3. **全局应用 URP：**
   - `Edit` -> `Project Settings` -> `Graphics` -> 拖入刚才创建的 URP Asset
   - `Edit` -> `Project Settings` -> `Quality` -> 在对应画质层级同样拖入 URP Asset *(注意：Quality 的设置优先级高于 Graphics)*

---

## 三、 IDE 与编辑器工作流 (Workflow)

### 1. 绑定外部代码编辑器 (External Editor)
- 路径：`Edit` -> `Preferences` -> `External Tools`
- 在 `External Script Editor` 下拉菜单中选择 Visual Studio 或 Rider。

### 2. Visual Studio 2022 配置指南
- **推荐方案（离线安装）：** VS2022 会有网络环境问题导致商店打不开。
  1. 浏览器访问 `https://marketplace.visualstudio.com/`
  2. 搜索并下载 `HLSL Tools for Visual Studio` 的 `.vsix` 文件。
  3. **完全关闭 VS2022**，双击下载好的 `.vsix` 文件无脑 Install。
- *(注：ShaderLabVS 插件因内核问题已不支持 VS2022，放弃使用即可。)*

### 3. Scene vs Game 预览陷阱
- **致命陷阱：** Play（运行）模式下修改的任何材质、参数，退出运行后会**全部重置丢失**。
- **TA 工作流：** 在 Scene 窗口的特效菜单（小风景画图标）中勾选 **`Always Refresh`**。这样时间 (`_Time`) 也会在编辑模式流逝，可安全调试水波动画且不丢失数据。