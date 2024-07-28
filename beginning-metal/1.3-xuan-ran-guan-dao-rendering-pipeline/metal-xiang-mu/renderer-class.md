# Renderer Class

创建一个名为 Renderer.swift 的新 Swift 文件，并用以下代码替换其内容：

```swift
import MetalKit

class Renderer: NSObject {
    init(metalView: MTKView) {
        super.init()
    }
}

extension Renderer: MTKViewDelegate {
    func mtkView(
        _ view: MTKView,
        drawableSizeWillChange size: CGSize
  ) {
  }

    func draw(in view: MTKView) {
        print("draw")
    }
}
```

在这里，你创建一个初始化程序，并使用两个 `MTKView` 委托方法使 `Renderer` 符合 `MTKViewDelegate`：

1. `mtkView(_:drawableSizeWillChange:):`每次窗口大小发生变化时调用。这允许你更新渲染纹理大小和相机投影。
2. `draw(in:):` 每帧调用一次。这是你编写渲染代码的地方。

➤ 打开 MetalView.swift，在 MetalView 中添加一个属性来保存渲染器：

```swift
@State private var renderer: Renderer?
```

➤ 将 body 更改为：

```swift
var body: some View {
    MetalViewRepresentable(metalView: $metalView)
        .onAppear {
            renderer = Renderer(metalView: metalView)
        }
}
```

在这里，当 metal 视图首次出现时，你将初始化渲染器。

