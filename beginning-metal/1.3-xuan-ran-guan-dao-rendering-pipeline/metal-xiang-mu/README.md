# Metal 项目

到目前为止，你一直在使用 Playgrounds 来了解 Metal。Playgrounds 非常适合测试和学习新概念，但了解如何使用 SwiftUI 设置完整的 Metal 项目也很重要。

➤ 在 Xcode 中，使用 Multiplatform App 模板创建一个新项目。

➤ 将你的项目命名为 Pipeline，并填写你的团队和组织标识符。将存储设置设置为 None，并取消选中所有复选框选项。

➤ 选择新项目的位置。

太好了，你现在有一个漂亮的新 SwiftUI 应用程序。ContentView.swift 是应用程序的主视图；这是你调用 Metal 视图的地方。

MetalKit 框架包含一个 MTKView，这是一个特殊的 Metal 渲染视图。这是 iOS 上的 UIView 和 macOS 上的 NSView。要与 UIKit 或 Cocoa UI 元素交互，你将使用位于 SwiftUI 和 MTKView 之间的 Representable 协议。

此配置有点复杂，因此在本章的资源文件夹中，你将找到一个预制的 MetalView.swift。

➤ 将此文件拖到你的项目中，确保选中所有复选框，以便你复制该文件并将其添加到应用程序的目标。

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

> 注意：有时 Xcode 不会复制文件，但仍然指向资源目录中的文件。你可能更愿意将文件复制到项目代码目录，然后从那里添加文件。

➤ 打开 MetalView.swift。

MetalView 是一个 SwiftUI View 结构，包含 MTKView 属性并托管 Metal 视图。MetalViewRepresentable 是 UIView 或 NSView 之间的接口，具体取决于操作系统。

➤ 打开 ContentView.swift，并将 body 的内容更改为：

```swift
VStack {
    MetalView()
      .border(Color.black, width: 2)
    Text("Hello, Metal!")
}
.padding()
```

在这里，你将 MetalView 添加到视图层次结构并为其添加边框。

➤ 使用 macOS 目标或 iOS 目标构建并运行你的应用程序。

你将看到托管的 MTKView。使用 SwiftUI 的优势在于，在 Metal 视图下分层 UI 元素(例如此处的“Hello Metal”文本)相对容易。

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

现在你可以选择。你可以子类化 MTKView，并用子类化后的 MTKView 替换 MetalView 中的 MTKView。在这种情况下，子类的 draw(\_:) 会在每一帧中被调用，你可以将绘图代码放在该方法中。

但是，在本书中，你将设置一个符合 MTKViewDelegate 的 Renderer 类，并将 Renderer 设置为 MTKView 的委托。MTKView 每帧都会调用一个委托方法，你将在此处放置必要的绘制代码。

> 注意：如果你来自不同的 API 世界，可能正在寻找游戏循环构造。你确实可以选择使用 CADisplayLink 进行计时，但 Apple 引入了 MetalKit 及其协议，以便更轻松地管理游戏循环。



