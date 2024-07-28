# 初始化

就像你在第一章中所做的那样，你需要设置 Metal 环境。

Metal 比 OpenGL 有一个很大的优势，因为你可以预先实例化一些对象，而不是在每一帧中创建它们。下图显示了你可以在应用程序启动时创建的一些 Metal 对象。

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

* `MTLDevice`：对 GPU 硬件设备的软件引用。
* `MTLCommandQueue`：负责每帧创建和组织 `MTLCommandBuffers`。
* `MTLLibrary`：包含来自顶点和片段着色器函数的源代码。
* `MTLRenderPipelineState`：设置绘制信息 — 例如要使用哪个着色器函数、要使用什么深度和颜色设置以及如何读取顶点数据。
* `MTLBuffer`：以可以发送到 GPU 的形式保存数据 — 例如顶点信息。

通常，你的应用中会有一个 `MTLDevice`、一个 `MTLCommandQueue` 和一个 `MTLLibrary` 对象。你还会有多个 `MTLRenderPipelineState` 对象，用于定义各种管道状态，以及多个 `MTLBuffers` 来保存数据。不过，在使用这些对象之前，你需要初始化它们。

➤ 打开 Renderer.swift，并将这些属性添加到 `Renderer`：

```swift
static var device: MTLDevice!
static var commandQueue: MTLCommandQueue!
static var library: MTLLibrary!
var mesh: MTKMesh!
var vertexBuffer: MTLBuffer!
var pipelineState: MTLRenderPipelineState!
```

为方便起见，所有这些属性目前都是隐式展开的可选项，但你可以稍后根据需要添加错误检查。

你正在使用 `device`、`commandQueue` 和 `library` 的类属性来确保每个属性只有一个。在极少数情况下，你可能需要多个，但在大多数应用中，一个就足够了。

➤ 仍然在 Renderer.swift 中，在 `super.init()` 之前将以下代码添加到 `init(metalView:)`：

```swift
guard
    let device = MTLCreateSystemDefaultDevice(),
    let commandQueue = device.makeCommandQueue() else {
        fatalError("GPU not available")
}
Self.device = device
Self.commandQueue = commandQueue
metalView.device = device
```

此代码初始化 GPU 并创建命令队列。

➤ 最后，在 `super.init()` 之后添加以下内容：

```swift
metalView.clearColor = MTLClearColor(
    red: 1.0,
    green: 1.0,
    blue: 0.8,
    alpha: 1.0)
metalView.delegate = self
```

此代码将 `metalView.clearColor` 设置为奶油色。它还将 `Renderer` 设置为 `metalView` 的委托，以便视图调用 `MTKViewDelegate` 绘制方法。

➤ 构建并运行应用程序以确保一切设置完毕并正常运行。如果一切正常，你将像以前一样看到 SwiftUI 视图，并且在调试控制台中，你将反复看到“draw”一词。使用此控制台语句验证你的应用是否在每一帧都调用 `draw(in:)`。

> 注意：你不会看到 metalView 的奶油色，因为你尚未要求 GPU 进行任何绘图。

