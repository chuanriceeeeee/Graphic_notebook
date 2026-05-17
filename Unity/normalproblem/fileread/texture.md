### 读取
- Unity 有极其强大的 Asset Database（资产数据库） 系统。
  - 当在操作系统里把一张 Noise.png 拖进 Unity 的 Assets 目录（比如建一个 Assets/Textures/Water 文件夹专门放它）时，Unity 会立刻为这张图生成一个唯一的 GUID（一长串乱码身份证号），并生成一个 .meta 文件。

- Unity 是靠 GUID 来认人的，不靠文件夹路径
- 在Shader的Properties中写槽位 Unity会自动生成一个interface拖拽texture

