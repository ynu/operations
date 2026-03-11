---
name: "graduate-query"
description: "Query graduate student basic information via curl commands. Invoke when user needs to query graduate student data including full query, pagination, filtering, etc."
---


# 研究生基本信息查询 Skill

## 功能说明

本 Skill 用于通过 curl 命令查询数据中台研究生基本信息（宽表）数据。支持全量查询、分页查询、条件过滤、模糊查询等多种查询方式。

## 接口信息

| 项目 | 内容 |
|------|------|
| 接口名称 | 研究生基本信息（宽表） |
| 请求路径 | `/open_api/customization/dwsgxxsyjsxsjbxx/full` |
| 请求方式 | GET/POST |
| 认证方式 | Access Token |

## 环境变量配置

在使用本 Skill 前，请确保已设置以下环境变量：

```bash
# API 基础配置
export API_HOST="your-api-host"          # 数据中台 API 地址
export API_KEY="your-app-key"            # 应用 Key
export API_SECRET="your-app-secret"      # 应用 Secret
export API_PROTOCOL="https"              # 协议类型（默认 https）

# 可选：基础路径（默认 /open_api）
export API_BASE_PATH="/open_api"
```

## 可用查询字段

以下是可用于查询的字段列表：

| 字段名 | 类型 | 描述 | 支持模糊查询 |
|--------|------|------|--------------|
| XH | string | 学号 | 否 |
| XM | string | 姓名 | 否 |
| WWXM | string | 外文姓名 | 否 |
| XMPY | string | 姓名拼音 | 否 |
| CYM | string | 曾用名 | 否 |
| XBM | string | 性别码 | 否 |
| XBMC | string | 性别名称 | 否 |
| CSRQ | string | 出生日期 | 否 |
| CSDM | string | 出生地码 | 否 |
| CSDMC | string | 出生地名称 | 否 |
| JG | string | 籍贯码 | 否 |
| JGMC | string | 籍贯名称 | 否 |
| MZM | string | 民族码 | 否 |
| MZMMC | string | 民族名称 | 否 |
| GJDQM | string | 国家地区码 | 否 |
| GJDQMC | string | 国家地区名称 | 否 |
| SFZJLXM | string | 身份证件类型码 | 否 |
| SFZJLXMC | string | 身份证件类型名称 | 否 |
| SFZJH | string | 身份证件号 | 否 |
| HYZKM | string | 婚姻状况码 | 否 |
| HYZKMC | string | 婚姻状况名称 | 否 |
| GATQWM | string | 港澳台侨外码 | 否 |
| GATQWMC | string | 港澳台侨外名称 | 否 |
| ZZMMM | string | 政治面貌码 | 否 |
| ZZMMMC | string | 政治面貌名称 | 否 |
| JKZKM | string | 健康状况码 | 否 |
| JKZKMC | string | 健康状况名称 | 否 |
| XXM | string | 血型码 | 否 |
| XXMC | string | 血型名称 | 否 |
| SFZJYXQ | string | 身份证件有效期 | 否 |
| SFDSZN | string | 是否独生子女码 | 否 |
| SFDSZNMC | string | 是否独生子女名称 | 否 |
| HKXZM | string | 户口性质码 | 否 |
| HKSZD | string | 户口所在地 | 否 |
| HKSZDMC | string | 户口所在地名称 | 否 |
| XSLBM | string | 学生类别码 | 否 |
| XSLBMC | string | 学生类别名称 | 否 |
| DSGH | string | 导师工号 | 否 |
| JDXWM | string | 就读学位码 | 否 |
| JDXLM | string | 就读学历码 | 否 |
| YXDM | string | 院系代码 | 否 |
| YXDMMC | string | 院系名称 | 否 |
| ZYDM | string | 专业代码 | 否 |
| ZYDMMC | string | 专业名称 | 否 |
| BJDM | string | 班级代码 | 否 |
| NJDM | string | 年级代码 | 否 |
| YJBYSJ | string | 预计毕业时间 | 否 |
| XJZTDM | string | 学籍状态代码 | 否 |
| XJZTMC | string | 学籍状态名称 | 否 |
| LXBZDM | string | 离校标志代码 | 否 |
| LXBZMC | string | 离校标志名称 | 否 |
| ZH_SFZX | string | 综合判断是否在校 | 否 |
| ZH_SFZXMC | string | 综合判断是否在校名称 | 否 |
| XZ | string | 学制 | 否 |
| PYCCM | string | 培养层次码 | 否 |
| PYCCMC | string | 培养层次名称 | 否 |
| TSTAMP | string | 时间戳 | 否 |
| PYFSM | string | 培养方式码 | 否 |
| PYFSMC | string | 培养方式名称 | 否 |
| SFLXSM | string | 是否留学生码 | 否 |
| SFLXSMC | string | 是否留学生名称 | 否 |
| LXDH | string | 联系电话 | 否 |

## 使用步骤

### 1. 获取 Access Token

```bash
# 设置基础 URL
export BASE_URL="${API_PROTOCOL:-https}://${API_HOST}${API_BASE_PATH:-/open_api}"

# 获取 Token
curl -X POST "${BASE_URL}/authentication/get_access_token" \
  -H "Content-Type: application/json" \
  -d "{\"key\":\"${API_KEY}\",\"secret\":\"${API_SECRET}\"}"
```

### 2. 设置 Access Token

```bash
export ACCESS_TOKEN="your-access-token"
```

## 查询示例

### 1. 全量查询

查询所有研究生基本信息：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{\"access_token\":\"${ACCESS_TOKEN}\"}"
```

### 2. 分页查询

查询第 1 页，每页 20 条数据：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "page":1,
    "per_page":20
  }'
```

### 3. 按学号精确查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "XH":"2024001001"
  }'
```

### 4. 按姓名查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "XM":"张三"
  }'
```

### 5. 按院系代码查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "YXDM":"01"
  }'
```

### 6. 按专业代码查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "ZYDM":"081200"
  }'
```

### 7. 按年级查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "NJDM":"2024"
  }'
```

### 8. 按学籍状态查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "XJZTDM":"1"
  }'
```

### 9. 按培养层次查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "PYCCM":"1"
  }'
```

### 10. 按导师工号查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "DSGH":"T2024001"
  }'
```

### 11. 多条件组合查询

按院系和专业组合查询：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "YXDM":"01",
    "ZYDM":"081200",
    "NJDM":"2024"
  }'
```

### 12. 指定返回字段

只返回学号、姓名、院系名称、专业名称：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "attr_whitelist":["XH","XM","YXDMMC","ZYDMMC"],
    "per_page":10
  }'
```

### 13. 排序查询

按学号升序排列：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "order":{"XH":"asc"},
    "per_page":20
  }'
```

按入学时间降序排列：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "order":{"NJDM":"desc"},
    "per_page":20
  }'
```

### 14. OR 条件查询

查询院系为 01 或 02 的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "or":[
      {"YXDM":"01"},
      {"YXDM":"02"}
    ]
  }'
```

### 15. IN 查询

查询多个指定学号的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "XH":{"in":["2024001001","2024001002","2024001003"]}
  }'
```

### 16. 比较查询（大于/小于）

查询 2020 年级之后的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "NJDM":{"gte":"2020"}
  }'
```

### 17. NULL 值查询

查询没有联系电话的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "LXDH":{"is null":""}
  }'
```

查询有联系电话的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "LXDH":{"is not null":""}
  }'
```

### 18. 不等于查询

查询非在校状态的学生：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "XJZTDM":{"<>":"1"}
  }'
```

### 19. 综合查询示例

多条件 + 排序 + 分页 + 指定字段：

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsyjsxsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token":"'${ACCESS_TOKEN}'",
    "YXDM":"01",
    "PYCCM":"1",
    "or":[
      {"XJZTDM":"1"},
      {"XJZTDM":"2"}
    ],
    "order":{"NJDM":"desc","XH":"asc"},
    "page":1,
    "per_page":20,
    "attr_whitelist":["XH","XM","YXDMMC","ZYDMMC","NJDM","XJZTMC"]
  }'
```

## 响应格式说明

### 成功响应

```json
{
  "code": "10000",
  "message": "ok",
  "description": "api请求成功",
  "result": {
    "data": [
      {
        "XH": "2024001001",
        "XM": "张三",
        "WWXM": null,
        "XMPY": "Zhang San",
        "CYM": null,
        "XBM": "1",
        "XBMC": "男",
        "CSRQ": "1998-05-20",
        "YXDM": "01",
        "YXDMMC": "计算机学院",
        "ZYDM": "081200",
        "ZYDMMC": "计算机科学与技术",
        "NJDM": "2024",
        "XJZTDM": "1",
        "XJZTMC": "在读",
        ...
      }
    ],
    "data_struct": {},
    "encrypted_field": "",
    "max_page": 10,
    "page": 1,
    "per_page": 20,
    "total": 200
  }
}
```

### 响应字段说明

| 字段名 | 类型 | 描述 |
|--------|------|------|
| code | string | 返回代码 |
| message | string | 返回消息 |
| description | string | 返回说明 |
| result | object | 返回结果 |
| result.data | array | 数据列表 |
| result.total | string | 总数据量 |
| result.page | string | 当前页数 |
| result.per_page | string | 每页数据量 |
| result.max_page | string | 最大页数 |
| result.encrypted_field | string | 加密字段列表 |

## 常用错误码

| 错误码 | 说明 |
|--------|------|
| 10000 | 请求成功 |
| 10006 | 必填参数缺失 |
| 10008 | 参数错误 |
| 10013 | 缺失必填参数 |
| 10014 | 参数类型无效 |
| 20007 | Token 过期 |
| 20009 | Token 错误 |
| 20010 | 无效的 Token |
| 20013 | 查询结果为空 |

## 注意事项

1. **Token 有效期**：Access Token 默认有效期为 7200 秒（2小时），过期后需要重新获取

2. **分页建议**：
   - 默认每页返回 10 条数据
   - 建议设置合理的 `per_page` 值（最大不超过 1000）
   - 使用分页时建议同时指定排序条件

3. **Oracle 数据库**：
   - 使用 Oracle 数据库时，分页查询必须带上排序条件
   - 排序条件尽量唯一或重复少

4. **字段类型**：
   - 字符串类型字段使用双引号包裹
   - 数值类型字段进行比较查询时不要加引号

5. **模糊查询**：
   - 本文档接口暂不支持模糊查询
   - 所有查询均为精确匹配

6. **安全性**：
   - 请勿将 App Key 和 App Secret 硬编码在代码中
   - 建议使用环境变量或配置文件管理敏感信息
   - Access Token 应妥善保管，避免泄露
