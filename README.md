# dev-blog

Jekyll + [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 테마로 만든 개인 기술/학업 블로그입니다.
GitHub Pages로 배포하도록 구성되어 있습니다.

## 폴더 구조

```
dev-blog/
├── _config.yml          # 사이트 설정 (제목, 작성자, URL 등)
├── Gemfile               # Ruby 의존성 (Chirpy 테마)
├── index.html            # 홈 페이지 진입점
├── _tabs/                # 상단 탭 메뉴 (소개, 카테고리, 태그, 아카이브)
├── _posts/                # 실제 발행되는 글 (파일명 형식: YYYY-MM-DD-제목.md)
└── .github/workflows/     # GitHub Actions 배포 워크플로
```

## 카테고리 체계

일자별 일지 대신, **주제/프로젝트 중심**으로 정리하기 위해 아래 규칙을 사용합니다.

- `categories: [기술, 기술명]` — 예: `[기술, React]`, `[기술, Spring]`
- `categories: [학업, 과목명]` — 예: `[학업, 알고리즘]`, `[학업, 운영체제]`
- `categories: [기술, 프로젝트명]` — 프로젝트 회고는 기술 카테고리 아래 프로젝트명을 하위 카테고리로 둡니다.

Chirpy는 카테고리를 최대 2단계까지 지원하므로, 위 방식이면 사이트의 `Categories` 탭에서
대분류(기술/학업) → 소분류(구체적 주제) 순으로 자동 정리됩니다.

예시 글 3개(`_posts/` 폴더)가 이미 이 구조를 따르고 있으니 참고하세요.

- `2026-08-01-react-useeffect-cleanup.md` — 기술 개념 정리 예시
- `2026-08-03-sorting-algorithms.md` — 학업(CS) 노트 예시
- `2026-08-05-todo-project-retrospective.md` — 프로젝트 회고 + **Velog 일지를 어떻게 재구성할지 Before/After 예시**

## 1. 로컬에서 미리보기 (선택)

```bash
bundle install
bundle exec jekyll serve
# http://localhost:4000 에서 확인
```

Ruby/Jekyll 설치가 번거롭다면 이 단계는 건너뛰고 바로 GitHub에 올린 뒤
Actions가 빌드하는 것을 확인해도 됩니다.

## 2. GitHub에 올리고 배포하기

1. GitHub에서 `<본인아이디>.github.io` 라는 이름으로 새 저장소를 만듭니다. (이 이름이어야 자동으로 사용자 사이트로 배포됩니다.)
2. 이 폴더 내용을 그 저장소에 push 합니다.
   ```bash
   cd dev-blog
   git init
   git remote add origin https://github.com/<본인아이디>/<본인아이디>.github.io.git
   git add .
   git commit -m "init: Jekyll Chirpy blog"
   git branch -M main
   git push -u origin main
   ```
3. GitHub 저장소 → **Settings → Pages** → Build and deployment 항목에서 **Source를 "GitHub Actions"**로 설정합니다.
4. `_config.yml`에서 아래 값을 본인 정보로 수정합니다.
   - `title`, `tagline`, `description`
   - `url`: `https://<본인아이디>.github.io`
   - `github.username`, `social.name`, `social.email`, `social.links`
5. `_tabs/about.md` 내용을 본인 소개로 수정합니다.
6. main 브랜치에 push하면 `.github/workflows/pages-deploy.yml`이 자동으로 빌드/배포합니다.
   Actions 탭에서 진행 상황을 확인할 수 있고, 완료되면 `https://<본인아이디>.github.io`에서 확인 가능합니다.

## 3. 새 글 작성하기

`_posts/` 폴더에 `YYYY-MM-DD-제목.md` 형식으로 파일을 만들고, 아래 형식으로 시작합니다.

```yaml
---
title: "글 제목"
date: 2026-08-10 21:00:00 +0900
categories: [기술, React]   # 또는 [학업, 과목명]
tags: [react, javascript]
---
```

## 4. 기존 Velog 글 이관 가이드

Velog는 "1일차/2일차" 식 일지라 그대로 옮기면 검색성이 떨어집니다.
`_posts/2026-08-05-todo-project-retrospective.md` 글의 하단 "마이그레이션 체크리스트" 섹션에
전체 절차가 정리되어 있습니다. 요약하면:

1. 같은 프로젝트/기술 주제로 묶인 일지 글들을 모은다.
2. "무엇을 했는가"가 아니라 "무엇을 배웠는가 / 어떤 문제를 어떻게 풀었는가" 중심으로 다시 쓴다.
3. `categories: [기술|학업, 세부주제]` 2단계 구조로 분류한다.
4. 코드 스니펫은 실제 코드에서 발췌해 정확성을 확인한다.

Velog 글 원문(마크다운/URL)을 전달해주시면, 다음 단계에서 실제 글들을 이 형식으로
직접 재구성해 드릴 수 있습니다.

## 참고 문서

- [Chirpy - Getting Started](https://chirpy.cotes.page/posts/getting-started/)
- [Chirpy - Writing a New Post](https://chirpy.cotes.page/posts/write-a-new-post/)
- [chirpy-starter 저장소](https://github.com/cotes2020/chirpy-starter)
