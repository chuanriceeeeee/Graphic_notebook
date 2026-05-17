# 📖 URP Shader & HLSL 核心语法字典

> **Tags:** #HLSL #URP #Shader #图形学算法 #UV字典

## 一、 核心变量类型与语义 (Types & Semantics)

### 1. 基础数据类型
- `float`, `float2`, `float3`, `float4`：高精度浮点数（类似于 GLSL 的 vec）。常用于坐标计算。
- `half`, `half2`, `half3`, `half4`：中精度浮点数。现代 URP **统一使用 half 处理颜色**（取代了过时的 fixed）。
- `int` / `uint`：32位有符号 / 无符号整数。
- `sampler2D`：传统的 2D 贴图采样器（URP 中已拆分为 Texture 和 Sampler）。

### 2. 语义 (Semantics) —— GPU 的包裹标签
语义是告诉硬件管线，这个变量到底装的是什么数据。格式：`[数据类型] [变量名] : [语义];`
- **应用到顶点 (APP to Vertex)：**
  - `: POSITION` - 模型的本地空间顶点坐标。
  - `: NORMAL` - 模型的本地法线。
  - `: TEXCOORD0` 到 `TEXCOORDn` - 模型的 UV 坐标通道。
- **系统值语义 (System-Value, SV_)：**
  - `: SV_POSITION` - 经过矩阵变换后的**裁剪空间最终坐标**，交给硬件直接画三角形，拥有极其特殊的地位。
  - `: SV_Target` - 片元着色器输出，渲染到屏幕的目标颜色。

---

## 二、 Built-in 到 URP 核心翻译表

### 1. 骨架与源码库 (Structure & Includes)
| Built-in (旧)              | URP (新)                                   | 说明                                      |
| :------------------------- | :----------------------------------------- | :---------------------------------------- |
| `CGPROGRAM` / `ENDCG`      | **`HLSLPROGRAM` / `ENDHLSL`**              | 彻底拥抱 HLSL。                           |
| `#include "UnityCG.cginc"` | **`#include "Packages/.../Core.hlsl"`**    | 核心库。内含矩阵宏转换等。                |
| *(隐式变量)*               | **`UnityInput.hlsl`**                      | URP 的基础变量全声明在这个文件里。        |
| *(旧版光照)*               | **`Lighting.hlsl`**                        | URP 光照计算逻辑源码。                    |
| 直接声明变量               | 放进 **`CBUFFER_START(UnityPerMaterial)`** | 触发 SRP Batcher 动态合批优化的必须操作。 |

### 2. 空间坐标转换宏 (Space Transforms)
| 功能             | URP 函数用法                         | 说明                                     |
| :--------------- | :----------------------------------- | :--------------------------------------- |
| **模型转裁剪**   | `TransformObjectToHClip(float3 xyz)` | 模型坐标 -> 屏幕裁剪坐标 SV_POSITION。   |
| **模型转世界**   | `TransformObjectToWorld(float3 xyz)` | 获取顶点的世界坐标。                     |
| **法线转世界**   | `TransformObjectToWorldNormal(n)`    | 获取世界空间法线（用于光照）。           |
| **获取相机坐标** | `GetCameraPositionWS()`              | **⚠️ 替代旧版 `_WorldSpaceCameraPos`**。  |
| **计算屏幕坐标** | `ComputeScreenPos(float4 clipPos)`   | 为屏幕空间特效准备（如深度采样、抓屏）。 |

### 3. 贴图与时间变量 (Textures & Built-ins)
| 旧版语法          | URP 新版语法                                            | 说明                                            |
| :---------------- | :------------------------------------------------------ | :---------------------------------------------- |
| `sampler2D _Tex;` | **`TEXTURE2D(_Tex);`** <br> **`SAMPLER(sampler_Tex);`** | DX12 现代规范：贴图与采样规则**必须分离声明**。 |
| `tex2D(_Tex, uv)` | **`SAMPLE_TEXTURE2D(_Tex, sampler_Tex, uv)`**           | 读取像素颜色。                                  |
| `tex2Dlod`        | **`SAMPLE_TEXTURE2D_LOD(...)`**                         | 顶点着色器采样贴图专用。                        |
| 时间秒数          | **`_Time.y`**                                           | 全局时间。乘以 UV 做流动动画。                  |
| 正弦时间          | **`_SinTime`**                                          | 包含了 `(t/8, t/4, t/2, t)` 的正弦值。          |

---

## 三、 UV 流转与图形学数学 (UV & Math)

### 1. 标准 UV 传递管线 (发球 -> 中转 -> 取色)
```hlsl
struct appdata {
    float4 vertex : POSITION;
    float2 uv : TEXCOORD0; // 发球：向模型索要第 0 套 UV
};

struct v2f {
    float4 vertex : SV_POSITION;
    float2 uv : TEXCOORD0; // 中转：留一个空位给插值硬件
};

v2f vert (appdata IN) {
    v2f OUT;
    OUT.vertex = TransformObjectToHClip(IN.vertex.xyz);
    OUT.uv = IN.uv; // 传球：将 UV 塞进快递箱 (可在此进行 TRANSFORM_TEX 缩放)
    return OUT;
}

half4 frag (v2f i) : SV_Target {
    // 终点站：拿着插值好的 UV 去采样贴图
    return SAMPLE_TEXTURE2D(_MyTex, sampler_MyTex, i.uv);
}