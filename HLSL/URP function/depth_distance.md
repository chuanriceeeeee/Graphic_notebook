- SampleSceneDepth(float2 uv)

> Note: A magic URP function to get raw depth value in [0, 1]range from the camera's depth texture.
 
- LinearEyeDepth(float rawDepth, _ZBufferParams)

>Note: Convert non-linear raw depth into linear eye depth (actual physical distance from the camera in meters).