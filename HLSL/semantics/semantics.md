- 硬件指令
- 分类
  - APP to Vertex 
    - 输入语义
  - System-Value Semanstics
    - 系统值语义
    - SV_
      - clip space下的最终坐标
      - 直接可以拿去画三角形 硬件管线特殊关照的值
- A. 从模型到顶点着色器 (App -> Vertex) ：它是“固定提取器”
  - 在这个阶段（即我们刚才模板里的 Attributes 结构体），语义是绝对固定、不可瞎编的。因为网格（Mesh）文件（比如 FBX/OBJ）里存的数据类型是固定的。
    -: POSITION -> 提取顶点坐标

    -: NORMAL -> 提取法线

    -: TANGENT -> 提取切线

    -: COLOR -> 提取顶点颜色

    -: TEXCOORD0 ~ TEXCOORD7 -> 提取第 1 到第 8 套 UV 坐标

- B. 从顶点到片元着色器 (Vertex -> Fragment) ：它是“快递箱”
在这个阶段（即我们刚才模板里的 Varyings 结构体），数据的提取已经完成了，现在的任务是插值传递。这时候，除了系统保留的特殊语义（如 : SV_POSITION），像 TEXCOORD 这样的语义就变成了通用的插值通道（快递箱）。
```
High-level shader language
struct Varyings {
    float4 positionHCS : SV_POSITION; 
    
    // 借用 TEXCOORD 的名义，传递任意数据
    float3 worldPos    : TEXCOORD0; // 我塞了一个世界坐标进去
    float  myShadow    : TEXCOORD1; // 我塞了一个算好的阴影值进去
};
```
  - 在这里，TEXCOORD0 已经和模型的 UV 没有任何关系了。它只是告诉 GPU：“给我分配第 0 号插值寄存器，把 worldPos 这个数据平滑插值后送到片元着色器里”。



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