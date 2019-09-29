{{indexmenu_n>7}}

# 工具箱

## 等待资源状态

**什么时候应该使用它?**

等待函数可以等待远程状态达到某一特定的状态，比如：

- 创建并等待资源创建完成
- 调用主机开关机并等待开机或关机完成
- 自定义等待的时间策略

例如:

```python
def mget_uhost_states(uhost_ids):
    resp = client.uhost().describe_uhost_instance({'UHostIds': uhost_ids})
    return [inst.get('State') for inst in resp.get('UHostSet')]

# Stop all instances
for uhost_id in uhost_ids:
    client.uhost().stop_uhost_instance({'UHostId': uhost_id})

# Wait all instances is stopped
wait.wait_for_state(
    target=['stopped'], pending=['pending'],
    timeout=300, # wait 5min
    refresh=lambda: (
        'stopped' if all([state == 'Stopped' for state in mget_uhost_states(uhost_ids)]) else 'pending'
    ),
)

# Remove all instances
for uhost_id in uhost_ids:
    client.uhost().terminate_uhost_instance({'UHostId': uhost_id})
```

默认情况下，等待函数将使用指数退避策略来决定两次请求之间的间隔。当达到超时时间时，将会抛出 `WaitTimeoutException` 异常。

