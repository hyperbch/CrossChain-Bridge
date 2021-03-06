# CrossChain-Bridge API

[JSON RPC API Reference](#json-rpc-api-reference)

[RESTful API Reference](#restful-api-reference)

## JSON RPC API Reference

JSON PRC API 通用调用格式：

```shell
curl -X POST -H "Content-Type:application/json" --data '{"jsonrpc":"2.0","method":"方法名","params":参数,"id":1}' SERVER_URL
```

其中，`SERVER_URL`格式为`http://host:port/rpc`

成功返回的通用格式：

```shell
{"jsonrpc":"2.0","result":返回值,"id":1}
```

错误返回的通用格式：

```shell
{"jsonrpc":"2.0","error":{"code":错误码,"message":"错误信息","data":附加备注},"id":1}
```

*以下为了简洁对每个 API 说明只列出`参数`和`返回值`两项*

[swap.GetServerInfo](#swapgetserverinfo)  
[swap.Swapin](#swapswapin)  
[swap.P2shSwapin](#swapp2shswapin)  
[swap.RetrySwapin](#swapretryswapin)  
[swap.Swapout](#swapswapout)  
[swap.GetSwapin](#swapgetswapin)  
[swap.GetSwapout](#swapgetswapout)  
[swap.GetSwapinHistory](#swapgetswapinhistory)  
[swap.GetSwapoutHistory](#swapgetswapouthistory)   
[swap.RegisterP2shAddress](#swapregisterp2shaddress)  
[swap.GetP2shAddressInfo](#swapgetp2shaddressinfo)  
[swap.RegisterAddress](#swapregisteraddress)  
[swap.GetRegisteredAddress](#swapgetregisteredaddress)  

### swap.GetServerInfo

查询服务信息

##### 参数：
```text
[] (空)
```
##### 返回值：
```text
成功返回服务信息，失败返回错误。
```

### swap.Swapin

申请换进置换

##### 参数：
```json
["充值交易哈希"]
```
##### 返回值：
```text
成功返回`Success`，失败返回错误。
```

### swap.P2shSwapin

申请换进置换 (BTC 专用接口)

支持每个用户一个专用充值地址

##### 参数：
```json
[{"txid":"充值交易哈希", "bind":"绑定地址"}]
```
##### 返回值：
```text
成功返回`Success`，失败返回错误。
```

### swap.RetrySwapin

重新申请换进置换

只有账户由于没有注册而申请置换失败的情形下才可以重新申请置换。

##### 参数：
```json
["充值交易哈希"]
```
##### 返回值：
```text
成功返回`Success`，失败返回错误。
```

### swap.Swapout

申请换出置换

##### 参数：
```json
["销毁交易哈希"]
```
##### 返回值：
```text
成功返回`Success`，失败返回错误。
```

### swap.GetSwapin

查询换进置换

##### 参数：
```json
["充值交易哈希"]
```
##### 返回值：
```text
成功返回换进置换信息，失败返回错误。
```

### swap.GetSwapout

查询换出置换

##### 参数：
```json
["销毁交易哈希"]
```
##### 返回值：
```text
成功返回换出置换信息，失败返回错误。
```

### swap.GetSwapinHistory

查询换进置换历史，支持分页，从 offset (默认0) 开始选取前 limit (默认20) 项

##### 参数：
```shell
[{"address":"账户地址", "offset":offset, "limit":limit}]
```

address 为 all 表示所有历史

limit 最大值为 100

##### 返回值：
```text
成功返回换进置换历史，失败返回错误。
```

### swap.GetSwapoutHistory

查询换出置换历史，支持分页，从 offset (默认0) 开始选取前 limit (默认20) 项

##### 参数：
```shell
[{"address":"账户地址", "offset":offset, "limit":limit}]
```

address 为 all 表示所有历史

limit 最大值为 100

##### 返回值：
```text
成功返回换出置换历史，失败返回错误。
```

### swap.RegisterP2shAddress

注册Ps2h充值地址 (BTC 专用接口)

##### 参数：
```json
["绑定地址"]
```
##### 返回值：
```text
成功返回绑定地址对应的Ps2h充值地址信息，失败返回错误。
```

### swap.GetP2shAddressInfo

获取Ps2h充值地址信息 (BTC 专用接口)

##### 参数：
```json
["P2sh地址"]
```
##### 返回值：
```text
成功返回Ps2h充值地址信息，失败返回错误。
```

### swap.RegisterAddress

注册账户地址

##### 参数：
```json
["账户地址"]
```
##### 返回值：
```text
成功返回`Success`，失败返回错误。
```

### swap.GetRegisteredAddress

获取注册账户地址

##### 参数：
```json
["账户地址"]
```
##### 返回值：
```text
成功返回注册账户信息，失败返回错误。
```

## RESTful API Reference

### GEt /serverinfo

查询服务信息

### GET /swapin/{txid}

查询换进置换，txid 为充值交易哈希

### GET /swapout/{txid}

查询换出置换，txid 为销毁交易哈希

### GET /swapin/history/{address}?offset=0&limit=20

查询换进置换历史，支持分页，addess 为账户地址

address 为 all 表示所有历史

limit 最大值为 100

### GET /swapout/history/{address}?offset=0&limit=20

查询换出置换历史，支持分页，addess 为账户地址

address 为 all 表示所有历史

limit 最大值为 100

### POST /swapin/post/{txid}

申请换进置换，txid 为充值交易哈希

### POST /swapin/post/{txid}/{bind}

申请 P2sh 换进置换，txid 为充值交易哈希， bind 为对应的绑定地址。（BTC 专用）

### POST /swapin/retry/{txid}

重新申请换进置换

只有账户由于没有注册而申请置换失败的情形下才可以重新申请置换。

### POST /swapout/post/{txid}

申请换出置换，txid 为销毁交易哈希

### GET /p2sh/{address}

获取 P2sh 地址信息，address 为 P2sh 地址。（BTC 专用）

### POST /p2sh/bind/{address}

注册 P2sh 地址，address 为绑定地址。（BTC 专用）

### GET /registered/{address}

获取注册账户地址信息

### POST /register/{address}

注册账户地址
