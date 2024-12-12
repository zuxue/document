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
| vendor_type | string | Y | 商户类型<br/>(缩写方式见下方<br/>`缩写对照表`)<br/>official 公众号商户<br/>alipay 支付宝<br/>person 个人商户<br/>fourth 第四方商户<br/>alipay_sub 支付宝授权商户<br/>alipay_zft 支付宝直付通商户 |
| product | string | N | 见下方 支付产品<br/>(若`vendor_type`使用`缩写方式`，则此参数忽略，否则必填) |
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
| type | string | Y | 商户类型<br/>(见下方`缩写对照表`) |
| name | string | N | 商品描述 |
| ip | string | N | IP |
| sign | string | Y | 签名 |


**提示：使用**`**省略支付产品**`**的写法**

方法：`vendor_type = 商户类型 + "@@" + 支付产品`

使用缩写方法传入`vendor_type` 则可`不传``product`，传了也会被忽略

`@@和数字方式二选一即可`

<h3 id="PQVD2">缩写对照表</h3>
请参照对照表传入`**vendor_type**`参数**，**推荐使用`**简写方式2**`

| **支付产品** | **简写方式1(**`**vendor_type**`**)** | **简写方式2(**`**vendor_type**`**)** |
| --- | --- | --- |
| 微信二维码支付 | official@@native | 0000 |
| 微信jsapi 支付 | official@@jsapi | 0001 |
| 微信h5支付(暂未开放) | official@@h5 | 0002 |
| 微信APP支付(限IOS) | official@@app | 0003 |
| 微信二级商户二维码支付 | wx_special@@native | 0100 |
| 微信二级商户jsapi支付 | wx_special@@jsapi | 0101 |
| 支付宝UID支付 | alipay@@uid | 0300 |
| 支付宝当面付 | alipay@@face_to_face | 0301 |
| 支付宝小程序JSAPI支付 | alipay@@mini_jsapi | 0302 |
| 支付宝h5支付 | alipay@@h5 | 0303 |
| 支付宝pc支付 | alipay@@pc | 0304 |
| 支付宝app支付 | alipay@@app | 0305 |
| 支付宝小程序预授权 | alipay@@mini_auth | 0306 |
| 支付宝H5预授权 | alipay@@pre_auth | 0307 |
| 支付宝商家扣款 | alipay@@agreement | 0308 |
| 支付宝UID红包 | alipay@@red_packet | 0309 |
| 支付宝当面付预授权 | alipay@@face_auth | 0310 |
| 支付宝生活号JSAPI支付 | alipay@@life_jsapi | 0311 |
| 支付宝当面付JSAPI支付 | alipay@@face_jsapi | 0312 |
| 支付宝当面付转手机网站支付 | alipay@@face_to_h5 | 0313 |
| 支付宝订单码支付 | alipay@@qr_code | 0314 |
| 新版手机网站 | alipay@@h5_to_h5 | 0315 |
| 个人收款码(固定金额二维码) | person@@persons | 0400 |
| 个人收款码(通用二维码) | person@@versatile | 0401 |
| 第四方黑云平台 | fourth@@heiyun | 0500 |
| 九天 | fourth@@jiutian | 0501 |
| 百事 | fourth@@pepsi | 0502 |
| SP | fourth@@sp | 0503 |
| 益远微信客服Native | fourth@@yiyuan | 0504 |
| 小雨 | fourth@@xiaoyu | 0505 |
| 源支付 | fourth@@yuan | 0506 |
| JPAY | fourth@@jpay | 0507 |
| SPU | fourth@@spu | 0508 |
| 云闪付 | fourth@@unipay | 0509 |
| 翰银 | fourth@@hand | 0510 |
| 派拉蒙 | fourth@@plameng | 0511 |
| obl平台 | fourth@@obl | 0512 |
| 盘古 | fourth@@pangu | 0513 |
| 九霄 | fourth@@jiuxiao | 0514 |
| 艺支付 | fourth@@yi | 0515 |
| 汇付 | fourth@@huifu | 0516 |
| 快付 | fourth@@kuai | 0517 |
| 橙子 | fourth@@orange | 0518 |
| 智付 | fourth@@zhifu | 0519 |
| 大地 | fourth@@dadi | 0520 |
| 大顺 | fourth@@dashun | 0521 |
| 微信收款单 | fourth@@wx_receipt | 0522 |
| COOL-PAY | fourth@@cool | 0523 |
| 林云 | fourth@@linyun | 0524 |
| 麒麟 | fourth@@qilin | 0525 |
| XX支付 | fourth@@xx | 0526 |
| 神州 | fourth@@shenzhou | 0527 |
| 新生 | fourth@@newlife | 0528 |
| 汇付天下 | fourth@@hftx | 0529 |
| ALAIN | fourth@@alain | 0530 |
| 路易发 | fourth@@luyifa | 0531 |
| 创新 | fourth@@innovate | 0532 |
| 神明 | fourth@@shenming | 0533 |
| AdaPay | fourth@@adapay | 0534 |
| 明智 | fourth@@mingzhi | 0535 |
| 连邦 | fourth@@lianbang | 0536 |
| 杉德 | fourth@@sand | 0537 |
| GoPay | fourth@@gopay | 0538 |
| 微宝付 | fourth@@webpay | 0539 |
| 大鸟 | fourth@@bigbirld | 0540 |
| 永盛 | fourth@@yongsheng | 0541 |
| CPAY | fourth@@cpay | 0542 |
| 朱雀 | fourth@@zhuque | 0543 |
| 飞龙 | fourth@@flylong | 0544 |
| 盈联 | fourth@@yinglian | 0545 |
| Aspx | fourth@@aspx | 0546 |
| NihaoPay | fourth@@nihao | 0547 |
| 起点 | fourth@@qidian | 0548 |
| 顺 | fourth@@shun | 0549 |
| 无名 | fourth@@unknown | 0550 |
| 拉卡拉 | fourth@@lakala | 0551 |
| 雷支付 | fourth@@lei | 0552 |
| JhPay | fourth@@jhpay | 0553 |
| Tb | fourth@@tb | 0554 |
| 运维 | fourth@@yunwei | 0555 |
| 易票联 | fourth@@ypl | 0556 |
| 优米 | fourth@@yomi | 0557 |
| GATE | fourth@@gate | 0558 |
| 联调 | fourth@@joint | 0559 |
| 天天 | fourth@@tiantian | 0563 |
| BOOK | fourth@@book | 0564 |
| gatepay | fourth@@gatepay | 0565 |
| 加付宝 | fourth@@jiafubao | 0566 |
| 五金 | fourth@@wujin | 0567 |
| ADD | fourth@@add | 0568 |
| 快付通 | fourth@@quick | 0569 |
| 来钱多 | fourth@@lqd | 0570 |
| 大师姐 | fourth@@senior | 0571 |
| 九森 | fourth@@jiusen | 0572 |
| 支付宝应用授权当面付 | alipay_sub@@face_to_face | 0600 |
| 支付宝应用授权小程序支付 | alipay_sub@@mini_jsapi | 0601 |
| 支付宝应用授权h5支付 | alipay_sub@@h5 | 0602 |
| 支付宝应用授权pc支付 | alipay_sub@@pc | 0603 |
| 支付宝应用授权app支付 | alipay_sub@@app | 0604 |
| 支付宝应用授权H5预授权支付 | alipay_sub@@pre_auth | 0605 |
| 支付宝应用授权小程序预授权支付 | alipay_sub@@mini_auth | 0606 |
| 支付宝应用授权当面付预授权支付 | alipay_sub@@face_auth | 0607 |
| 支付宝应用授权当面付JSAPI支付 | alipay_sub@@face_jsapi | 0608 |
| 支付宝直付通当面付 | alipay_zft@@face_to_face | 0700 |
| 支付宝直付通h5支付 | alipay_zft@@h5 | 0701 |
| 支付宝直付通pc支付 | alipay_zft@@pc | 0702 |
| 支付宝直付通app支付 | alipay_zft@@app | 0703 |
| 支付宝直付通h5合单支付 | alipay_zft@@h5_combine | 0704 |
| 支付宝直付通app合单支付 | alipay_zft@@app_combine | 0705 |
| 支付宝直付通当面付JSAPI支付 | alipay_zft@@face_jsapi | 0706 |
| 支付宝现金红包 | alipay_zft@@red_packet | 0707 |


![画板](https://cdn.nlark.com/yuque/0/2024/jpeg/1108504/1711106531760-c74eee1f-5683-4c02-ab46-49a55fd1a587.jpeg)

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

<h3 id="cN5a2">错误码</h3>
| **code** | **message** |
| --- | --- |
| 0 | 成功 |
| 2000 | 下单失败 |


<h3 id="uLuYV">状态码</h3>
| **status** | **message** |
| --- | --- |
| success | 成功 |
| 其他 | 失败 |


