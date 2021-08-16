# 什么是Open Api

>API的全称是应用编程接口（Application Programming Interface），把服务封装成一系列计算机
易识别的数据接口开放出去，供第三方开发者使用，这种行为就叫做开放API，与之对应的，所开放的API就被称作openAPI。

# 如何设计
设计时需要考虑几个问题。
- 请求的合法性
- 请求是否被篡改
- 重复请求

如何保证合法性呢，事先服务端给客户端指定的一个标识，要求每次请求都带上该参数，标明身份。另外

# 客户端请求服务端时携带的参数

| 参数 | 描述 | 数据来源 | 作用 |
| --- | --- | --- | --- |
| `appId` | 应用程序id | 服务端授权颁发 | 验证合法性 |
| `appSecert` | 应用程序密钥 | 服务端授权颁发 | 用于进行签名 |
| `timestamp` | 时间戳 | 当前客户端的时间 | （不考虑时差问题）约定本次请求的有效期，指定时间内有效，否则不予反馈 |
| `nonce` | 随机数 | 当前客户端随机生成的数 | 约定时间内保证有充足的随机数产生，若存在重复将不予反馈 |
| `sign` | 签名值 | 签名值 = 约定处理（约定处理（请求参数） + 密钥 + 随机数 + 时间戳） | 服务端用同样的方式进行处理后（签名值） == 客户方（签名值），false不予反馈 |

# 具体实现
```java

import java.security.MessageDigest;

public class MD5Util {

    /**
     * 获取md5
     *
     * @param message 消息
     * @return {@link String}
     */
    public static String getMD5(String message) {
        String md5 = "";
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] messageByte = message.getBytes("UTF-8");
            byte[] md5Byte = md.digest(messageByte);
            md5 = bytesToHex(md5Byte);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return md5;
    }


    /**
     * 二进制转十六进制
     *
     * @param bytes 字节
     * @return {@link String}
     */
    public static String bytesToHex(byte[] bytes) {
        StringBuffer hexStr = new StringBuffer();
        int num;
        for (int i = 0; i < bytes.length; i++) {
            num = bytes[i];
            if(num < 0) {
                num += 256;
            }
            if(num < 16){
                hexStr.append("0");
            }
            hexStr.append(Integer.toHexString(num));
        }
        return hexStr.toString().toUpperCase();
    }
}
```