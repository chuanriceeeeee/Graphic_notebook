- 硬件指令
- 分类
  - APP to Vertex 
    - 输入语义
  - System-Value Semanstics
    - 系统值语义
    - SV_
      - clip space下的最终坐标
      - 直接可以拿去画三角形 硬件管线特殊关照的值
- 语法
```High-level shader language
[数据类型] [变量名] : [语义];
```

```HLSL
// Input structure
struct VS_INPUT
{
    float3 Position : POSITION;
    float2 TexCoord : TEXCOORD;
};

// Output structure
struct VS_OUTPUT
{
    float4 Position : SV_POSITION;
    float2 TexCoord : TEXCOORD;
};

cbuffer ConstantBuffer : register(b0)
{
    matrix worldViewProj; //Combined world, view, and projection matrix
};

VS_OUTPUT main(VS_INPUT input)
{
    VS_OUTPUT output;
    output.Position = mul(float4(input.Position, 1.0f), worldViewProj);
    output.TexCoord = input.TexCoord;
    return output;
}
```



- POSITION
- TEXCOORD
- SV_POSITION
- NORMAL