# mark\_as\_advanced

将 cmake 缓存的变量标记为高级。

```
mark_as_advanced([CLEAR|FORCE] VAR [VAR2 ...])
```

将指定的缓存变量标记为高级。除非显示高级选项开启，否则高级变量将不会在任何 cmake 的 GUI 中显示。 如果 **CLEAR** 是第一个参数，那么意味着变量会从高级中清除（不再是高级变量）。 如果 **FORCE** 是第一个参数，则变量会被标记为高级。如果没有指定 **FORCE** 或者 **CLEAR**，新的值会被标记为高级，但是如果变量已经有了一个高级/非高级状态，那么命令则不会生效。

该命令在 script mode 中无效。

