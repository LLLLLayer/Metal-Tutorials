# 渲染管道

你准备好研究 GPU 管道了吗？太好了，让我们开始吧！

在下图中，您可以看到管道的各个阶段。

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

图形管道将顶点带入多个阶段，在此期间顶点的坐标在不同空间之间转换。

> 注意：本章介绍即时模式渲染 (IMR) 架构。Apple 自 A11 以来的 iOS 芯片和 macOS 的 Apple 芯片都使用基于图块的渲染 (TBR)。新的 Metal 功能能够利用 TBR。但是，为简单起见，您将从对通用 GPU 架构的基本了解开始。如果您想预览一些差异，请观看 Apple 的 WWDC 2020 视频[将你的 Metal 应用带到 Apple 芯片 Mac](https://developer.apple.com/videos/play/wwdc2020/10631/)。

作为 Metal 程序员，你只关心顶点和片段处理阶段，因为它们是此管道中仅有的两个可编程阶段。在本章后面，你将编写顶点着色器和片段着色器。对于所有不可编程的流水线阶段，例如顶点获取、基元组装和光栅化，GPU 都有专门设计的硬件单元来服务这些阶段。

