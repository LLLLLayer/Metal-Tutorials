# MetalView

## MetalView

现在你有了 Playground，你将创建一个要渲染的视图。

➤ 通过添加以下内容导入你将使用的两个主要框架：

{% code fullWidth="false" %}
```swift
import PlaygroundSupport
import MetalKit
```
{% endcode %}

PlaygroundSupport 可让我们在 Assistant Editor 中查看实时视图，而 MetalKit 是一个使使用 Metal 更容易的框架。MetalKit 有一个名为 `MTKView` 的自定义视图和许多方便的方法，用于加载纹理、使用 Metal 缓冲区(Buffers)以及与另一个有用的框架交互：Model I/O，你将​​在后面了解它。

➤ 现在，添加以下内容：&#x20;

```swift
guard let device = MTLCreateSystemDefaultDevice() else { 
    fatalError("GPU is not support") 
} 
```

此代码通过创建设备来检查合适的 GPU：

> 注意：你遇到错误了吗？如果你不小心创建了 iOS 游乐场而不是 macOS 游乐场，你将收到 `fatalError`，因为某些设备不支持 iOS 模拟器。

要设置视图，请添加以下内容：&#x20;

```swift
let frame = CGRect(x: 0, y: 0, width: 600, height: 600)
let view = MTKView(frame: frame, device: device)
view.clearColor
  = MTLClearColor(red: 1, green: 1, blue: 0.8, alpha: 1)
```

此代码为 Metal 渲染器配置 `MTKView`。`MTKView` 是 macOS 上 `NSView` 的子类，也是 iOS 上 `UIView` 的子类。`MTLClearColor` 表示 RGBA 值，颜色值存储在 `clearColor` 中，用于设置视图的颜色。

### Model

Model I/O 是一个与 Metal 和 SceneKit 集成的框架。其主要目的是加载在 Blender 或 Maya 等应用中创建的 3D 模型，并设置数据缓冲区以便于渲染。

您无需加载 3D 模型，而是加载 Model I/O 基本 3D 形状（也称为图元）。3D 图元通常是立方体、球体、圆柱体或圆环。

➤ 将此代码添加到 Playground 的末尾：

```swift
// 1
let allocator = MTKMeshBufferAllocator(device: device)
// 2
let mdlMesh = MDLMesh(
  sphereWithExtent: [0.75, 0.75, 0.75],
  segments: [100, 100],
  inwardNormals: false,
  geometryType: .triangles,
  allocator: allocator)
// 3
let mesh = try MTKMesh(mesh: mdlMesh, device: device)
```

查看代码：

1. 分配器 `allocator` 管理网格数据的内存。
2. Model I/O 创建一个具有指定大小的球体，并返回一个包含数据缓冲区中所有顶点信息的 `MDLMesh`。
3. 为了让 Metal 能够使用网格，你需要将其从 Model I/O 网格转换为 MetalKit 网格 `MTKMesh`。

### 队列(Queue)、缓冲区(Buffer)和编码器(Encoder)

每个帧都包含你发送给 GPU 的命令。你将这些命令包装在渲染命令编码器(Render Command Encoder)中。命令缓冲区(Command Buffer)组织这些命令编码器，命令队列(Command Queue)组织命令缓冲区。

