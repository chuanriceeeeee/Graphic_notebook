- _Time.y

> Note: Global elapsed time in seconds. Usually multiplied with UV coordinates to create scrolling animations (like water ripples).

- TEXTURE2D(_Name) & SAMPLER(sampler_Name)

>Note: Modern URP way to declare a texture and its sampler separately. (Replaces old sampler2D).

- SAMPLE_TEXTURE2D(_Name, sampler_Name, uv)

>Note: Read the pixel color from the texture at the given UV coordinate. (Replaces old tex2D).