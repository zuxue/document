### 正常下单
POST

`domain + /open/order/create`

`Content-Type: application/json`

请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| mch_id | string | Y | 商户号 |
| out_trade_order | string | Y | 外部订单号(用户订单号) |
| notify_url | string | Y | 订单完成异步通知url |
| amount | int | Y | 金额(单位:分) |
| group_code | string | Y | 群组code(通道编码) |
| callback_url | string | N | 订单完成跳转url |
| desc | string | N | 商品描述 |
| sign | string | Y | 签名 |


返回数据

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "out_trade_order": "xxxxxxx",
        "order_num": "xxxxxxx",
        "amount": 1000,
        "pay_type": "url",
        "pay_data": {
            "url": "https://xxx.xxx.com/xxx/xxx/xxx/xxx",
            "form": "",
            "param": ""
        },
        "sign": "xxxxxxxxxx"
    }
}
```

### 订单查询
POST

`domain + /open/order/query`

`Content-Type: application/json`

请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| mch_id | string | Y | 商户号 |
| out_trade_order | string | Y | 外部订单号(用户订单号) |
| sign | string | Y | 签名 |


返回数据

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "out_trade_order": "2025122500000000",
        "order_num": "202512251509100NQSTh",
        "status": "Paid", // Paid 为已支付，Unpaid 为未支付
        "sign": "8A4E8F6A190324DD9D89CD31EED3D35D"
    }
}
```

### 异步回调
**请求方式：**POST

`Content-Type: application/json`

**回调URL：**该链接是通过基础下单接口中的请求参数“notify_url”来设置的，请确保回调URL是外部可正常访问的，且不能携带后缀参数，否则可能导致商户无法接收到微信的回调通知信息。回调URL示例：“http://**.**.com/**/**”

**通知规则**

用户支付完成后，平台会把相关支付结果和用户信息发送给商户，商户需要接收处理该消息，并返回应答。

对后台通知交互时，如果收到商户的应答不符合规范或超时，平台认为通知失败

在发送通知后接收到字符串`SUCCESS`时视为应答成功

**通知报文**

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| order_num | string | Y | 平台订单号 |
| out_trade_order | string | Y | 外部订单号(用户订单号) |
| pay_amount | int | Y | 支付金额(单位:分) |
| status | string | Y | 订单状态   SUCCESS 支付成功 |
| sign | string | Y | 签名 |


```json
{
    "order_num": "202512251509100NQSTh",
    "out_trade_order": "2025122500000000",
    "pay_amount": 100,
    "sign": "A67175D3DB42C7C76FED1792AF6083F5",
    "status": "SUCCESS"
}
```

### 签名方法
1. 将参数名按ASCLL字典序顺序排序
2. 将参数按照key=>value键值对的形式拼接，形成一个字符串 类似 { a=b&b=c&c=d }
3. 在字符串末尾拼接{ &key=`key` } key 为每个用户的 密钥
4. 将最终生成的字符串进行md5加密 得到一个32位字符串
5. 将md5值转为大写

#### php示例代码
常规接入签名方法

```php
/**
 * 签名
 * @param array $arr  参与签名的数组
 * @param string $key 用户密钥
 * @return string
 */
function makeSign(array $arr, string $key)
{
    $arr = array_filter($arr, function ($val) {
        return ($val === '' || $val === null) ? false : true;
    });
    if (isset($arr['sign'])) {
        unset($arr['sign']);
    }
    ksort($arr);
    $str = http_build_query($arr);
    $str = urldecode($str) . '&key=' . $key;
    $str = md5($str);
    return  strtoupper($str);
}
```

#### golang示例代码
```go
// 参数:
//   params - 参与签名的参数键值对，会自动过滤掉'sign'键、nil值和空字符串值
//   key    - 用于签名计算的密钥
// 返回值:
//   MD5签名字符串
func MakeSign(params map[string]any, key string) string {
	keys := make([]string, 0, len(params))
	for k, v := range params {
		if k == "sign" || v == nil {
			continue
		}
		if s, ok := v.(string); ok && s == "" {
			continue
		}
		keys = append(keys, k)
	}
	sort.Strings(keys)
	toSignParts := make([]string, 0, len(keys))
	for _, k := range keys {
		valueStr := fmt.Sprintf("%v", params[k])
		toSignParts = append(toSignParts, k+"="+valueStr)
	}
	queryString := strings.Join(toSignParts, "&")
	finalStringToSign := queryString + "&key=" + key
	hash := md5.Sum([]byte(finalStringToSign))
	return fmt.Sprintf("%X", hash)
}
```

#### java示例代码
```java
import java.util.*;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class SignUtil {
    public static String makeSign(Map<String, Object> params, String key) {
        List<String> keys = new ArrayList<>();
        for (Map.Entry<String, Object> entry : params.entrySet()) {
            if ("sign".equals(entry.getKey()) || entry.getValue() == null) {
                continue;
            }
            if (entry.getValue() instanceof String && ((String) entry.getValue()).isEmpty()) {
                continue;
            }
            keys.add(entry.getKey());
        }
        Collections.sort(keys);
        StringBuilder toSignBuilder = new StringBuilder();
        for (int i = 0; i < keys.size(); i++) {
            String k = keys.get(i);
            String valueStr = String.valueOf(params.get(k));
            toSignBuilder.append(k).append("=").append(valueStr);
            if (i < keys.size() - 1) {
                toSignBuilder.append("&");
            }
        }
        toSignBuilder.append("&key=").append(key);
        String finalStringToSign = toSignBuilder.toString();
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] hashBytes = md.digest(finalStringToSign.getBytes());
            StringBuilder hexString = new StringBuilder();
            for (byte b : hashBytes) {
                String hex = Integer.toHexString(0xff & b);
                if (hex.length() == 1) {
                    hexString.append('0');
                }
                hexString.append(hex);
            }
            return hexString.toString().toUpperCase();
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("MD5 algorithm not found", e);
        }
    }
}
```

#### python示例代码
```python
import hashlib

def make_sign(params, key):
    filtered_params = {}
    for k, v in params.items():
        if k == 'sign' or v is None:
            continue
        if isinstance(v, str) and v == "":
            continue
        filtered_params[k] = v
    
    sorted_keys = sorted(filtered_params.keys())
    to_sign_parts = []
    for k in sorted_keys:
        value_str = str(filtered_params[k])
        to_sign_parts.append(f"{k}={value_str}")
    
    query_string = "&".join(to_sign_parts)
    final_string_to_sign = query_string + "&key=" + key
    hash_md5 = hashlib.md5(final_string_to_sign.encode()).hexdigest()
    return hash_md5.upper()
```

