---
name: "skill-generator"
description: "Generate query skills from DOCX API documentation. Invoke when user wants to create a curl-based query skill from a Word document containing data platform API interface specifications."
---

# Skill 生成器

本 Skill 用于从用户提供的 DOCX 接口文档中自动生成可用于 curl 查询的 Skill。

## 功能说明

1. **提取接口信息**：从 DOCX 文件中提取业务接口定义（3.4节）
2. **整合 API 规范**：结合数据中台 API 访问通用规范
3. **生成 Skill**：创建包含完整 curl 查询示例的 Skill 文件

## 前置依赖

使用本 Skill 前，请确保已安装 Python 依赖：

```bash
pip install python-docx
```

## 工作流程

### 步骤 1: 获取 DOCX 文件路径

确认用户提供的 DOCX 文件路径，例如：`./业务接口文档.docx`

### 步骤 2: 转换 DOCX 为 Markdown

使用内置脚本将 DOCX 转换为 Markdown（从 skill 目录执行）：

```bash
python ./scripts/convert_docx_to_md.py <input.docx> <temp_output.md>
```

示例：
```bash
python ./scripts/convert_docx_to_md.py ./业务接口文档.docx ./temp_api_doc.md
```

### 步骤 3: 验证文档类型

读取生成的 Markdown 文件，验证是否为数据中台接口文档：

**验证要点**：
- 文档标题包含"对接文档"、"API文档"、"接口文档"等关键词
- 包含"数据中台"、"数据平台"、"数据开放服务"等相关描述
- 包含接口列表、应用详情、认证方式等章节
- 包含 RESTful API 相关描述

**如果不是数据中台接口文档**：终止工作流程，告知用户此文档不是有效的数据中台数据接口对接文档

### 步骤 4: 提取 3.4 节业务接口信息

在 Markdown 文件中定位并提取 3.4 节内容：

**提取内容包括**：
- 接口名称（3.4节标题）
- 接口描述
- 请求路径（如 `/open_api/customization/xxx/full`）
- 请求方式（GET/POST）
- 请求参数表（参数名、类型、必填、描述等）
- 返回参数表（参数名、类型、描述等）
- 返回示例

**提取规则**：
- 只提取 3.4 节内容，不包含其他章节
- 识别 3.4 节标题作为接口名称
- 提取接口路径中的 `{api_path}` 部分

### 步骤 5: 创建业务接口 Markdown 文件

将提取的 3.4 节内容保存为临时文件：

```
业务接口_<接口名称>.md
```

示例：`业务接口_本科生基本信息.md`

### 步骤 6: 生成查询 Skill

基于提取的接口信息和数据中台 API 规范，生成新的 Skill 文件。

#### 6.1 确定 Skill 名称

根据接口名称生成 skill 名称：
- 接口名称："本科生基本信息（宽表）"
- Skill 名称：`undergraduate-query` 或 `zksjbxx-query`

#### 6.2 构建 Skill 内容结构

生成的 Skill 必须包含以下内容：

**Frontmatter**：
```yaml
---
name: "<skill-name>"
description: "Query <interface-name> via curl commands. Invoke when user needs to query <data-type> data including full query, pagination, filtering, etc."
---
```

**正文结构**：
1. **功能说明** - 简要说明 Skill 用途
2. **接口信息** - 接口路径、请求方式、认证方式
3. **环境变量配置** - 需要的环境变量列表
4. **可用查询字段** - 从请求参数表提取的可用于查询的字段
5. **使用步骤** - 获取 Token、设置变量
6. **查询示例** - 各种查询类型的 curl 命令
7. **响应格式说明** - 返回数据结构说明
8. **常用错误码** - 错误码对照表
9. **注意事项** - 使用提示

#### 6.3 生成查询示例

根据数据中台 API 规范，为每个可查询字段生成示例：

**必须包含的查询类型**：

1. **全量查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{\"access_token\":\"${ACCESS_TOKEN}\"}"
```

2. **分页查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"page\":1,
    \"per_page\":20
  }"
```

3. **精确查询**（针对每个关键字段）
```bash
# 按<字段名>查询
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":\"<value>\"
  }"
```

4. **模糊查询**（支持模糊查询的字段）
```bash
# 模糊查询<字段名>
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":\"%%<value>%%\"
  }"
```

5. **多条件组合查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field1>\":\"<value1>\",
    \"<field2>\":\"<value2>\"
  }"
```

6. **指定返回字段**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"attr_whitelist\":[\"<field1>\",\"<field2>\"],
    \"per_page\":10
  }"
```

7. **排序查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"order\":{\"<field>\":\"asc\"},
    \"per_page\":20
  }"
```

8. **OR 条件查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"or\":[
      {\"<field>\":\"<value1>\"},
      {\"<field>\":\"<value2>\"}
    ]
  }"
```

9. **比较查询**（针对数值/日期字段）
```bash
# 大于查询
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":{\"gte\":\"<value>\"}
  }"
```

10. **IN 查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":{\"in\":[\"<value1>\",\"<value2>\"]}
  }"
```

11. **NULL 值查询**
```bash
# 查询为空的记录
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":{\"is null\":\"\"}
  }"
```

12. **不等于查询**
```bash
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field>\":{\"<>\":\"<value>\"}
  }"
```

13. **综合查询示例**
```bash
# 多条件+排序+分页+指定字段
curl -X POST "${BASE_URL}/<api_path>" \
  -H "Content-Type: application/json" \
  -d "{
    \"access_token\":\"${ACCESS_TOKEN}\",
    \"<field1>\":\"<value1>\",
    \"or\":[
      {\"<field2>\":\"<value2>\"},
      {\"<field2>\":\"<value3>\"}
    ],
    \"order\":{\"<sort_field>\":\"asc\"},
    \"page\":1,
    \"per_page\":20,
    \"attr_whitelist\":[\"<field_a>\",\"<field_b>\"]
  }"
```

### 步骤 7: 创建 Skill 文件

1. 创建 Skill 目录：`.trae/skills/<skill-name>/`
2. 创建 Skill 文件：`.trae/skills/<skill-name>/SKILL.md`
3. 将生成的内容写入文件

### 步骤 8: 清理临时文件

1. 删除 DOCX 转换的临时 Markdown 文件（`./temp_api_doc.md`）
2. 删除提取的业务接口临时文件（`./业务接口_xxx.md`）
3. 保留生成的 Skill 文件

### 步骤 9: 安全检查

检查生成的 Skill 文件中是否包含敏感信息：
- App Key (key)
- App Secret (secret)
- access_token 示例值
- 真实的服务器地址、密码等

**如果发现敏感信息**：
- 将敏感值替换为占位符（如 `xxxxxxxxxxxxxxxx` 或 `[YOUR_KEY]`）
- 或删除包含敏感信息的段落

### 步骤 10: 返回结果

向用户报告：
1. 成功生成的 Skill 名称和路径
2. Skill 支持的主要功能
3. 使用建议

## 数据中台 API 规范参考

### 环境变量

| 环境变量 | 说明 | 示例 |
|---------|------|------|
| `API_HOST` | API 主机地址 | `dmp.example.com` |
| `API_KEY` | 应用 Key | `your-app-key` |
| `API_SECRET` | 应用 Secret | `your-app-secret` |
| `API_PROTOCOL` | 协议类型 | `https`（默认） |
| `API_BASE_PATH` | API 基础路径 | `/open_api`（默认） |

### 认证流程

```bash
# 1. 获取 Access Token
curl -X POST "${BASE_URL}/authentication/get_access_token" \
  -H "Content-Type: application/json" \
  -d "{\"key\":\"${API_KEY}\",\"secret\":\"${API_SECRET}\"}"

# 2. 设置 Token
export ACCESS_TOKEN="your-access-token"
```

### 查询操作符

| 操作符 | 含义 | 示例 |
|--------|------|------|
| `%%value%%` | 模糊查询（包含） | `"XM": "%%张%%"` |
| `value%%` | 模糊查询（开头） | `"XM": "张%%"` |
| `%%value` | 模糊查询（结尾） | `"XM": "%%三"` |
| `gt` | 大于 | `"age": {"gt": 18}` |
| `lt` | 小于 | `"age": {"lt": 60}` |
| `gte` | 大于等于 | `"age": {"gte": 18}` |
| `lte` | 小于等于 | `"age": {"lte": 60}` |
| `in` | 包含在列表中 | `"status": {"in": ["1", "2"]}` |
| `is null` | 为空 | `"field": {"is null": ""}` |
| `is not null` | 不为空 | `"field": {"is not null": ""}` |
| `<>` | 不等于 | `"field": {"<>": "value"}` |

### 分页参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `page` | 当前页数 | 1 |
| `per_page` | 每页数据条数 | 10 |

### 排序参数

```json
{
  "order": {
    "field1": "asc",
    "field2": "desc"
  }
}
```

### 常用错误码

| 错误码 | 说明 |
|--------|------|
| 10000 | 请求成功 |
| 20007 | Token 过期 |
| 20009 | Token 错误 |
| 20010 | 无效的 Token |
| 20013 | 查询结果为空 |

## 使用示例

用户请求："帮我从这份接口文档生成查询 skill"

执行流程：

1. **确认文件路径**
   - 用户提供的文件：`./本科生基本信息接口.docx`

2. **转换文档**（在 skill 目录中执行）
   ```bash
   python ./scripts/convert_docx_to_md.py \
     ./本科生基本信息接口.docx ./temp_api_doc.md
   ```

3. **验证文档**
   - 读取 `./temp_api_doc.md`
   - 确认包含"数据中台"、"对接文档"等关键词
   - 确认是有效的接口文档

4. **提取 3.4 节**
   - 定位 3.4 节标题："本科生基本信息（宽表）"
   - 提取接口路径：`/open_api/customization/dwsgxxsbzksjbxx/full`
   - 提取请求参数表和返回参数表

5. **创建临时文件**
   - 保存为 `./业务接口_本科生基本信息.md`

6. **生成 Skill**
   - Skill 名称：`undergraduate-query`
   - 创建目录：`.trae/skills/undergraduate-query/`
   - 生成 `SKILL.md` 文件

7. **清理临时文件**
   - 删除 `./temp_api_doc.md`
   - 删除 `./业务接口_本科生基本信息.md`

8. **安全检查**
   - 确认无敏感信息泄露

9. **返回结果**
   - 报告生成的 Skill 路径：`.trae/skills/undergraduate-query/SKILL.md`
   - 说明 Skill 支持的功能

## 注意事项

1. **文档验证**：必须严格验证文档类型，确保是数据中台接口文档
2. **只提取 3.4 节**：其他章节（如认证、错误码等）使用通用规范
3. **字段识别**：正确识别哪些字段可用于查询条件
4. **敏感信息**：最后一步必须检查并清理敏感信息
5. **Skill 命名**：使用简洁、有意义的英文名称
6. **查询示例**：为每个可查询字段生成相应的示例
7. **Token 有效期**：提醒用户 Token 默认 7200 秒有效期
8. **Oracle 分页**：提醒用户使用分页时添加排序条件
