# 创建顶点着色器(Vertex Shader)

现在是时候看看顶点处理的实际操作了。你即将编写的顶点着色器非常小，但它封装了本节和后续章节中所需的大部分必要顶点着色器语法。

➤ 使用 Metal File 模板创建一个新文件，并将其命名为 `Shaders.metal`。然后，在文件末尾添加以下代码：

```swift
// 1
struct VertexIn {
    float4 position [[attribute(0)]];
};

// 2
vertex float4 vertex_main(const VertexIn vertexIn [[stage_in]]) {
    return vertexIn.position;
}
```

查看代码：

1. 创建一个 struct VertexIn 来描述与你之前设置的顶点描述符匹配的顶点属性。在本例中，只需定位即可。
2. 实现一个顶点着色器 vertex\_main，它接收 VertexIn 结构并以 float4 类型返回顶点位置。

请记住，顶点在顶点缓冲区中编入索引。顶点着色器通过 \[\[stage\_in]] 属性获取当前顶点索引，并解压当前索引处为顶点缓存的 VertexIn 结构。

计算单元可以(一次)处理顶点批次，直至其最大着色器核心数。此批次可以完全放入 CU 缓存中，因此可以根据需要重复使用顶点。批次将使 CU 保持繁忙状态，直到处理完成，但其他 CU 应该可用于处理下一批。

一旦顶点处理完成，就会清除下一批顶点的缓存。此时，顶点现在已排序和分组，准备发送到原始组装阶段。

<figure><img src="../../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

回顾一下，CPU 向 GPU 发送了你从模型网格创建的顶点缓冲区。你使用顶点描述符配置了顶点缓冲区，该描述符告诉 GPU 顶点数据的结构。在 GPU 上，你创建了一个结构来封装顶点属性。顶点着色器将此结构作为函数参数接收，并通过 \[\[stage\_in]] 限定符确认位置来自 CPU，通过顶点缓冲区中的 \[\[attribute(0)]] 位置。然后，顶点着色器处理所有顶点并将它们的位置作为 float4 返回。

> 注意：当你使用带有属性的顶点描述符时，你不必匹配类型。MTLBuffer 位置是 float3，而 VertexIn 将位置定义为 float4。

称为分发器的特殊硬件单元将分组的顶点块发送到原始组装阶段。

