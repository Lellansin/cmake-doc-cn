# add\_definitions

添加 `-D 定义名` 标志到源文件.

```
add_definitions(-DFOO -DBAR ...)
```

添加定义到编译器命令行参数，包括当前目录和子目录的源文件在编译时都会用上。该命令可用于添加任何标志，但是它旨在添加预处理器的定义。标记以 `-D` 或 `/D` 开头，看起来像预处理器定义，会自动添加到[`COMPILE_DEFINITIONS`](https://cmake.org/cmake/help/v3.0/prop_dir/COMPILE_DEFINITIONS.html#prop_dir:COMPILE_DEFINITIONS)当前目录的目录属性中。重要的定义可以留在最左边，避免因为向后兼容的原因而被覆盖转换。关于将预处理器定义添加到特定作用域和配置的详细信息参见 [`directory`](https://cmake.org/cmake/help/v3.0/prop_dir/COMPILE_DEFINITIONS.html#prop_dir:COMPILE_DEFINITIONS)、[`target`](https://cmake.org/cmake/help/v3.0/prop_tgt/COMPILE_DEFINITIONS.html#prop_tgt:COMPILE_DEFINITIONS)、[`sourcefile`](https://cmake.org/cmake/help/v3.0/prop_sf/COMPILE_DEFINITIONS.html#prop_sf:COMPILE_DEFINITIONS) `COMPILE_DEFINITIONS` 属性的文档。

有关定义 buildsystem 属性的更多信息，请参阅 [ `cmake-buildsystem(7)`](https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#manual:cmake-buildsystem%287%29) 。

