# 포트폴리오 사이트 구축 TODO

이 저장소(`Younsuk11/portfolio`)를 al-folio v1 스타터에서 **개인 포트폴리오 사이트**로 바꾸는 작업 기록입니다.

목표 구조 — **모두 구현됨**:

- **메인 페이지**: 이력 + 소개 + 논문/개발프로젝트 **카드**(각 카드에 상세 페이지 링크)
- **논문 상세 페이지**: `/research/<slug>/` — 논문 1편당 1페이지
- **개발 프로젝트 상세 페이지**: `/projects/<slug>/` — 프로젝트 1개당 1페이지
- 모든 콘텐츠의 원본은 [`raw/`](../raw/README.md), 웹사이트 파일은 `raw/` 바깥에 생성

> `docs/`는 `_config.yml`의 `exclude:`에 들어 있어서 이 파일은 사이트에 배포되지 않습니다.

---

## 현재 상태 (2026-08-09)

| 단계 | 내용                                                  | 상태         |
| ---- | ----------------------------------------------------- | ------------ |
| 0    | 저장소·주소·로컬 빌드 환경                            | ✅ 완료      |
| 1    | RenderCV CI 실패 해결                                 | ✅ 완료      |
| 2    | `raw/` 원본 자료 정리                                 | ✅ 완료      |
| 3    | 정보 구조 설계 (`_research` 컬렉션, 카드, 네비게이션) | ✅ 완료      |
| 4    | 데모(Einstein) 콘텐츠 제거                            | ✅ 완료      |
| 5    | `raw/` → 웹사이트 콘텐츠 생성                         | ✅ 완료      |
| 6    | 검증 및 배포                                          | 🔶 배포 직전 |

### 완성된 사이트 구조

```
/portfolio/                       about — 소개·이력 + 논문 3편 / 프로젝트 1건 카드
/portfolio/cv/                    CV (RenderCV PDF 다운로드 버튼 포함)
/portfolio/research/              논문 목록 (가로 카드 + Details/Paper/Code 버튼)
/portfolio/research/tscnf/
/portfolio/research/breaking-bad/
/portfolio/research/llm-causal-discovery/
/portfolio/publications/          papers.bib 기반 목록 (Abs/Bib/Code/HTML/Website 버튼)
/portfolio/projects/              프로젝트 목록
/portfolio/projects/meta-reviewer-helper/
```

### 자주 쓰는 명령어

```bash
cd "~/Library/CloudStorage/GoogleDrive-yuemyoun@gmail.com/내 드라이브/Git_blog/new_blog-"
bundle exec jekyll serve      # → http://localhost:4000/portfolio/
bundle exec jekyll build      # baseurl은 _config.yml에서 읽으므로 --baseurl 불필요

# CV PDF 재생성 (RenderCV는 격리 venv에 설치되어 있음)
rendercv render _data/cv.yml --settings assets/rendercv/settings.yaml
rm -f assets/rendercv/rendercv_output/*.typ    # CI와 동일하게 .typ은 지움
```

---

## 남은 일

### 배포 (6단계)

- [x] `npm ci` → `npm run lint:prettier` 통과 (`npx prettier . --write` 로 8개 파일 자동 수정 완료)
- [x] `bundle exec jekyll build` 경고 없이 성공 (약 1.4초)
- [x] 내부 링크 무결성 확인 — 깨진 링크 0개
- [x] `_site/`에 `raw/` 내용이 들어가지 않은 것 확인
- [ ] GitHub **Settings → Actions → General → Workflow permissions → Read and write**
- [ ] `main`에 push → `Deploy site` 워크플로 성공 확인
- [ ] **Settings → Pages → Source: Deploy from a branch → `gh-pages`** (`gh-pages`는 자동 생성, 직접 수정 금지)
- [ ] 실제 주소 `https://younsuk11.github.io/portfolio/` 접속 확인

### 블로킹 가능성 있는 것

- [ ] **저장소가 private임** — 개인 계정 무료 플랜에서는 GitHub Pages가 private 저장소를 게시하지 못함(Pro 이상 필요).
      `gh-pages` 브랜치는 생성돼 있지만 실제 사이트가 열리는지 확인 필요. 안 열리면 저장소를 **public으로 전환**

### 링크가 나오면 채울 것

- [ ] **Google Scholar 프로필 ID** — 이름이 아니라 `citations?user=` 뒤의 ID가 필요.
      [`_data/socials.yml`](../_data/socials.yml)의 `scholar_userid` 주석 해제 후 입력
- [ ] **UAI 2026 proceedings 링크 2건** (TSCNF, Breaking Bad) — 나오면 아래 세 군데를 함께 수정
  - [`_research/tscnf.md`](../_research/tscnf.md), [`_research/breaking-bad.md`](../_research/breaking-bad.md) — `paper_pending:` → `paper_url:`
  - [`_bibliography/papers.bib`](../_bibliography/papers.bib) — `html = {...}` 추가
  - [`_data/cv.yml`](../_data/cv.yml) — 주석 처리된 `# url:` 해제
- [ ] **TSCNF 코드 저장소** — 공개되면 `_research/tscnf.md`의 `code_pending:` → `code_url:`, `papers.bib`에 `code = {...}`

### 콘텐츠 판단이 필요한 것

- [ ] **LG CNS 업무 서술의 공개 범위** — 사내 정보에 해당하는 부분이 없는지 본인 확인
      (현재 기재: AI 리서치 / Prebuilt Agent / MCP tool — 일반적 직무 설명 수준)
- [ ] **Meta-Reviewer Helper의 "My contribution" 절 유지 여부** — 논문 3편은 기여 내용을 싣지 않기로 확정했으나,
      이건 논문이 아니라 팀 개발 프로젝트라 별도 결정이 필요. 현재는 **유지** 중
- [ ] **프로젝트 페이지 기술 스택** — PyTorch/TensorFlow 등 프레임워크가 원본 자료 어디에도 명시돼 있지 않아
      추측해서 적지 않았음. 적으려면 직접 확인 필요
- [ ] **Breaking Bad 논문을 이력서 원본에도 추가** — `raw/optional/bio/`의 CV·포트폴리오 PDF에 이 논문이 없음
      (웹사이트에는 이미 반영됨)

### 미룬 것 (의도적)

- [ ] **불필요한 상류 워크플로 정리** — 실제로 실패가 쌓이면 그때 정리
      후보: `star-history`, `lighthouse-badger`, `deploy-image`, `deploy-docker-tag`, `docker-slim`, `release`,
      `update-screenshots`, `copilot-setup-steps`, `prettier-comment-on-pr`, `axe`, `broken-links*`, `unit-tests`,
      `visual-regression`, `upgrade-check`, `codeql`, `update-tocs`.
      **반드시 남길 것**: `deploy.yml`. `update-citations`는 Scholar ID를 넣은 뒤에만 의미가 있음
- [ ] (선택) Node 20 deprecation 경고 제거 — `actions/checkout@v4` → `@v5`, `actions/setup-python@v5` → `@v6`
- [ ] (선택) **ImageMagick 미설치** — 로컬 빌드 시 `convert: command not found` 다수.
      `imagemagick.enabled: true` 때문이며 반응형 이미지 생성만 스킵됨(빌드는 성공). `brew install imagemagick`
- [ ] (선택) 로컬 폴더 이름이 아직 `new_blog-` — 저장소 이름(`portfolio`)과 맞추려면 폴더도 변경 가능

---

## 이 저장소에서 발견한 제약 (다음 작업 때 반드시 참고)

1. **Bootstrap 그리드 클래스가 전부 존재하지 않는다.** 테마의 Tailwind 빌드가 gem에서 실제로 쓰이는 클래스만 생성하므로
   `col-md-5`, `col-md-7` 등은 CSS에 없다. 사용 가능한 것은 **`col-md-2/3/4/6/8/10/12`** 뿐 — 없는 클래스를 쓰면
   조용히 무시되어 레이아웃이 세로로 쌓인다.
2. **논문 버튼 스타일은 재사용할 수 없다.** `.publications ol.bibliography li .links a.btn`에만 스코프되어 있어,
   다른 페이지에서 버튼을 쓰려면 페이지 안 `<style>` 블록으로 직접 정의해야 한다
   (`_sass/`는 AGENTS.md 스톱사인). 색은 `var(--global-text-color)` 등 테마 토큰을 쓰면 다크모드가 따라온다.
3. **gem의 `projects.liquid` / `projects_horizontal.liquid`는 카드 전체를 `<a>`로 감싼다.** 카드 안에 버튼을 넣으면
   링크가 중첩되므로, 버튼이 필요하면 카드 마크업을 페이지에 직접 써야 한다.
4. **`papers.bib`의 `website` 필드는 baseurl이 자동으로 붙지 않는다.** `/portfolio/research/...`처럼 직접 써야 하며,
   `_config.yml`의 baseurl을 바꾸면 이 필드들도 같이 고쳐야 한다.
5. **`_data/cv.yml`의 제약 3가지**
   - 최상위에 `label` / `image` / `summary` / `address`를 넣으면 RenderCV가 거부해 CI가 깨진다
   - `Awards` 항목에는 `authors`가 필수다 (RenderCV가 그 섹션을 PublicationEntry로 추론)
   - `releaseDate: 2026`처럼 연도만 쓰면 YAML이 정수로 파싱해 렌더링이 `TypeError`로 죽는다 → **따옴표 필수**
6. **CV 섹션 이름은 아무거나 쓸 수 없다.** 웹 CV 페이지는 Education / Experience / Volunteer / Awards / Publications /
   Skills / Languages / Interests / Certificates / Projects / References 만 전용 템플릿을 갖는다.
   그 외 이름(예: `Activities`)은 **`bullet` 항목으로 써야** 웹에서도 렌더링된다 — company/position 형태로 쓰면
   PDF에는 나오지만 웹에서는 빈 카드가 된다.
7. **`jekyll-archives-v2`는 블로그가 없으면 크래시한다** (`undefined method 'docs' for nil`). `_config.yml`의 플러그인
   목록에서 주석 처리해 두었다. 블로그를 다시 열려면 주석만 풀면 된다.
8. **`external_sources`는 데모 RSS를 실제로 끌어온다.** 템플릿 기본값이 medium.com/@al-folio와 blog.google 글을
   `/blog/`에 생성하고 있었다. 비워두었다.
9. **인라인 수식은 `$...$`가 아니라 `$$...$$`로 쓴다.** 한 줄에 밑줄(`_`)이 있는 인라인 수식이 두 개 있으면
   kramdown이 그 밑줄을 이탤릭 문법으로 짝지어 수식이 통째로 깨진다.
10. **`rendercv[full]`은 버전을 고정한다** ([`requirements.txt`](../requirements.txt) `==2.8`).
    미고정 시 상위 릴리스가 조용히 설치되며 스키마가 바뀌어 CI가 깨진다.

---

## 참고

- [README_KO.md](../README_KO.md) — 한국어 전체 사용 가이드
- [docs/CUSTOMIZE.md](CUSTOMIZE.md) — 페이지·논문·CV 커스터마이징 상세
- [docs/INSTALL.md](INSTALL.md) — 설치·배포
- [AGENTS.md](../AGENTS.md) — 어떤 변경이 이 저장소 소유이고 어떤 게 gem 소유인지 (스톱사인 포함)
- [raw/README.md](../raw/README.md) — 원본 자료 → 웹사이트 콘텐츠 변환 규칙
- `raw/papers/*/SUMMARY.md`, `raw/dev_projects/*/SUMMARY.md` — 각 논문·프로젝트의 정리 노트와 미결 항목
- `raw/optional/*/*.md` — 이력·학력·수상·자격증 정리 (CV의 source of truth)
