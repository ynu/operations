# Trae Skills

本目录包含用于 Trae IDE 的 Skills，用于数据中台数据查询和文档处理。

## Skills 列表

| Skill | 描述 |
|-------|------|
| `docx-api-extractor` | 从 DOCX 接口文档中提取业务接口信息 |
| `graduate-query` | 研究生基本信息查询（curl） |
| `query-classroom-info` | 本科生教室信息查询 |
| `skill-generator` | 从 DOCX 文档自动生成查询 Skill |
| `teacher-query` | 教职工基本信息查询（curl） |
| `undergraduate-query` | 本科生基本信息查询（curl） |

## 使用方式

将所需 Skill 文件夹复制到项目的 `.trae/skills/` 目录下即可使用。

## 依赖

部分 Skill 需要 Python 依赖：

```bash
pip install python-docx
```
