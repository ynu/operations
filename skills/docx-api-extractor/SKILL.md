---
name: "docx-api-extractor"
description: "Extracts API documentation from DOCX files. Invoke when user wants to extract business API interface documentation from a Word document (.docx), especially for data platform API integration documents."
---

# DOCX API文档提取器

此技能用于从用户提供的 DOCX 文件中提取数据中台数据接口对接文档中的**3.4节业务接口部分**，其他内容不需要提取。

## 脚本说明

本 Skill 包含一个独立的 Python 脚本 `convert_docx_to_md.py`，用于将 DOCX 文件转换为 Markdown 格式。

### 脚本特点

- **独立运行**: 脚本不依赖特定的目录结构，可以复制到任何位置使用
- **参数化**: 通过命令行参数接收输入和输出文件路径
- **路径灵活**: 支持相对路径和绝对路径

### 脚本使用方法

#### 1. 直接使用（通过脚本路径）

```bash
# 如果脚本在当前目录的 scripts 文件夹中
python ./scripts/convert_docx_to_md.py <input.docx> <output.md>

# 或者使用脚本的绝对路径
python /absolute/path/to/convert_docx_to_md.py <input.docx> <output.md>
```

#### 2. 复制到项目目录使用

```bash
# 将脚本复制到项目目录
cp ./scripts/convert_docx_to_md.py ./convert_docx_to_md.py

# 在项目目录中执行
python ./convert_docx_to_md.py ./api_doc.docx ./output.md
```

#### 3. 添加到系统 PATH

```bash
# 将脚本所在目录添加到 PATH
export PATH=$PATH:/absolute/path/to/scripts

# 然后可以直接调用
convert_docx_to_md.py <input.docx> <output.md>
```

### 依赖安装

脚本依赖 `python-docx` 库：

```bash
pip install python-docx
```

## 工作流程

### 步骤 1: DOCX 转 Markdown

1. 使用 `convert_docx_to_md.py` 脚本将 DOCX 文件转换为 Markdown 格式
   - 示例命令: `python convert_docx_to_md.py <input.docx> <temp_output.md>`
   
2. 读取生成的临时 Markdown 文件内容

3. **验证文档类型**: 检查文档内容是否包含以下特征，确认是"数据中台数据接口对接文档":
   - 文档标题包含"对接文档"、"API文档"、"接口文档"等关键词
   - 包含"数据中台"、"数据平台"、"数据开放服务"等相关描述
   - 包含接口列表、应用详情、认证方式等章节
   - 包含 RESTful API 相关描述

4. **如果不是数据中台接口文档**: 终止工作流程，告知用户此文档不是有效的数据中台数据接口对接文档

### 步骤 2: 定位并提取 3.4 节业务接口

1. 在生成的 Markdown 文件中**只查找 3.4 节**（业务接口部分）
   - 3.4 节通常标题为具体的业务接口名称，如"本科生基本信息（宽表）"
   - 该部分包含：接口描述、请求路径、请求方式、请求参数表、返回参数表、返回示例

2. **提取 3.4 节内容**: 
   - 只提取 3.4 节的内容
   - **不包含**：API文件简介、应用详情、接口访问地址、获取access_token等其他章节
   - 创建新的 Markdown 文件，文件命名格式: `业务接口_<接口名称>.md`

3. **如果找不到 3.4 节**: 告知用户文档中未找到 3.4 节业务接口部分

### 步骤 3: 清理临时文件

1. 删除步骤 1 生成的临时 Markdown 文件（完整的 DOCX 转换文件）
2. 保留提取的业务接口文件

### 步骤 4: 安全检查

1. **检查敏感信息**: 仔细检查生成的业务接口 Markdown 文件中是否包含以下敏感信息：
   - App Key (key)
   - App Secret (secret)
   - access_token 示例值
   - 密码、密钥等认证信息

2. **如果发现敏感信息**: 
   - 将敏感值替换为占位符（如 `xxxxxxxxxxxxxxxx` 或 `[YOUR_KEY]`）
   - 或删除包含敏感信息的段落

3. **最终确认**: 确保输出的文件不包含任何真实的认证凭据

## 使用示例

用户请求: "帮我从这份接口文档中提取业务接口部分"

执行流程:
1. 确认用户提供的 DOCX 文件路径
2. 执行脚本转换文档: `python convert_docx_to_md.py ./input.docx ./temp.md`
3. 验证文档是数据中台接口文档
4. **只提取 3.4 节**"本科生基本信息（宽表）"的内容
5. 将提取内容保存到 `业务接口_本科生基本信息.md`
6. 删除临时转换文件 `./temp.md`
7. 检查并清理敏感信息（key、secret 等）
8. 返回提取结果

## 注意事项

- 必须严格验证文档类型，避免处理错误的文档
- **只提取 3.4 节业务接口部分**，其他章节（如认证、错误码、调用示例等）不需要提取
- 敏感信息检查是最后一步，必须仔细执行
- 如果找不到 3.4 节，需要向用户说明情况
- 脚本可以独立使用，不依赖特定的目录结构
