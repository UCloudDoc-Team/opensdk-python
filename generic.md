

# 泛化调用

如何调用 SDK 尚未支持的 API ？可以使用泛化调用方式。

**NOTE** 如果没有必须使用的理由，不建议使用泛化方式调用 API，因为无法享受 OpenAPI 提供的兼容性保证。

## 调用方式

```python
from ucloud.core import exc
from ucloud.client import Client

client = Client({
    "project_id": "...",
    "public_key": "...",
    "private_key": "...",
})

d = {
    "ResourceIds": ['uhost-xxx'],
    "QueryAll": True,
    "OrderTypes": ["OT_BUY"],
    "OrderStates": ["OS_FINISHED"],
    "EndTime": 1566545741,
    "BeginTime": 1566545741 - 1200,
}

try:
    resp = client.invoke("DescribeOrderDetailInfo", d)
except exc.RetCodeException as e:
    resp = e.json()
```
