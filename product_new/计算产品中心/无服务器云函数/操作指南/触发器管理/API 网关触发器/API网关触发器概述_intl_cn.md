您可以通过编写 SCF 云函数来实现 Web 后端服务，并通过 API 网关对外提供服务。API 网关会将请求内容以参数形式传递给函数，并将函数返回作为响应返回给请求方。

API 网关触发器具有以下特点：
- **Push 模型**：API 网关在接受到 API 请求后，如果 API 在网关上的后端配置了对接云函数，函数将会被触发运行，同时 API 网关会将 API 请求的相关信息。例如具体接受到请求的服务和 API 规则，请求的实际路径、方法、请求的 path、header、query 等参数，全部封装为请求入参，以 event 入参的形式发送给触发的函数。
- **同步调用**：API 网关以同步调用的方式来调用函数，会在 API 网关中配置的超时时间未到前等待函数返回。有关调用类型的更多信息，请参阅 [调用类型](https://intl.cloud.tencent.com/document/product/583/9694#.E8.B0.83.E7.94.A8.E7.B1.BB.E5.9E.8B)。

## API 网关触发器配置
API 网关触发器分别支持在 **云函数控制台**或在**API网关控制台**中进行配置。
 - 在**云函数控制台**中，支持在触发方式中添加API网关触发器；支持选取已有 API 服务或新建 API 服务；支持请求方法（目前支持 **"ANY"，"GET"，"HEAD"，"POST"，"PUT"，"DELETE"** 六种方法的请求）、发布环境（测试、预发布及发布环境）及鉴权方式（API网关密钥对）的定义。
 - 在 **API 网关控制台**中配置 API 规则时，后端配置可选 Cloud Function，且在选择 Cloud Function 后，即可选择与 API 服务相同地域的云函数。在 API 网关控制台上，可以配置及管理更高阶的 API 服务，如限流计划、黑白名单等。

在 API 网关配置对接云函数时，也需要配置超时时间。API 网关中的请求超时时间和云函数的运行超时时间，两者分别生效。超时规则如下：
* API 网关超时时间 > 云函数超时时间：云函数超时先生效，API 请求响应为`200 http code`，但返回内容为云函数超时报错内容。
* API 网关超时时间 < 云函数超时时间：API 网关超时先生效，API 请求响应为`5xx http code`，标识请求超时。

## API 网关触发器绑定限制
 API 网关中，一条 API 规则仅能绑定一个云函数，但一个云函数可以被多个 API 规则绑定为后端。同时，目前 API 网关触发器仅支持同地域云函数绑定，例如广州区创建的云函数，仅支持被广州区创建的 API 服务中规则所绑定和触发。如果您想要使用特定地域的 API 网关配置来触发云函数，可以通过在对应地域下创建函数来实现。

## 请求与响应
针对 API 网关发送到云函数的请求处理方式，和云函数响应给 API 网关的返回值处理方式，称为请求方法和响应方法。请求方法和响应方法规划和实现的分别有透传方式和集成方式。

### 集成请求与透传请求
集成请求，是指 API 网关会将 HTTP 请求内容，转换为请求数据结构；请求数据结构作为函数的 event 输入参数，传递给函数并进行处理。具体请求数据结构说明见如下。

透传请求，是指 API 网关会将 HTTP 请求的 body 内容作为函数的 event 输入参数传递给函数，透传请求目前还处于规划过程中。对于透传请求，要求 HTTP 请求的 body 内容为 JSON 数据结构内容。

>当前 API 网关触发器触发云函数时，全部使用的为集成请求。

<a id="datastructures"></a>
#### API 网关触发器的集成请求事件消息结构
在 API 网关触发器接收到请求时，会将类似以下 JSON 格式的事件数据发送给绑定的云函数。
```
{
  "requestContext": {
    "serviceId": "service-f94sy04v",
    "path": "/test/{path}",
    "httpMethod": "POST",
    "requestId": "c6af9ac6-7b61-11e6-9a41-93e8deadbeef",
    "identity": {
      "secretId": "abdcdxxxxxxxsdfs"
    },
    "sourceIp": "10.0.2.14",
    "stage": "release"
  },
  "headers": {
    "Accept-Language": "en-US,en,cn",
    "Accept": "text/html,application/xml,application/json",
    "Host": "service-3ei3tii4-251000691.ap-guangzhou.apigateway.myqloud.com",
    "User-Agent": "User Agent String"
  },
  "body": "{\"test\":\"body\"}",
  "pathParameters": {
    "path": "value"
  },
  "queryStringParameters": {
    "foo": "bar"
  },
  "headerParameters":{
    "Refer": "10.0.2.14"
  },
  "stageVariables": {
    "stage": "release"
  },
  "path": "/test/value",
  "queryString": {
    "foo" : "bar",
    "bob" : "alice"
  },
  "httpMethod": "POST"
}
```

数据结构内容详细说明如下：

|    结构名    | 内容 |
| ---------- | --- |
| requestContext |  请求来源的 API 网关的配置信息、请求标识、认证信息、来源信息。其中：<li>serviceId，path，httpMethod 指向 API 网关的服务Id、API 的路径和方法；<li>stage 指向请求来源 API 所在的环境；<li>requestId 标识当前这次请求的唯一 ID；<li>identity 标识用户的认证方法和认证的信息；<li>sourceIp 标识请求来源 IP |
| path       |  记录实际请求的完整 Path 信息 |
| httpMethod | 记录实际请求的 HTTP 方法 |
| queryString | 记录实际请求的完整 Query 内容 |
| body | 记录实际请求的完整 Body 内容 |
| headers | 记录实际请求的完整 Header 内容 |
| pathParameters | 记录在 API 网关中配置过的 Path 参数以及实际取值 |
| queryStringParameters | 记录在 API 网关中配置过的 Query 参数以及实际取值 |
| headerParameters | 记录在 API 网关中配置过的 Header 参数以及实际取值 |

>- 在 API 网关迭代过程中， requestContext 内的内容可能会增加更多。目前会保证数据结构内容仅增加，不删除，不对已有结构进行破坏。
> - 实际请求时的参数数据可能会在多个位置出现，可根据业务需求选择使用。

### 集成响应与透传响应
集成响应，是指 API 网关会将云函数的返回内容进行解析，并根据解析内容构造 HTTP 响应。通过使用集成响应，可以通过代码自主控制响应的状态码、headers、body 内容，可以实现非 JSON 格式的内容响应，例如响应 XML、HTML、甚至 JS 内容。在使用集成响应时，需要按照 [API 网关触发器的集成响应返回数据结构](#apiStructure)，才可以被 API 网关成功解析，否则会出现  `{"errno":403,"error":"Invalid scf response format. please check your scf response format."}` 错误信息。

透传响应，是指 API 网关将云函数的返回内容直接传递给 API 请求方。通常这种响应的数据格式直接确定为 JSON 格式，状态码根据函数执行的状态定义，函数执行成功即为 200 状态码。通过透传响应，用户可以自行获取到 JSON 格式后在调用位置解析结构，获取结构内的内容。

>- 如果当前通过 API 网关控制台配置的 API 网关触发器，处理响应的方式默认为透传响应。如需开启集成响应，请在 API 配置中的后端配置位置，勾选**启用集成响应**，并在代码中按如下说明的数据结构返回内容。
> - 如果当前通过云函数控制台配置的 API 网关触发器，默认已开启集成响应功能，请注意返回数据的格式。

<span id="apiStructure"></span>
#### API 网关触发器的集成响应返回数据结构
在 API 网关设置为集成响应时，需要返回类似如下内容的数据结构。
```
{
    "isBase64Encoded": False,
    "statusCode": 200,
    "headers": {"Content-Type":"text/html"},
    "body": "<html><body><h1>Heading</h1><p>Paragraph.</p></body></html>"
}
```
数据结构内容详细说明如下：

|    结构名    | 内容 |
| ---------- | --- |
| isBase64Encoded |  指明 body 内的内容是否为 Base64 编码后的二进制内容，取值需要为 JSON 格式的 true 或 false |
| statusCode | HTTP 返回的状态码，取值需要为 Integer 值 |
| headers | HTTP 返回的头部内容，取值需要为多个 key-value 对象 |
| body | HTTP 返回的 body 内容 |
