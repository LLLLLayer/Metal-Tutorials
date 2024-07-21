# USD 文件格式

➤ 在 Finder 中，导航至 Documents ▸ Shared Playground Data。

在这里，你将找到导出的文件 primitive.usda。

➤ 使用纯文本编辑器打开 primitive.usda。

以下是一个示例 USD 文件，它描述了一个具有四个角顶点的平面图元。圆锥体 USD 文件看起来类似，只是它有更多的顶点数据。

```
#usda 1.0
(
    defaultPrim = "plane"
    endTimeCode = 0
    startTimeCode = 0
    timeCodesPerSecond = 60
    upAxis = "Y"
)

def Mesh "plane"
{
    uniform bool doubleSided = 0
    float3[] extent = [(0, -0.5, -0.5), (0, 0.5, 0.5)]
    int[] faceVertexCounts = [3, 3]
    int[] faceVertexIndices = [3, 2, 1, 3, 1, 0]
    normal3f[] normals = [(-1, 0, 0), (-1, 0, 0), (-1, 0, 0), (-1, 0, 0)]
    point3f[] points = [(0, 0.5, 0.5), (0, -0.5, 0.5), (-0, -0.5, -0.5), (0, 0.5, -0.5)]
    float2[] primvars:Texture_uv = [(1, 1), (0, 1), (0, 0), (1, 0)] (
        interpolation = "vertex"
    )
}
```

文件首先描述一般特征，例如动画时间和哪个方向向上。然后文件描述网格。

以下是平面 USD 的细分：

1. extent：网格的大小。创建圆锥时，在 coneWithExtent 中，你在所有轴上指定 1。最小顶点位置值为 -0.5，最大值为 0.5。
2. faceVertexCounts：此平面由两个三角形组成，每个三角形有三个顶点。你的圆锥将有许多三角形。
3. faceVertexIndices：此平面有四个顶点，每个角一个。索引顺序是渲染这些顶点的顺序。要组成两个三角形，你需要渲染六个顶点。
4. normals：表面法线。法线是一个正交指向的矢量 - 即直接向外。稍后你将阅读有关法线的更多信息。
5. points：每个顶点的位置。平面有四个顶点。你的锥体将有许多顶点。
6. Texture\_uv：UV 坐标决定顶点在 2D 纹理上的位置。纹理上的坐标称为 uv 坐标，而不是 xy 坐标，但它们的作用相同。你的锥体不使用这些 UV 坐标，因为你没有对其应用任何自定义纹理。
