# 3D 文件格式

有几种标准的 3D 文件格式。以下是每种格式的概述：

1. OBJ：这种由 Wavefront Technologies 开发的格式已经存在很长时间了，几乎每个 3D 应用程序都支持导入和导出扩展名为 .obj 的 OBJ 文件。你可以使用附带的 .mtl 文件指定材质（纹理和表面属性），但是，这种格式不支持动画。
2. glTF：GL 传输格式由 Khronos 开发，后者负责监督 Vulkan 和 OpenGL。这种格式相对较新，仍在积极开发中。由于其灵活性，它得到了强大的社区支持。它支持动画模型。
3. .blend：这是原生 Blender 文件格式。
4. .fbx：Autodesk 拥有的专有格式。这是一种支持动画的常用格式，但由于它是专有的并且没有单一标准，因此越来越不受青睐。
5. USD：通用场景描述是 Pixar 推出的一种可扩展的开源格式，完整文档位于 https://openusd.org。USD 文件可以引用许多模型和文件，因此团队中的每个人都可以处理场景的不同部分。USD 文件可以有几种不同的扩展名。.usd 可以是 ASCII 或二进制。.usda 是人类可读的 ASCII。.usdz 是一个 USD 存档文件，其中包含模型或场景所需的一切。Apple 使用 USDZ 格式来处理他们的 AR 模型。

OBJ 文件仅包含一个模型，而 glTF 和 USD 文件是整个场景的容器，包括模型、动画、摄像机和灯光。

在本书中，你将主要使用 USD 格式。

> 注意：你可以使用 Apple 的 Reality Converter 将 3D 文件转换为 USDZ。 Apple 还提供了用于验证和检查 USDZ 文件的工具 (https://apple.co/3gykNcI)，以及示例 USDZ 文件库 (https://apple.co/3iJzMBW)。
