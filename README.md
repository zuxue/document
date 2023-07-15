<a name="HT8d0"></a>
### 下单
POST<br />`domain + /collection/place/create`<br />`Content-Type: application/x-www-form-urlencoded;charset=utf-8`<br />请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| unique | string | Y | 用户唯一标识 |
| order_number | string | Y | 订单号 |
| fee | int | Y | 金额(单位:分)<br />当商户类型为person时需要能被100整除，即为正整数的**元 |
| jump_url | string | Y | 订单完成跳转url |
| notice_url | string | Y | 订单完成异步通知url |
| vendor_type | string | Y | 商户类型<br />(缩写方式见下方<br />`缩写对照表`)<br />official 公众号商户<br />alipay 支付宝<br />person 个人商户<br />fourth 第四方商户<br />alipay_sub 支付宝授权商户<br />alipay_zft 支付宝直付通商户 |
| product | string | N | 见下方 支付产品<br />(若`vendor_type`使用<br />`缩写方式`，则此参数忽略，否则必填) |
| desc | string | N | 商品描述 |
| sign | string | Y | 签名 |

<a name="yRJBg"></a>
#### 商户类型与产品对应关系
| **商户类型** | **支付产品** | **描述** |
| --- | --- | --- |
| official | jsapi | 微信jsapi 支付 |
|  | h5 | 微信h5支付(暂未开放) |
|  | native | 微信二维码支付 |
| wx_special | jsapi | 微信二级商户jsapi(服务商) |
|  | native | 微信二级商户二维码支付(服务商) |
| alipay | h5 | 支付宝h5支付 |
|  | pc | 支付宝pc页面支付 |
|  | app | 支付宝app支付 |
|  | uid | 支付宝uid支付 |
|  | face_to_face | 支付宝当面付 |
|  | app | 支付宝app支付 |
|  | jsapi | 支付宝jsapi支付 |
| person | person | 个人收款码(固定金额) |
|  | versatile | 个人收款码(通用二维码) |
| fourth | heiyun | 第四方黑云平台 |
|  | unipay | 云闪付 |
|  | jiutian | 九天 |
|  | pepsi | 百事 |
|  | sp | SP |
|  | yiyuan | 益远微信客服Native |
|  | xiaoyu | 小雨 |
|  | yuan | 源支付(个码) |
|  | spu | SPU |
|  | jpay | JPAY |
|  | hand | 翰银 |
|  | plameng | 派拉蒙 |
|  | obl | obl平台 |
|  | pangu | 盘古 |
|  | jiuxiao | 九霄 |
|  | yi | 艺支付 |
|  | huifu | 汇付 |
|  | kuai | 快付 |
|  | orange | 橙子 |
|  | zhifu | 智付 |
|  | dadi | 大地 |
|  | dashun | 大顺 |
|  | wx_receipt | 微信收款单 |
|  | cool | COOL-PAY |
|  | linyun | 林云 |
|  | qilin | 麒麟 |
|  | xx | XX支付 |
|  | shenzhou | 神州 |
|  | newlife | 新生 |
|  | hftx | 汇付天下 |
|  | alain | ALAIN |
|  | luyifa | 路易发 |
|  | innovate | 创新 |
|  | shenming | 神明 |
|  | adapay | AdaPay |
|  | mingzhi | 明智支付 |
|  | lianbang | 连邦 |
|  | sand | 杉德 |
|  | gopay | GoPay |
|  | webpay | 微宝付 |
|  | bigbirld | 大鸟 |
|  | yongsheng | 永盛 |
|  | cpay | CPAY |
|  | zhuque | 朱雀 |
|  | flylong | 飞龙 |
|  | yinglian | 盈联 |
|  | aspx | Aspx |
|  | nihao | NihaoPay |
|  | qidian | 起点 |
|  | shun | 顺 |
|  | unknown | 无名 |
|  | lakala | 拉卡拉 |
|  | lei | 雷支付 |
|  | jhpay | JhPay |
|  | tb | Tb |
|  | yunwei | 运维 |
| alipay_sub | h5 | 支付宝应用授权h5支付 |
|  | pc | 支付宝应用授权pc页面支付 |
|  | app | 支付宝应用授权app支付 |
|  | face_to_face | 支付宝应用授权当面付 |
|  | mini_program | 支付宝应用授权小程序支付 |
| alipay_zft | h5 | 支付宝直付通商户h5支付 |
|  | pc | 支付宝直付通商户pc页面支付 |
|  | app | 支付宝直付通商户app支付 |
|  | face_to_face | 支付宝直付通商户当面付 |
|  | mini_program | 支付宝直付通商户小程序支付 |

**提示：也可以使用**`**省略支付产品**`**的写法**<br />方法：`vendor_type = 商户类型 + "@@" + 支付产品`<br />使用缩写方法传入`vendor_type` 则可不传`product`，传了也会被忽略<br />`@@和数字方式二选一即可`
<a name="PvTs3"></a>
#### 缩写对照表
| **支付产品** | **简写方式1(**`**vendor_type**`**)** | **简写方式2(**`**vendor_type**`**)** |
| --- | --- | --- |
| 微信二维码支付 | official@@native | 0000 |
| 微信jsapi 支付 | official@@jsapi | 0001 |
| 微信h5支付(暂未开放) | official@@h5 | 0002 |
| 微信二级商户二维码支付 | wx_special@@native | 0100 |
| 微信二级商户jsapi支付 | wx_special@@jsapi | 0101 |
| 支付宝UID支付 | alipay@@uid | 0300 |
| 支付宝当面付 | alipay@@face_to_face | 0301 |
| 支付宝小程序支付 | alipay@@mini_program | 0302 |
| 支付宝h5支付 | alipay@@h5 | 0303 |
| 支付宝pc支付 | alipay@@pc | 0304 |
| 支付宝app支付 | alipay@@app | 0305 |
| 支付宝jsapi支付 | alipay@@jsapi | 0306 |
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
| 支付宝应用授权当面付 | alipay_sub@@face_to_face | 0600 |
| 支付宝应用授权小程序支付 | alipay_sub@@mini_program | 0601 |
| 支付宝应用授权h5支付 | alipay_sub@@h5 | 0602 |
| 支付宝应用授权pc支付 | alipay_sub@@pc | 0603 |
| 支付宝应用授权app支付 | alipay_sub@@app | 0604 |
| 支付宝应用授权jsapi支付 | alipay_sub@@jsapi | 0605 |
| 支付宝直付通当面付 | alipay_zft@@face_to_face | 0700 |
| 支付宝直付通小程序支付 | alipay_zft@@mini_program | 0701 |
| 支付宝直付通h5支付 | alipay_zft@@h5 | 0702 |
| 支付宝直付通pc支付 | alipay_zft@@pc | 0703 |
| 支付宝直付通app支付 | alipay_zft@@app | 0704 |
| 支付宝直付通jsapi支付 | alipay_zft@@jsapi | 0705 |

![](https://cdn.nlark.com/yuque/0/2023/jpeg/1108504/1689414490996-74ebbf1c-efc0-48a6-a026-0ef75703eb41.jpeg)<br />返回数据
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
<a name="ePma4"></a>
### 发起支付
跳转地址<br />可以自己拼接也可以从下单接口返回的数据中获取<br />`domain + /collection/import/{vendor_type}/{product}/{order_num}`  

| **变量名** | **说明** |
| --- | --- |
| vendor_type | 商户类型(下单时传入) |
| product | 支付产品(下单时传入) |
| order_num | 下单后返回的平台订单号 |

<a name="nuWZb"></a>
### 同步跳转
GET<br />用户支付完成后，平台会自动`302跳转`至下单时传入的`jump_url`中并携带以下参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| order_num | string | Y | 用户订单号 |
| platform_order | string | Y | 平台订单号 |
| rand_str | string | Y | 随机字符串 |
| fee | int | Y | 用户实付金额(单位:分) |
| sign | string | Y | 签名 |

<a name="GTp7M"></a>
### 订单查询
POST<br />`domain + /api/order/trade/query`<br />`Content-Type: application/x-www-form-urlencoded;charset=utf-8`<br />请求参数

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| unique | string | Y | 用户唯一标识 |
| order_num | string | N | 平台订单号 |
| outer_order | string | N | 用户订单号 |
| rand_str | string | Y | 随机字符串 |
| sign | string | Y | 签名 |

`**order_num** 与 **outer_order** 二选一即可`<br />返回数据
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
<a name="xMUbr"></a>
### 异步回调
**请求方式：**POST<br />`Content-Type: application/x-www-form-urlencoded;charset=utf-8`<br />**回调URL：**该链接是通过基础下单接口中的请求参数“notice_url”来设置的，请确保回调URL是外部可正常访问的，且不能携带后缀参数，否则可能导致商户无法接收到微信的回调通知信息。回调URL示例：“http://**.**.com/**/**”<br />**通知规则**<br />用户支付完成后，平台会把相关支付结果和用户信息发送给商户，商户需要接收处理该消息，并返回应答。<br />对后台通知交互时，如果收到商户的应答不符合规范或超时，平台认为通知失败，平台会通过一定的策略定期重新发起通知，尽可能提高通知的成功率，但不保证通知最终能成功。（通知频率为15s/15s/30s/3m/10m/20m/30m/30m/30m/60m/3h/3h/3h/6h/6h - 总计 24h4m）<br />在发送通知后接收到字符串`SUCCESS`时视为应答成功<br />**通知报文**

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| order_num | string | Y | 用户侧订单号 |
| platform_order | string | Y | 平台订单号 |
| rand_str | string | Y | 下单传入的随机字符串 |
| original_price | int | Y | 原下单金额(单位:分) |
| real_price | int | Y | 实际支付金额(单位:分) |
| order_message | string | Y | 订单状态描述 |
| order_status | string | Y | 订单状态<br />CREATE 订单创建<br />SUCCESS 已支付<br />CANCEL 已取消<br />REFUND 已退款 |
| sign | string | Y | 签名 |

<a name="Ix2gq"></a>
### 签名方法

1. 将参数名按ASCLL字典序顺序排序
2. 将参数按照key=>value键值对的形式拼接，形成一个字符串 类似 { a=b&b=c&c=d }
3. 在字符串末尾拼接{ &cert=`cert` } cert 为每个用户的 密钥
4. 将最终生成的字符串进行md5加密 得到一个32位字符串
5. 将md5值转为大写
<a name="zk8mG"></a>
#### php示例代码
```php
/**
* 签名
* @param array $arr  参与签名的数组
* @param string $cert 用户密钥
* @return string
*/
protected static function makeSign(array $arr, string $cert): string
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
<a name="cN5a2"></a>
### 错误码
| **code** | **message** |
| --- | --- |
| 0 | 成功 |
| 2000 | 下单失败 |

<a name="uLuYV"></a>
### 状态码
| **status** | **message** |
| --- | --- |
| success | 成功 |
| 其他 | 失败 |

