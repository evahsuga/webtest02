# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Japanese blog creation assistant web application (`ブログ作成支援アプリ`) that helps users generate SEO-optimized blog articles through an interactive process. The application is built as a single-page HTML application with embedded CSS and JavaScript.

## Development Commands

Since this is a static HTML application, no build process or package installation is required:

```bash
# Development server (recommended)
python -m http.server 8000
# then visit http://localhost:8000

# Alternative: open directly in browser
open index.html

# No linting or testing commands - single file architecture
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

### Critical JavaScript Functions

- `generateOutline()` - Main orchestration function that processes user input and generates article structure
- `showHeadingEditor()` - Displays heading editing interface with drag-and-drop functionality  
- `generateSEOOptimizedSections()` - Creates exactly 3 middle sections using main keyword + sub-keywords combinations
- `fetchReferenceContent()` - Attempts to fetch and analyze reference articles via WebFetch API
- `startWriting()` - Generates Markdown template with embedded hints and guidance
- `saveArticle()` / `loadArticleByIndex()` - Local storage management for drafts

### State Management Pattern

The application uses global variables and DOM manipulation:
- `window.currentReferenceContent` - Stores fetched reference article content
- `window.currentHeadings` - Manages heading editor state
- Three main UI cards (`#topicCard`, `#outlineCard`, `#editorCard`) controlled via display states

### Data Flow

1. User fills interview form → `generateOutline()` called
2. Reference URL fetched (if provided) → stored in `window.currentReferenceContent`
3. SEO-optimized outline generated using keywords → displayed in outline section
4. **Critical Flow**: User clicks "見出しを編集する" → `showHeadingEditor()` → heading modification
5. User clicks "この見出しで記事を作成" → `startWriting()` generates Markdown template
6. Template includes HTML comments with hints, keywords, and reference content
7. User writes article → can save to localStorage or preview

**Important**: The heading editing step (step 4) is mandatory for proper user workflow - this was a recent critical bug fix.

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
- Preserve the SEO optimization logic in section generation - exactly 3 middle sections (##2, ##3, ##4)
- **Critical**: Ensure heading editor functionality works properly - common issues include:
  - Button visibility problems (multiple CSS display properties required)
  - Event handler attachment (use both onclick attributes and addEventListener)
  - Proper state management for `window.currentHeadings`
- Ensure localStorage functionality remains intact for draft management
- Test the complete workflow: interview → outline → heading editing → template → writing → saving

### Common Development Issues

- **Heading Editor Bugs**: The "見出しを編集する" button requires strengthened CSS display properties and dual event handling
- **WebFetch Integration**: Reference article fetching may fail silently - always check `window.currentReferenceContent`
- **SEO Section Generation**: Must maintain exactly 3 middle sections for optimal Japanese blog structure