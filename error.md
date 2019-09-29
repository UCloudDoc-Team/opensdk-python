{{indexmenu_n>3}}

# 错误处理

了解如何处理不同类型的 SDK 异常，包括参数错误，RetCode 不为 0 的业务异常等。

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
    resp = client.unet().describe_eip()
except exc.ValidationException as e:
    print('参数校验错误', e)
except exc.RetCodeException as e:
    print('后端返回 RetCode 不为 0 错误', e)
except exc.UCloudException as e:
    print('SDK 其它错误', e)
except Exception as e:
    print('其它错误', e)
else:
    print('成功返回', resp)
```
