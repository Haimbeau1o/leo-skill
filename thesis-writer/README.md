# Thesis-Writer: 学位论文智能撰写系统

## 简介

Thesis-Writer 是一个专为计算机深度学习方向硕士论文设计的智能撰写辅助系统。系统遵循河北大学LaTeX模板规范,提供从选题到定稿的全流程支持。

## 特性

- 🎯 **五阶段渐进式流程**: 需求分析 → 大纲设计 → 内容撰写 → 润色优化 → 格式定稿
- 🤖 **AI + 人工协作模式**: AI生成建议,用户审核确认,确保质量
- 📚 **丰富知识库**: 大纲模板、写作指南、综述框架、降重策略等
- 🔧 **实用工具集**: 论文搜索、格式检查、AI检测、改写助手等
- 📖 **LaTeX模板支持**: 完全适配河北大学硕士论文模板

## 快速开始

### 方式1: 通过AI对话使用(推荐)

直接告诉AI:

```
我需要写一篇关于[你的研究方向]的硕士论文
```

或者使用命令:

```
/SKILL
```

AI将自动激活thesis-writer系统,引导你完成论文撰写。

### 方式2: 了解完整workflow

阅读详细的skill文档:

```
file:///Volumes/passport/paper/.agent/workflows/thesis-writer/SKILL.md
```

## 系统架构

```
thesis-writer/
├── SKILL.md                          # 核心skill定义
├── README.md                         # 本文件
├── scripts/                          # 辅助工具脚本
│   ├── paper_search.py              # 论文搜索
│   ├── latex_formatter.py           # LaTeX格式化
│   ├── ai_detector.py               # AI痕迹检测
│   ├── paraphrase_assistant.py      # 改写助手
│   └── thesis_checker.py            # 论文检查
├── references/                       # 参考知识库
│   ├── outline_templates.md         # 大纲模板
│   ├── writing_guide.md             # 写作指南
│   ├── related_work_framework.md    # 综述框架
│   ├── latex_guide.md               # LaTeX指南
│   ├── paraphrasing_strategies.md   # 改写策略
│   └── review_checklist.md          # 评审标准
└── templates/                        # 论文模板
    └── HBUthesis/                   # 河北大学模板
```

## 工作流程

### 论文结构类型

系统支持三种论文结构:

#### 📌 单工作结构 (传统结构)
- **适用**: 一个核心方法 + 对应实验
- **结构**: 第3章方法 + 第4章实验
- **示例**: 提出一个新的注意力机制

#### 📌 双工作结构 (知网常见) ⭐
- **适用**: 两个相关但独立的完整工作
- **结构**: 第3章工作1(方法+实验) + 第4章工作2(方法+实验)
- **示例**: 工作1提出基础模型,工作2在此基础上增加可控性
- **关系类型**:
  - 递进: 工作2基于工作1改进
  - 互补: 从不同角度解决同一问题
  - 并列: 解决相关但独立的子问题

#### 📌 多工作结构
- **适用**: 三个及以上独立工作
- **结构**: 每章一个完整工作

### 五阶段流程
- 收集研究方向、核心问题、已有成果
- 生成需求文档

### 阶段2: 大纲设计与确认
- 生成学术规范的论文大纲
- 迭代优化直到用户满意

### 阶段3: 章节撰写与填充
- 逐章节撰写内容
- 每章完成后确认

### 阶段4: 润色优化与去重
- 语言润色
- 去AI痕迹
- 查重降重

### 阶段5: 格式审查与定稿
- LaTeX格式检查
- 编译输出
- 质量审核

## 使用示例

### 示例1: 完整论文撰写

```
用户: 我需要写一篇关于多模态情感分析的硕士论文

AI: [激活thesis-writer]
让我们开始需求分析...

Q1: 你的研究聚焦于多模态情感分析的哪个具体问题?
Q2: 你打算使用什么方法?
...

[根据回答生成大纲]
[逐章节撰写]
[润色优化]
[格式定稿]
```

### 示例2: 特定阶段使用

```
用户: 我的论文初稿完成了,需要润色和降重

AI: 好的,我们直接进入阶段4(润色优化与去重)
请提供你的初稿...

[执行润色和降重]
```

## 辅助工具

### 论文搜索 (paper_search.py)

从学术数据库搜索相关论文:

```bash
python scripts/paper_search.py \
  --query "deep learning attention" \
  --venue "NeurIPS,ICML" \
  --years "2020-2024" \
  --output papers.json
```

### LaTeX格式化 (latex_formatter.py)

检查和修正LaTeX格式:

```bash
python scripts/latex_formatter.py \
  --input thesis.tex \
  --check-only
```

### AI检测 (ai_detector.py)

检测AI生成痕迹:

```bash
python scripts/ai_detector.py \
  --input thesis.tex \
  --output report.json
```

### 改写助手 (paraphrase_assistant.py)

辅助降重改写:

```bash
python scripts/paraphrase_assistant.py \
  --input segments.txt \
  --output suggestions.txt
```

### 论文检查 (thesis_checker.py)

全面质量检查:

```bash
python scripts/thesis_checker.py \
  --input thesis.tex \
  --output report.md
```

## 知识库

系统包含以下参考资源,AI会根据需要自动加载:

- **大纲模板** (`outline_templates.md`): 不同类型论文的标准结构
- **写作指南** (`writing_guide.md`): 学术语言和表达技巧
- **综述框架** (`related_work_framework.md`): 文献整理和对比方法
- **LaTeX指南** (`latex_guide.md`): 河北大学模板使用说明
- **改写策略** (`paraphrasing_strategies.md`): 降重和去AI技巧
- **评审标准** (`review_checklist.md`): 论文质量自查清单

## 依赖安装

```bash
cd /Volumes/passport/paper/.agent/workflows/thesis-writer
pip install -r requirements.txt
```

主要依赖:
- scholarly: 学术论文搜索
- arxiv: ArXiv论文API
- pylatexenc: LaTeX处理
- nltk/jieba: 文本处理

## LaTeX环境

需要安装:
- XeLaTeX编译器
- 河北大学论文模板 (已包含在templates/目录)
- 中文字体: 宋体、黑体、仿宋、楷体

## 最佳实践

1. **分阶段推进**: 不要跳跃,按五阶段顺序完成
2. **及时确认**: 每个关键节点与AI确认,避免方向错误
3. **人工把关**: 技术细节和创新点由用户审核
4. **版本管理**: 保留各阶段版本,便于回溯
5. **真实数据**: 实验结果必须真实,AI仅辅助分析

## 注意事项

⚠️ **技术准确性**: AI生成的公式和技术描述需用户验证

⚠️ **创新性**: AI无法判断创新点是否重复,需补充文献调研

⚠️ **实验结果**: 必须使用真实实验数据

⚠️ **查重标准**: 以学校指定查重系统为准

⚠️ **学术诚信**: 系统旨在辅助写作,请遵守学术规范

## 支持的论文类型

- ✅ 方法创新型: 提出新模型/新方法
- ✅ 改进型: 改进现有方法
- ✅ 综述型: 综合性对比研究
- ✅ 应用型: 应用导向研究

## FAQ

**Q: 可以用于其他学校的论文吗?**

A: 可以。虽然默认使用河北大学模板,但核心的写作流程和指导适用于所有硕士论文。只需更换LaTeX模板即可。

**Q: 可以写博士论文吗?**

A: 核心流程适用,但博士论文要求更高的创新性和深度,需要更多人工介入和调整。

**Q: AI生成的内容会被查重吗?**

A: 会。系统在阶段4专门提供去AI痕迹和降重功能,确保内容符合学术规范。

**Q: 需要自己准备什么?**

A: 需要准备:
- 研究方向和核心想法
- 已完成的实验结果(如果有)
- 参考的论文列表
- 对论文的基本构想

**Q: 能保证通过答辩吗?**

A: 系统提供专业的写作辅助,但论文质量最终取决于研究内容本身。系统帮助你规范表达,但创新性和技术深度需要你的研究工作支撑。

## 版本历史

- **v1.0** (2026-01-09): 初始版本
  - 实现五阶段完整流程
  - 提供辅助工具脚本
  - 建立知识库体系
  - 适配河北大学LaTeX模板

## 贡献

如有改进建议或发现问题,欢迎反馈!

## 许可

本系统基于skills-maker框架构建,遵循相同许可协议。

---

**准备好开始了吗?**

告诉AI你的研究方向,开启你的论文撰写之旅!

```
我需要写一篇关于[你的研究方向]的硕士论文
```
