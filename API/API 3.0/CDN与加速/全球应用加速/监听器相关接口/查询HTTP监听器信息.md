## 1. 接口描述

接口请求域名： gaap.tencentcloudapi.com 。

该接口（DescribeHTTPListeners）用来查询HTTP监听器信息。

默认接口请求频率限制：20次/秒。

## 2. 输入参数

以下请求参数列表仅列出了接口请求参数和部分公共参数，完整公共参数列表见 [公共请求参数](/document/api/608/36935)。

| 参数名称 | 必选 | 类型 | 描述 |
|---------|---------|---------|---------|
| Action | 是 | String | 公共参数，本接口取值：DescribeHTTPListeners |
| Version | 是 | String | 公共参数，本接口取值：2018-05-29 |
| Region | 否 | String | 公共参数，本接口不需要传递此参数。 |
| ProxyId | 否 | String | 通道ID |
| ListenerId | 否 | String | 过滤条件，按照监听器ID进行精确查询 |
| ListenerName | 否 | String | 过滤条件，按照监听器名称进行精确查询 |
| Port | 否 | Integer | 过滤条件，按照监听器端口进行精确查询 |
| Offset | 否 | Integer | 偏移量，默认为0 |
| Limit | 否 | Integer | 限制数量，默认为20个 |
| SearchValue | 否 | String | 过滤条件，支持按照端口或监听器名称进行模糊查询，该参数不能与ListenerName和Port同时使用 |

## 3. 输出参数

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| TotalCount | Integer | 监听器数量|
| ListenerSet | Array of [HTTPListener](/document/api/608/37023#HTTPListener) | HTTP监听器列表|
| RequestId | String | 唯一请求 ID，每次请求都会返回。定位问题时需要提供该次请求的 RequestId。|

## 4. 示例

### 示例1 查询HTTP监听器信息

查询HTTP监听器信息

#### 输入示例

```
https://gaap.tencentcloudapi.com/?Action=DescribeHTTPListeners
&ProxyId=link-n9ha8jzl
&Offset=0
&Limit=20
&<公共请求参数>
```

#### 输出示例

```
{
  "Response": {
    "TotalCount": 4,
    "ListenerSet": [
      {
        "Protocol": "HTTP",
        "ListenerId": "listener-4e99n24b",
        "Port": 80,
        "ListenerStatus": 0,
        "ListenerName": "http80",
        "CreateTime": 1563335063
      },
      {
        "Protocol": "HTTP",
        "ListenerId": "listener-jmi21mrf",
        "Port": 8080,
        "ListenerStatus": 0,
        "ListenerName": "http8080",
        "CreateTime": 1564590945
      },
      {
        "Protocol": "HTTP",
        "ListenerId": "listener-1grazsz5",
        "Port": 18080,
        "ListenerStatus": 0,
        "ListenerName": "http18080",
        "CreateTime": 1564590963
      },
      {
        "Protocol": "HTTP",
        "ListenerId": "listener-l6z66uqb",
        "Port": 28080,
        "ListenerStatus": 0,
        "ListenerName": "http28080",
        "CreateTime": 1564640821
      }
    ],
    "RequestId": "4c43a7f7-3527-4e2a-9998-bbd363e4b095"
  }
}
```


## 5. 开发者资源

### API Explorer

**该工具提供了在线调用、签名验证、SDK 代码生成和快速检索接口等能力，能显著降低使用云 API 的难度，推荐使用。**

* [API 3.0 Explorer](https://console.cloud.tencent.com/api/explorer?Product=gaap&Version=2018-05-29&Action=DescribeHTTPListeners)

### SDK

云 API 3.0 提供了配套的开发工具集（SDK），支持多种编程语言，能更方便的调用 API。

* [Tencent Cloud SDK 3.0 for Python](https://github.com/TencentCloud/tencentcloud-sdk-python)
* [Tencent Cloud SDK 3.0 for Java](https://github.com/TencentCloud/tencentcloud-sdk-java)
* [Tencent Cloud SDK 3.0 for PHP](https://github.com/TencentCloud/tencentcloud-sdk-php)
* [Tencent Cloud SDK 3.0 for Go](https://github.com/TencentCloud/tencentcloud-sdk-go)
* [Tencent Cloud SDK 3.0 for NodeJS](https://github.com/TencentCloud/tencentcloud-sdk-nodejs)
* [Tencent Cloud SDK 3.0 for .NET](https://github.com/TencentCloud/tencentcloud-sdk-dotnet)

### 命令行工具

* [Tencent Cloud CLI 3.0](https://cloud.tencent.com/document/product/440/6176)

## 6. 错误码

以下仅列出了接口业务逻辑相关的错误码，其他错误码详见 [公共错误码](/document/api/608/36938#.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81)。

| 错误码 | 描述 |
|---------|---------|
| FailedOperation | 操作失败 |
| FailedOperation.NotSupportOldVersionProxy | 仅支持Version2.0的通道。 |
| FailedOperation.NotSupportProxyGroup | 该操作不支持通道组。 |
| InternalError | 内部错误 |
| InvalidParameter | 参数错误 |
| InvalidParameterValue | 参数取值错误 |
| MissingParameter | 缺少参数错误 |
| ResourceNotFound | 资源不存在 |
| UnauthorizedOperation | 未授权操作 |
| UnknownParameter | 未知参数错误 |
