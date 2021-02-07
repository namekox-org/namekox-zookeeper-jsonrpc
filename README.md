# Install
```shell script
pip install -U namekox-zookeeper-jsonrpc
```

# Example
> ping.py
```python
# ! -*- coding: utf-8 -*-
#
# author: forcemain@163.com


from namekox_jsonrpc.core.entrypoints import jsonrpc
from namekox_webserver.core.entrypoints.app import app
from namekox_zookeeper_jsonrpc.core.proxy import Proxy
from namekox_zookeeper_jsonrpc.core.mixin import Registry
from namekox_jsonrpc.constants import DEFAULT_JSONRPC_PORT


SERVICE_NAME = 'ping'
SERVICE_PORT = DEFAULT_JSONRPC_PORT


class Ping(Registry(name=SERVICE_NAME, roptions={'port': SERVICE_PORT})):
    name = SERVICE_NAME
    description = u'PING服务'

    @app.api('/api/ping/', methods=['GET'])
    def ping(self, request):
        target_service = Proxy(self).ping
        # call async
        target_service.pong.call_async()
        # call sync
        return target_service.pong()

    @jsonrpc.rpc(name='pong')
    def pong(self):
        print('Cur call stack: {}'.format(self.ctx.data))
        return 'pong'
```

# Running
> config.yaml
```yaml
ZOOKEEPER:
  ping:
    hosts: 127.0.0.1:2181
WEBSERVER:
  host: 0.0.0.0
  port: 80
```
> namekox run ping
```shell script
2021-02-07 11:12:28,542 DEBUG load container class from namekox_core.core.service.container:ServiceContainer
2021-02-07 11:12:28,543 DEBUG starting services ['ping']
2021-02-07 11:12:28,543 DEBUG starting service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_jsonrpc.core.entrypoints.rpc.server.JSONRpcServer:server, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_jsonrpc.core.entrypoints.rpc.handler.JSONRpcHandler:pong]
2021-02-07 11:12:28,545 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_connect(args=(), kwargs={}, tid=handle_connect)
2021-02-07 11:12:28,546 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_connect(args=(), kwargs={}, tid=handle_connect)
2021-02-07 11:12:28,547 DEBUG service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_jsonrpc.core.entrypoints.rpc.server.JSONRpcServer:server, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_jsonrpc.core.entrypoints.rpc.handler.JSONRpcHandler:pong] started
2021-02-07 11:12:28,547 DEBUG starting service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk]
2021-02-07 11:12:28,548 INFO Connecting to 127.0.0.1:2181
2021-02-07 11:12:28,548 DEBUG Sending request(xid=None): Connect(protocol_version=0, last_zxid_seen=0, time_out=30000, session_id=0, passwd='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00', read_only=None)
2021-02-07 11:12:28,571 INFO Zookeeper connection established, state: CONNECTED
2021-02-07 11:12:28,583 DEBUG Sending request(xid=1): GetChildren(path='/namekox', watcher=<bound method ChildrenWatch._watcher of <kazoo.recipe.watchers.ChildrenWatch object at 0x10aec1f10>>)
2021-02-07 11:12:28,584 DEBUG Sending request(xid=2): Create(path='/namekox/ping.17e33434-2b04-444f-9192-29b2a21c9a19', data='{"port": 64923, "address": "127.0.0.1"}', acl=[ACL(perms=31, acl_list=['ALL'], id=Id(scheme='world', id='anyone'))], flags=1)
2021-02-07 11:12:28,589 DEBUG Sending request(xid=3): GetChildren(path='/namekox', watcher=None)
2021-02-07 11:12:28,594 DEBUG service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk] started
2021-02-07 11:12:28,595 DEBUG Sending request(xid=4): Create(path='/namekox/ping.17e33434-2b04-444f-9192-29b2a21c9a19', data='{"port": 64923, "address": "127.0.0.1"}', acl=[ACL(perms=31, acl_list=['ALL'], id=Id(scheme='world', id='anyone'))], flags=1)
2021-02-07 11:12:28,596 DEBUG services ['ping'] started
2021-02-07 11:12:28,598 DEBUG Received response(xid=1): []
2021-02-07 11:12:28,607 DEBUG Received EVENT: Watch(type=4, state=3, path=u'/namekox')
2021-02-07 11:12:28,608 DEBUG Received response(xid=2): u'/namekox/ping.17e33434-2b04-444f-9192-29b2a21c9a19'
2021-02-07 11:12:28,609 DEBUG Received response(xid=3): [u'ping.17e33434-2b04-444f-9192-29b2a21c9a19']
2021-02-07 11:12:28,610 DEBUG Received error(xid=4) NodeExistsError()
2021-02-07 11:12:28,611 DEBUG Sending request(xid=5): GetChildren(path='/namekox', watcher=<bound method ChildrenWatch._watcher of <kazoo.recipe.watchers.ChildrenWatch object at 0x10aec1f10>>)
2021-02-07 11:12:28,611 DEBUG Sending request(xid=6): GetData(path='/namekox/ping.17e33434-2b04-444f-9192-29b2a21c9a19', watcher=None)
2021-02-07 11:12:28,613 DEBUG Received response(xid=5): [u'ping.17e33434-2b04-444f-9192-29b2a21c9a19']
2021-02-07 11:12:28,614 DEBUG Sending request(xid=7): GetData(path='/namekox/ping.17e33434-2b04-444f-9192-29b2a21c9a19', watcher=None)
2021-02-07 11:12:28,616 DEBUG Received response(xid=6): ('{"port": 64923, "address": "127.0.0.1"}', ZnodeStat(czxid=3968, mzxid=3968, ctime=1612667548591, mtime=1612667548591, version=0, cversion=0, aversion=0, ephemeralOwner=72072886995320832, dataLength=39, numChildren=0, pzxid=3968))
2021-02-07 11:12:28,618 DEBUG Received response(xid=7): ('{"port": 64923, "address": "127.0.0.1"}', ZnodeStat(czxid=3968, mzxid=3968, ctime=1612667548591, mtime=1612667548591, version=0, cversion=0, aversion=0, ephemeralOwner=72072886995320832, dataLength=39, numChildren=0, pzxid=3968))
2021-02-07 11:12:46,364 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10aef4d90>, ('127.0.0.1', 64991)), kwargs={}, tid=handle_request)
2021-02-07 11:12:46,365 DEBUG spawn worker thread handle ping:ping(args=(<Request 'http://127.0.0.1/api/ping/' [GET]>,), kwargs={}, context={})
2021-02-07 11:12:46,370 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10aec18d0>, ('127.0.0.1', 64992)), kwargs={}, tid=handle_request)
2021-02-07 11:12:46,371 DEBUG spawn worker thread handle ping:pong(args=[], kwargs={}, context={})
127.0.0.1 - - [07/Feb/2021 11:12:46] "POST /pong HTTP/1.1" 200 155 0.001361
Cur call stack: {'parent_call_id': None, 'call_id': 'cf8cf42d-1efc-42e7-bb25-eceeefefb23e'}
2021-02-07 11:12:46,373 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10af106d0>, ('127.0.0.1', 64993)), kwargs={}, tid=handle_request)
2021-02-07 11:12:46,374 DEBUG spawn worker thread handle ping:pong(args=[], kwargs={}, context={})
Cur call stack: {'parent_call_id': None, 'call_id': 'ce943091-65fa-4d72-8935-9735e589ca25'}
127.0.0.1 - - [07/Feb/2021 11:12:46] "POST /pong HTTP/1.1" 200 157 0.000908
127.0.0.1 - - [07/Feb/2021 11:12:46] "GET /api/ping/ HTTP/1.1" 200 215 0.011509
```
> curl http://127.0.0.1/api/ping/
```json
{
    "errs": "",
    "code": "Request:Success",
    "data": "pong",
    "call_id": "2b8415b1-38d4-478b-bc59-a300cb75feaf"
}
```
