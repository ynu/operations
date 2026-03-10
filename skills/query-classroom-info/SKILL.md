---
name: "query-classroom-info"
description: "查询本科生教室信息。当用户询问教室信息、查询教室、查找教室、或提到教室相关查询时立即调用。"
---

# 本科生教室信息查询

## 触发条件

当用户表达以下意图时，立即使用此Skill：
- "查询教室信息"
- "查找教室"
- "教室在哪里"
- "有哪些教室"
- "文渊楼教室"
- 任何包含"教室"的查询请求

**注意**：如果用户只说"教室信息"，默认指的是**本科生教室信息**。

## 执行步骤

### 1. 读取环境变量

从 `.env` 文件读取以下配置：
- `API_HOST` - 数据中台地址
- `API_KEY` - 应用Key
- `API_SECRET` - 应用Secret
- `API_PROTOCOL` - 协议（默认https）

### 2. 获取Access Token

```bash
curl -s -X POST "https://{API_HOST}/open_api/authentication/get_access_token" \
  -H "Content-Type: application/json" \
  -d '{"key":"{API_KEY}","secret":"{API_SECRET}"}'
```

从返回结果中提取 `access_token`。

### 3. 查询教室数据

```bash
curl -s -X POST "https://{API_HOST}/open_api/customization/tgxjxbzksjsjbxx/full" \
  -H "Content-Type: application/json" \
  -d '{
    "access_token": "{获取到的token}",
    "page": 1,
    "per_page": 20
  }'
```

### 4. 处理用户查询条件

如果用户有特定查询条件，在请求体中添加：

- **按教学楼查询**：`"JXLMC": "文渊楼"`
- **按校区查询**：`"XQDM": "DL"`
- **按教室名称模糊查询**：`"JASMC": "%%101%%"`
- **按座位数范围查询**：`"SKZWS": {"gte": "50", "lte": "100"}`

### 5. 返回结果给用户

**重要**：用户可能不是技术人员，返回结果时：
- ✅ 告诉用户数据来自"数据中台"
- ✅ 使用友好的语言描述
- ❌ 不要展示curl命令
- ❌ 不要展示JSON原始数据
- ❌ 不要展示技术细节（如token、API路径等）

## 返回格式示例

**成功返回：**
```
已从数据中台查询到本科生教室信息：

共找到 2295 间教室，为您展示前 20 间：

| 教室名称 | 教学楼 | 校区 | 上课座位数 | 考试座位数 |
|---------|--------|------|-----------|-----------|
| 文渊楼101 | 文渊楼 | 东陆校区 | 176 | 87 |
| 文渊楼102 | 文渊楼 | 东陆校区 | 120 | 60 |
| ... | ... | ... | ... | ... |
```

**按条件查询返回：**
```
已从数据中台查询到文渊楼的教室信息：

共找到 XX 间教室：

| 教室名称 | 上课座位数 | 考试座位数 | 允许排课 |
|---------|-----------|-----------|---------|
| 文渊楼101 | 176 | 87 | 否 |
| ... | ... | ... | ... |
```

## 数据字段说明

| 字段 | 含义 |
|------|------|
| JASMC | 教室名称 |
| JXLMC | 教学楼名称 |
| XQDM | 校区代码（DL=东陆, CG=呈贡） |
| SKZWS | 上课座位数 |
| KSZWS | 考试座位数 |
| SFYXPK | 是否允许排课（1=是, 0=否） |
| SFYXKS | 是否允许考试（1=是, 0=否） |
| SFYXJY | 是否允许借用（1=是, 0=否） |

## 查询操作

### 全量数据查询

使用请求体传参：
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

### 指定列查询

使用 `attr_whitelist` 参数指定所需列名：
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "attr_whitelist": ["JASMC", "JXLMC", "SKZWS"]
}
```

### 分页

使用 `page` 和 `per_page` 参数：

| 参数 | 说明 | 默认值 |
|------|------|--------|
| page | 当前页数 | 1 |
| per_page | 每页数据条数 | 10 |

**示例：**
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "page": 2,
    "per_page": 100
}
```

> **注意**：Oracle 数据库采用堆排序，每次取出的数据是无序的。使用分页时，请务必带上排序条件。

### 排序

使用 `order` 参数：
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "order": {
        "JXLMC": "asc",
        "JASMC": "asc"
    }
}
```

- `asc`：升序
- `desc`：降序

### 条件查询

#### 简单条件查询（相等）
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "JXLMC": "文渊楼"
}
```

#### 模糊查询

使用 `%%` 标记模糊查询：
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "JASMC": "%%101%%"
}
```

- `文%`：匹配以"文"开头
- `%101%`：匹配包含"101"
- `%楼`：匹配以"楼"结尾

> URL 传参时，需将 `%` 转码为 `%25`

#### 比较查询

```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "SKZWS": {
        "gte": 50,
        "lte": 100
    }
}
```

| 操作符 | 含义 |
|--------|------|
| gt | 大于 (>) |
| lt | 小于 (<) |
| gte | 大于等于 (>=) |
| lte | 小于等于 (<=) |
| in | 包含在列表中 |

#### 复杂逻辑查询

使用 `and` 和 `or`：
```json
{
    "access_token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "or": [
        {"JXLMC": "文渊楼"},
        {"JXLMC": "文澜楼"}
    ]
}
```

#### NULL 值查询

```json
// 查询 NULL
{
    "SFYXPK": {
        "is null": ""
    }
}

// 查询非 NULL
{
    "SFYXPK": {
        "is not null": ""
    }
}
```

#### 不等于、不包含查询

```json
// 不等于
{
    "XQDM": {
        "<>": "DL"
    }
}

// 不包含
{
    "JXLMC": {
        "not in": ["文渊楼", "文澜楼"]
    }
}
```

## 数据类型说明

### 字符串类型

使用双引号包裹：
```json
{
    "JXLMC": "文渊楼"
}
```

### 数字类型

比较查询时，不要使用引号：
```json
{
    "SKZWS": {
        "gt": 50
    }
}
```

### 时间类型

默认格式：`yyyy-mm-dd hh24:mi:ss`

```json
{
    "create_time": {
        "gt": "2018-01-01 00:00:00",
        "lt": "2019-01-01 00:00:00"
    }
}
```

Oracle 数据库可使用 `to_date` 函数：
```json
{
    "create_time": {
        "gt": "to_date('20181231', 'yyyymmdd')"
    }
}
```

## 注意事项

1. Token有效期为7200秒，每次查询都需要重新获取
2. 数据中台返回的数据可能较多，默认展示前20条
3. 如果用户需要更多数据，可以告知总数量并提供翻页选项
4. 校区代码：DL=东陆校区，CG=呈贡校区
5. **Oracle 分页**：使用分页时务必添加排序条件
6. **数据类型**：数字类型比较时不要加引号，避免字符串比较导致错误
7. **URL 编码**：URL 传参时，特殊字符需要编码（如 `%` → `%25`）
