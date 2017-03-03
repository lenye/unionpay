# unionpay

中国银联接口 golang实现方法，可做后台请求银联使用

# 使用方法

```go
package main

import (
	"fmt"

	"github.com/panjjo/unionpay"
)

func main() {
	var err error
	//初始化证书
	//pfxpath和pfxpwd同时存在，privatepath和certpath同时存在，两组传任意一组皆可
	err = unionpay.LoadCert(&unionpay.CertPathInfo{

		//银联提供的pfx证书存放路径,商户私钥
		PfxPath: "/tmp/up/700000000000001_acp.pfx",

		//pfx证书密码
		PfxPwd: "000000",

		//数据加密证书路径，银联公钥
		EncryptCertPath: "/tmp/up/verify_sign_acp.cer",

		//用户私钥地址，私钥通过pfx解析得到
		// openssl pkcs12 -in xxxx.pfx -nodes -out server.pem 生成为原生格式pem 私钥
		// openssl rsa -in server.pem -out server.key  生成为rsa格式私钥文件
		PrivatePath: "/tmp/up/private.key",

		//用户证书，通过pfx解析得到
		// openssl pkcs12 -in xxxx.pfx -clcerts -nokeys -out key.cert
		CertPath: "/tmp/up/key.cert",
	})
	if err != nil {
		fmt.Println(err)
		return
	}
	//配置环境信息
	unionpay.SetConfig(&unionpay.Config{
		// 商户号
		MerId: "777290058143951",
		// 频道类型
		ChannelType: "07",
		// 商户接入类型
		AccessType: "0",
		//域名信息
		Url: "https://gateway.test.95516.com",
	})
	//创建请求渠道类型
	o, err := unionpay.NewOrder(unionpay.ApiConfig{})
	if err != nil {
		fmt.Println(err)
		return
	}
	// 调用实名认证接口
	fmt.Println(o.RealNameAuth("12aa23232", "6226388000000095", "123456jlajflafjaljf", &unionpay.CustomerInfo{
		"", "", "张三", "", "", "", "", "18100000000",
	}))
}


```
