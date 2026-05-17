# way1

- 点击顶部菜单：Extensions (扩展) -> Manage Extensions (管理扩展)。

- 在右上角搜索并安装以下两个插件：

- 插件 1：HLSL Tools for Visual Studio

- 作用： 赋予 VS 解析 HLSLPROGRAM 内部代码的能力。它会为你提供语法高亮、自动缩进、甚至大部分内置数学函数（如 dot, lerp, saturate）的拼写补全。


- 时代的眼泪别用了 只支持2019
```
- 插件 2：ShaderLabVS (或者类似名字的 ShaderLab 支持插件)

- 作用： 专治外层的 Properties 和 SubShader 块，给你提供 Tags、Blend、ZWrite 等 Unity 专属命令的语法高亮和补全。

- 安装完重启 VS，你的代码世界就会从“黑白瞎子”变成“彩色向导”。
```
# way2
- 打开浏览器： 直接访问微软官方的 Visual Studio 网页版插件市场：
 https://marketplace.visualstudio.com/

- 搜索插件： 在网页顶部的搜索框里，分别搜索我们刚才提到的那两个插件：

- 输入 HLSL Tools for Visual Studio

- 下载文件： 点进插件详情页，点击右侧那个醒目的 Download 按钮。你会下载到一个后缀名为 .vsix 的文件。

- 关闭 VS：  极其重要的一步！ 在安装前，必须把现在开着的 Visual Studio 2022 完全关闭。

- 双击安装： 找到你刚才下载的 .vsix 文件，直接双击它。这时候会弹出一个独立的 VSIX Installer 安装程序，无脑点击 Install（安装）即可。