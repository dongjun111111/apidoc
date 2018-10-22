Hermes 测试环境接口

API endpoint: https://hermes.ocx.im

# 目前支持的币种

1. ETH
2. ERC20 代币
3. BTC

# 通过官方途径获取

1. appkey:  用于 HTTP API 认证

2. appsecret:  用于 HTTP API 签名

# 认证与签名

Hermes 认证基于 OAuth2 bearer token 认证，签名使用 HMAC sha256，发起 HTTP 请求前通过在 header 设置

1. X-Hermes-Key: <appkey>

2. X-Hermes-Signature: 参考下述签名步骤得到的 hash 值

## 签名步骤

1. 签名 payload 是对 HTTP verb, HTTP path 和 HTTP queries(body) 进行组装, 如

```
POST|/v1beta/addresses|currency_code=eth&sn=user_id
```

2. 对上述组装后的字符串使用 `HMAC-SHA256` 加密算法进行 `hash` 计算:

```
hash = HMAC-SHA256(payload, appsecret).to_hex
```

# API 列表

1. `POST /v1beta/addresses` 生成特定币种的地址

```json
# Request

{
  "currency_code": "eth",
  "sn": "user id",
}

# Response
{
  "data": {
    "currency_code": "eth",
    "address": "0x84Eaeddd4952c22fE69222f8382200F3048bE24C"
  }
}
```


2. `GET /v1beta/accounts/{currency_code}`  获取特定币种的余额

```json
# Response
{
  "data":{
    "currency_code": "eth",
    "balance": 99.5,
    "locked": 0.5
   }
}
```

3. `POST /v1beta/withdraws` 申请提现

```json
# Request

{
  "currency_code": "eth",
  "amount": 1.0,
  "address": "valid currency address to which your withdrawal wants to send",
  "external_uuid": "uuid on merchant side",
}

# Response

{
  "currency_code": "eth",
  "txid": "blockchain txid",
  "state": "withdrawing",    // withdrawing|sent
  "from": "0x0001",
  "to": "0x0002",
  "value": 1.0,
  "memo": "withdrawal memo",
  "confirmations": 1,
  "external_uuid": ""
}
```

# 提供回调接口, 如 https://merchantapp.com/webhook

Hermes 会在针对充值到账、提现到账进行回调通知商户








