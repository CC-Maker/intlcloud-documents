## 简介

您可以通过对象存储控制台，对存放在存储桶中的对象设置加密，以防止信息被泄露。关于加密的更多信息，请参见 [服务端加密概述](https://intl.cloud.tencent.com/document/product/436/18145)。下面将为您详细介绍如何设置对象加密。

>
> - 访问加密对象与访问未加密的对象在体验上并无差别，但前提是您已拥有该对象的访问权限。
> - 服务端加密仅加密对象数据而不加密对象元数据，而且使用服务端加密的对象必须使用有效签名访问，不可被匿名用户访问。
> - 在列出存储桶中对象时，列表会返回所有对象的列表，无论对象是否加密。

## 操作步骤

1. 登录 [对象存储控制台](https://console.cloud.tencent.com/cos5)。
2. 在左侧导航栏中，单击【存储桶列表】。
3. 选择需要添加存储桶策略的存储桶，进入存储桶详情页。
![](https://main.qcloudimg.com/raw/156823c7ad23708feb85f5d682c61d50.png)
4. 单击【文件列表】，找到您要设置加密的对象，在其右侧操作栏中，单击【详情】。
![](https://main.qcloudimg.com/raw/6065855393a30c809d60b4d0027628a6.png)
5. 下拉页面找到【服务端加密】配置项，选择对应的加密方式，目前支持以下两种加密方式：
 - SSE-COS：由 COS 托管密钥的服务端加密方式，更多关于 SSE-COS 的信息，请参见 [服务端加密概述：SSE-COS](https://intl.cloud.tencent.com/document/product/436/18145)。
 - SSE-KMS：由腾讯云密钥管理系统 KMS 托管密钥的服务端加密方式，可选择使用默认密钥或自建密钥。关于密钥信息，可参见 [创建 KMS 密钥](https://cloud.tencent.com/document/product/573/8875)， 关于 SSE-KMS 的更多信息，请参见 [服务端加密概述：SSE-KMS](https://intl.cloud.tencent.com/document/product/436/18145)。
![](https://main.qcloudimg.com/raw/48c8b3a3d32170db7172397398c8a883.png)

>
> - 如您首次使用 SSE-KMS 加密，需要 [开通 KMS 服务](https://buy.cloud.tencent.com/kms)。
> - 目前 SSE-KMS 加密，仅支持北京、上海、广州地域。
