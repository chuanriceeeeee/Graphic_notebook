# 🌊 URP 卡通水面高级特性实战 (DepthNormals & Foam)

> **Tags:** #URP #ToonWater #法线采样 #深度运算 #动态泡沫

## 一、 URP 原生法线深度提取 (告别 Replacement Shader)

### 1. 时代眼泪的终结
- **旧版 Built-in 管线坑点：** 开启 `DepthNormals` 时，底层会把 16位深度和 16位法线强行塞进一张 32位图里，导致深度精度报废。老教程常通过 C# 生成副相机并用 Replacement Shader 单独画法线。
- **URP 现代解法：** 直接在主相机脚本中请求 `depthTextureMode = DepthTextureMode.DepthNormals`。URP 会极度阔气地生成两张独立的高清图：`_CameraDepthTexture` 和 `_CameraNormalsTexture`。老旧的替换渲染代码必须**全部删掉**。

### 2. Shader 提取写法
```hlsl
// 1. 引入双库
#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/DeclareDepthTexture.hlsl"
#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/DeclareNormalsTexture.hlsl"
```
// 2. 提取世界法线
float3 underwaterNormal = SampleSceneNormals(spaceUV);

### 3.极简点乘法测坡度
// 水面法线默认朝上 float3(0,1,0)。两法线点乘，刚好等于水底法线的 Y 分量！
// normalDot 越接近 1 说明越平坦，越接近 0 说明越陡峭。
float normalDot = saturate(dot(underwaterNormal, waterNormal));

// 动态插值计算当前像素应该拥有的泡沫触发距离
float dynamicFoamDistance = lerp(_FoamDistanceMin, _FoamDistanceMax, normalDot);

// 将动态距离代入原始深度截断公式
float foamDepthDifference = saturate(depthDifference / dynamicFoamDistance);


### 4.多维度参数解耦设计 (Decoupling)
// 1. 先抽出未经任何强度污染的纯净底座 (-1 到 1 的方向向量)
float2 rawDistortion = SAMPLE_TEXTURE2D(_Distortion, sampler, UV).xy * 2 - 1;

// 2. UV 偏移和法线偏移，各自拿底座乘以自己的独立控制变量
float2 uvOffset = rawDistortion * _UVDistortionAmount;
float3 normalOffset = float3(rawDistortion.x * _NormalAmount, 1, rawDistortion.y * _NormalAmount);