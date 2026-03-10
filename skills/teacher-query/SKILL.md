---
name: "teacher-query"
description: "Query teacher/staff basic information via curl commands. Invoke when user needs to query teacher data including full query, pagination, filtering, etc."
---

# 教职工基本信息查询

## 功能说明

本 Skill 用于通过 curl 命令查询数据中台教职工基本信息（宽表）数据。

## 接口信息

| 项目 | 内容 |
|------|------|
| 接口名称 | 教职工基本信息（宽表） |
| 请求路径 | `/open_api/customization/dwsgxjgjzgjbxx/full` |
| 请求方式 | GET/POST |
| 认证方式 | Access Token |

## 环境变量配置

在使用本 Skill 前，请确保已设置以下环境变量：

```bash
# API 服务器配置
export API_HOST="your-api-host"        # API 主机地址
export API_PROTOCOL="https"            # 协议类型（默认 https）

# 应用认证信息（从应用详情获取）
export API_KEY="your-app-key"          # App Key
export API_SECRET="your-app-secret"    # App Secret

# Token（通过认证接口获取）
export ACCESS_TOKEN="your-access-token"
```

## 获取 Access Token

```bash
# 获取 Token
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/authentication/get_access_token" \
  -H "Content-Type: application/json" \
  -d "{\"key\":\"${API_KEY}\",\"secret\":\"${API_SECRET}\"}"

# 设置 Token（从返回结果中提取）
export ACCESS_TOKEN="your-access-token"
```

**Token 有效期**：7200 秒（2小时），过期后需重新获取。

## 可用查询字段

### 基本信息字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| GH | 工号 | 否 |
| XM | 姓名 | 否 |
| XMPY | 姓名拼音 | 否 |
| WWXM | 外文姓名 | 否 |
| CYM | 曾用名 | 否 |
| XBM | 性别码 | 否 |
| XBMC | 性别名称 | 否 |
| CSRQ | 出生日期 | 否 |
| NL | 年龄 | 否 |

### 身份相关字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| SFZJLXM | 身份证件类型码 | 否 |
| SFZJLXMC | 身份证件类型名称 | 否 |
| SFZJH | 身份证件号 | 否 |
| GJDQM | 国籍/地区码 | 否 |
| GJDQMC | 国籍/地区码名称 | 否 |
| MZM | 民族码 | 否 |
| MZMC | 民族名称 | 否 |
| ZZMMM | 政治面貌码 | 否 |
| ZZMMMC | 政治面貌名称 | 否 |

### 单位部门字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| SZDWBM | 所在单位编码 | 否 |
| SZDWMC | 所在单位名称 | 否 |
| SZKSBM | 所在科室编码 | 否 |
| SZKSMC | 所在科室名称 | 否 |
| XQBM | 校区编码 | 否 |

### 岗位职务字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| DZZW | 党政职务 | 否 |
| DZZWJBDM | 党政职务级别 | 否 |
| DZZWJBMC | 党政职务级别名称 | 否 |
| ZYJSZWDM | 专业技术职务 | 否 |
| ZYJSZWMC | 专业技术职务名称 | 否 |
| ZYJSGWLBDM | 专业技术岗位类别 | 否 |
| ZYJSGWLBMC | 专业技术岗位类别名称 | 否 |
| GLGWDJDM | 管理岗位等级 | 否 |
| GLGWDJMC | 管理岗位等级名称 | 否 |
| GQGWDJDM | 工勤岗位等级 | 否 |
| GQGWDJMC | 工勤岗位等级名称 | 否 |
| SFZRJS | 是否专任教师 | 否 |
| SFSJT | 是否双肩挑 | 否 |

### 学历学位字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| ZGXLM | 最高学历码 | 否 |
| ZGXLMC | 最高学历名称 | 否 |
| ZGXWM | 最高学位码 | 否 |
| ZGXWMC | 最高学位名称 | 否 |
| BYXX | 毕业学校 | 否 |
| XWSYDW | 学位授予单位 | 否 |

### 联系方式字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| YDDH | 移动电话 | 否 |
| DZYX | 电子邮箱 | 否 |
| TXDZ | 通讯地址 | 否 |
| BGLXDH | 办公联系电话 | 否 |

### 状态类别字段

| 字段名 | 描述 | 模糊查询 |
|--------|------|----------|
| JZGLBM | 教职工类别码 | 否 |
| JZGLBMC | 教职工类别名称 | 否 |
| JZGDQZTM | 教职工当前状态码 | 否 |
| JZGDQZTMC | 教职工当前状态名称 | 否 |
| DQZTDM | 当前状态 | 否 |
| DQZTMC | 当前状态名称 | 否 |
| SFZGZZ | 是否在岗在职 | 否 |

### 分页参数

| 字段名 | 描述 | 默认值 |
|--------|------|--------|
| page | 当前页数 | 1 |
| per_page | 每页最大数据量 | 10 |

## 查询示例

### 1. 全量查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{\"access_token\":\"${ACCESS_TOKEN}\"}"
```

### 2. 分页查询

```bash
# 查询第2页，每页20条
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"page\":2,
    \"per_page\":20
  }"
```

### 3. 按工号精确查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"GH\":\"12345\"
  }"
```

### 4. 按姓名查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XM\":\"张三\"
  }"
```

### 5. 按单位查询

```bash
# 按所在单位编码查询
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SZDWBM\":\"1001\"
  }"

# 按所在单位名称查询
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SZDWMC\":\"计算机学院\"
  }"
```

### 6. 按性别查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XBMC\":\"男\"
  }"
```

### 7. 按教职工类别查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"JZGLBMC\":\"专任教师\"
  }"
```

### 8. 按当前状态查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"JZGDQZTMC\":\"在岗\"
  }"
```

### 9. 按专业技术职务查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"ZYJSZWMC\":\"教授\"
  }"
```

### 10. 按政治面貌查询

```bash
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"ZZMMMC\":\"中共党员\"
  }"
```

### 11. 模糊查询（姓名）

```bash
# 查询姓名包含"张"的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XM\":\"%%张%%\"
  }"

# 查询姓名以"李"开头的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XM\":\"李%%\"
  }"
```

### 12. 多条件组合查询

```bash
# 查询计算机学院的男教师
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SZDWMC\":\"计算机学院\",
    \"XBMC\":\"男\"
  }"

# 查询在岗的专任教师
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"JZGLBMC\":\"专任教师\",
    \"JZGDQZTMC\":\"在岗\"
  }"
```

### 13. 指定返回字段

```bash
# 只返回工号、姓名、单位、职务
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"attr_whitelist\":[\"GH\",\"XM\",\"SZDWMC\",\"ZYJSZWMC\"],
    \"per_page\":10
  }"
```

### 14. 排序查询

```bash
# 按工号升序排序
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"order\":{\"GH\":\"asc\"},
    \"per_page\":20
  }"

# 按出生日期降序排序（先出生的在前）
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"order\":{\"CSRQ\":\"desc\"},
    \"per_page\":20
  }"
```

### 15. OR 条件查询

```bash
# 查询计算机学院或软件学院的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"or\":[
      {\"SZDWMC\":\"计算机学院\"},
      {\"SZDWMC\":\"软件学院\"}
    ]
  }"
```

### 16. 比较查询（年龄）

```bash
# 查询年龄大于30岁的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"NL\":{\"gt\":30}
  }"

# 查询年龄在30到50岁之间的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"NL\":{\"gte\":30,\"lte\":50}
  }"
```

### 17. IN 查询

```bash
# 查询多个指定单位的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SZDWMC\":{\"in\":[\"计算机学院\",\"软件学院\",\"信息学院\"]}
  }"
```

### 18. NULL 值查询

```bash
# 查询没有填写电子邮箱的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"DZYX\":{\"is null\":\"\"}
  }"

# 查询已填写电子邮箱的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"DZYX\":{\"is not null\":\"\"}
  }"
```

### 19. 不等于查询

```bash
# 查询非专任教师的教职工
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"JZGLBMC\":{\"<>\":\"专任教师\"}
  }"
```

### 20. 综合查询示例

```bash
# 查询计算机学院在岗的男教师，按工号排序，每页10条，只返回关键字段
curl -X POST "${API_PROTOCOL}://${API_HOST}/open_api/customization/dwsgxjgjzgjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SZDWMC\":\"计算机学院\",
    \"XBMC\":\"男\",
    \"JZGDQZTMC\":\"在岗\",
    \"order\":{\"GH\":\"asc\"},
    \"page\":1,
    \"per_page\":10,
    \"attr_whitelist\":[\"GH\",\"XM\",\"XBMC\",\"SZDWMC\",\"ZYJSZWMC\",\"JZGLBMC\"]
  }"
```

## 响应格式说明

### 成功响应示例

```json
{
  "code": 10000,
  "message": "ok",
  "description": "api请求成功",
  "result": {
    "data": [
      {
        "GH": "12345",
        "XM": "张三",
        "XBMC": "男",
        "SZDWMC": "计算机学院",
        "ZYJSZWMC": "教授",
        ...
      }
    ],
    "data_struct": {},
    "encrypted_field": "",
    "max_page": 10,
    "page": 1,
    "per_page": 10,
    "total": 100
  },
  "uuid": "xxx"
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

## 常用错误码

| 错误码 | 说明 |
|--------|------|
| 10000 | 请求成功 |
| 20007 | Token 过期 |
| 20009 | Token 错误 |
| 20010 | 无效的 Token |
| 20013 | 查询结果为空 |
| 10006 | 必填参数缺失 |
| 10008 | 参数错误 |

## 注意事项

1. **Token 有效期**：Token 默认有效期为 7200 秒，过期后需要重新获取
2. **分页排序**：使用 Oracle 数据库时，分页查询建议添加排序条件以确保数据顺序稳定
3. **字段类型**：字符串类型字段请使用双引号包裹
4. **模糊查询**：使用 `%%` 包裹查询值进行模糊匹配，如 `%%张%%` 表示包含"张"的数据
5. **敏感信息**：请勿在代码中硬编码 App Key 和 App Secret
