- saturate(float x)

>Note: Clamp the value strictly between 0 and 1. (Similar to std::clamp(x, 0.0, 1.0) in C++).

- lerp(A, B, t)

>Note: Linear interpolation. Blend between A and B based on factor t.


- **`saturate(x)`：** GPU 硬件级别的超快限制函数。
  - 等价于 `clamp(x, 0, 1)`，强行把值按在 0 到 1 之间，防止颜色溢出（避免“白内障”发光现象）。
- **`step(edge, x)`：** 硬件优化的二元截断刀。`x > edge` 返回 1，否则返回 0。用于制造卡通渲染的硬阴影或水面的锐利浮沫。
- **`smoothstep(min, max, x)`：** 软截断。
  - 用于消除 `step` 带来的 Temporal Aliasing（时间性锯齿/边缘闪烁抖动），让边缘有极其微小的平滑过渡。