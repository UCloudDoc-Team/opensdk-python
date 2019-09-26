# Python SDK 使用说明

| 快速导航 | 版本 | 链接                                         |
| -------- | ---- | -------------------------------------------- |
| 源码仓库 | 3.5+ | https://github.com/ucloud/ucloud-sdk-python3 |
| 源码仓库 | 2.7+ | https://github.com/ucloud/ucloud-sdk-python2 |
| 文档     | 3.5+ | https://ucloud.github.io/ucloud-sdk-python3  |
| 文档     | 2.7+ | https://ucloud.github.io/ucloud-sdk-python2  |

以下示例以 Python 3 为例，如使用 Python 2，请将示例中的 `ucloud-sdk-python3` 替换为 `ucloud-sdk-python2` 即可。

> **注意**：PSF（Python 软件基金会，Python Software Fundation）宣布，将在 2020 年停止对 Python 2 的支持，建议使用 Python 3 版本，以获取长期支持。

## 安装

使用 pip 安装（推荐）：

```bash
pip install ucloud-sdk-python3
```

从源码安装（不推荐）：

```bash
git clone https://github.com/ucloud/ucloud-sdk-python3.git
cd ucloud-sdk-python3
python setup.py install
```

## 初次使用

目前，Python SDK 使用 PublicKey/PrivateKey 作为唯一的鉴权方式，该公私钥可以从以下途径获取：

- [UAPI 密钥管理](https://console.ucloud.cn/uapi/apikey)

下面提供一个简单的示例：

```python
from ucloud.core import exc
from ucloud.client import Client

client = Client({
    "region": "cn-bj2",
    "project_id": "...",
    "public_key": "...",
    "private_key": "...",
})

try:
    resp = client.uhost().create_uhost_instance({
        'Name': 'sdk-python-quickstart',
        'Zone': 'cn-bj2-02',
        'ImageId': 'uimage-xxx',  # 此处应替换成真正的 image id
        'LoginMode': "Password",
        'Password': "Foobar42",
        'CPU': 1,
        'Memory': 1,
        'Disks': [{
            'Size': 20,
            'Type': 'CLOUD_SSD'
        }],
    })
except exc.UCloudException as e:
    print(e)
else:
    print(resp)
```

将上述代码中 client 相关配置，以及主机的 image id 等，替换成自己的配置，即可创建一台云主机。

在该示例中，使用 Python SDK 完成了一个创建云主机的请求。至此，已经涵盖了 SDK 的基本核心用法，可以构建自己的脚本啦！

SDK 中的每一个 api 调用都有详细的注释文档，
可以通过 Editor/IDE 跳转到具体的方法中查看（也可以[查看接口文档](https://ucloud.github.io/ucloud-sdk-python3/services.html#uhost)），
并根据 IDE 自动补全和报错信息继续探索 SDK 的用法。

如果需要了解这段代码提及但未完全覆盖的使用技巧，请参考：

- [通用配置](configure.md)，了解如何配置 SDK，如日志、重试、服务访问端点（公有云、专有云）等
- [错误处理](error.md)，了解如何处理不同类型的 SDK 异常，包括参数错误，RetCode 不为 0 的业务异常等
- [类型系统](typesystem.md)，了解 SDK 如何校验参数，并规范化 API 的返回值。
- [请求中间件](middleware.md)，了解如何拦截 SDK 发起的请求，并统一添加额外的逻辑。
- [泛化调用](generic.md)，如何调用 SDK 尚未支持的 API（不建议使用此类 API，因为没有兼容性保证）
- [工具箱](helpers.md)，SDK 提供的额外支持，如密码生成器、状态轮询函数等

## 获取更多示例

### 基于场景的示例

SDK 提供了部分基于场景的示例，并提供了对应的资源销毁逻辑，可以点击以下链接查看源码：

- [批量创建云主机](https://github.com/ucloud/ucloud-sdk-python3/tree/master/examples/uhost)
- [创建基于负载均衡器的两层架构](https://github.com/ucloud/ucloud-sdk-python3/tree/master/examples/two-tier)，ULB + UHost

### 基于请求的示例

控制台 UAPI 产品提供了基于请求的示例，可以在控制台填写请求参数，自动生成 SDK 样例，可以直接拷贝使用，详情请见：

- [打开 UAPI](https://console.ucloud.cn/uapi/ucloudapi)
- 选择希望调用的 API，如 [CreateUHostInstance](https://console.ucloud.cn/uapi/detail?id=CreateUHostInstance)
- 填写参数，拷贝界面右侧 Python SDK 的示例代码
- 保存请求代码为 `main.py`
- `pip install ucloud-sdk-python3`
- `python ./main.py`

