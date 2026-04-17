---
name: markdown-translator
description: Translate a markdown file to Chinese and save as filename_ZH.md in the same directory. Use when user asks to translate a .md file and save as filename_ZH.md.
---

# Markdown Translator Skill

Translates an English markdown file to Simplified Chinese and saves the result as `filename_ZH.md` in the same directory.

## Task

1. Read the source markdown file
2. Translate ALL content to Simplified Chinese, without exception (only preserve paths and code as-is)
3. Save the translation to `<filename>_ZH.md` in the same directory as the source file

## Translation Rules

> **Principle: Translate everything, no exceptions.** The only things preserved as-is are paths and code — everything else must be translated to Chinese.

### Must Translate

- **Headings / 标题**: Must translate (e.g., "Background" → "背景", "Summary" → "总结")
- **Body text / 正文**: Must translate to natural, fluent Simplified Chinese
- **Links / 链接**: href URL value preserved as-is, link display text must translate
- **Image references / 图片引用**: src path preserved as-is, alt text must translate
- **Tables / 表格**: Both header and all cell text must translate, table structure preserved
- **Bold/Italic / 加粗/斜体**: Keep formatting markers, text inside must translate
- **Back-navigation links / 返回导航链接**: Fully translate (e.g., "← Back to Claude Code Best Practice" → "← 返回 Claude Code 最佳实践")
- **Attribution / 作者署名**: Translate display name, preserve URL (e.g., `[@trq212](https://x.com/trq212)` stays the same — only if display name were "Thariq" would it become `[@Thariq](...)`)
- **Date mentions / 日期提及**: Translate to Chinese format (e.g., "April 16, 2026" → "2026 年 4 月 16 日")
- **Table of contents / 目录**: Fully translate
- **Badge labels / 徽章标签**: Translate any text labels (preserve image URLs in badges)

### Absolutely Do NOT Translate (Hard Rules)

1. Paths inside `src="..."` (image paths, file paths)
2. URLs inside `href="..."`
3. Anything inside code fences `` ``` ``
4. Content inside inline code `` `code` ``
5. Markdown syntax markers (`**`, `*`, `[]()`, `` ``` ``, etc.)
6. Table column/row separators (`|`, `---`)

### Translation Quality

- Maintain original tone and style (technical documentation — neutral and accurate)
- Proper nouns: on first occurrence, keep English with parenthetical Chinese translation (e.g., "Claude Code（Claude 代码）")
- Heading hierarchy must match the original exactly after translation

## Output

Write the translated markdown to `<filename>_ZH.md` in the **same directory** as the source file. Do not change the file's relative paths for images or links.

## Insight

After completing, briefly explain:
1. Key translation decisions made (e.g., preserved image paths, kept code blocks)
2. Terminology choices if any were non-obvious
