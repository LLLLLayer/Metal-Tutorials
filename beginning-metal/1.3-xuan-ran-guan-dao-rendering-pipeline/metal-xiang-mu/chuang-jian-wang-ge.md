# 创建网格

您已经使用 Model I/O 创建了一个球体和一个圆锥体；现在是时候创建一个立方体了。

➤ 在 `init(metalView:)` 中，在调用 `super.init()` 之前，添加以下内容：

<pre class="language-swift"><code class="lang-swift">// create the mesh
let allocator = MTKMeshBufferAllocator(device: device)
let size: Float = 0.8
let mdlMesh = MDLMesh(
    boxWithExtent: [size, size, size],
    segments: [1, 1, 1],
<strong>    inwardNormals: false,
</strong>    geometryType: .triangles,
    allocator: allocator)
do {
    mesh = try MTKMesh(mesh: mdlMesh, device: device)
} catch {
<strong>    print(error.localizedDescription)
</strong>}
</code></pre>

此代码创建立方体网格，就像你在上一章中所做的那样。

➤ 然后，设置包含要发送到 GPU 的顶点数据的 MTLBuffer。

```swift
vertexBuffer = mesh.vertexBuffers[0].buffer
```

此代码将网格数据放入 MTLBuffer 中。接下来，您需要设置管道状态，以便 GPU 知道如何渲染数据。
