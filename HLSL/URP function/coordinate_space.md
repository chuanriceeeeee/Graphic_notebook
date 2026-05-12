- TransformObjectToHClip(float3 xyz)

> Note: Transform the vertex position from Object Space to Clip Space. (只接收三维向量).

- ComputeScreenPos(float4 clipPos)

> Note: Compute the screen position. Used to prepare coordinates for screen-space effects.

- screenPos.xy / screenPos.w

>Note: Perspective Divide (透视除法). Get real normalized 2D UV in screen space.