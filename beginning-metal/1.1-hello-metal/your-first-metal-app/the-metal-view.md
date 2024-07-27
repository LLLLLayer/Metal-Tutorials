# Metal 视图

## Metal View

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

你无需加载 3D 模型，而是加载 Model I/O 基本 3D 形状（也称为图元）。3D 图元通常是立方体、球体、圆柱体或圆环。

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

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

➤ 添加此代码以创建命令队列：

```swift
guard let commandQueue = device.makeCommandQueue() else {
    fatalError("Could not create a command queue")
}
```

你应该在应用程序开始时设置设备和命令队列，并且通常应该始终使用相同的设备和命令队列。

每个渲染过程都必须尽快完成，因此你将在应用程序开始时预加载对象。你将模型加载到缓冲区中，生成着色器函数并创建管道状态对象(Pipeline state objects)。

在每一帧上，你将创建一个命令缓冲区和至少一个描述渲染过程的渲染命令编码器。渲染命令编码器是一个轻量级对象，它设置 GPU 的管道状态并告诉 GPU 在渲染过程中要使用哪些缓冲区。

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 着色器函数

着色器函数是在 GPU 上运行的小程序。你可以使用 MSL (C++ 的一个子集)编写这些程序。通常，你会专门为着色器函数创建一个带有 .metal 扩展名的单独文件，现在，可哟创建一个包含着色器函数代码的多行字符串，并将其添加到你的 Playground 中：

```swift
let shader = """
#include <metal_stdlib>
using namespace metal;

struct VertexIn {
    float4 position [[attribute(0)]];
};

vertex float4 vertex_main(const VertexIn vertex_in [[stage_in]]) {
    return vertex_in.position;
}

fragment float4 fragment_main() {
    return float4(1, 0, 0, 1);
}
"""
```

这里有两个着色器函数：一个名为 `vertex_main` 的顶点函数和一个名为 `fragment_main` 的片段函数。顶点函数通常是你操纵顶点位置的地方，而片段函数是你指定像素颜色的地方。

要设置包含这两个函数的 Metal 库，请添加以下内容：

```swift
let library = try device.makeLibrary(source: shader, options: nil)
let vertexFunction = library.makeFunction(name: "vertex_main")
let fragmentFunction = library.makeFunction(name: "fragment_main")
```

&#x20;编译器将检查这些函数是否存在，并将它们提供给管道描述符(Pipeline Descriptor)。

### 管道状态(Pipeline State)

在 Metal 中，你可以为 GPU 设置管道状态。通过设置此状态，你可以告诉 GPU，在状态改变之前，不会发生任何变化。在 GPU 处于固定状态的情况下，它可以更高效地运行。管道状态包含 GPU 所需的各种信息，例如它应该使用哪种像素格式以及是否应该使用深度渲染。管道状态还保存你刚刚创建的顶点和片段函数。

但是，你不会直接创建管道状态，而是通过描述符创建它。此描述符包含管道需要知道的所有内容，你只需针对特定渲染情况更改必要的属性。

➤ 添加以下代码：

```swift
let pipelineDescriptor = MTLRenderPipelineDescriptor()
pipelineDescriptor.colorAttachments[0].pixelFormat = .bgra8Unorm
pipelineDescriptor.vertexFunction = vertexFunction
pipelineDescriptor.fragmentFunction = fragmentFunction
```

在这里，你将像素格式指定为四个 8 位无符号整数，颜色像素顺序为蓝/绿/红/alpha。你还设置了两个着色器函数。

你将使用顶点描述符向 GPU 描述顶点在内存中的布局方式。Model I/O 在加载球体网格时会自动创建顶点描述符，因此你只需使用那个即可。

➤ 添加此代码：

<pre class="language-swift"><code class="lang-swift"><strong>pipelineDescriptor.vertexDescriptor =
</strong>    MTKMetalVertexDescriptorFromModelIO(mesh.vertexDescriptor)
</code></pre>

现在，你已使用必要信息设置了管道描述符。`MTLRenderPipelineDescriptor` 有许多其他属性，但目前，你将使用默认值。

➤ 现在，添加此代码：

```swift
let pipelineState =
    try device.makeRenderPipelineState(descriptor: pipelineDescriptor)
```

此代码从描述符创建管道状态。创建管道状态需要宝贵的处理时间，因此上述所有操作都应一次性设置。在实际应用中，你可能会创建多个管道状态来调用不同的着色函数或使用不同的顶点布局。
