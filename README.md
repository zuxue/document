为了更好的兼容性，本平台支持两种下单模式，`<font style="color:#74B602;">正常下单</font>`与`<font style="color:#117CEE;">仿易支付下单</font>`

使用不同的下单方式需要使用`<font style="color:#DF2A3F;">相对应的加签方式</font>`，具体签名方式见`<font style="color:#DF2A3F;">文档底部</font>`

<h3 id="HT8d0"><font style="color:#74B602;">正常下单</font></h3>
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
| vendor_type | string | Y | 商户类型<br/>(缩写方式见下方<br/>`<font style="color:#FFFFFF;background-color:#DF2A3F;">缩写对照表</font>`)<br/>official 公众号商户<br/>alipay 支付宝<br/>person 个人商户<br/>fourth 第四方商户<br/>alipay_sub 支付宝授权商户<br/>alipay_zft 支付宝直付通商户 |
| product | string | N | 见下方 支付产品<br/>(若`<font style="color:#FFFFFF;background-color:#DF2A3F;">vendor_type</font>`使用`<font style="color:#FFFFFF;background-color:#DF2A3F;">缩写方式</font>`，则此参数忽略，否则必填) |
| desc | string | N | 商品描述 |
| devide | string | N | iphone | android 用户设备类型 |
| sign | string | Y | 签名 |


<h3 id="tBbFo"><font style="color:#117CEE;">仿易支付下单(使用form表单提交)</font></h3>
POST

`domain + /submit`

`Content-Type: application/x-www-form-urlencoded`

请求参数

`仅下表中的参数参与签名，其余参数均<font style="color:#DF2A3F;">不</font>参与签名`

| **参数名** | **参数类型** | **是否必填** | **示例、说明** |
| --- | --- | --- | --- |
| pid | string | Y | 用户唯一标识 |
| out_trade_no | string | Y | 订单号 |
| money | float | Y | 金额(单位:元) |
| return_url | string | N | 订单完成跳转url |
| notify_url | string | Y | 订单完成异步通知url |
| type | string | Y | 商户类型<br/>(见下方`<font style="color:#FFFFFF;background-color:#DF2A3F;">缩写对照表</font>`) |
| name | string | N | 商品描述 |
| ip | string | N | IP |
| sign | string | Y | 签名 |


**<font style="color:#DF2A3F;">提示：使用</font>**`**<font style="color:#DF2A3F;">省略支付产品</font>**`**<font style="color:#DF2A3F;">的写法</font>**

方法：`vendor_type = 商户类型 + "@@" + 支付产品`

使用缩写方法传入`vendor_type` 则可`<font style="color:#DF2A3F;">不传</font>``product`，传了也会被忽略

`@@和数字方式<font style="color:#DF2A3F;">二选一</font>即可`

<h4 id="PvTs3">缩写对照表</h4>
| **支付产品** | **简写方式1(**`**vendor_type**`**)** | **简写方式2(**`**vendor_type**`**)** |
| --- | --- | --- |
| <font style="color:#FFFFFF;">微信二维码支付</font> | official@@native | 0000 |
| <font style="color:#FFFFFF;">微信jsapi 支付</font> | official@@jsapi | 0001 |
| <font style="color:#FFFFFF;">微信h5支付(暂未开放)</font> | official@@h5 | 0002 |
| <font style="color:#FFFFFF;">微信APP支付(限IOS)</font> | official@@app | 0003 |
| <font style="color:#FFFFFF;">微信二级商户二维码支付</font> | wx_special@@native | 0100 |
| <font style="color:#FFFFFF;">微信二级商户jsapi支付</font> | wx_special@@jsapi | 0101 |
| <font style="color:#FFFFFF;">支付宝UID支付</font> | alipay@@uid | 0300 |
| <font style="color:#FFFFFF;">支付宝当面付</font> | alipay@@face_to_face | 0301 |
| <font style="color:#FFFFFF;">支付宝小程序JSAPI支付</font> | alipay@@mini_jsapi | 0302 |
| <font style="color:#FFFFFF;">支付宝h5支付</font> | alipay@@h5 | 0303 |
| <font style="color:#FFFFFF;">支付宝pc支付</font> | alipay@@pc | 0304 |
| <font style="color:#FFFFFF;">支付宝app支付</font> | alipay@@app | 0305 |
| <font style="color:#FFFFFF;">支付宝小程序预授权</font> | alipay@@mini_auth | 0306 |
| <font style="color:#FFFFFF;">支付宝H5预授权</font> | alipay@@pre_auth | 0307 |
| <font style="color:#FFFFFF;">支付宝商家扣款</font> | alipay@@agreement | 0308 |
| <font style="color:#FFFFFF;">支付宝UID红包</font> | alipay@@red_packet | 0309 |
| <font style="color:#FFFFFF;">支付宝当面付预授权</font> | alipay@@face_auth | 0310 |
| <font style="color:#FFFFFF;">支付宝生活号JSAPI支付</font> | alipay@@life_jsapi | 0311 |
| <font style="color:#FFFFFF;">支付宝当面付JSAPI支付</font> | alipay@@face_jsapi | 0312 |
| <font style="color:#FFFFFF;">支付宝当面付转手机网站支付</font> | alipay@@face_to_h5 | 0313 |
| <font style="color:#FFFFFF;">支付宝订单码支付</font> | alipay@@qr_code | 0314 |
| <font style="color:#FFFFFF;">新版手机网站</font> | alipay@@h5_to_h5 | 0315 |
| <font style="color:#FFFFFF;">个人收款码(固定金额二维码)</font> | person@@persons | 0400 |
| <font style="color:#FFFFFF;">个人收款码(通用二维码)</font> | person@@versatile | 0401 |
| <font style="color:#FFFFFF;">第四方黑云平台</font> | fourth@@heiyun | 0500 |
| <font style="color:#FFFFFF;">九天</font> | fourth@@jiutian | 0501 |
| <font style="color:#FFFFFF;">百事</font> | fourth@@pepsi | 0502 |
| <font style="color:#FFFFFF;">SP</font> | fourth@@sp | 0503 |
| <font style="color:#FFFFFF;">益远微信客服Native</font> | fourth@@yiyuan | 0504 |
| <font style="color:#FFFFFF;">小雨</font> | fourth@@xiaoyu | 0505 |
| <font style="color:#FFFFFF;">源支付</font> | fourth@@yuan | 0506 |
| <font style="color:#FFFFFF;">JPAY</font> | fourth@@jpay | 0507 |
| <font style="color:#FFFFFF;">SPU</font> | fourth@@spu | 0508 |
| <font style="color:#FFFFFF;">云闪付</font> | fourth@@unipay | 0509 |
| <font style="color:#FFFFFF;">翰银</font> | fourth@@hand | 0510 |
| <font style="color:#FFFFFF;">派拉蒙</font> | fourth@@plameng | 0511 |
| <font style="color:#FFFFFF;">obl平台</font> | fourth@@obl | 0512 |
| <font style="color:#FFFFFF;">盘古</font> | fourth@@pangu | 0513 |
| <font style="color:#FFFFFF;">九霄</font> | fourth@@jiuxiao | 0514 |
| <font style="color:#FFFFFF;">艺支付</font> | fourth@@yi | 0515 |
| <font style="color:#FFFFFF;">汇付</font> | fourth@@huifu | 0516 |
| <font style="color:#FFFFFF;">快付</font> | fourth@@kuai | 0517 |
| <font style="color:#FFFFFF;">橙子</font> | fourth@@orange | 0518 |
| <font style="color:#FFFFFF;">智付</font> | fourth@@zhifu | 0519 |
| <font style="color:#FFFFFF;">大地</font> | fourth@@dadi | 0520 |
| <font style="color:#FFFFFF;">大顺</font> | fourth@@dashun | 0521 |
| <font style="color:#FFFFFF;">微信收款单</font> | fourth@@wx_receipt | 0522 |
| <font style="color:#FFFFFF;">COOL-PAY</font> | fourth@@cool | 0523 |
| <font style="color:#FFFFFF;">林云</font> | fourth@@linyun | 0524 |
| <font style="color:#FFFFFF;">麒麟</font> | fourth@@qilin | 0525 |
| <font style="color:#FFFFFF;">XX支付</font> | fourth@@xx | 0526 |
| <font style="color:#FFFFFF;">神州</font> | fourth@@shenzhou | 0527 |
| <font style="color:#FFFFFF;">新生</font> | fourth@@newlife | 0528 |
| <font style="color:#FFFFFF;">汇付天下</font> | fourth@@hftx | 0529 |
| <font style="color:#FFFFFF;">ALAIN</font> | fourth@@alain | 0530 |
| <font style="color:#FFFFFF;">路易发</font> | fourth@@luyifa | 0531 |
| <font style="color:#FFFFFF;">创新</font> | fourth@@innovate | 0532 |
| <font style="color:#FFFFFF;">神明</font> | fourth@@shenming | 0533 |
| <font style="color:#FFFFFF;">AdaPay</font> | fourth@@adapay | 0534 |
| <font style="color:#FFFFFF;">明智</font> | fourth@@mingzhi | 0535 |
| <font style="color:#FFFFFF;">连邦</font> | fourth@@lianbang | 0536 |
| <font style="color:#FFFFFF;">杉德</font> | fourth@@sand | 0537 |
| <font style="color:#FFFFFF;">GoPay</font> | fourth@@gopay | 0538 |
| <font style="color:#FFFFFF;">微宝付</font> | fourth@@webpay | 0539 |
| <font style="color:#FFFFFF;">大鸟</font> | fourth@@bigbirld | 0540 |
| <font style="color:#FFFFFF;">永盛</font> | fourth@@yongsheng | 0541 |
| <font style="color:#FFFFFF;">CPAY</font> | fourth@@cpay | 0542 |
| <font style="color:#FFFFFF;">朱雀</font> | fourth@@zhuque | 0543 |
| <font style="color:#FFFFFF;">飞龙</font> | fourth@@flylong | 0544 |
| <font style="color:#FFFFFF;">盈联</font> | fourth@@yinglian | 0545 |
| <font style="color:#FFFFFF;">Aspx</font> | fourth@@aspx | 0546 |
| <font style="color:#FFFFFF;">NihaoPay</font> | fourth@@nihao | 0547 |
| <font style="color:#FFFFFF;">起点</font> | fourth@@qidian | 0548 |
| <font style="color:#FFFFFF;">顺</font> | fourth@@shun | 0549 |
| <font style="color:#FFFFFF;">无名</font> | fourth@@unknown | 0550 |
| <font style="color:#FFFFFF;">拉卡拉</font> | fourth@@lakala | 0551 |
| <font style="color:#FFFFFF;">雷支付</font> | fourth@@lei | 0552 |
| <font style="color:#FFFFFF;">JhPay</font> | fourth@@jhpay | 0553 |
| <font style="color:#FFFFFF;">Tb</font> | fourth@@tb | 0554 |
| <font style="color:#FFFFFF;">运维</font> | fourth@@yunwei | 0555 |
| <font style="color:#FFFFFF;">易票联</font> | fourth@@ypl | 0556 |
| <font style="color:#FFFFFF;">优米</font> | fourth@@yomi | 0557 |
| <font style="color:#FFFFFF;">GATE</font> | fourth@@gate | 0558 |
| <font style="color:#FFFFFF;">联调</font> | fourth@@joint | 0559 |
| <font style="color:#FFFFFF;">天天</font> | fourth@@tiantian | 0563 |
| <font style="color:#FFFFFF;">BOOK</font> | fourth@@book | 0564 |
| <font style="color:#FFFFFF;">gatepay</font> | fourth@@gatepay | 0565 |
| <font style="color:#FFFFFF;">加付宝</font> | fourth@@jiafubao | 0566 |
| <font style="color:#FFFFFF;">五金</font> | fourth@@wujin | 0567 |
| <font style="color:#FFFFFF;">ADD</font> | fourth@@add | 0568 |
| <font style="color:#FFFFFF;">快付通</font> | fourth@@quick | 0569 |
| <font style="color:#FFFFFF;">来钱多</font> | fourth@@lqd | 0570 |
| <font style="color:#FFFFFF;">大师姐</font> | fourth@@senior | 0571 |
| <font style="color:#FFFFFF;">九森</font> | fourth@@jiusen | 0572 |
| <font style="color:#FFFFFF;">支付宝应用授权当面付</font> | alipay_sub@@face_to_face | 0600 |
| <font style="color:#FFFFFF;">支付宝应用授权小程序支付</font> | alipay_sub@@mini_jsapi | 0601 |
| <font style="color:#FFFFFF;">支付宝应用授权h5支付</font> | alipay_sub@@h5 | 0602 |
| <font style="color:#FFFFFF;">支付宝应用授权pc支付</font> | alipay_sub@@pc | 0603 |
| <font style="color:#FFFFFF;">支付宝应用授权app支付</font> | alipay_sub@@app | 0604 |
| <font style="color:#FFFFFF;">支付宝应用授权H5预授权支付</font> | alipay_sub@@pre_auth | 0605 |
| <font style="color:#FFFFFF;">支付宝应用授权小程序预授权支付</font> | alipay_sub@@mini_auth | 0606 |
| <font style="color:#FFFFFF;">支付宝应用授权当面付预授权支付</font> | alipay_sub@@face_auth | 0607 |
| <font style="color:#FFFFFF;">支付宝应用授权当面付JSAPI支付</font> | alipay_sub@@face_jsapi | 0608 |
| <font style="color:#FFFFFF;">支付宝直付通当面付</font> | alipay_zft@@face_to_face | 0700 |
| <font style="color:#FFFFFF;">支付宝直付通h5支付</font> | alipay_zft@@h5 | 0701 |
| <font style="color:#FFFFFF;">支付宝直付通pc支付</font> | alipay_zft@@pc | 0702 |
| <font style="color:#FFFFFF;">支付宝直付通app支付</font> | alipay_zft@@app | 0703 |
| <font style="color:#FFFFFF;">支付宝直付通h5合单支付</font> | alipay_zft@@h5_combine | 0704 |
| <font style="color:#FFFFFF;">支付宝直付通app合单支付</font> | alipay_zft@@app_combine | 0705 |
| <font style="color:#FFFFFF;">支付宝直付通当面付JSAPI支付</font> | alipay_zft@@face_jsapi | 0706 |
| <font style="color:#FFFFFF;">支付宝现金红包</font> | alipay_zft@@red_packet | 0707 |


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
**<font style="color:rgb(51, 51, 51);">请求方式：</font>**<font style="color:rgb(51, 51, 51);">POST</font>

`Content-Type: application/x-www-form-urlencoded;charset=utf-8`

**<font style="color:rgb(51, 51, 51);">回调URL：</font>**<font style="color:rgb(51, 51, 51);">该链接是通过基础下单接口中的请求参数“</font>notice_url<font style="color:rgb(51, 51, 51);">”来设置的，请确保回调URL是外部可正常访问的，且不能携带后缀参数，否则可能导致商户无法接收到微信的回调通知信息。回调URL示例：“http://**.**.com/**/**”</font>

**<font style="color:rgb(51, 51, 51);">通知规则</font>**

<font style="color:rgb(51, 51, 51);">用户支付完成后，平台会把相关支付结果和用户信息发送给商户，商户需要接收处理该消息，并返回应答。</font>

<font style="color:rgb(51, 51, 51);">对后台通知交互时，如果收到商户的应答不符合规范或超时，平台认为通知失败，平台会通过一定的策略定期重新发起通知，尽可能提高通知的成功率，但不保证通知最终能成功。（通知频率为15s/15s/30s/3m/10m/20m/30m/30m/30m/60m/3h/3h/3h/6h/6h - 总计 24h4m）</font>

<font style="color:rgb(51, 51, 51);">在发送通知后接收到字符串</font>`<font style="color:#DF2A3F;">SUCCESS</font>`时视为应答成功

**<font style="color:rgb(51, 51, 51);">通知报文</font>**

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

<h4 id="zk8mG">php示例代码</h4>
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

<h3 id="j90qf">仿易支付签名方法</h3>
1. 将参数名按ASCLL字典序顺序排序
2. 将参数按照key=>value键值对的形式拼接，形成一个字符串 类似 { a=b&b=c&c=d }
3. 在字符串末尾拼接{ cert } cert 为每个用户的 密钥
4. 将最终生成的字符串进行md5加密 得到一个32位字符串

<h4 id="VjcOO">php示例代码</h4>
```php
/**
  * 仿易支付签名
  * @param array $arr  参与签名的数组
  * @param string $cert 用户密钥
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


