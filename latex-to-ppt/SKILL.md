---
name: latex-to-ppt
description: Generates a rich, structured PPT Markdown (20-30 slides) from a LaTeX thesis or academic paper, ready for use in Gamma, Kimi, or similar AI presentation tools. Use this skill when a user wants to create a presentation from their thesis or research paper.
---

# LaTeX Thesis → PPT Markdown Generator

This skill converts a LaTeX academic thesis into a high-quality, visually structured PPT Markdown (20–30 slides) ready for import into Gamma, Kimi, Beautiful.ai, or any AI presentation generator.

## When to Use This Skill

Use this skill when the user wants to:
- Create a thesis defense presentation from their `.tex` file
- Generate structured slides from academic research
- Produce a high-quality presentation for academic/conference use

---

## Step 1: Read the Thesis

Read the LaTeX thesis file thoroughly. Extract the following:

1. **Title, Author, Supervisor, Affiliation, Date** (from cover page)
2. **Abstract** (both Chinese and English if present)
3. **Chapter structure** (all `\chapter{}` and `\section{}` headings)
4. **Core technical content** per chapter:
   - Problem motivation and significance
   - Proposed methods/models and their components
   - Key formulas and their intuitive meaning
   - Experimental setup (datasets, baselines, metrics)
   - Main experimental results (tables, figures, key numbers)
   - Ablation study highlights
   - Visualization insights (attention heatmaps, etc.)
5. **Conclusion & Future Work**
6. **Any numeric results** that make strong talking points (e.g., "MSE reduced by 6.7%")

> Tip: Pay special attention to `\caption{}` text in figures and tables — these are often the most slide-ready summaries.

---

## Step 2: Design the Slide Structure

Plan the slides before writing. A good thesis presentation has **three layers**:

1. **Context Layer** (slides 1–5): Why does this problem matter? What's been tried? What's still missing?
2. **Method Layer** (slides 6–18): What did we propose? How does it work? Walk through components modularly.
3. **Validation Layer** (slides 19–25): What did the experiments show? What does ablation reveal? What do visualizations tell us?
4. **Wrap-up** (slides 26–28): Conclusions, future directions, Q&A.

For a **two-work thesis**, allocate approximately equal depth to each work, with a clear "bridge" slide explaining the progression from Work 1 → Work 2.

---

## Step 3: Write the PPT Markdown

Write a complete Markdown document that can be directly pasted into Gamma, Kimi, or similar tools. Follow these rules:

### Structural Rules

- Use `---` to separate slides
- Use `# Title` for the slide heading
- Target **20–30 slides** total
- Use **bullet points** (not dense paragraphs) for all content
- No more than **5–6 bullets per slide**
- Each bullet should be **one sentence or a short phrase**

### Content Rules

- **Lead with numbers**: "MSE reduced by 6.7% on Electricity dataset" beats "performance improved"
- **Name modules clearly**: Use the exact names from the paper (e.g., MV-FDE, SAM, TimeBridge)
- **Contrast baseline vs. proposed**: "Standard attention → diluted weights | SAM → focused weights"
- **Use emoji or icons sparingly** to make slides visually scannable
- **For method slides**: describe what → why → how (in that order)

### Visualization Guidance

On slides showing figures/tables:
- Reference what would appear: `[Figure: Architecture diagram showing dual-path flow]`
- Describe what the visualization proves in 1 sentence
- List 2–3 key takeaways as bullets

### Slide Template Library

**Title Slide:**
```markdown
# [Paper Title]

**Author** | Supervisor: [Name] | [Affiliation]

[Degree type] · [Defense Date]

> [One-sentence thesis statement]
```

**Problem/Motivation Slide:**
```markdown
# Challenge: [Problem Name]

> [One-sentence hook about why this problem matters]

- 📍 **Real impact**: [Application domain example]
- ⚠️ **Core bottleneck**: [What goes wrong technically]
- 📊 **Scale**: [Quantify the difficulty, e.g., "100 variables = 10,000 attention pairs"]
- 🔍 **Root cause**: [Deeper technical reason the problem exists]
```

**Method Overview Slide:**
```markdown
# [Model Name]: Overview

> Design philosophy: "[Slogan or design principle]"

| Stage | Module | Role |
|-------|--------|------|
| Input | [Module A] | [What it does] |
| Encoding | [Module B] | [What it does] |
| Output | [Module C] | [What it does] |

**Key innovation**: [One-sentence differentiator vs. prior work]
```

**Module Deep-Dive Slide:**
```markdown
# [Module Name]

**Goal**: [What problem this module solves]

1. **Step 1**: [Operation] → [Output]
2. **Step 2**: [Operation] → [Output]
3. **Step 3**: [Operation] → [Output]

💡 **Insight**: [Why this design choice is better than the alternative]
```

**Experiment Results Slide:**
```markdown
# Results: [Dataset or Setting Name]

[Figure: Performance comparison table/chart]

**Key findings**:
- 🏆 [Model name] achieves best MSE of **[X.XXX]** on [Dataset]
- 📉 **[X.X]% MSE reduction** vs. [Baseline] on [Dataset]
- 📈 Advantage grows with longer horizon: L=720 shows strongest gains
- 🔑 [Any surprising or noteworthy result]
```

**Ablation Slide:**
```markdown
# Ablation Study

| Variant | [Dataset1] MSE | [Dataset2] MSE |
|---------|----------------|----------------|
| Full model | **X.XXX** | **X.XXX** |
| w/o [Module A] | X.XXX (+X.X%) | X.XXX |
| w/o [Module B] | X.XXX (+X.X%) | X.XXX |
| Baseline | X.XXX | X.XXX |

**Takeaway**: [Module A] + [Module B] = synergistic effect (1+1 > 2)
```

**Visualization/Interpretation Slide:**
```markdown
# Interpretability: [What We're Visualizing]

[Figure: Visualization image]

- 📊 **[Dataset 1]**: shows [Pattern A] → [Interpretation]
- 📊 **[Dataset 2]**: shows [Pattern B] → [Interpretation]
- 🔍 Proves: [What this visualization demonstrates about the model]
```

**Conclusion Slide:**
```markdown
# Summary & Contributions

| Work | Problem Solved | Key Method | Performance |
|------|---------------|------------|-------------|
| [Work 1] | [Problem] | [Method] | [Top metric] |
| [Work 2] | [Problem] | [Method] | [Top metric] |

**Technical Roadmap**: [Metaphor for progression, e.g., "From 1D to 2D modeling"]

> [Closing statement about impact or future direction]
```

---

## Step 4: Quality Checklist

Before delivering the slides, verify:

- [ ] Slides 1–3: Hook the audience on why the problem matters
- [ ] At least 1 slide clearly shows the **limitation of prior work** (preferably with a diagram or contrast table)  
- [ ] Method section has both an **overview** slide and **per-module** slides
- [ ] At least 2 slides with **quantitative results** (numbers, percentages)
- [ ] Ablation results are presented (shows scientific rigor)
- [ ] At least 1 **visualization** slide (attention heatmaps, fusion matrices, etc.)
- [ ] Final slide has a clear conclusion + future work
- [ ] No slide exceeds 6 bullet points
- [ ] All module names match the paper exactly
- [ ] Each slide has a clear "one thing to remember" takeaway

---

## Output Format

Return the complete PPT Markdown as a code block in a `.md` file, with this header:

```
<!-- PPT Markdown: [Paper Title] -->
<!-- Slides: [N] | Generated from: [path to .tex file] -->
<!-- Compatible with: Gamma, Kimi, Beautiful.ai, Marp -->
```

Also provide a brief **Slide Index** at the top (list of slide numbers and titles) so the user can quickly navigate.
