### 深度图 (Depth Texture) 丢失的“抓包”指南
- **全局总开关：** 必须在 URP Asset (Graphics Settings) 里勾选 `Depth Texture`。
- **Scene 视图一票否决权：** Scene 窗口的特效下拉菜单（小风景画图标）里，必须勾选 **`Post Processing`**，否则引擎会为了省电跳过深度渲染。
- **强力驱魔三招：** 如果全开了还是无效，尝试以下操作刺激引擎刷新缓存：
  1. 关闭并重开 Scene 标签页（最有效）。
  2. 开启 URP Asset 里的 `Opaque Texture`。
  3. 关闭 URP Asset 里的 `MSAA` 抗锯齿。