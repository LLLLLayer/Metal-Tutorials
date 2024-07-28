# 渲染帧

MTKView 为每个帧调用 `draw(in:)`；您将在此处设置 GPU 渲染命令。

➤ 在 draw(in:) 中，将 print 语句替换为：

```swift
guard
    let commandBuffer = Self.commandQueue.makeCommandBuffer(),
    let descriptor = view.currentRenderPassDescriptor,
    let renderEncoder =
        commandBuffer.makeRenderCommandEncoder(
        descriptor: descriptor) else {
    return
}
```

你将向 GPU 发送一系列包含在命令编码器(Command encoder)中的命令。在一帧中，你可能有多个命令编码器，命令缓冲区(Command buffer)负责管理这些命令。

你使用渲染过程描述符(Render pass descriptor)创建渲染命令编码器。它包含 GPU 将绘制到的渲染目标纹理。在复杂的应用中，一帧中可能有多条渲染过程，以及多个目标纹理。稍后你将学习如何将渲染过程链接在一起。

➤ 继续添加此代码：

```swift
// drawing code goes here

// 1
renderEncoder.endEncoding()
// 2
guard let drawable = view.currentDrawable else {
    return
}
commandBuffer.present(drawable)
// 3
commandBuffer.commit()
```

以下是代码的详细内容：

1. 将 GPU 命令添加到命令编码器后，结束其编码。
2. 将视图的可绘制纹理呈现给 GPU。
3. 提交命令缓冲区时，将编码的命令发送到 GPU 以供执行。

