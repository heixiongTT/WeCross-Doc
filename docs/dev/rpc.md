# JSON-RPC API

WeCross提供了[Java-SDK](./sdk.html)，方便Java项目直接引入，其它语言的项目则可通过调用JSON-RPC API完成跨链开发。

## 状态码

当RPC调用遇到错误时，返回的响应对象必须包含错误结果字段，该字段有下列成员参数：

- errorCode: 使用数值表示该异常的错误类型，必须为整数。
- message: 对该错误的简单描述字符串。

状态码及其对应的含义如下：  

| code    | 含义            |
| :-------| :------------- |
| 0       | 执行成功  |
| 10000   | 内部错误  |
| 10001   | URI访问路径错误  |
| 10002   | URI查询字段错误  |
| 2xxxx   | 网络包错误  |
| 3xxxx   | 跨链账户错误 |
| 4xxxx   | 资源调用错误 |
| 5xxxx   | 交易查询错误 |
| 6xxxx   | 两阶段事务错误 |
| 7xxxx   | 哈希时间锁合约错误 |

## login
登录接口

#### 接口URL
> http://127.0.0.1:8250/auth/login

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Body参数

```json
{
	"version": "1",
	"data": {
		"username": "alice",
		"password": "123456"
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.username     | alice |  必填 | 跨链账户名 |
| data.password     | 123456 |  必填 | 密码 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "success",
	"data": {
		"errorCode": 0,
		"message": "success",
		"credential": "Bearer eyJpYXRtaWxsIjoxNjA2OTkyMDc5NzU0LCJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJzaGFyZW9uZyIsIm5iZiI6MTYwNjk5MjA3OSwiaXNzIjoib3JnMSIsImV4cCI6MTYwNzAxMDA3OSwiaWF0IjoxNjA2OTkyMDc5fQ.9_IhhFwBajPrucruAx05noBUzFrHc7Xfl2fUObhdkX8",
		"universalAccount": {
			"username": "shareong",
			"uaID": "3059301306072a8648ce3d020106082a811ccf5501822d034200044f7f2e394493742fa58bf17b22ed73fd92125be9ca7c093c516531572bac91a7608578ef6724a3115a1126047cb50762fc6f4e1eb0b4fb8a4c3efe6d1982c356",
			"pubKey": "3059301306072a8648ce3d020106082a811ccf5501822d034200044f7f2e394493742fa58bf17b22ed73fd92125be9ca7c093c516531572bac91a7608578ef6724a3115a1126047cb50762fc6f4e1eb0b4fb8a4c3efe6d1982c356",
			"isAdmin": false
		}
	}
}
```

## logout
登出接口

#### 接口URL
> http://127.0.0.1:8250/auth/logout

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{}
```

#### 成功响应示例
```json
{
	"version": "1.0",
	"errorCode": 0,
	"message": "success",
	"data": {
		"errorCode": 0,
		"message": "success"
	}
}
```

## listResources
获取指定区块链的资源列表

#### 接口URL
> http://127.0.0.1:8250/sys/listResources?path=payment.bcos&offset=0&size=1

#### 请求方式
> GET

#### Content-Type
> application/json

#### 请求Query参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| path     | payment.bcos | 必填 | 区块链路径 |
| offset     | 0 | 必填 | 偏移量 |
| size     | 1 | 必填 | 获取资源的数量，最大1024 |


#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |


#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"total": 13,
		"resourceDetails": [
			{
				"path": "payment.bcos.HelloWorld",
				"distance": 0,
				"stubType": "BCOS2.0",
				"properties": {
					"BCOS_PROPERTY_CHAIN_ID": "1",
					"BCOS_PROPERTY_GROUP_ID": "1"
				},
				"checksum": null
			}
		]
	}
}
```

## detail
获取资源的详情，若资源路径path=zone.chain.name，则访问路径为：```resource/zone/chain/name/detail```

#### 接口URL
> http://127.0.0.1:8250/resource/payment/bcos/HelloWorld/detail

#### 请求方式
> GET

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |


#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"path": "payment.bcos.HelloWorld",
		"distance": 0,
		"stubType": "BCOS2.0",
		"properties": {
			"BCOS_PROPERTY_CHAIN_ID": "1",
			"BCOS_PROPERTY_GROUP_ID": "1"
		},
		"checksum": null
	}
}
```


## call
基于只读方式调用资源，若资源路径path=zone.chain.name，则访问路径为：```resource/zone/chain/name/call```

#### 接口URL
> http://127.0.0.1:8250/resource/payment/bcos/HelloWorld/call

#### 请求方式
> POST

#### Content-Type
> application/json 

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": "1",
	"path": "payment.bcos.HelloWorld",
	"data": {
		"method": "get",
		"args": [],
		"options": {
			"XA_TRANSACTION_ID": "c7ba79423f9b4cacb5e2ee52d07f5831"
		}
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.method     | get |  必填 | 调用方法 |
| data.args     | {} |  必填 | 参数列表 |
| data.options     | - |  选填 | 调用选项，访问参与事务的资源需要该字段 |
| data.options.XA_TRANSACTION_ID     | c7ba79423f9b4cacb5e2ee52d07f5831 |  必填 | 事务ID |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"errorCode": 0,
		"message": "success",
		"hash": "",
		"blockNumber": 0,
		"result": [
			"Hello World"
		]
	}
}
```


## sendTransaction
基于发交易的方式调用资源，若资源路径path=zone.chain.name，则访问路径为：```resource/zone/chain/name/sendTransaction```

#### 接口URL
> http://127.0.0.1:8250/resource/payment/bcos/HelloWorld/sendTransaction

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": "1",
	"data": {
		"method": "set",
		"args": [
			"Hello WeCross"
		],
		"options": {
			"XA_TRANSACTION_ID": "c7ba79423f9b4cacb5e2ee52d07f5831",
			"XA_TRANSACTION_SEQ": 1607330421667
		}
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.method     | set |  必填 | 调用方法 |
| data.args     | Hello WeCross |  必填 | 参数列表 |
| data.options     | - |  选填 | 调用选项，访问参与事务的资源需要该字段 |
| data.options.XA_TRANSACTION_ID     | c7ba79423f9b4cacb5e2ee52d07f5831 |  必填 | 事务ID |
| data.options.XA_TRANSACTION_SEQ     | 1607330421647 |  必填 | 事务Seq，需保证递增 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"errorCode": 0,
		"message": "success",
		"hash": "",
		"blockNumber": 0,
		"result": [
			"Hello World"
		]
	}
}
```

## listTransactions
获取指定区块链的交易列表

#### 接口URL
> http://127.0.0.1:8250/trans/listTransactions?path=payment.bcos&blockNumber=10&offset=0&size=2

#### 请求方式
> GET

#### Content-Type
> application/json

#### 请求Query参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| path     | payment.bcos | 必填 | 区块链路径 |
| blockNumber     | 10 | 必填 | 块高 |
| offset     | 0 | 必填 | 偏移量 |
| size     | 2 | 必填 | 获取交易的数量，最大1024 |

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 成功响应示例
```javascript
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"nextBlockNumber": 8,
		"nextOffset": 0,
		"transactions": [
			{
				"txHash": "0xd9a1f1415826c0a8b891ddb6998b21b581d81c3cbbf624cbaa6664cfffff747e",
				"blockNumber": 10
			},
			{
				"txHash": "0x11420a62d2f81dae948148a47d5ee04983cf319122b192f8acc751084be2e015",
				"blockNumber": 9
			}
		]
	}
}
```

## getTransaction
根据块高和哈希获取交易详情

#### 接口URL
> http://127.0.0.1:8250/trans/getTransaction?path=payment.bcos&txHash=0xba938113bdbae8e57dcf68f96b1edd37f288959ecc0e51c0b409901c27dabafc&blockNumber=1673

#### 请求方式
> GET

#### Content-Type
> application/json

#### 请求Query参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| path     | payment.bcos | 必填 | 区块链路径 |
| txHash     | 0xba938113bdbae8e57dcf68f96b1edd37f288959ecc0e51c0b409901c27dabafc | 必填 | 交易哈希 |
| blockNumber     | 1673 | 必填 | 区块高度 |


#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |


#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"path": "payment.bcos.HelloWorld",
		"username": "shareong",
		"blockNumber": 1673,
		"txHash": "0xba938113bdbae8e57dcf68f96b1edd37f288959ecc0e51c0b409901c27dabafc",
		"xaTransactionID": "c7ba79423f9b4cacb5e2ee52d07f5831",
		"xaTransactionSeq": 1607330421667,
		"method": "set",
		"args": [
			"Hello WeCross"
		],
		"result": [],
		"byProxy": true,
		"txBytes": "eyJoYXNoIjoiMHhiYTkzODExM2JkYmFlOGU1N2RjZjY4Zjk2YjFlZGQzN2YyODg5NTllY2MwZTUxYzBiNDA5OTAxYzI3ZGFiYWZjIiwibm9uY2UiOjEzMDUwMDA2MzYxOTI1MzA0NDU0MTE0MjE5MDk0MDMyMTM0NzU3MjU1NDgyNjEzOTQ5MjgxODAxOTI1MjE5MTQxMjMyODI3MjA1NjgsImJsb2NrSGFzaCI6IjB4ZmNlNmQzMDQxMDc3MWNkMDI0OWMzNDk3NjBkNjJiMzhlOTdiZmFiNTQ2ZGE2MDMxNjQzNzY1ZTk2ZGE5NmQ4ZCIsImJsb2NrTnVtYmVyIjoxNjczLCJ0cmFuc2FjdGlvbkluZGV4IjowLCJmcm9tIjoiMHgzOTlhOGNlODUzZjk2YmRmNzIzZTNjOWY5OGY1NDM4ZjA4ZGEyZWEwIiwidG8iOiIweDc5ODhiN2Y1YjE4NDVjODFkMjE3YWJhZjZlOWNlZmExODlkM2E3YmIiLCJ2YWx1ZSI6MCwiZ2FzUHJpY2UiOjMwMDAwMDAwMDAwMCwiZ2FzIjozMDAwMDAwMDAwMDAsImlucHV0IjoiMHhkMWIwNGI4MzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwYzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxNzYzYzViZGJhMzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxNDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTgwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDFjMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMjA2MTM4MzM2MjMyMzM2NTY0MzAzNjYzNjEzNDM4NjEzMzYyNjQzODY0NjI2NDM3NjU2NjMxMzUzNTYyNjE2MzYzMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAyMDYzMzc2MjYxMzczOTM0MzIzMzY2Mzk2MjM0NjM2MTYzNjIzNTY1MzI2NTY1MzUzMjY0MzAzNzY2MzUzODMzMzEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDE3NzA2MTc5NmQ2NTZlNzQyZTYyNjM2ZjczMmU0ODY1NmM2YzZmNTc2ZjcyNmM2NDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMGI3MzY1NzQyODczNzQ3MjY5NmU2NzI5MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDA2MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMjAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDBkNDg2NTZjNmM2ZjIwNTc2NTQzNzI2ZjczNzMwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMCIsImNyZWF0ZXMiOm51bGwsInB1YmxpY0tleSI6bnVsbCwicmF3IjpudWxsLCJyIjpudWxsLCJzIjpudWxsLCJ2IjowLCJub25jZVJhdyI6IjEzMDUwMDA2MzYxOTI1MzA0NDU0MTE0MjE5MDk0MDMyMTM0NzU3MjU1NDgyNjEzOTQ5MjgxODAxOTI1MjE5MTQxMjMyODI3MjA1NjgiLCJibG9ja051bWJlclJhdyI6IjE2NzMiLCJ0cmFuc2FjdGlvbkluZGV4UmF3IjoiMCIsInZhbHVlUmF3IjoiMCIsImdhc1ByaWNlUmF3IjoiMzAwMDAwMDAwMDAwIiwiZ2FzUmF3IjoiMzAwMDAwMDAwMDAwIn0=",
		"receiptBytes": "eyJ0cmFuc2FjdGlvbkhhc2giOiIweGJhOTM4MTEzYmRiYWU4ZTU3ZGNmNjhmOTZiMWVkZDM3ZjI4ODk1OWVjYzBlNTFjMGI0MDk5MDFjMjdkYWJhZmMiLCJ0cmFuc2FjdGlvbkluZGV4IjowLCJibG9ja0hhc2giOiIweGZjZTZkMzA0MTA3NzFjZDAyNDljMzQ5NzYwZDYyYjM4ZTk3YmZhYjU0NmRhNjAzMTY0Mzc2NWU5NmRhOTZkOGQiLCJibG9ja051bWJlciI6MTY3MywiZ2FzVXNlZCI6MTM4MzA1OCwiY29udHJhY3RBZGRyZXNzIjoiMHgwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwIiwicm9vdCI6IjB4ZTU5ZmU4Y2MzOGM1MTYwMjNlMjc2NTdkYmIwMWE3NzZiOTJhYWRlN2EyNzc1ZDQzN2E1OTRkZmYwMjM2MWE2NyIsInN0YXR1cyI6IjB4MCIsIm1lc3NhZ2UiOm51bGwsImZyb20iOiIweDM5OWE4Y2U4NTNmOTZiZGY3MjNlM2M5Zjk4ZjU0MzhmMDhkYTJlYTAiLCJ0byI6IjB4Nzk4OGI3ZjViMTg0NWM4MWQyMTdhYmFmNmU5Y2VmYTE4OWQzYTdiYiIsImlucHV0IjoiMHhkMWIwNGI4MzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwYzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxNzYzYzViZGJhMzAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAxNDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMTgwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDFjMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMjA2MTM4MzM2MjMyMzM2NTY0MzAzNjYzNjEzNDM4NjEzMzYyNjQzODY0NjI2NDM3NjU2NjMxMzUzNTYyNjE2MzYzMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAyMDYzMzc2MjYxMzczOTM0MzIzMzY2Mzk2MjM0NjM2MTYzNjIzNTY1MzI2NTY1MzUzMjY0MzAzNzY2MzUzODMzMzEwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDE3NzA2MTc5NmQ2NTZlNzQyZTYyNjM2ZjczMmU0ODY1NmM2YzZmNTc2ZjcyNmM2NDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMGI3MzY1NzQyODczNzQ3MjY5NmU2NzI5MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDA2MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMjAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDBkNDg2NTZjNmM2ZjIwNTc2NTQzNzI2ZjczNzMwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMCIsIm91dHB1dCI6IjB4MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAyMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAiLCJsb2dzIjpbXSwibG9nc0Jsb29tIjoiMHgwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMCIsInR4UHJvb2YiOm51bGwsInJlY2VpcHRQcm9vZiI6bnVsbH0="
	}
}
```

## startXATransaction
开始事务

#### 接口URL
> http://127.0.0.1:8250/xa/startXATransaction

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
    "version":"1",
    "data":{
        "xaTransactionID":"1c199e6d72744133992c139c3f12ba07",
        "paths":[
            "payment.fabric.mycc",
            "payment.bcos.HelloWorld"
        ]
    }
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.xaTransactionID     | 1c199e6d72744133992c139c3f12ba07 |  必填 | 事务ID |
| data.paths     | ["payment.fabric.mycc", "payment.bcos.HelloWorld"] |  必填 | 参与事务的资源列表 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"status": 0,
		"chainErrorMessages": []
	}
}
```

## commitXATransaction
提交事务，注意：提交和回滚事务只需要传入参与该事务的区块链的路径就可以了，无需传入所有参与事务的资源

#### 接口URL
> http://127.0.0.1:8250/xa/commitXATransaction

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": "1",
	"data": {
		"xaTransactionID": "1c199e6d72744133992c139c3f12ba17",
		"paths": [
			"payment.fabric",
			"payment.bcos"
		]
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.xaTransactionID     | 1c199e6d72744133992c139c3f12ba17 |  必填 | 事务ID |
| data.paths     | ["payment.fabric", "payment.bcos"] |  必填 | 参与该事务的区块链列表 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"status": 0,
		"chainErrorMessages": []
	}
}
```

## rollbackXATransaction
回滚事务，注意：提交和回滚事务只需要传入参与该事务的区块链的路径就可以了，无需传入所有参与事务的资源

#### 接口URL
> http://127.0.0.1:8250/xa/rollbackXATransaction

#### 请求方式
> POST

#### Content-Type
> application/json


#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": "1",
	"data": {
		"xaTransactionID": "1c199e6d72744133992c139c3f12ba08",
		"paths": [
			"payment.fabric",
			"payment.bcos"
		]
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.xaTransactionID     | 1c199e6d72744133992c139c3f12ba08 |  必填 | 事务ID |
| data.paths     | ["payment.fabric", "payment.bcos"] |  必填 | 参与该事务的区块链列表 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"status": 0,
		"chainErrorMessages": []
	}
}
```

## listXATransactions
获取事务列表，结果根据事务的开启时间排序，同时返回是否已经获取完毕，以及下一次请求的偏移

#### 接口URL
> http://127.0.0.1:8250/xa/listXATransactions

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": "1",
	"data": {
		"size": 2,
		"offsets": {}
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.size     | 2 |  必填 | 获取的事务数量，最大为1024 |
| data.offsets     | {} |  必填 | 偏移量，如果是空则代表查询所有的区块链 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"xaList": [
			{
				"xaTransactionID": "1c199e6d72744133992c139c3f12ba08",
				"username": "shareong",
				"status": "rolledback",
				"timestamp": 1607333274,
				"paths": [
					"payment.bcos.HelloWorld",
					"payment.fabric.mycc"
				]
			},
			{
				"xaTransactionID": "1c199e6d72744133992c139c3f12ba17",
				"username": "shareong",
				"status": "committed",
				"timestamp": 1607332955,
				"paths": [
					"payment.bcos.HelloWorld",
					"payment.fabric.mycc"
				]
			}
		],
		"nextOffsets": {
			"payment.bcos": 15,
			"payment.fabric": 9
		},
		"finished": false
	}
}
```

## getXATransaction
根据事务ID获取事务详情

#### 接口URL
> http://127.0.0.1:8250/xa/getXATransaction

#### 请求方式
> POST

#### Content-Type
> application/json

#### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| Content-Type     | application/json |  必填 | - |
| Authorization     | |  必填 | login返回的credential |

#### 请求Body参数

```json
{
	"version": 1,
	"data": {
		"xaTransactionID": "1c199e6d72744133992c139c3f12ba10",
		"paths": [
			"payment.bcos",
			"payment.fabric"
		]
	}
}
```
| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| version     | 1 |  必填 | 接口版本 |
| data.xaTransactionID     | 1c199e6d72744133992c139c3f12ba10 |  必填 | 事务ID |
| data.paths     | ["payment.fabric", "payment.bcos"] |  必填 | 参与该事务的区块链列表 |

#### 成功响应示例
```json
{
	"version": "1",
	"errorCode": 0,
	"message": "Success",
	"data": {
		"xaList": [
			{
				"xaTransactionID": "1c199e6d72744133992c139c3f12ba08",
				"username": "shareong",
				"status": "rolledback",
				"timestamp": 1607333274,
				"paths": [
					"payment.bcos.HelloWorld",
					"payment.fabric.mycc"
				]
			},
			{
				"xaTransactionID": "1c199e6d72744133992c139c3f12ba17",
				"username": "shareong",
				"status": "committed",
				"timestamp": 1607332955,
				"paths": [
					"payment.bcos.HelloWorld",
					"payment.fabric.mycc"
				]
			}
		],
		"nextOffsets": {
			"payment.bcos": 15,
			"payment.fabric": 9
		},
		"finished": false
	}
}
```

