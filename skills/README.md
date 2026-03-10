# Skills 目录

本目录包含用于辅助工作的各种 Skill（技能），每个 Skill 都定义了特定的工作流程和操作指南。

## Skill 列表

### 1. docx-api-extractor

**用途**: 从 Word 文档 (.docx) 中提取数据中台数据接口对接文档中的业务接口部分。

**触发条件**: 当用户需要从 DOCX 格式的接口文档中提取业务 API 接口文档时调用。

**主要功能**:
- 将 DOCX 文件转换为 Markdown 格式
- 验证文档是否为数据中台接口文档
- 提取 3.4 节业务接口部分内容
- 自动清理临时文件
- 检查并移除敏感信息（key、secret 等）

**文件结构**:
```
docx-api-extractor/
├── SKILL.md                    # Skill 定义和使用说明
└── scripts/
    └── convert_docx_to_md.py   # DOCX 转 Markdown 脚本
```

**使用方式**:
```bash
# 使用脚本转换 DOCX 文件
python docx-api-extractor/scripts/convert_docx_to_md.py input.docx output.md
```

---

### 2. query-classroom-info

**用途**: 查询本科生教室信息。

**触发条件**: 当用户询问教室信息、查询教室、查找教室、或提到教室相关查询时立即调用。

**主要功能**:
- 从数据中台查询本科生教室信息
- 支持按教学楼、校区、教室名称、座位数等条件筛选
- 返回格式化的教室信息表格

**查询条件支持**:
- 按教学楼查询（如：文渊楼）
- 按校区查询（DL=东陆, CG=呈贡）
- 按教室名称模糊查询
- 按座位数范围查询

**数据字段**:
- 教室名称 (JASMC)
- 教学楼名称 (JXLMC)
- 校区代码 (XQDM)
- 上课座位数 (SKZWS)
- 考试座位数 (KSZWS)
- 是否允许排课/考试/借用

**文件结构**:
```
query-classroom-info/
└── SKILL.md                    # Skill 定义和使用说明
```

---

## 如何使用 Skills

1. **查看 Skill 详情**: 进入每个 Skill 的目录，阅读 `SKILL.md` 文件了解详细的使用方法
2. **按需调用**: 根据用户的请求内容，判断应该调用哪个 Skill
3. **遵循流程**: 严格按照 Skill 中定义的工作流程执行

## 添加新 Skill

如需添加新的 Skill，请：

1. 在本目录下创建新的子目录，目录名使用小写字母和连字符（如：`my-new-skill`）
2. 在子目录中创建 `SKILL.md` 文件，包含以下内容：
   - Skill 名称和描述（frontmatter 格式）
   - 详细的触发条件
   - 完整的工作流程
   - 使用示例
   - 注意事项
3. 如有配套脚本，可创建 `scripts/` 目录存放
4. 更新本 README.md，添加新 Skill 的说明

## 注意事项

- 所有 Skill 都应该有明确的触发条件，避免误调用
- Skill 的工作流程应该清晰、可执行
- 涉及敏感信息的 Skill 必须包含安全检查步骤
- 定期检查和更新 Skill，确保其有效性
