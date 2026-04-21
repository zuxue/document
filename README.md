为了更好的兼容性，本平台支持两种下单模式，`正常下单`与`仿易支付下单`

使用不同的下单方式需要使用`相对应的加签方式`，具体签名方式见`文档底部`

<h3 id="HT8d0">正常下单</h3>
POST

`domain + /collection/place/create`

`Content-Type: application/x-www-form-urlencoded;charset=utf-8`

请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| unique | string | Y | 用户唯一标识 |
| order_number | string | Y | 订单号 |
| fee | int | Y | 金额(单位:分)<br/>当商户类型为person时需要能被100整除，即为正整数的**元 |
| jump_url | string | Y | 订单完成跳转url |
| notice_url | string | Y | 订单完成异步通知url |
| vendor_type | string | Y | 通道编码 |
| desc | string | N | 商品描述 |
| devide | string | N | iphone | android 用户设备类型 |
| sign | string | Y | 签名 |


<h3 id="tBbFo">仿易支付下单(使用form表单提交)</h3>
POST

`domain + /submit`

`Content-Type: application/x-www-form-urlencoded`

请求参数

`仅下表中的参数参与签名，其余参数均不参与签名`

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| pid | string | Y | 用户唯一标识 |
| out_trade_no | string | Y | 订单号 |
| money | float | Y | 金额(单位:元) |
| return_url | string | N | 订单完成跳转url |
| notify_url | string | Y | 订单完成异步通知url |
| type | string | Y | 通道编码 |
| name | string | N | 商品描述 |
| ip | string | N | IP |
| sign | string | Y | 签名 |




返回数据

```json
{
    "status": "success",
    "message": "下单成功",
    "code": 0,
    "platform_order": "2022111549545756", // 本平台订单号
    "outer_order": "aabbccdd", // 用户订单号,下单时传入的
    // 支付跳转地址
    "payment_url": "http://**.***.com/collection/import/person/person/2022111549545756",
    "sign": "D0B1116E21904C69884F648C4990359C" // 签名
}
```

<h3 id="ePma4">发起支付</h3>
跳转地址

可以自己拼接也可以从下单接口返回的数据中获取

`domain + /collection/import/{vendor_type}/{product}/{order_num}`  

| **变量名** | **说明** |
| --- | --- |
| vendor_type | 商户类型(下单时传入) |
| product | 支付产品(下单时传入) |
| order_num | 下单后返回的平台订单号 |


<h3 id="nuWZb">同步跳转</h3>
GET

用户支付完成后，平台会自动`302跳转`至下单时传入的`jump_url`中并携带以下参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| order_num | string | Y | 用户订单号 |
| platform_order | string | Y | 平台订单号 |
| rand_str | string | Y | 随机字符串 |
| fee | int | Y | 用户实付金额(单位:分) |
| sign | string | Y | 签名 |


<h3 id="GTp7M">订单查询</h3>
POST

`domain + /api/order/trade/query`

`Content-Type: application/x-www-form-urlencoded;charset=utf-8`

请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| unique | string | Y | 用户唯一标识 |
| order_num | string | N | 平台订单号 |
| outer_order | string | N | 用户订单号 |
| rand_str | string | Y | 随机字符串 |
| sign | string | Y | 签名 |


`**order_num** 与 **outer_order** 二选一即可`

返回数据

```json
{
    "status": 200,
    "message": "success",
    "data": {
        "trade_status": "FAIL", // 支付状态 FAIL 未支付 SUCCESS 已支付
        "order_num": "2022111610010110", // 平台侧订单号
        "outer_order": "2022111610010110", // 用户侧订单号
        "rand_str": "aabb", // 查询传入的随机字符串
        "sign": "CB185ADD9DA2C5ED984D98404379644C" // 签名
    }
}
```

<h3 id="xMUbr">异步回调</h3>
**请求方式：**POST

`Content-Type: application/x-www-form-urlencoded;charset=utf-8`

**回调URL：**该链接是通过基础下单接口中的请求参数“notice_url”来设置的，请确保回调URL是外部可正常访问的，且不能携带后缀参数，否则可能导致商户无法接收到微信的回调通知信息。回调URL示例：“http://**.**.com/**/**”

**通知规则**

用户支付完成后，平台会把相关支付结果和用户信息发送给商户，商户需要接收处理该消息，并返回应答。

对后台通知交互时，如果收到商户的应答不符合规范或超时，平台认为通知失败，平台会通过一定的策略定期重新发起通知，尽可能提高通知的成功率，但不保证通知最终能成功。（通知频率为15s/15s/30s/3m/10m/20m/30m/30m/30m/60m/3h/3h/3h/6h/6h - 总计 24h4m）

在发送通知后接收到字符串`SUCCESS`时视为应答成功

**通知报文**

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| order_num | string | Y | 用户侧订单号 |
| platform_order | string | Y | 平台订单号 |
| rand_str | string | Y | 下单传入的随机字符串 |
| original_price | int | Y | 原下单金额(单位:分) |
| real_price | int | Y | 实际支付金额(单位:分) |
| order_message | string | Y | 订单状态描述 |
| order_status | string | Y | 订单状态<br/>CREATE 订单创建<br/>SUCCESS 已支付<br/>CANCEL 已取消<br/>REFUND 已退款 |
| sign | string | Y | 签名 |


<h3 id="Ix2gq">正常下单签名方法</h3>
1. 将参数名按ASCLL字典序顺序排序
2. 将参数按照key=>value键值对的形式拼接，形成一个字符串 类似 { a=b&b=c&c=d }
3. 在字符串末尾拼接{ &cert=`cert` } cert 为每个用户的 密钥
4. 将最终生成的字符串进行md5加密 得到一个32位字符串
5. 将md5值转为大写

<h4 id="YOLYZ">php示例代码</h4>
常规接入签名方法

```php
/**
 * 签名
 * @param array $arr  参与签名的数组
 * @param string $cert 用户密钥
 * @return string
 */
private static function makeSign(array $arr, string $cert)
{
    $arr = array_filter($arr, function ($val) {
        return ($val === '' || $val === null) ? false : true;
    });
    if (isset($arr['sign'])) {
        unset($arr['sign']);
    }
    ksort($arr);
    $str = http_build_query($arr);
    $str = urldecode($str) . '&cert=' . $cert;
    $str = md5($str);
    return  strtoupper($str);
}
```

<h3 id="j90qf">仿易支付签名方法</h3>
1. 将参数名按ASCLL字典序顺序排序
2. 将参数按照key=>value键值对的形式拼接，形成一个字符串 类似 { a=b&b=c&c=d }
3. 在字符串末尾拼接{ cert } cert 为每个用户的 密钥
4. 将最终生成的字符串进行md5加密 得到一个32位字符串

<h4 id="VjcOO">php示例代码</h4>
仿易支付签名方法

```php
/**
  * 仿易支付签名
  * @param array $arr  参与签名的数组
  * @param string $cert 用户密钥
  * @return string
  */
protected static function likeYiMakeSign($arr, $cert)
{
    ksort($arr);
    reset($arr);
    $fieldstring = array();
    foreach ($arr as $key => $value) {
        if (!empty($value)) {
            $fieldstring[] =  $key . "=" . $value;
        }
    }
    $fieldstring = implode("&", $fieldstring);
    return md5($fieldstring . $cert);
}
```

<h3 id="uLuYV">成功请求响应内容</h3>
成功请求响应

| **status** | **message** |
| --- | --- |
| success | 下单成功(用于下单接口) |
| 200 | 请求成功(其余接口) |


```json
{
    "status": "success",
    "message": "下单成功",
    "code": 0,
    "platform_order": "2022111549545756",
    "outer_order": "aabbccdd",
    "payment_url": "http://**.***.com/collection/import/person/person/2022111549545756",
    "sign": "D0B1116E21904C69884F648C4990359C"
}
```

```json
{
    "status": 200,
    "message": "success",
    "data": {
        "trade_status": "FAIL",
        "order_num": "2022111610010110",
        "outer_order": "2022111610010110",
        "rand_str": "aabb",
        "sign": "CB185ADD9DA2C5ED984D98404379644C"
    }
}
```

<h3 id="Unr4a">错误请求响应内容</h3>
失败请求响应

```json
{
  "status": 400,
  "message": "未知用户",
  "data": []
}
```

