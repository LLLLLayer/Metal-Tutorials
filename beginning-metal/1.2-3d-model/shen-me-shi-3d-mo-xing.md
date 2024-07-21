# 什么是 3D 模型？

3D 模型由顶点组成。每个顶点使用 x、y 和 z 值表示 3D 空间中的一个点。

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

正你你在上一章中看到的，你将这些顶点发送到 GPU 进行渲染。你需要三个顶点来创建一个三角形，而 GPU 能够高效地渲染三角形。为了显示较小的细节，3D 模型还可以使用纹理。你将在第 8 节“纹理”中了解有关纹理的更多信息。

➤ 打开本章的 Playground。

此 Playground 包含两个页面，分别名为“渲染和导出 3D 模型”和“导入火车”，以及 USDZ 格式的火车模型。如果你没有看到这些项目，你需要使用左上角的图标隐藏/显示项目导航器。

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

要显示文件扩展名，请打开 Xcode Settings，然后在 General Tab 上，选择 File Extensions: Show All

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

➤ 从项目导航器中选择“渲染并导出 3D 模型”。

此页面包含第 1 节“Hello, Metal!”中的最终代码。在 Playground 的实时视图中检查渲染的球体。请注意球体如何渲染为实心红色形状并显示为平面。

要查看每个三角形的边缘，你可以以线框形式(Wireframe)渲染模型。

➤ 要以线框形式渲染，请在 `renderEncoder.setVertexBuffer(...)` 之后添加以下代码：&#x20;

```swift
renderEncoder.setTriangleFillMode(.lines) 
```

此代码告诉 GPU 渲染线条而不是实心三角形。

➤ 运行 Playground：

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

这里出现了一点视觉错觉。虽然看起来可能不像，但 GPU 渲染的是直线。球体边缘看起来弯曲的原因在于 GPU 渲染的三角形数量。如果渲染的三角形较少，弯曲的模型看起来会“块状”。

现在你可以真正看到球体的 3D 特性。模型的三角形水平分布均匀，但由于你是在二维屏幕上查看，因此它们在球体边缘看起来比中间的三角形小。

在 Blender 或 Maya 等 3D 应用中，你通常会操作点、线和面。点是顶点；线(也称为边)是顶点之间的线；面是三角形的平坦区域。

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

顶点通常被排列成三角形，因为 GPU 硬件专门用于处理它们。GPU 的核心指令期望看到一个三角形。在所有可能的形状中，为什么是三角形？

* 三角形的点数是二维多边形中最少的。
* 无论你如何移动三角形的点，这三个点总是在同一平面上。
* 从任何顶点开始分割三角形时，它总是变成两个三角形。

在 3D 应用程序中建模时，通常使用四边形（四点多边形）。四边形适用于细分或平滑算法。 使用 Model I/O 框架导入模型时，Model I/O 会将这些四边形转换为三角形。
