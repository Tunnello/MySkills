---
name: social-media-creator
description: Use when the user wants to create social media content for WeChat Official Account and Xiaohongshu, or mentions generating posts, articles, or image prompts for these platforms. Also triggers on slash-command-like inputs such as /social-post or when the user asks for content creation paired with image generation prompts.
---

# Social Media Creator

## Overview

This skill guides the agent through a two-step workflow to produce social media content based on a user-provided topic:

1. **Search and Confirm** — Research the topic, find a recent news hook, extract key facts with sources, and iterate with the user through 2-3 rounds of dialogue to lock in the final angle.
2. **Generate and Save** — Produce a complete WeChat Official Account article, a Xiaohongshu note, 3-6 academic-style image generation prompts with captions, and a source list. Save everything to `3pics/<topic>/内容.md`.

**Hard constraints:**
- Never fabricate information. Every key fact must be traceable to a searched source.
- **Perspective is everything.** Data is welcome, but it must serve an argument — don't just list facts. Every piece of content must have a unique, novel angle: a contrarian take, an overlooked insight, or a sharp thesis. Use data selectively to back your claims, then analyze the main product, its competitors, and its strategic advantages. Compare and contrast. Tell the reader something they don't already know.
- **Praise and criticize in equal measure.** No product or company is flawless. Every analysis must include both genuine strengths AND substantive weaknesses. Don't be a cheerleader. When the data supports it, be sharp — a well-aimed critique backed by evidence is more valuable than a dozen generic compliments. The best commentary makes the reader think "that's true and I hadn't thought of it that way," not "this reads like PR."
- Image prompts must be written entirely in Chinese. They must follow an academic presentation style: include key information (data, timelines, comparisons), use a blue/gray/white color palette, and avoid meaningless flashy backgrounds.

## Step 1: Search, Analyze, and Confirm (Dialogue + Iteration)

When triggered, immediately perform the following:

1. **Search the topic.** Use web search to find the most relevant and authoritative sources. Do not limit yourself to a specific language or domain — choose sources based on topic relevance. Prioritize: official reports, academic papers, mainstream media. If using lower-credibility sources (forums, social media), explicitly flag their reliability.

2. **Identify a news hook.** Find the most recent news event or development related to the topic. Prioritize events from the last 6 months; if nothing suitable is found within that window, explicitly tell the user the news is older and ask if they still want to use it. This hook serves as the entry point for the first image and the opening of the articles.

3. **Extract key facts.** Pull core data, trends, and viewpoints from the search results. Every fact must be paired with its source URL or citation.

4. **Analyze competitive landscape.** Research and compare the main product/company with its key competitors. Identify: product strengths and weaknesses, strategic positioning, business model differences, market share dynamics, technological moats, and overlooked competitive threats. This analysis must form the backbone of the content — not a data dump, but a sharp analytical framework.

5. **Draft a content outline with a unique thesis.** Based on the search results and competitive analysis, craft an outline for both the WeChat article and the Xiaohongshu note. The outline must be built around a clear, novel argument or insight — not a chronological summary. Examples of good theses: "Why X's real competitor isn't Y but Z", "The hidden cost of X's growth strategy", "X won the battle but may lose the war".

6. **Iterate with the user to lock in the angle.** Present to the user:
   - **Recommended news hook** (with source)
   - **Core thesis / unique angle** (1-2 sentences)
   - **Proposed outline** (WeChat + Xiaohongshu)
   - **Key competitive insights and sources**
   - Ask: "Does this angle work? Please confirm or suggest refinements."

   After the user responds, engage in 1-2 more rounds of dialogue to sharpen the angle and outline until both sides agree it's locked. Don't rush to generation — the quality of the final output depends on nailing the perspective here.

**Edge cases:**
- If search yields insufficient reliable information, state this clearly and suggest either a topic change or a broader search scope.
- If the strongest news hook has weak relevance to the topic, present 2-3 alternatives for the user to choose from.
- If the user rejects the outline twice, suggest the user either refine the topic significantly or switch to a different subject rather than looping indefinitely.

## Step 2: Generate and Save (After User Confirmation)

Once the user confirms or modifies the outline, generate the complete content and save it.

### Save location

- Directory: `3pics/<topic>/`
  - `<topic>` should be derived from the user's input, sanitized into a safe folder name (remove special characters, limit length, preserve Chinese characters).
- File: `3pics/<topic>/内容.md`
- Only Step 2 content is saved. Step 1 materials remain in the dialogue history only.

### Output structure inside 内容.md

```markdown
# <topic>

## 公众号版

### 标题
(Max 30 characters, professional depth, attention-grabbing)

### 摘要
(100-200 characters summarizing the core argument)

### 正文
(Long-form, clear structure, smooth transitions between paragraphs. Every data point must include a source citation inline or in the source section.)

## 小红书版

### 标题
(Max 20 characters, colloquial, emotional value or curiosity hook)

### 正文
(Short paragraphs, 1-3 lines each. Use emojis for readability. Avoid academic tone — write like talking to a friend.)

### 标签
5-10 relevant tags in `#tagname` format.

## 图片叙事逻辑（总览）

将每张图的叙事逻辑按图序汇总，写一段给用户看的文字。这不属于Prompt指令，是最终输出中用户会直接阅读的内容。

**核心原则：阅读体验上，是先有这段叙事逻辑，再有的图。** 叙事逻辑本身就是一个完整、自洽的小文章——没有图也能读懂论点。图是这段文字的视觉延伸，是辅助说明，不是主角。不要在文字里出现"如图X所示"这类PPT式的僵化指引，直接告诉读者"怎么回事"就行。

**写作要求：**

1. **有起承转合。** 不是"图1干了什么、图2干了什么"的流水账。要写出故事的推进感——从哪里开始（起），矛盾如何展开（承），视角或认知在何处翻转（转），最后落到哪里（合）。读者读完这一段，已经拿住了整条论述的骨架。

2. **平等的口吻，不是老师。** 你和读者是一起看这件事的人，不是你在台上他在台下。不要用"你会发现""值得注意的是""可以看出"这种居高临下的句式——它们在暗示"我知道而你不知道"。用"这里有意思的是""换个角度""仔细看数据会发现"——你在分享一个你发现的、他也可能会发现的东西。偶尔可以质疑自己刚得出的结论（"照这个逻辑推下去……但这里有个坑"），让读者感觉你和他在一起想问题，不是在做汇报。

3. **禁止套模板。** 每一篇的叙事逻辑都必须是对这篇具体主题的具体讲述。如果一个句子可以原封不动搬到另一篇关于完全不同话题的文章里，这个句子就不合格。

4. **结尾提取关键词。** 叙事逻辑正文写完后，另起一行，用 `#关键词` 格式提炼 **恰好5个** 关键词，分为两组：
   - **前2个是发现词**：用于文章在平台上的搜索发现和算法推荐。选取这个话题下流量最大、搜索最多的热词，帮助文章被更多人看到。可以是品类名、公司名、行业通用术语。
   - **后3个是判断词**：文章自己的核心判断和观点。不是事实描述，是这篇文章独有的立场和洞察。读者看到这3个词，就知道这篇文章的论点是什么、站哪边。
   
   格式：`#关键词 发现词1 #发现词2 | #判断词1 #判断词2 #判断词3`（用 `|` 分隔两组）。不要写成标签堆砌——每个词都必须有信息量。

示例感受（不是模板）：
> 豆包是中国AI的规模之王——3.45亿月活，断层第一，比行业第二到第四名加起来还大。但"最大"和"最强"是两件事。把它掰开：推理不如DeepSeek，长文本不如Kimi，企业服务不如千问。"什么都会，什么都不精"——这个悖论是整条故事线的火药。拉远看，五家国产AI排一起，五种活法五种定价权。照这个逻辑推，你可能觉得豆包悬了——付费时代谁会为"什么都会但什么都不精"付钱？但换个角度：豆包的用户到底是谁？30%在四线以下，40%用语音。这群人不是极客，是所有竞品都没碰到的普通人。判断得翻面。再往商业逻辑推：每天1.2亿算力成本，三档付费不是收割是分层。不是贪婪，是生存。走完规模、竞争、用户、付费四条线，收成一个问题：如果你不是字节，你该学什么？
>
> #关键词 豆包 #AI付费 | #什么都会什么都不精 #下沉市场才是基本盘 #不是收割是分层

## 图片 Prompts

> 每张图包含五个字段。其中**叙事逻辑**和**一句话总结**是写给人看的——用商业洞察的语言，让人读完就知道这张图在故事里的位置和核心判断。**用途**和**Prompt**是写给AI画图的指令。**配文**是图在文章中的解说。

### 图1：信息罗列与核心观点
**叙事逻辑**：开篇图——把读者拽进话题。先铺近期关键事实建立语境，再亮出核心论点制造张力。这张图回答"这篇文章要说什么？为什么要现在说？"是整个故事弧线的起点。
**用途**：First image/cover — summarize key developments from recent months related to the topic, then present the core novel viewpoint/argument. Hook the reader with a compelling thesis, not just a timeline.
**一句话总结**：(Max 100 characters, a standalone sharp insight summarizing what this image argues — not a label, but a punchy claim.)
**Prompt**：(All image prompts must be written in Chinese — the AI image generator works best with Chinese descriptions for Chinese-language content.)
**配文**：Short caption explaining the image's role in the article.

### 图2：竞品分析
**叙事逻辑**：承接图1的核心矛盾，横向展开——"在这个问题上，其他玩家在做什么？"通过对比竞品的优劣势，让图1的论点有参照系。不是一个孤立的数据表格，是对图1矛盾的横向注释。
**用途**：Competitive analysis — a single image comparing the main product/company with its key competitors. Highlight each player's strengths and weaknesses, strategic positioning, and competitive moats. Do NOT spread competitive analysis across multiple images; consolidate it here.
**一句话总结**：(Max 100 characters)
**Prompt**：...
**配文**：...

### 图3（可选）：自主选题
**叙事逻辑**：深挖层——在横向对比（图2）之后，选择一个方向纵向深入。可以是产业链利润分配、用户画像反差、商业模式脆弱性、或一个被忽略的关键变量。这张图的责任是"让人看到之前没注意到的东西"，不重复图1和图2。
**用途**：Agent-chosen topic. Pick a novel angle that supports the core thesis — data trends, hidden risks, business model comparison, technology deep-dive, or market dynamics. Use data selectively to back the point. Avoid repeating content from image 1 or 2.
**一句话总结**：(Max 100 characters)
**Prompt**：...
**配文**：...

(Continue with optional images 4-5 as needed. Each image must have its own **叙事逻辑** explaining where it sits in the story arc and why this image comes at this point in the sequence. Every image must also have a **一句话总结** within 100 characters.)

### 图N：给创业者的启示
**叙事逻辑**：收束图——从行业故事回到读者自身。前几张图讲了"发生了什么、谁在做什么、有什么问题"，这张图回答"所以呢？我怎么用？"提炼可操作的教训，是整条故事线的落脚点和价值交付。
**用途**：Final image — actionable insights and lessons for entrepreneurs and practitioners in this field. Distill the key strategic takeaways from the analysis into a concise, visual format.
**一句话总结**：(Max 100 characters)
**Prompt**：...
**配文**：Summary of the key lessons for entrepreneurs.

## 信息来源

1. [Source Title](URL) — Brief description of what was cited.
2. ...
```

### Generation rules

- **Image count**: 3-6 images.
  - **图1（固定）**：信息罗列与核心观点 — summarize recent key developments and present the article's core novel thesis. Must include both factual context AND a sharp argument.
  - **图2（固定）**：竞品分析 — a single image dedicated to competitive comparison. Cover each major competitor's strengths and weaknesses, strategic positioning, and competitive moats. Consolidate all competitive analysis here; do not spread across multiple images.
  - **图3至图N-1（自主选题）**：Agent chooses content and direction. Each image must have a distinct, non-redundant angle. Options include but are not limited to: data trends, business model deep-dive, hidden risks or contradictions, technology evolution, market dynamics, user growth analysis, or an overlooked perspective. Every image must present a novel viewpoint — do not just display data, use data to make an argument.
  - **图N（固定）**：创业者启示 — the final image must always be "给创业者的启示" (Insights for Entrepreneurs): distilled strategic lessons and actionable takeaways for practitioners in the relevant field.
- **Prompt structure for each image** (all prompts must be written in Chinese):
  - Subject description (must include key information: numbers, timelines, flowcharts).
  - Color palette requirement (blue/gray/white dominant, low saturation, no neon or flashy gradients).
  - Style description (academic presentation, infographic, clean and minimal).
  - Aspect ratio: default to 3:4 (vertical/portrait) for all images unless the user explicitly specifies a different ratio (e.g., 16:9 for a WeChat cover, 1:1 for a body image).
- **Captions**: Explain what the image illustrates and how it fits into the article/note.
- **图片叙事逻辑（总览）**：A paragraph for the user to read — not instructions, not a template. Tone must be collaborative, not lecturing: you and the reader are figuring this out together. No "你会发现/值得注意的是/可以看出" — use "换个角度"/"仔细看数据"/"这里有意思的是". Must have 起承转合. Never use AI-isms. Every sentence must be about *this* specific topic.
- **一句话总结**：Every image must include a one-sentence summary (max 100 characters). This summary must be a standalone, punchy insight — not a label like "图3：数据分析", but a sharp statement like "产业链利润被两端瓜分，创作者在中间被挤压。"

After saving, tell the user the exact file path.

## Constraints and Error Handling

| Scenario | Agent Behavior |
|----------|----------------|
| Search returns no results | Clearly inform the user. Suggest a topic change or broader keywords. Do not proceed with fabrication. |
| Search results are low quality | Flag low-credibility sources explicitly. Ask the user whether to proceed with caveats or refine the topic. |
| User rejects the Step 1 outline | Acknowledge feedback, re-search or adjust the angle, and present a revised outline. |
| Factual contradictions found during generation | Pause generation. Report the contradiction to the user and wait for resolution before continuing. |
| File save fails (missing directory / no permissions) | Attempt to create the directory first. If that fails, output the complete markdown content in the dialogue and ask the user to save it manually. |
| Topic name contains unsafe characters for a folder name | Sanitize: remove `/\\:?*"<>|`, trim whitespace, limit to 50 characters. Preserve Chinese characters. |
