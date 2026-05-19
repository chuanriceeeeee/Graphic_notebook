# 🐎 Rider x Unity 主程键盘流速查字典 (CheatSheet)

> **Tags:** #Rider #IDE #快捷键 #Unity开发 #效率工具 #代码重构

## 一、 🚀 万能之神 (The God Keys)
> **如果整个 Rider 你只能记住两个快捷键，必须是这两个！**

- **`Alt + Enter`：万能意图操作 (Show Intention Actions)**
  - **怎么用：** 无论遇到报错、黄线警告，还是想把 `if` 换成 `switch`，甚至在 Unity 里拼错了一个 `Tag` 名字，光标放上去，按下 `Alt + Enter`，Rider 会直接把解决药方喂到你嘴里！
- **`双击 Shift`：全知全能搜索 (Search Everywhere)**
  - **怎么用：** 连按两下 Shift。不管是搜类名、搜文件名、搜脚本里的某个变量、甚至是搜 Rider 的设置面板，全都可以靠它一键直达。

---

## 二、 🗺️ 虚空索敌与导航 (Navigation)
> **告别用鼠标在 Project 面板里一个个点开文件夹的低效行为。**

- `Ctrl + N` / `Ctrl + T`：**全局搜类名 (Go to Class/Type)**。找 C# 脚本的绝对神技。
- `Ctrl + Shift + N`：**全局搜文件 (Go to File)**。用来找 Shader、材质球、JSON 配置文件。
- `Ctrl + Shift + F`：**全局文本硬搜 (Find in Files)**。当你忘了某个报错字符串在哪个脚本里时用这个。
- `Ctrl + 鼠标左键` / `F12`：**转到定义 (Go to Declaration)**。按住看这个函数/变量是在哪写的。
- `Ctrl + -` (减号)：**返回上次位置 (Navigate Back)**。看完定义后，瞬间跳回刚才看代码的地方。*(对应 `Ctrl + Shift + -` 为前进)*
- `Alt + ↑ / ↓`：**在方法之间跳跃**。在当前脚本里，光标瞬间跳到上一个/下一个函数的名字处。
- `Shift + F4`：**将当前 Tab 撕裂成独立窗口**。双屏写代码、抄别人接口时的必备神技。

---

## 三、 ✍️ 键盘刺客：光标与编辑 (Editing)
> **主程写代码手是不需要离开键盘去摸鼠标的。**

- `Ctrl + D`：**向下复制当前行 (Duplicate Line)**。写大段重复赋值逻辑时狂按即可。
- `Ctrl + Y` / `Ctrl + X`：**瞬间删除当前行 (Delete Line)**。不需要选中，光标在这一行按就行。
- `Alt + Shift + ↑ / ↓`：**上下移动当前行 (Move Line)**。把当前这行代码向上或向下“挤”过去。
- `Ctrl + W`：**递进式选中 (Extend Selection)**。
  - **绝技：** 连按！第一次按选中当前单词，再按选中括号里的内容，再按选中整行，再按选中整个函数体！按 `Ctrl + Shift + W` 缩小选中范围。
- `Ctrl + /`：**单行注释** (`//`)。
- `Ctrl + Shift + /`：**多行注释** (`/* ... */`)。
- `Ctrl + Alt + L`：**全自动格式化 (Reformat Code)**。代码写得像狗屎一样乱？按下它，瞬间排列得整整齐齐。

---

## 四、 🏗️ 架构师之手：生成与重构 (Refactoring)
> **Rider 碾压 VS 的核心护城河。**

- `Alt + Insert`：**生成代码生成器 (Generate)**。一键生成构造函数、Getter/Setter、甚至 Unity 的 `Awake`/`Update` 等生命周期函数。
- `Shift + F6`：**安全重命名 (Rename)**。
  - **极其重要：** 绝对不要手改变量名！用这个修改，Rider 会自动把整个工程里所有引用了这个变量的地方全部安全改掉，不会报任何错！
- `Ctrl + Alt + V`：**提取变量 (Extract Variable)**。
  - 例如你写了 `GetComponent<Rigidbody>().velocity`，光标放在后面按快捷键，它自动帮你补全成 `var rigidbody = GetComponent<Rigidbody>();`
- `Ctrl + Alt + M`：**提取方法 (Extract Method)**。
  - 选中一段又臭又长的 `if` 逻辑，按下它，自动帮你把这段逻辑打包成一个独立的新函数。
- `Ctrl + Shift + R`：**重构菜单大全 (Refactor This...)**。不知道按哪个重构快捷键时，按这个呼出全家桶。

---

## 五、 🐛 抓虫大师：Unity 联调 (Debugging)

- `F9`：**在当前行打断点 / 取消断点**。
- `F5` / 点击右上角绿色小虫子：**附加到 Unity 并启动调试 (Attach to Unity)**。
- **(在断点卡住时)：**
  - `F10`：**步过 (Step Over)**。执行这一行代码，但不钻进函数内部。
  - `F11`：**步入 (Step Into)**。如果这行代码是个函数，直接钻进去看里面的执行过程。
  - `Shift + F11`：**步出 (Step Out)**。看懂了，不想一步步看了，直接把当前函数跑完并跳出来。
  - `Alt + 鼠标左键点击变量`：**瞬间计算表达式 (Evaluate Expression)**，直接查看当前这一帧这个变量的值。

---

## 六、 🎮 Unity 特殊外挂 (Unity Specific)

- **直接查看 Serialized 变量：** 在代码里看到 `[SerializeField]` 变量旁边的 `Unity 图标`，点击它可以直接查看当前场景里谁挂载了这个脚本，并且这个值在面板里填的是什么，**不需要切回 Unity 就能看！**
- **查找高昂性能消耗：** Rider 会在 `Update`、`LateUpdate` 函数以及它们调用的链条旁边画上**红色的波浪线或耗时标记**，警告你这里的代码每帧都在跑，尽量不要在这里用 `GetComponent` 或 `Instantiate`。