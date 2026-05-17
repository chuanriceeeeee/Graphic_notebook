```c++
// ==========================================
// 1. 发球点：在 appdata 里向模型网格索要 UV
// ==========================================
struct appdata
{
    float4 vertex : POSITION;
    // 告诉引擎：请把模型第 0 套 UV 拿给我，装进这个叫 uv 的变量里
    float2 uv : TEXCOORD0; 
};

// ==========================================
// 2. 中转站：在 v2f 里准备一个空位用来传递
// ==========================================
struct v2f
{
    float4 vertex : SV_POSITION;
    float4 screenPosition : TEXCOORD0; 
    // 准备一个空位传给 frag。
    // 注意：因为 TEXCOORD0 被屏幕坐标占用了，这里必须顺延用 TEXCOORD1
    float2 uv : TEXCOORD1; 
};

// ==========================================
// 3. 传球动作：在 Vertex Shader 里赋值
// ==========================================
v2f vert (appdata IN)
{
    v2f OUT;
    OUT.vertex = TransformObjectToHClip(IN.vertex.xyz);
    OUT.screenPosition = ComputeScreenPos(OUT.vertex);
    
    // 【核心动作】把拿到的原始 UV，直接塞进包裹里传下去
    // 你也可以在这里对 UV 进行缩放，比如 OUT.uv = IN.uv * _NoiseScale;
    OUT.uv = IN.uv; 
    
    return OUT;
}

// ==========================================
// 4. 终点站：在 Fragment Shader 里使用 UV 采样贴图
// ==========================================
half4 frag (v2f i) : SV_Target
{
    // 拿到快递箱 i 里的 uv 坐标，去贴图上取色！
    float colorValue = SAMPLE_TEXTURE2D(_MyTexture, sampler_MyTexture, i.uv).r;
    
    return half4(colorValue, colorValue, colorValue, 1.0);
}
```