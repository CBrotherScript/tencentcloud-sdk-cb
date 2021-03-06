# 简介
这是一个用CBrother脚本实现的腾讯云SDK，参照腾讯云官方其他语言版本SDK进行的封装，方便CB开发者对接腾讯云后台。
这个sdk是CB开发者自发提供和维护的，所以实现中用到哪里写到哪里。因为有些功能的测试存场景需要提前申请资质所以暂时空缺，如果你使时发现某项功能本sdk没有支持，可以联系作者添加，如果你能提供测试环境那是再好不过。

# 使用SDK
下载源码放到你的工程根目录下即可

# 示例
## 发送短信(sms)
```javascript
import ./common/Credential
import ./common/ClientProfile
import ./sms/models/SendSmsRequest
import ./sms/SmsClient

/*可选项，暂不支持，后续扩展
const BUCKET_NAME = "BUCKET_NAME";
const COS_REGION = "REGION";*/

const SECRET_ID = "************";
const SECRET_KEY = "***********";

/**
 ** 发送短信
 */
function main() {
    /* 必要步骤：
     * 实例化一个认证对象，入参需要传入腾讯云账户密钥对secretId，secretKey。
    */
    var cred = new Credential(SECRET_ID, SECRET_KEY);
   
    /* 非必要步骤:
    * 实例化一个客户端配置对象 */
    var profile = new ClientProfile();
    profile.setSignMethod("HmacSHA1");    // 设置签名方式
    profile.setReqMethod("GET");

    var smsRequst = new SendSmsRequest();

    /* 短信应用ID */
    smsRequst.setSmsSdkAppid("*********");

    /* 短信签名内容: 使用 UTF-8 编码，必须填写已审核通过的签名 */
    smsRequst.setSign("腾讯云");

    /* 模板 ID */
    smsRequst.setTemplateID("*******");

    /* 下发手机号码，采用 e.164 标准 */
    var phoneNumberSet_ = new Array();
    phoneNumberSet_.add("+868621212313123");
    smsRequst.setPhoneNumberSet(phoneNumberSet_);

    /* 模板参数: 若无模板参数，则设置为空*/
    var templateParamSet_ = ["654321"];
    smsRequst.setTemplateParamSet(templateParamSet_);

    var client = new SmsClient(cred, "", profile);
    
    /* 通过 client 对象调用 SendSms 方法发起请求。注意请求方法名与请求对象是对应的
    返回的 res 是一个 SendSmsResponse 类的实例，与请求对象对应 */
    var sendSmsResponse = client.SendSms(smsRequst);

    //获取SendStatus
    var sendstatus = sendSmsResponse.getSendStatusSet();

    // 也可以取出单个值，你可以通过官网接口文档或跳转到response对象的定义处查看返回字段的定义
    print res.getRequestId();
}
```
