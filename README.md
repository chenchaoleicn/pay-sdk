# pay-sdk
## 背景
在支付宝、银联、微信等支付平台接入时，支付宝的SDK包最简洁，其内部设计具有良好的扩展性。
在支付宝SDK包原设计上，增加了定制功能(对称加密、非对称加密、数据校验、http链路跟踪等)，并对其核心进行了重构。
为了在设计多个SDK时进行代码复用，将其核心功能拆分出来，即为sdk-core项目 https://github.com/chenchaoleicn/sdk-core.git ，
将其业务相关部分拆分出来，即为当前项目，项目中涉及的业务数据来自于支付宝开放平台api https://docs.open.alipay.com/api/
## 使用
下文中提到的IThirdpayClient等类来自于sdk-core项目 https://github.com/chenchaoleicn/sdk-core.git
```
    IThirdpayClient client = new DefaultThirdpayClient(
            "<your gateway url>",
            "<your merchant no>",
            ThirdpayConstants.RSA_TYPE_V1,
            "<your merchant private key>",
            "<pay platform public key>");
    // 创建API对应的request
    ThirdpayTradePagePayRequest request = new ThirdpayTradePagePayRequest();
    ThirdpayTradePagePayModel requestModel = new ThirdpayTradePagePayModel();
    requestModel.setOutTradeNo("12345678");
    requestModel.setProductCode("1");
    requestModel.setTotalAmount("100");
    requestModel.setSubject("订单主题");
    requestModel.setBody("订单描述");
    requestModel.setTimeoutExpress("1c");
    requestModel.setReturnUrl("<your return url>");
    requestModel.setNotifyUrl("<your notify url>");
    
    ThirdpayTradePagePayResponse response = null;
    try {
        response = client.execute(request);
    } catch (ThirdpayApiException e) {
        throw new RuntimeException(e);
    }
    if (!response.isSuccess()) {
        System.out.println("调用失败"
                + ", 响应码:" + response.getCode() + ", 响应信息:" + response.getMsg()
                + ", 响应子码:" + response.getSubCode() + ", 响应子信息:" + response.getSubMsg()
        );
    }
    // todo 使用response处理业务
```
## 贡献
欢迎提交代码，有问题可以建```issue```。
