# 设置 Metal Library

首先，设置 `MTLLibrary` 并确保顶点和片段着色器函数存在。

```swift
// create the shader function library
let library = device.makeDefaultLibrary()
Self.library = library
let vertexFunction = library?.makeFunction(name: "vertex_main")
let fragmentFunction =
    library?.makeFunction(name: "fragment_main")
```

在这里，你使用一些着色器函数指针设置了默认 library。你将在本章后面创建这些着色器函数。与 OpenGL 着色器不同，这些函数是在编译项目时编译的，这比动态编译函数更有效率。结果存储在库中。
