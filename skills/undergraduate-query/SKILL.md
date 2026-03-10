---
name: "undergraduate-query"
description: "Query undergraduate student basic information via curl commands. Invoke when user needs to query undergraduate student data including full query, pagination, filtering, etc."
---

# 本科生基本信息查询 Skill

## 功能说明

本 Skill 用于通过 curl 命令查询数据中台的本科生基本信息接口，支持多种查询方式。

## 接口信息

- **接口路径**: `/open_api/customization/dwsgxxsbzksjbxx/full`
- **请求方式**: GET/POST
- **认证方式**: Access Token

## 环境变量配置

使用前请确保已配置以下环境变量：

| 环境变量 | 说明 | 示例 |
|---------|------|------|
| `API_HOST` | 数据中台 API 主机地址 | `dmp.example.com` |
| `API_KEY` | API 应用 Key | `your-app-key` |
| `API_SECRET` | API 应用 Secret | `your-app-secret` |
| `API_PROTOCOL` | 协议类型 | `https`（默认） |
| `API_BASE_PATH` | API 基础路径 | `/open_api`（默认） |

## 可用查询字段

| 字段名 | 说明 | 示例值 |
|--------|------|--------|
| XH | 学号 | "20210001" |
| XM | 姓名 | "张三" |
| XBM/XBMC | 性别码/性别名称 | "1"/"男" |
| YXDM/YXDMMC | 院系代码/院系名称 | "01"/"计算机学院" |
| ZYDM/ZYDMMC | 专业代码/专业名称 | "0809"/"计算机科学与技术" |
| NJDM | 年级代码 | "2021" |
| BJBM/BJMC | 班级代码/班级名称 | "2101"/"计算机2101班" |
| SFZJ/SFZJMC | 是否在籍/是否在籍名称 | "1"/"在籍" |
| SFZX/SFZXMC | 是否在校/是否在校名称 | "1"/"在校" |
| PYCCM/PYCCMC | 培养层次码/培养层次名称 | "1"/"本科" |
| MZM/MZMC | 民族码/民族名称 | "01"/"汉族" |
| ZZMMM/ZZMMMC | 政治面貌码/政治面貌名称 | "01"/"中共党员" |
| CSRQ | 出生日期 | "2000-01-01" |
| SFZJH | 身份证件号 | "11010120000101xxxx" |
| LXDH | 联系电话 | "13800138000" |
| DZYX | 电子邮箱 | "student@example.edu.cn" |

## 使用步骤

### 步骤1：获取 Access Token

```bash
# 设置环境变量
export API_HOST="your-api-host"
export API_KEY="your-app-key"
export API_SECRET="your-app-secret"
export API_PROTOCOL="https"
export API_BASE_PATH="/open_api"

# 构建 BASE_URL
BASE_URL="${API_PROTOCOL}://${API_HOST}${API_BASE_PATH}"

# 获取 access_token
curl -X POST "${BASE_URL}/authentication/get_access_token" \
  -H "Content-Type: application/json" \
  -d "{\"key\":\"${API_KEY}\",\"secret\":\"${API_SECRET}\"}"
```

响应示例：
```json
{
  "code": 10000,
  "message": "ok",
  "result": {
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "expires_in": 7200
  }
}
```

### 步骤2：设置 Token 变量

```bash
export ACCESS_TOKEN="your-access-token"
```

## 查询示例

### 1. 全量查询（获取所有数据）

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{\"access_token\":\"${ACCESS_TOKEN}\"}"
```

### 2. 分页查询

```bash
# 查询第1页，每页20条
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"page\":1,
    \"per_page\":20
  }"

# 查询第2页，每页50条
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"page\":2,
    \"per_page\":50
  }"
```

### 3. 按学号精确查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XH\":\"20210001\"
  }"
```

### 4. 按姓名模糊查询

```bash
# 查询姓名包含"张"的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XM\":\"%%张%%\"
  }"

# 查询姓名以"张"开头的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XM\":\"张%%\"
  }"
```

### 5. 按院系查询

```bash
# 按院系代码查询
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"YXDM\":\"01\"
  }"

# 按院系名称模糊查询
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"YXDMMC\":\"%%计算机%%\"
  }"
```

### 6. 按专业和年级查询

```bash
# 查询计算机专业2021级学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"ZYDM\":\"0809\",
    \"NJDM\":\"2021\"
  }"
```

### 7. 按班级查询

```bash
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"BJBM\":\"2101\"
  }"
```

### 8. 按在籍状态查询

```bash
# 查询在籍学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SFZJ\":\"1\"
  }"

# 查询在校学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"SFZX\":\"1\"
  }"
```

### 9. 多条件组合查询

```bash
# 查询计算机学院2021级在籍男生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"YXDM\":\"01\",
    \"NJDM\":\"2021\",
    \"XBM\":\"1\",
    \"SFZJ\":\"1\"
  }"
```

### 10. 指定返回字段

```bash
# 只返回学号、姓名、院系、专业
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"attr_whitelist\":[\"XH\",\"XM\",\"YXDMMC\",\"ZYDMMC\"],
    \"per_page\":10
  }"
```

### 11. 排序查询

```bash
# 按学号升序排列
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"order\":{\"XH\":\"asc\"},
    \"per_page\":20
  }"

# 按年级降序、学号升序排列
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"order\":{\"NJDM\":\"desc\",\"XH\":\"asc\"},
    \"per_page\":20
  }"
```

### 12. 使用 OR 条件查询

```bash
# 查询院系为01或02的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"or\":[
      {\"YXDM\":\"01\"},
      {\"YXDM\":\"02\"}
    ]
  }"
```

### 13. 比较查询

```bash
# 查询2020级及以后的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"NJDM\":{\"gte\":\"2020\"}
  }"

# 查询2019级到2021级的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"NJDM\":{\"gte\":\"2019\",\"lte\":\"2021\"}
  }"
```

### 14. IN 查询

```bash
# 查询多个指定学号的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"XH\":{\"in\":[\"20210001\",\"20210002\",\"20210003\"]}
  }"
```

### 15. 查询 NULL 值

```bash
# 查询联系电话为空的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"LXDH\":{\"is null\":\"\"}
  }"

# 查询联系电话不为空的学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"LXDH\":{\"is not null\":\"\"}
  }"
```

### 16. 不等于查询

```bash
# 查询非汉族学生
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"MZM\":{\"<\">\":\"01\"}
  }"
```

### 17. 完整综合查询示例

```bash
# 查询计算机学院2021级或2022级的在籍学生，按学号升序排列，每页20条
curl -X POST "${BASE_URL}/customization/dwsgxxsbzksjbxx/full" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"YXDM\":\"01\",
    \"SFZJ\":\"1\",
    \"or\":[
      {\"NJDM\":\"2021\"},
      {\"NJDM\":\"2022\"}
    ],
    \"order\":{\"XH\":\"asc\"},
    \"page\":1,
    \"per_page\":20,
    \"attr_whitelist\":[\"XH\",\"XM\",\"XBMC\",\"YXDMMC\",\"ZYDMMC\",\"NJDM\",\"BJMC\",\"SFZJMC\"]
  }"
```

## 响应格式说明

成功响应示例：
```json
{
  "code": 10000,
  "description": "api请求成功",
  "message": "ok",
  "result": {
    "data": [
      {
        "XH": "20210001",
        "XM": "张三",
        "XBMC": "男",
        "YXDMMC": "计算机学院",
        "ZYDMMC": "计算机科学与技术",
        "NJDM": "2021",
        "BJMC": "计算机2101班",
        "SFZJ": "1",
        "SFZJMC": "在籍"
      }
    ],
    "max_page": 10,
    "page": 1,
    "per_page": 20,
    "total": 200
  }
}
```

## 常用错误码

| 错误码 | 说明 |
|--------|------|
| 10000 | 请求成功 |
| 20007 | Token 过期 |
| 20009 | Token 错误 |
| 20010 | 无效的 Token |
| 20013 | 查询结果为空 |

## 注意事项

1. **Token 有效期**：默认 7200 秒，过期后需要重新获取
2. **分页排序**：Oracle 数据库采用堆排序，使用分页时建议添加排序条件
3. **URL 编码**：URL 传参时，特殊字符（如 `%`）需要编码为 `%25`
4. **模糊查询**：使用 `%%` 标记模糊查询，如 `%%张%%` 表示包含"张"
5. **数字比较**：比较查询时数字不要加引号，避免字符串比较
