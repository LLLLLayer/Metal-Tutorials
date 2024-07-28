# 创建管道状态(Pipeline State)

要配置 GPU 的状态，你需要创建一个管道状态对象 (Pipeline state object，PSO)。此管道状态可以是用于渲染顶点的渲染管道状态，也可以是用于运行计算内核的计算管道状态。

➤ 继续在 super.init() 之前添加代码：

```swift
// create the pipeline state object
let pipelineDescriptor = MTLRenderPipelineDescriptor()
pipelineDescriptor.vertexFunction = vertexFunction
pipelineDescriptor.fragmentFunction = fragmentFunction
pipelineDescriptor.colorAttachments[0].pixelFormat =
    metalView.colorPixelFormat
pipelineDescriptor.vertexDescriptor =
    MTKMetalVertexDescriptorFromModelIO(mdlMesh.vertexDescriptor)
do {
    pipelineState =
      try device.makeRenderPipelineState(
        descriptor: pipelineDescriptor)
} catch {
    fatalError(error.localizedDescription)
}
```

PSO 为 GPU 保存潜在状态。GPU 需要知道其完整状态，然后才能开始管理顶点。在这里，你可以设置 GPU 将调用的两个着色器函数以及 GPU 将写入的纹理的像素格式。你还可以设置管道的顶点描述符；这就是 GPU 知道如何解释你将在网格数据 MTLBuffer 中呈现的顶点数据的方式。

> 注意：如果你需要使用不同的数据缓冲区布局或调用不同的顶点或片段函数，则需要额外的管道状态。创建管道状态相对耗时 - 这就是你提前执行此操作的原因 - 但在帧期间切换管道状态快速而高效。

初始化已完成，你的项目已编译。接下来，您将开始绘制模型。

