# 关键点

1. 渲染意味着从三维点(three-dimensional points)创建图像。
2. 帧(Frame)是 GPU 每秒渲染六十次(最佳)的图像。
3. 设备(Device)是硬件 GPU 的软件抽象。
4. 3D 模型由顶点网格(Vertex mesh)和阴影材料(Shading materials)组成，并分组为子网格(Submeshes)。
5. 在应用程序开始时创建命令队列(Command queue)。此操作组织你将在每一帧中创建的命令缓冲区(Command buffer)和命令编码器(Command encoders)。
6. 着色器(Shader)函数是在 GPU 上运行的程序。你可以在这些程序中定位顶点并为像素着色。
7. 渲染管道状态(Render pipeline state)将 GPU 固定为特定状态。它可以设置 GPU 应该运行哪些着色器函数以及如何格式化顶点布局。

学习计算机图形学很难。Metal API 是现代的，学习起来很轻松，但你需要预先了解大量信息。即使你现在感到不知所措，也要继续阅读下一章。重复将有助于你的理解。
