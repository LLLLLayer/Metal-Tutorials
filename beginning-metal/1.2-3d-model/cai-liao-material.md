# 材料(Material)

材料描述 3D 渲染器应如何为顶点着色。例如，顶点应该是光滑且有光泽的吗？粉红色的？反光的？

材料属性可以包括：

1. 漫反射(Diffuse)：表面的基本颜色。
2. 金属色(Metallic)：描述表面是否为金属。
3. 粗糙度(Roughness)：描述表面的粗糙程度。如果表面的粗糙度为 0，则表面完全平坦且有光泽。

### 材料组(Material Groups)

➤ 在 Blender 中，打开 train.blend，你将在本章的资源目录中找到它。

此文件是操场中 .usdz 火车的原始 Blender 文件。

➤ 左键单击模型以选择它，然后按 Tab 进入编辑模式。

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

与纯灰色的锥体不同，火车模型有几种颜色。这些颜色在 Material groups 中定义 - 每种颜色一个。在 Blender 屏幕的右侧，你将看到“属性”面板，其中已选择 Material context (即垂直图标列表底部的图标)，顶部是此模型中的材质列表。

➤ 选择 Body，然后单击材质列表下方的 Select。

分配给此材质的顶点现在为橙色。

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>



请注意顶点是如何分成不同组或材质的。这种分离使得在 Blender 中选择各个部分更加容易，并且还使你能够分配不同的颜色。

> 注意：将此模型渲染到你的 Playground 时，渲染器将渲染每个材质组，但你的 Metal 着色器不会渲染正确的颜色。验证模型外观的一种方法是在 Blender 中查看它。

➤ 返回 Xcode，从项目导航器中打开 Import Train  Playground 页面。此 Playground 渲染(但不导出)线框锥体。

➤ 在 Playground 的资源文件夹中，找到并检查 train.usdz 模型。在窗口上拖动以围绕模型移动视图相机。

> 注意：Playground 资源文件夹中的文件可用于所有 Playground 页面。每个页面的资源文件夹中的文件仅适用于该页面。

➤ 在 Import Train 中，删除创建 MDLMesh 锥体的行：&#x20;

```swift
let mdlMesh = MDLMesh(
    coneWithExtent: [1, 1, 1],
    segments: [10, 10],
    inwardNormals: false,
    cap: true,
    geometryType: .triangles,
    allocator: allocator)
```

替换刚刚删除的代码，在其位置添加以下代码：

```swift
guard let assetURL = Bundle.main.url(
    forResource: "train",
    withExtension: "usdz") else {
    fatalError()
}
```

此代码设置 USD 文件的 URL。
