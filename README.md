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


from namekox_xmlrpc.core.entrypoints import xmlrpc
from namekox_webserver.core.entrypoints.app import app
from namekox_xmlrpc.constants import DEFAULT_XMLRPC_PORT
from namekox_service.core.mixin.zookeeper import ZooKeeperRegistry
from namekox_service.core.proxy.zookeeper.xmlrpc import XMLRpcProxy


SERVICE_NAME = 'ping'
SERVICE_PORT = DEFAULT_XMLRPC_PORT


class Ping(ZooKeeperRegistry(name=SERVICE_NAME, roptions={'port': SERVICE_PORT})):
    name = SERVICE_NAME

    @app.api('/api/ping/', methods=['GET'])
    def ping(self, request):
        target_service = XMLRpcProxy(self).ping
        # call async
        target_service.pong.call_async()
        # call sync
        return target_service.pong()

    @xmlrpc.rpc(name='pong')
    def pong(self):
        print('Cur call stack: {}'.format(self.ctx.data))
        return 'pong'
```

# Running
> config.yaml
```yaml
XMLRPC:
  host: 0.0.0.0
  port: 5000
ZOOKEEPER:
  ping:
    hosts: 127.0.0.1:2181
WEBSERVER:
  host: 0.0.0.0
  port: 80
```
> namekox run ping
```shell script
2020-12-18 10:32:26,293 DEBUG load container class from namekox_core.core.service.container:ServiceContainer
2020-12-18 10:32:26,295 DEBUG starting services ['ping']
2020-12-18 10:32:26,295 DEBUG starting service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.server.XMLRpcServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.handler.XMLRpcHandler:pong]
2020-12-18 10:32:26,297 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_connect(args=(), kwargs={}, tid=handle_connect)
2020-12-18 10:32:26,298 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_connect(args=(), kwargs={}, tid=handle_connect)
2020-12-18 10:32:26,299 DEBUG service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.server.XMLRpcServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.handler.XMLRpcHandler:pong] started
2020-12-18 10:32:26,299 DEBUG starting service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk]
2020-12-18 10:32:26,300 INFO Connecting to 127.0.0.1:2181
2020-12-18 10:32:26,301 DEBUG Sending request(xid=None): Connect(protocol_version=0, last_zxid_seen=0, time_out=10000, session_id=0, passwd='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00', read_only=None)
2020-12-18 10:32:26,304 INFO Zookeeper connection established, state: CONNECTED
2020-12-18 10:32:26,305 DEBUG Sending request(xid=1): GetChildren(path='/namekox', watcher=<bound method ChildrenWatch._watcher of <kazoo.recipe.watchers.ChildrenWatch object at 0x10d25ca90>>)
2020-12-18 10:32:26,306 DEBUG Received response(xid=1): []
2020-12-18 10:32:26,312 DEBUG Sending request(xid=2): Exists(path='/namekox', watcher=None)
2020-12-18 10:32:26,313 DEBUG Received response(xid=2): ZnodeStat(czxid=74, mzxid=74, ctime=1606123632647, mtime=1606123632647, version=0, cversion=750, aversion=0, ephemeralOwner=0, dataLength=0, numChildren=0, pzxid=1405)
2020-12-18 10:32:26,314 DEBUG Sending request(xid=3): Create(path='/namekox/ping.e881b500-64a2-4592-b0d9-9ca1fc6456a1', data='{"port": 5000, "address": "127.0.0.1"}', acl=[ACL(perms=31, acl_list=['ALL'], id=Id(scheme='world', id='anyone'))], flags=1)
2020-12-18 10:32:26,316 DEBUG Received EVENT: Watch(type=4, state=3, path=u'/namekox')
2020-12-18 10:32:26,317 DEBUG Received response(xid=3): u'/namekox/ping.e881b500-64a2-4592-b0d9-9ca1fc6456a1'
2020-12-18 10:32:26,318 DEBUG Sending request(xid=4): GetChildren(path='/namekox', watcher=<bound method ChildrenWatch._watcher of <kazoo.recipe.watchers.ChildrenWatch object at 0x10d25ca90>>)
2020-12-18 10:32:26,318 DEBUG service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk] started
2020-12-18 10:32:26,319 DEBUG services ['ping'] started
2020-12-18 10:32:26,319 DEBUG Received response(xid=4): [u'ping.e881b500-64a2-4592-b0d9-9ca1fc6456a1']
2020-12-18 10:32:26,319 DEBUG Sending request(xid=5): GetData(path='/namekox/ping.e881b500-64a2-4592-b0d9-9ca1fc6456a1', watcher=None)
2020-12-18 10:32:26,322 DEBUG Received response(xid=5): ('{"port": 5000, "address": "127.0.0.1"}', ZnodeStat(czxid=1407, mzxid=1407, ctime=1608258746314, mtime=1608258746314, version=0, cversion=0, aversion=0, ephemeralOwner=72106114295857166, dataLength=38, numChildren=0, pzxid=1407))
2020-12-18 10:32:58,231 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10d277f50>, ('127.0.0.1', 55238)), kwargs={}, tid=handle_request)
2020-12-18 10:32:58,264 DEBUG spawn worker thread handle ping:ping(args=(<Request 'http://127.0.0.1/api/ping/' [GET]>,), kwargs={}, context={})
2020-12-18 10:32:58,285 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10d25c7d0>, ('127.0.0.1', 55239)), kwargs={}, tid=handle_request)
2020-12-18 10:32:58,291 DEBUG spawn worker thread handle ping:pong(args=(), kwargs={}, context={})
127.0.0.1 - - [18/Dec/2020 10:32:58] "POST /pong HTTP/1.1" 200 228 0.006222
Cur call stack: {'call_id_stack': ['5813904e-efe0-42b7-b63d-c9db8589030e']}
2020-12-18 10:32:58,294 DEBUG spawn manage thread handle ping:namekox_webserver.core.entrypoints.app.server:handle_request(args=(<eventlet.greenio.base.GreenSocket object at 0x10d28d590>, ('127.0.0.1', 55240)), kwargs={}, tid=handle_request)
2020-12-18 10:32:58,297 DEBUG spawn worker thread handle ping:pong(args=(), kwargs={}, context={})
Cur call stack: {'call_id_stack': ['4be23b50-863f-461f-9a10-dda5b05b519a']}
127.0.0.1 - - [18/Dec/2020 10:32:58] "POST /pong HTTP/1.1" 200 242 0.004576
127.0.0.1 - - [18/Dec/2020 10:32:58] "GET /api/ping/ HTTP/1.1" 200 239 0.046061
^C2020-12-18 10:33:21,069 DEBUG stopping services ['ping']
2020-12-18 10:33:21,070 DEBUG stopping service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.server.XMLRpcServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.handler.XMLRpcHandler:pong]
2020-12-18 10:33:21,071 DEBUG wait service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.server.XMLRpcServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.handler.XMLRpcHandler:pong] stop
2020-12-18 10:33:21,071 DEBUG service ping entrypoints [ping:namekox_webserver.core.entrypoints.app.handler.ApiServerHandler:ping, ping:namekox_webserver.core.entrypoints.app.server.WebServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.server.XMLRpcServer:server, ping:namekox_xmlrpc.core.entrypoints.rpc.handler.XMLRpcHandler:pong] stopped
2020-12-18 10:33:21,071 DEBUG stopping service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk]
2020-12-18 10:33:21,071 DEBUG Sending request(xid=6): Close()
2020-12-18 10:33:21,083 DEBUG Received EVENT: Watch(type=4, state=3, path=u'/namekox')
2020-12-18 10:33:21,085 INFO Closing connection to 127.0.0.1:2181
2020-12-18 10:33:21,085 INFO Zookeeper session lost, state: CLOSED
2020-12-18 10:33:21,089 DEBUG service ping dependencies [ping:namekox_context.core.dependencies.ContextHelper:ctx, ping:namekox_config.core.dependencies.ConfigHelper:cfg, ping:namekox_zookeeper.core.dependencies.ZooKeeperHelper:zk] stopped
2020-12-18 10:33:21,090 DEBUG services ['ping'] stopped
2020-12-18 10:33:21,092 DEBUG killing services ['ping']
2020-12-18 10:33:21,092 DEBUG service ping already stopped
2020-12-18 10:33:21,092 DEBUG services ['ping'] killed
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
