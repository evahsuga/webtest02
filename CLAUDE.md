# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Japanese blog creation assistant web application (`ブログ作成支援アプリ`) that helps users generate SEO-optimized blog articles through an interactive process. The application is built as a single-page HTML application with embedded CSS and JavaScript.

## Development Setup

Since this is a static HTML application, no build process or package installation is required:

```bash
# Open the application directly in a browser
open index.html
# or use a local server (recommended for development)
python -m http.server 8000
# then visit http://localhost:8000
```

## Architecture

The application follows a single-file architecture pattern with distinct functional layers:

### Core Components

1. **Topic Interview Section** (`#topicCard`) - Collects user input including:
   - Topic/theme
   - Article purpose
   - Related experience
   - Main keywords and sub-keywords (for SEO optimization)
   - Reference article URLs

2. **Outline Generation Section** (`#outlineCard`) - Displays AI-generated article structure

3. **Article Editor Section** (`#editorCard`) - Markdown-based editor with template generation

### Key JavaScript Functions

- `generateOutline()` - Main orchestration function that processes user input and generates article structure
- `generateSectionsWithReference()` - SEO-optimized section generation using keywords
- `generateSEOOptimizedSections()` - Creates exactly 3 middle sections using main keyword + sub-keywords combinations
- `fetchReferenceContent()` - Attempts to fetch and analyze reference articles
- `startWriting()` - Generates Markdown template with embedded hints and guidance
- `saveArticle()` / `loadArticleByIndex()` - Local storage management for drafts

### Data Flow

1. User fills interview form → `generateOutline()` called
2. Reference URL fetched (if provided) → stored in `window.currentReferenceContent`
3. SEO-optimized outline generated using keywords → displayed in outline section
4. User clicks "この構成で記事を書く" → `startWriting()` generates Markdown template
5. Template includes HTML comments with hints, keywords, and reference content
6. User writes article → can save to localStorage or preview

### SEO Optimization Features

The application generates article structures with:
- Main keyword integration in headings (`${mainKeyword}とは？基本的な概念を解説`)
- Sub-keyword combinations in middle sections (`${sub1}と${sub2}から学ぶ${keyword}の基本`)
- Exactly 3 middle sections (##2, ##3, ##4) plus intro and conclusion
- Reference article integration when URLs are provided

### Storage

- Uses browser localStorage for article persistence
- Articles stored with metadata (title, content, timestamps)
- No external database or backend required

## Japanese Language Context

The application is entirely in Japanese and targets Japanese blog writers. All generated content, UI text, and templates are in Japanese. When modifying the application, maintain consistency with Japanese content structure and SEO practices.

## Modification Guidelines

When making changes:
- Maintain the single-file architecture unless adding significant complexity
- Preserve the SEO optimization logic in section generation
- Keep the 3-section middle structure for optimal article flow
- Ensure localStorage functionality remains intact for draft management
- Test the full workflow: interview → outline → template → editing → saving