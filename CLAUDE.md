# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Jekyll-based static blog (using Chirpy theme 7.3.x) for documenting algorithm problem solutions from Baekjoon Online Judge (BOJ). Solutions are written in Kotlin and organized by solved.ac CLASS difficulty levels (1-7).

- **Site URL**: https://potato-cuisine.dev
- **Repository**: hoxymola/hoxymola.github.io (GitHub Pages)

## Development Commands

### Local Development Server
```bash
bash tools/run.sh
```
Starts Jekyll with live reload at http://127.0.0.1:4000. Options: `-H [HOST]` for custom host, `-p` for production mode.

### Build and Test for Production
```bash
bash tools/test.sh
```
Builds site in production mode and runs html-proofer for link validation.

### VSCode Tasks
- **Run Jekyll Server** (default build task): `./tools/run.sh`
- **Build Jekyll Site**: `./tools/test.sh`

## Architecture

### Key Directories
- `_posts/` - Markdown blog posts (algorithm solutions)
- `_tabs/` - Static pages (about, archives, categories, tags)
- `_layouts/` - Custom layouts extending Chirpy theme
- `_includes/` - Template partials (footer, head, sidebar, post-summary, related-posts, toc-status)
- `_sass/` - SCSS stylesheets organized by component/layout/theme
- `_plugins/` - Custom Jekyll plugins (posts-lastmod-hook.rb for modification tracking)
- `tools/` - Build scripts (run.sh, test.sh)
- `assets/` - Static assets including problem-specific image folders

### Configuration
- `_config.yml` - Main Jekyll config (theme, site settings, comments via Giscus, analytics)
- `Gemfile` - Ruby dependencies (Jekyll, Chirpy theme, html-proofer)

## Post Format

Posts use this frontmatter structure:
```yaml
---
title: "[백준/코틀린] XXXX번: Problem Title"
difficulty: 브론즈 5
categories: [백준, CLASS 1]
tags: [Kotlin]
description: "Problem description"
---
```

## Post Writing Style

### 글 구조
1. **링크** - 백준 문제 링크
2. **개념 설명** (선택) - 문제에 필요한 알고리즘/자료구조 설명 (유니온 파인드, 다익스트라 등)
3. **풀이** - 문제 해결 접근법
4. **코드** - Kotlin 풀이 코드
5. **연관 문제** (선택) - 관련 문제 링크

### 풀이 설명 흐름 (SEO 최적화)

**Google 색인을 위해 모든 게시물에 충분한 설명이 필요합니다.** "생략"은 사용하지 않습니다.

**원칙:**
- `## 풀이` 섹션 하나에 웬만하면 모든 내용 작성 (불필요한 섹션 분리 금지)
- 공간복잡도는 작성하지 않음
- 시간복잡도는 명백하지 않은 경우에만 작성
- 알고리즘 개념 설명은 풀이 흐름에 자연스럽게 녹여내기

**포함할 내용:**
1. **핵심 아이디어**: 어떤 접근법을 사용하는지
2. **왜 이 접근법이 올바른지**: 정당성 근거 (그리디 문제에서 특히 중요)
3. **DP 문제**: 상태 정의를 반드시 명시 (`dp[i]`가 무엇을 의미하는지)

### 강조 및 포맷팅
- **핵심 개념 강조**: `<span class="txt_bg">핵심 키워드</span>`
- **코드/변수/복잡도**: 백틱 사용 (`` `BFS` ``, `` `O(n log n)` ``)
- **줄바꿈 없이 연결**: 줄 끝에 `\` 사용
- **섹션 간 공백**: `<br>` 태그
- **이미지**: `![alt](../assets/문제번호/파일명){: w="너비" }` 다음 줄에 `_캡션_`
- **라이트/다크 모드 이미지**: `{: .light }`, `{: .dark }` 클래스 사용
- **참고 자료**: `> [제목](URL) {: .prompt-info }`
- **연관 문제**: `## 연관 문제` 섹션에 내부 링크

### 풀이 예시
```markdown
## 풀이
<span class="txt_bg">`dp[y][x][d]`: 끝이 `(y, x)`에 있고 방향이 `d`인 파이프의 경우의 수</span>

현재 위치 `(cy, cx)`에 특정 방향의 파이프를 놓기 위해,\
이전 위치 `(py, px)`에서 올 수 있는 경우들을 역으로 계산합니다.
```

### 개념 설명 예시 (복잡한 알고리즘)
```markdown
## 유니온 파인드 (Union-Find)
<span class="txt_bg">유니온 파인드</span>란?\
두 개의 연산을 통해 서로 겹치지 않는 여러 집합을 관리하는 자료구조입니다.
- **Union**: 두 원소를 같은 집합으로 합친다.
- **Find**: 해당 원소가 속한 집합을 찾는다.
```

## Deployment

Automated via GitHub Actions (`.github/workflows/pages-deploy.yml`):
1. Checkout with full history
2. Setup Ruby 3.3 with bundler caching
3. Build site
4. Run html-proofer
5. Deploy to GitHub Pages