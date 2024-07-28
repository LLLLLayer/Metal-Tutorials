# 绘图

现在是时候设置 GPU 绘制帧所需的命令列表了。换句话说，你将：

* 设置管道状态以配置 GPU 硬件。
* 为 GPU 提供顶点数据。
* 使用网格的子网格组发出绘制调用。

➤ 仍然在 draw(in:) 中，将注释 //  替换为：

```swift
renderEncoder.setRenderPipelineState(pipelineState)
renderEncoder.setVertexBuffer(vertexBuffer, offset: 0, index: 0)
for submesh in mesh.submeshes {
    renderEncoder.drawIndexedPrimitives(
      type: .triangle,
      indexCount: submesh.indexCount,
      indexType: submesh.indexType,
      indexBuffer: submesh.indexBuffer.buffer,
      indexBufferOffset: submesh.indexBuffer.offset)
}
```

很好，你设置了 GPU 命令来设置管道状态和顶点缓冲区，并对网格的子网格执行绘制调用。当你在 draw(in:) 末尾提交命令缓冲区时，你就是在告诉 GPU 数据和管道已准备就绪，现在是 GPU 接管的时候了。
