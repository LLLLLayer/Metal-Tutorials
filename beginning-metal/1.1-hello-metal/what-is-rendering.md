# 什么是渲染(Rendering)

在 3D 计算机图形学中，你需要获取一堆点(Points)，将它们连接在一起并在屏幕上创建图像，此图像称为渲染。

从点渲染图像涉及计算屏幕上每个像素的亮度(Light)和阴影(Shade)。光线在场景中反射，因此你必须决定照明的复杂程度以及每幅图像的渲染时间。皮克斯电影中的单个图像可能需要几天才能渲染，而游戏需要实时渲染，玩家可以立即看到图像。

渲染 3D 图像的方法有很多，但大多数都是从在 Blender 或 Maya 等建模应用程序中构建的模型开始的。例如，这个在 Blender 中构建的火车模型：

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

与所有其他模型一样，此模型由顶点(Vertices)组成。顶点是指三维空间中两个或多个直线、曲线或几何形状的边缘相交的点，例如立方体的角。模型中的顶点数量可能从立方体中的几个到更复杂模型中的数千甚至数百万不等。

3D 渲染器(3D Renderer)将使用模型加载器代码(Model loader code)读取这些顶点，该代码解析顶点列表。然后，渲染器将顶点传递给 GPU，着色器函数(Shader Functions)处理顶点以创建最终图像或纹理，并将其发送回 CPU 并显示在屏幕上。

以下渲染使用 3D 火车模型和一些不同的着色技术，使火车看起来像是用闪亮的铜制成的：

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

从导入模型顶点到在屏幕上生成最终图像的整个过程通常称为渲染管道(Rendering Pipeline)。渲染管道是发送到 GPU 的命令列表(Commands list)，以及构成最终图像的资源(顶点、材质(Materials)和灯光(Lights))。

管道包括可编程和不可编程功能。管道的可编程部分称为顶点函数(Vertex functions)和片段函数(Fragment functions)，你可以在其中手动影响渲染模型的最终外观。你将在本书后面详细了解每个函数。
