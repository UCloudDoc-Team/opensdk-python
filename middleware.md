

# 请求中间件

了解如何拦截 SDK 发起的请求，并统一添加额外的逻辑。

UCloud SDK 为 Client 或 Transport 级别的请求提供了请求中间件的特性。

该特性允许在 请求/响应 的生命周期中添加自定义的逻辑。

例如，Client 级别的中间件，可以拦截参数/响应字典:

```python
from ucloud.client import Client

client = Client({'Region': 'cn-bj2', ...})


@client.middleware.request
def log_params(req):
    print('[REQ]', req)


@client.middleware.response
def log_response(resp):
    print('[RESP]', resp)
```

或者 Transport 级别的中间件，可以拦截 HTTP 的网络请求:

```python
from ucloud.client import Client
from ucloud.core.transport import RequestsTransport

transport = RequestsTransport()


@transport.middleware.request
def log_request(req):
    print('[REQ]', req)


@transport.middleware.response
def log_response(resp):
    print('[RESP]', resp)

client = Client({'Region': 'cn-bj2', ...}, transport=transport)
```
