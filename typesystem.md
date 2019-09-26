# 类型系统

由于 Python 是一个动态强类型的语言，为了应对现代软件工程的挑战，增强开发者对于大规模应用 Python SDK 的信心，
除了在 Python 3 中启用 Type Hints 特性外，UCloud Python SDK 还基于类型系统实现了运行时的类型检查。

例如:

```python
client.uhost().create_uhost_instance({
    'CPU': "i am not a integer",
})
```

该段代码将抛出 `ValidationException` 异常，因为 CPU 期望一个整数类型，并且多个必填参数没有被正确设置。

