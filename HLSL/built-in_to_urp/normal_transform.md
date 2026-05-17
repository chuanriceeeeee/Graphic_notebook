# Built-in 到 URP 翻译字典

## 1. 骨架与结构 (Structure & Types)

| Built-in (旧)              | URP (新)                                   | 说明 (Note)                                                                              |
| :------------------------- | :----------------------------------------- | :--------------------------------------------------------------------------------------- |
| `CGPROGRAM` ... `ENDCG`    | **`HLSLPROGRAM` ... `ENDHLSL`**            | URP 全面抛弃 CG 语言，拥抱纯正的 HLSL。                                                  |
| `#include "UnityCG.cginc"` | **`#include "Packages/.../Core.hlsl"`**    | 核心库的路径变长了，这是最常见的报错来源。                                               |
| `fixed`, `fixed4`          | **`half`, `half4`**                        | `fixed`（极低精度）在现代 GPU 上已废弃，URP 统一使用 `half` 处理颜色，`float` 处理坐标。 |
| 直接声明变量               | 放进 **`CBUFFER_START(UnityPerMaterial)`** | 为了触发 SRP Batcher 优化，数值参数必须进缓冲区。                                        |

## 2. 空间转换宏 (Space Transforms)

| Built-in (旧)                 | URP (新)                              | 说明 (Note)                                      |
| :---------------------------- | :------------------------------------ | :----------------------------------------------- |
| `UnityObjectToClipPos(v)`     | **`TransformObjectToHClip(v)`**       | 模型坐标 -> 屏幕裁剪坐标 (必须传 3 维向量 xyz)。 |
| `UnityObjectToWorldNormal(n)` | **`TransformObjectToWorldNormal(n)`** | 模型法线 -> 世界法线 (用于光照计算)。            |
| `UnityObjectToWorldDir(dir)`  | **`TransformObjectToWorldDir(dir)`**  | 模型方向 -> 世界方向。                           |
| `mul(unity_ObjectToWorld, v)` | **`TransformObjectToWorld(v.xyz)`**   | 模型坐标 -> 世界坐标。                           |

## 3. 贴图与采样 (Textures & Sampling) —— **最大的语法改变**

| Built-in (旧)                            | URP (新)                                                        | 说明 (Note)                                           |
| :--------------------------------------- | :-------------------------------------------------------------- | :---------------------------------------------------- |
| `sampler2D _MainTex;`                    | **`TEXTURE2D(_MainTex);`** <br> **`SAMPLER(sampler_MainTex);`** | 现代 API（DX12/Vulkan）要求贴图和采样器必须分离声明。 |
| `tex2D(_MainTex, uv);`                   | **`SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, uv);`**          | 取色宏，必须把贴图和采样器一起传进去。                |
| `tex2Dlod(_MainTex, float4(uv, 0, mip))` | **`SAMPLE_TEXTURE2D_LOD(_MainTex, sampler_MainTex, uv, mip)`**  | 采样特定层级的 Mipmap（常用于顶点着色器）。           |

## 4. 深度与内置变量 (Depth & Built-ins)

| Built-in (旧)                    | URP (新)                                    | 说明 (Note)                                     |
| :------------------------------- | :------------------------------------------ | :---------------------------------------------- |
| `_Time.y`                        | **`_Time.y`**                               | 全局时间（单位：秒）。**没有变！**              |
| `LinearEyeDepth(depth)`          | **`LinearEyeDepth(depth, _ZBufferParams)`** | URP 需要传第二个参数来处理跨平台 Z 轴反转问题。 |
| `tex2D(_CameraDepthTexture, uv)` | **`SampleSceneDepth(uv)`**                  | URP 提供的高级深度采样魔法函数。                |

## 5. 光照 (Lighting)

| Built-in (旧)          | URP (新)                                                         | 说明 (Note)                                      |
| :--------------------- | :--------------------------------------------------------------- | :----------------------------------------------- |
| `_LightColor0`         | **`Light mainLight = GetMainLight();`**<br>**`mainLight.color`** | URP 封装了光照结构体，获取光照信息更加面向对象。 |
| `_WorldSpaceLightPos0` | **`mainLight.direction`**                                        | 获取主平行光的方向。                             |

## 6. 内置全局变量
| 变量类型     | Built-in (旧)          | URP (新)                     | 包含的数据说明                                                 |
| :----------- | :--------------------- | :--------------------------- | :------------------------------------------------------------- |
| **时间**     | `_Time`                | **`_Time` (没变)**           | `(t/20, t, t*2, t*3)` t 是从游戏启动开始的秒数。               |
| **正弦时间** | `_SinTime`             | **`_SinTime` (没变)**        | `(t/8, t/4, t/2, t)` 的正弦值。波浪动画最常用。                |
| **余弦时间** | `_CosTime`             | **`_CosTime` (没变)**        | `(t/8, t/4, t/2, t)` 的余弦值。                                |
| **增量时间** | `unity_DeltaTime`      | **`unity_DeltaTime` (没变)** | `(dt, 1/dt, smoothDt, 1/smoothDt)` 帧间隔时间。                |
| **屏幕参数** | `_ScreenParams`        | **`_ScreenParams` (没变)**   | `(width, height, 1+1/w, 1+1/h)` 屏幕像素宽高。                 |
| **深度参数** | `_ZBufferParams`       | **`_ZBufferParams` (没变)**  | 用于 `LinearEyeDepth` 等函数的内部计算。                       |
| **相机位置** | `_WorldSpaceCameraPos` | **`GetCameraPositionWS()`**  | **⚠️ 变了！** 强烈建议使用 URP 封装的这个函数获取相机世界坐标。 |