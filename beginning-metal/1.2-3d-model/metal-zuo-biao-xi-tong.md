# Metal 坐标系统

所有模型都有一个原点。原点是网格的位置。火车的原点位于 \[0, 0, 0]。在 Blender 中，这会将火车置于场景的正中央。

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Metal NDC(标准化设备坐标，Normalized Device Coordinate)系统是一个 2 个单位宽、2 个单位高、1 个单位深的框，其中 X 表示右/左，Y 表示上/下，Z 表示屏幕内/外。

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

标准化意味着调整到标准比例。在屏幕上，你可能会在宽度为 0 到 375 的屏幕坐标中定位一个位置，而 Metal 标准化坐标系统并不关心屏幕的物理宽度是多少 - 其沿 X 轴的坐标为 -1.0 到 1.0。在第 6 节“坐标空间”中，你将了解各种坐标系和空间。由于火车的原点位于 \[0,0,0]，因此火车出现在屏幕的中间位置，即 Metal 坐标系中的 \[0,0,0]。

GPU 根据顶点函数的输出渲染顶点位置。你的 Playground 目前包含一个非常简单的顶点函数，它返回传递给它的顶点位置。

➤ 在你的 Playground 中，找到 let shader = """..."""

shader 是一个文本字符串，其中包含 Metal 库加载和编译的着色器函数代码。通过更改此字符串中的 vertex\_in.position，你可以更改每个顶点的渲染位置。

在着色器文本字符串中，将 return vertex\_in.position; 更改为：

```swift
float4 position = vertex_in.position;
position.y -= 1.0;
return position;
```

请小心地按照图示添加此代码。由于代码包含在字符串中，因此编译器无法识别错误。

在这里，你从渲染的每个顶点的 y 位置减去 1.0。NDC 在 y 轴上的 -1.0 位于屏幕底部。如果你还不太明白发生了什么，请不要担心，因为你将在第 4 节“顶点函数”中重新讨论此主题。

➤ 运行 Playground。车轮现在出现在屏幕底部。

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

现在车轮已修好，你可以解决失踪火车的案件了！
