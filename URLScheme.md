# ShadowsocksR URL Scheme

## 关于 Base64

定义  `Base64` 为 **URL Safe Base64**，**不带 Padding （没有末尾的等于号）**。

下文中 `Base64 Encoded String`， `base64()`  等均服从此规则。



## URL Scheme

#### Definition

```
ssr://base64({body}/?{params})
```



#### Part 1: `body`

```
{server_address}:{server_port}:{protocol}:{method}:{obfs}:{password}
```

参数说明如下：

|       参数名称       |          值类型          |   说明   |
| :--------------: | :-------------------: | :----: |
| `server_address` |        IPv4地址         | 服务端地址  |
|  `server_port`   |        Integer        | 服务端端口号 |
|    `protocol`    |        String         |   协议   |
|     `method`     |        String         |  加密方式  |
|      `obfs`      |        String         |  混淆方式  |
|    `password`    | Base64 Encoded String |   密码   |



#### Part 2: `params`

所有参数均可选，采用  `application/x-www-form-urlencoded`  方式编码。

参数说明如下：

|     参数名称     |          值类型          |  说明  |     备注     |
| :----------: | :-------------------: | :--: | :--------: |
| `obfsparam`  | Base64 Encoded String | 混淆参数 |            |
| `protoparam` | Base64 Encoded String | 协议参数 |            |
|  `remarks`   | Base64 Encoded String |  备注  |            |
|   `group`    | Base64 Encoded String | 组名称  |            |
|  `udpport`   |        Integer        |      | 仅 C# 客户端支持 |
|    `uot`     |        Integer        |      | 仅 C# 客户端支持 |

##### For example:

```
obfsparam=YnJlYWt3YTExLm1vZQ&remarks=5rWL6K-V5Lit5p
```



## Example

#### 服务器配置样例

|  配置项  | 值                    |
| :---: | :------------------- |
| 服务器IP | `127.0.0.1`          |
|  端口   | `1234`               |
|  密码   | `aaabbb`             |
|  加密   | `aes-128-cfb`        |
|  协议   | `auth_aes128_md5`    |
| 协议参数  | (空)                  |
|  混淆   | `tls1.2_ticket_auth` |
| 混淆参数  | `breakwa11.moe`      |
|  备注   | `测试中文`               |



#### JSON

```javascript
{
  "server_address": "127.0.0.1",
  "server_port": "1234",
  "method": "aes-128-cfb",
  "password": "aaabbb",  // => Base64:YWFhYmJi
  "protocol": "auth_aes128_md5",
  "obfs": "breakwa11.moe",
  "remark": "测试中文"
}
```



#### URL (Without Remark)

```
ssr://MTI3LjAuMC4xOjEyMzQ6YXV0aF9hZXMxMjhfbWQ1OmFlcy0xMjgtY2ZiOnRsczEuMl90aWNrZXRfYXV0aDpZV0ZoWW1KaS8_b2Jmc3BhcmFtPVluSmxZV3QzWVRFeExtMXZaUQ
```



#### URL (With Remark)

```
ssr://MTI3LjAuMC4xOjEyMzQ6YXV0aF9hZXMxMjhfbWQ1OmFlcy0xMjgtY2ZiOnRsczEuMl90aWNrZXRfYXV0aDpZV0ZoWW1KaS8_b2Jmc3BhcmFtPVluSmxZV3QzWVRFeExtMXZaUSZyZW1hcmtzPTVyV0w2Sy1WNUxpdDVwYUg
```

**注: 如果你生成的 「不带备注」 的结果结果与上面的不一致，请检查实现代码，否则可能导致部分环境下识别错误。**
