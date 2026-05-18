### `TA_Alpha混合与预乘污染踩坑记.md`
*(包含：AlphaBlend 原理、Premultiplied Alpha、Unity 混合指令连环坑)*

```markdown
# 👻 TA Alpha 混合与预乘污染踩坑记 (Alpha Blending)

> **Tags:** #混合模式 #Blend #StraightAlpha #PremultipliedAlpha #BugFix

## 一、 Alpha Blending 核心公式推导

最经典的 **Source-Over (源在上)** 半透明混合物理公式：
- **混合颜色：** `color = (top.rgb * top.a) + (bottom.rgb * (1 - top.a))`
  *(解析：顶层颜色按透明度显现 + 底层颜色透过顶层缝隙显现)*
- **最终透明度：** `alpha = top.a + bottom.a * (1 - top.a)`
  *(警告：绝对不是直接相加！而是顶层自身的 alpha + 底层透过来的有效 alpha)*

---

## 二、 预乘 Alpha (Premultiplied) vs 直通 Alpha (Straight)

### 1. 概念与“污染”的真相
当执行上述公式的 `color` 计算后，得出的 RGB 数值实际上已经被 `top.a` 压暗过了。
- **Straight Alpha (直通/纯净)：** RGB 只存纯粹的颜色发光值，不管透明度（比如纯红的半透明玻璃，RGB依然是 1,0,0）。
- **Premultiplied Alpha (预乘/污染)：** RGB 的值在存储前，就已经提前乘过了自己的 Alpha。

### 2. 还原纯净数据 (洗白解药)
如果你硬算出了公式结果，但下家系统需要纯净的 Straight Alpha，你必须把颜色强行**除以最终的 alpha**，把亮度提回来：
`return float4(color / alpha, alpha);`

---

## 三、 连环凶案：Unity 混合指令重复计算

### 1. Bug 现象：半透明发黑过曝
如果 Shader 返回了预乘过的颜色，但 `Pass` 里依然写着 `Blend SrcAlpha OneMinusSrcAlpha`，Unity 硬件底层会**再乘一次 Alpha**。
结果导致 $Alpha^2$ 衰减，半透明边缘极其暗淡、产生难看的黑边。

### 2. 主程的“两扇门” (解决方案)
写半透明 Shader 时，必须保证吐出的颜色和硬件混合指令“门当户对”：

- **🚪 路线一：纯净流 (标准物件 / 水面)**
  - **Shader 吐出：** 纯净的 `Straight Alpha`。
  - **硬件指令：** `Blend SrcAlpha OneMinusSrcAlpha` (让显卡去帮你乘那一刀)。
- **🚪 路线二：高级预乘流 (VFX 特效 / 多层火光叠加)**
  - **Shader 吐出：** 脏脏的 `Premultiplied Alpha`。
  - **硬件指令：** `Blend One OneMinusSrcAlpha` (强行将 Src 的系数改为 One，告诉显卡：我的 RGB 已经乘过了，你乘以 1 原样拿走即可！)。

### 3. 同一图层内的颜色切换 (Lerp 才是神)
在同一个 Shader 中，针对同一个平面（如水面和泡沫的切换），**绝对不要去手写复杂的 AlphaBlend 函数！**
由于水和泡沫最终都要交给底层显卡去和海底混合，它们自己内部切换只需要最基础的插值即可：
```hlsl
// surfaceNoise 是 0 或 1 的遮罩。0 显示水，1 显示泡沫。
return lerp(waterColor, _FoamColor, surfaceNoise);