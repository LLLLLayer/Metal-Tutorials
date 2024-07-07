# 背景介绍

## Metal 简介

Metal 是用于图形处理单元或 GPU 的统一、低级、低开销的应用程序编程接口(API)。它是统一的，因为它适用于 3D 图形和数据并行计算范式。Metal 是一个低级 API，因为它为程序员提供了对 GPU 的近乎直接的访问。最后，Metal 是一个低开销的 API，因为它通过多线程和资源预编译来降低运行时成本。

除了技术定义外，Metal 是使用苹果 GPU 的并行处理能力来可视化数据或解决数值挑战的最合适方式。它还为机器学习、图像/视频处理或本书描述的图形渲染量身定制。

## 关于这本书

这本书向你介绍 Metal 中的低级图形编程——苹果在图形处理单元(GPU)上编程的框架。在这本书中，将学习制作游戏引擎的许多基础知识，并逐渐组合自己的引擎。一旦你的游戏引擎完成，你将能够组合 3D 场景，并编程你自己的简单 3D 游戏。由于你将从头开始构建 3D 游戏引擎，因此你将能够自定义你在屏幕上看到的内容的各个方面。

## Metal 的前世今生

从历史上看，我们有两个选择来利用 GPU：OpenGL 和仅限 Windows 的 DirectX。2013 年，GPU 供应商AMD 宣布了 Mantle 项目，以努力改进 GPU API，并提出 Direct3D(DirectX的一部分)和 OpenGL 的替代方案。AMD是 第一个为 GPU 的低级访问创建真正的低开销 API 的。Mantle 承诺能够生成比类似 API 多 9 倍的绘制调用(绘制到屏幕上的对象数量)，并引入了异步命令队列，以便图形和计算工作负载可以并行运行。不幸的是，该项目在成为主流 API 之前就被终止了。

Metal 于 2014 年 6 月 2 日在全球开发者大会(WWDC)上宣布，最初仅在 A7 或更新的 GPU 上提供。苹果创造了一种新语言，通过着色器(Shader)功能直接对 GPU 进行编程。这是基于 C++11 规范的 Metal 着色语言(Metal Shading Language，MSL)。一年后，在 WWDC 2015 上，苹果宣布了两个 Metal 子框架：MetalKit 和 Metal Performance Shaders(MPS)。2018 年，MPS 作为光线追踪加速器首次亮相。

API 继续发展，与苹果内部设计的新苹果 GPU 的配合使用。Metal 2 在许多新功能中增加了对虚拟现实(VR)、增强现实(AR）和加速机器学习(ML)的支持，包括图 Image Blocks, Tile Shading 和 Threadgroup Sharing。MSL 现在基于 C++14 规范。

2022年，苹果推出了带有 MetalFX 新框架的 Metal 3，用于升级较低的分辨率。Metal 3 的功能还包括直接从磁盘快速加载纹理资源，用于添加或减少几何形状的网格着色器，以及在 Metal 中使用 C/C++。

## 为什么使用 Metal

Metal 是一个一流的图形 API。这意味着 Metal 可以授权图形管道，如游戏引擎：

* Unity 和 Unreal Engine：当今两个领先的跨平台游戏引擎非常适合针对一系列控制台、桌面和移动设备的游戏程序员。然而，这些引擎并不总是跟上 Metal 的新功能。例如，在 Unity 中，镶嵌(Tessellation)被长期延迟，网格着色器仍然不受支持。如果你想使用最新的 Metal 开发，并使用 Apple Silicon 的强大功能，你不能总是依赖第三方引擎。
* Divinity - Original Sin 2: Larian Studios 与苹果密切合作，利用 Metal 和 Apple GPU 硬件，将他们令人惊叹的 3A 游戏带到 iPad 上。这确实是一种令人惊叹的视觉体验。
* The Witness：这款屡获殊荣的益智游戏有一个在 Metal 上运行的定制引擎。通过利用 Metal，iPad 版本与桌面版本一样华丽，强烈推荐给益智游戏爱好者。
* 其他：《杀手》、《生化奇兵》、《杀出重围》、《黑手党》、《星际争霸》、《魔兽世界》、《堡垒之夜》、《虚幻竞技场》、《蝙蝠侠》、《我的世界》等著名游戏。

Metal 并不局限于游戏世界，有许多应用程序受益于用于图像和视频处理的 GPU 加速：

* Procreate：一个用于素描、绘画和插图的应用程序。自从转换为 Metal 以来，它的运行速度比以前快了四倍。
* Pixelmator：一款基于 Metal 的应用，提供图像失真工具。他们能够实现由 Metal 2 提供支持的新绘画引擎和动态绘画混合技术。
* Affinity Photo：可在 iPad 上使用。据开发者 Serif 介绍，“使用 Metal 可让用户轻松处理大型超高分辨率照片或可能包含数千层的复杂构图。”
* Metal，尤其是 MPS 子框架，在卷积神经网络 (CNN) 的机器学习和深度学习领域非常有用。

## 什么时候使用 Metal

GPU 属于 Flynn 分类法中称为单指令多数据 (SIMD) 的特殊计算类别。简单来说，GPU 是针对吞吐量(一个单位时间内可以处理多少数据)进行优化的处理器，而 CPU 针对延迟(处理单个数据单位需要多少时间)进行优化。大多数程序以串行方式执行：它们接收输入、处理输入、提供输出，然后重复该循环。

这些循环有时会执行计算密集型任务，例如大型矩阵乘法，这需要 CPU 花费大量时间进行串行处理，即使在少数几个内核上以多线程方式处理也是如此。相比之下，GPU 有数百甚至数千个内核，它们比 CPU 内核更小，内存更少，但可以执行快速并行数学计算。

在以下情况下选择 Metal：

1. 你希望尽可能高效地渲染 3D 模型。
2. 你希望你的游戏拥有自己独特的风格，也许带有自定义照明和阴影。
3. 你将执行密集的数据处理，例如计算和更改每一帧屏幕上每个像素的颜色，就像处理图像和视频时一样。
4. 你有大型数值问题，例如科学模拟，你可以将其划分为独立的子问题以进行并行处理。
5. 你需要并行处理多个大型数据集，例如当你训练深度学习模型时。

## 本书目标读者

本书适合有兴趣学习 3D 图形或深入了解游戏引擎工作原理的中级 Swift 开发人员。如果你不了解 Swift，你仍然可以继续学习，因为所有代码说明都包含在书中。你将获得一般的图形知识，但如果您你了解 Swift 基础知识，就不会那么困惑。我们推荐[《Swift Apprentice：Fundamentals》](https://www.kodeco.com/books/swift-apprentice-fundamentals)一书。

掌握一些 C++ 知识也会很有用。编写 GPU 着色器函数时使用的 MSL 基于 C++。同样，你需要的所有代码都包含在书中。

如果你是 iOS/macOS 开发或 Metal 的初学者，你应该从头到尾阅读本书。

如果你是高级开发人员，或者已经有使用 Metal 的经验，你可以逐章跳读或将本书作为参考。



