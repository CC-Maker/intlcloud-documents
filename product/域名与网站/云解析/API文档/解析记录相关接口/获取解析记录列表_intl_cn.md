## 1. 接口描述
本接口(RecordList)用于获取域名的解析记录。
接口请求域名：<font style="color:red">cns.api.qcloud.com</font>

## 2. 输入参数
以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见 [公共请求参数](https://intl.cloud.tencent.com/document/api/377/4153) 页面。其中，此接口的 Action 字段为 RecordList。

| 参数名称 | 是否必选  | 类型 | 描述 |
|---------|---------|---------|---------|
| domain | 是 | String | 要操作的域名（主域名，不包括 www，例如：qcloud.com） |
| offset | 否 | Int | 偏移量，默认为0。关于offset的更进一步介绍参考 API 简介中的相关小节。 |
| length | 否 | Int | 返回数量，默认 20，最大值 100。关于limit的更进一步介绍参考 API 简介中的相关小节。 |
| subDomain | 否 | String | （过滤条件）根据子域名进行过滤 |
| recordType | 否 | String | （过滤条件）根据记录类型进行过滤
| qProjectId | 否 | Int | （过滤条件）项目ID |

## 3. 输出参数
| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code | Int | 公共错误码, 0表示成功，其他值表示失败。 |
| message | String | 模块错误信息描述，与接口相关。|
| data | Array | API返回的数据。 |

data 字段里面包含了域名的信息、解析记录的条数信息，以及解析记录的详情，其中，

domain 为域名的信息，描述为：

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| id    |  String | 域名的ID  |
| name  | String | 域名 |
| punycode | String | punycode编码后的域名 |
| grade | String | 域名的等级 |
| owner | String | 域名所有者的邮箱帐号 |
| ext_status | String | 域名扩展的状态信息，"notexist"、"dnserror"、"" 分别代表"域名未注册"、"DNS设置错误"、"正常" |
| ttl | Int | 域名下的解析记录默认的 TTL 值 |
| min_ttl | Int | 当前域名允许的最小的 TTL |
| dnspod_ns | Array | 域名应该设置的 NS 地址 |
| status | String | 域名的状态，"enable"、"pause"、"spam"、"lock" 分别代表"正常"、"暂停解析"、"已被封禁"、"已锁定" |
| q_project_id | Int | 域名所在项目的ID |


info 为该域名解析记录的条数信息，字段描述为：

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| sub_domains | String | 解析记录的总数 |
| record_total | String | 有过滤条件，当前实际返回的记录条数 |

records 包含每一条解析记录的详细信息，字段描述为：

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| id | Int | 解析记录的ID
| ttl | Int | 记录的 TTL 值 |
| value | String | 记录的值 |
| enabled | Int | 记录的暂停、启用状态，1 和 0 分别代表启用和暂停 |
| updated_on | String | 解析记录的最后修改时间 |
| q_project_id | Int | 解析记录所属的项目ID |
| name | String | 子域名 |
| line | String |解析记录的线路名称 |
| line_id| String | 解析记录的线路编号 |
| type | String | 解析记录的类型 |
| remark | String | 解析记录的备注信息 |
| mx | Int | MX 记录的优先级，非 MX 记录的话，该值为 0 |


## 4. 示例
```
https://cns.api.qcloud.com/v2/index.php?
&<公共请求参数>
&Action=RecordList
&offset=0
&length=20
&domain=qq.com
```

返回示例如下：

```
{
	"code": 0,
	"message": "",
	"codeDesc": "Success",
	"data": {
		"domain": {
			"id": "55309561",
			"name": "yizeroapitest.com",
			"punycode": "yizeroapitest.com",
			"grade": "DP_Free",
			"owner": "909619400@qq.com",
			"ext_status": "dnserror",
			"ttl": 600,
			"min_ttl": 600,
			"dnspod_ns": ["f1g1ns1.dnspod.net", "f1g1ns2.dnspod.net"],
			"status": "enable",
			"q_project_id": 0
		},
		"info": {
			"sub_domains": "4",
			"record_total": "4"
		},
		"records": [
            {
				"id": 281628246,
				"ttl": 86400,
				"value": "f1g1ns1.dnspod.net.",
				"enabled": 1,
				"status": "enabled",
				"updated_on": "2017-02-20 10:15:47",
				"q_project_id": 0,
				"name": "@",
				"line": "\u9ed8\u8ba4",
				"line_id": "0",
				"type": "NS",
				"remark": "",
				"mx": 0,
				"hold": "hold"
			},
            {
				"id": 281628247,
				"ttl": 86400,
				"value": "f1g1ns2.dnspod.net.",
				"enabled": 1,
				"status": "enabled",
				"updated_on": "2017-02-20 10:15:47",
				"q_project_id": 0,
				"name": "@",
				"line": "\u9ed8\u8ba4",
				"line_id": "0",
				"type": "NS",
				"remark": "",
				"mx": 0,
				"hold": "hold"
			},
            {
				"id": 281817767,
				"ttl": 600,
				"value": "119.29.18.115",
				"enabled": 1,
				"status": "enabled",
				"updated_on": "2017-02-20 20:00:39",
				"q_project_id": 0,
				"name": "www",
				"line": "\u9ed8\u8ba4",
				"line_id": "0",
				"type": "A",
				"remark": "",
				"mx": 0
			},
            {
				"id": 281817768,
				"ttl": 600,
				"value": "203.195.160.18",
				"enabled": 1,
				"status": "enabled",
				"updated_on": "2017-02-20 20:00:39",
				"q_project_id": 0,
				"name": "www",
				"line": "\u9ed8\u8ba4",
				"line_id": "0",
				"type": "A",
				"remark": "",
				"mx": 0
			}
		]
	}
}
```
