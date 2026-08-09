# al-folio 한국어 사용 가이드

`al-folio`는 연구자와 학술 조직을 위한 반응형 [Jekyll](https://jekyllrb.com/) 웹사이트 스타터입니다. 개인 소개, 논문 목록, 이력서, 프로젝트, 강의, 소식, 블로그 등을 Markdown·YAML·BibTeX 파일로 관리할 수 있습니다.

> 이 문서는 프로젝트의 [README.md](README.md)를 바탕으로 실제 사용 순서에 맞게 정리한 한국어 안내서입니다. 세부 옵션은 [공식 문서 목차](docs/README.md)를 함께 참고하세요.

## 먼저 알아둘 점

- `v1.x`의 al-folio는 테마 전체를 저장소에 복사한 구조가 아니라, 실행 기능을 버전이 지정된 Ruby 플러그인 gem으로 불러오는 **얇은 스타터(thin starter)** 입니다.
- 새 사이트는 원본 저장소를 Fork하지 말고 GitHub의 **Use this template** 기능으로 만드는 것이 권장됩니다.
- 기능 플러그인을 추가하거나 제거할 때는 반드시 [`Gemfile`](Gemfile)과 [`_config.yml`](_config.yml)을 함께 수정해야 합니다. 한쪽에만 등록하면 기능이 동작하지 않습니다.
- 레이아웃, Sass, Liquid 태그와 같은 공통 실행 코드는 대부분 플러그인이 소유합니다. 저장소별 콘텐츠와 설정은 이 프로젝트에서 수정하고, 공통 기능 자체의 변경은 해당 `al-org-dev` 플러그인 저장소에서 진행합니다. 자세한 구분은 [docs/BOUNDARIES.md](docs/BOUNDARIES.md)를 참고하세요.

## 1. 내 사이트 저장소 만들기

1. [al-folio 템플릿 생성 페이지](https://github.com/new?template_name=al-folio&template_owner=alshedivat)를 엽니다.
2. **Create a new repository**를 선택합니다.
3. 원하는 주소에 따라 저장소 이름을 정합니다.
   - 개인 또는 조직 홈페이지: `<GitHub-사용자명>.github.io`
   - 프로젝트 홈페이지: 원하는 저장소 이름(예: `research-lab`)
4. **Create repository from template**을 눌러 독립된 저장소를 만듭니다.

이미 Fork했다면 그대로 사용할 수 있습니다. 다만 개인 사이트 변경 사항을 원본 `alshedivat/al-folio`에 실수로 Pull Request하지 않도록 별도 브랜치에서 작업하고, Push 대상이 자신의 저장소인지 확인하세요.

## 2. 기본 정보 설정하기

[`_config.yml`](_config.yml)을 열어 최소한 다음 값을 자신의 정보로 바꿉니다.

```yaml
title: 나의 연구 홈페이지
first_name: Gil-Dong
last_name: Hong
lang: ko

url: https://your-username.github.io
baseurl:
```

배포 형태에 따라 `url`과 `baseurl`은 다음과 같이 설정합니다.

| 배포 형태        | 저장소 이름               | `url`                             | `baseurl`       | 최종 주소                                       |
| ---------------- | ------------------------- | --------------------------------- | --------------- | ----------------------------------------------- |
| 개인/조직 사이트 | `your-username.github.io` | `https://your-username.github.io` | 비워 둠         | `https://your-username.github.io`               |
| 프로젝트 사이트  | 예: `research-lab`        | `https://your-username.github.io` | `/research-lab` | `https://your-username.github.io/research-lab/` |

`baseurl`이 비어 있더라도 키 자체는 삭제하지 마세요. 이 원본 저장소의 기본값은 `/al-folio`이므로, 현재 설정 그대로 실행하면 주소에도 `/al-folio/`가 붙습니다.

자주 사용하는 화면 설정도 같은 파일에서 바꿀 수 있습니다.

```yaml
back_to_top: true
footer_fixed: true
navbar_fixed: true
max_width: 930px
search_enabled: true
```

전체 설정 설명은 [사용자 정의 가이드](docs/CUSTOMIZE.md#configuration)를 참고하세요.

## 3. 로컬에서 실행하기

### Docker 사용(권장)

[Docker](https://docs.docker.com/get-docker/)와 Docker Compose를 설치한 뒤 저장소 루트에서 실행합니다.

```bash
docker compose pull
docker compose up
```

이 저장소의 기본 설정에서는 브라우저로 `http://localhost:8080/al-folio/`에 접속합니다. 자신이 만든 개인 사이트에서 `baseurl`을 비웠다면 `http://localhost:8080/`로 접속합니다. 파일을 수정하면 잠시 후 결과가 자동으로 반영됩니다.

백그라운드 실행과 종료는 다음 명령을 사용합니다.

```bash
docker compose up -d
docker compose logs --tail=80
docker compose down
```

### Ruby/Jekyll로 직접 실행

직접 실행 방식은 레거시 경로이며 Docker가 더 권장됩니다. Ruby와 Bundler가 준비되어 있다면 다음과 같이 실행할 수 있습니다.

```bash
bundle install
./bin/setup-python-deps  # Jupyter 포스트를 사용할 때 권장
bundle exec jekyll serve
```

이 저장소의 기본 설정에서는 `http://localhost:4000/al-folio/`로 접속합니다. 자세한 환경별 설치 방법은 [설치 및 배포 문서](docs/INSTALL.md)를 참고하세요. Windows에서는 WSL 사용이 권장됩니다.

## 4. 콘텐츠 작성하기

대부분의 콘텐츠는 기존 예제 파일을 복사한 뒤 front matter와 본문을 수정하는 방식으로 만들 수 있습니다.

| 목적          | 수정 위치                                                                                | 사용 방법                                               |
| ------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| 소개와 프로필 | [`_pages/about.md`](_pages/about.md), `assets/img/prof_pic.jpg`                          | 자기소개를 작성하고 프로필 이미지를 교체                |
| 소셜 링크     | [`_data/socials.yml`](_data/socials.yml)                                                 | 이메일, GitHub, LinkedIn 등 계정 입력                   |
| 일반 페이지   | [`_pages/`](_pages/)                                                                     | 기존 페이지를 복사하고 `title`, `permalink`, `nav` 수정 |
| 블로그 글     | [`_posts/`](_posts/)                                                                     | `YYYY-MM-DD-title.md` 형식으로 파일 생성                |
| 프로젝트      | [`_projects/`](_projects/)                                                               | 기존 프로젝트 Markdown을 복사하여 수정                  |
| 소식          | [`_news/`](_news/)                                                                       | 인라인 소식 또는 링크형 소식 추가                       |
| 논문          | [`_bibliography/papers.bib`](_bibliography/papers.bib)                                   | BibTeX 항목 추가; PDF는 `assets/pdf/`에 저장 가능       |
| 이력서        | [`_data/cv.yml`](_data/cv.yml) 또는 [`assets/json/resume.json`](assets/json/resume.json) | RenderCV 또는 JSONResume 형식으로 작성                  |
| 강의·도서     | [`_teachings/`](_teachings/), [`_books/`](_books/)                                       | 컬렉션 예제를 복사하여 수정                             |

블로그 글의 기본 예시는 다음과 같습니다.

```markdown
---
layout: post
title: 첫 번째 글
date: 2026-08-09 10:00:00
description: 글에 대한 짧은 설명
tags: jekyll research
categories: guide
---

여기에 Markdown으로 본문을 작성합니다.
```

작성 기능은 수식과 TikZ, 코드 하이라이팅, Jupyter Notebook, Mermaid·Plotly 차트, 이미지 갤러리, 영상·오디오 삽입 등을 지원합니다. 자세한 예시는 [사용자 정의 가이드](docs/CUSTOMIZE.md#creating-new-blog-posts)에서 확인할 수 있습니다.

## 5. 주요 기능 설정하기

기능은 플러그인이 설치되어 있고, `_config.yml`의 기능 플래그가 켜져 있으며, 필요한 경우 페이지가 해당 기능을 사용하도록 설정되어야 표시됩니다. API 키나 ID가 필요한 기능은 플러그인이 포함되어 있어도 값을 입력하기 전까지 동작하지 않습니다.

| 기능                | 설정 위치                                                                                                              |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 전체 검색           | `_config.yml`의 `search_enabled`                                                                                       |
| 댓글(Giscus)        | `_config.yml`의 `giscus:`                                                                                              |
| Google 등 분석 도구 | [`docs/ANALYTICS.md`](docs/ANALYTICS.md)                                                                               |
| 뉴스레터            | `_config.yml`의 `newsletter:`                                                                                          |
| 쿠키 동의           | [GDPR 쿠키 설정](docs/CUSTOMIZE.md#gdpr-cookie-consent-dialog)                                                         |
| 논문·인용 표시      | [`_bibliography/papers.bib`](_bibliography/papers.bib) 및 [논문 추가 안내](docs/CUSTOMIZE.md#adding-a-new-publication) |
| GitHub 저장소 카드  | [사용자 및 저장소 정보 설정](docs/CUSTOMIZE.md#modifying-the-user-and-repository-information)                          |
| CV                  | [CV 정보 수정](docs/CUSTOMIZE.md#modifying-the-cv-information)                                                         |

플러그인 목록과 각 기능의 소유 저장소는 [README의 Plugin Ecosystem](README.md#plugin-ecosystem) 및 [경계 문서](docs/BOUNDARIES.md)를 참고하세요.

## 6. GitHub Pages에 배포하기

1. 자신의 GitHub 저장소에서 **Settings → Actions → General → Workflow permissions**로 이동합니다.
2. **Read and write permissions**를 선택하고 저장합니다.
3. 변경 사항을 `main` 브랜치에 Commit하고 Push합니다.
4. **Actions** 탭에서 `Deploy site` 워크플로가 성공할 때까지 기다립니다.
5. **Settings → Pages → Build and deployment**에서 배포 Source를 브랜치로 설정합니다.
6. 배포 브랜치는 `main`이 아닌 **`gh-pages`**를 선택합니다. 이 브랜치는 자동 생성되므로 직접 수정하지 마세요.
7. Pages 배포가 끝나면 설정한 주소로 접속합니다.

이후 `main` 브랜치에 Push할 때마다 GitHub Actions가 사이트를 자동으로 다시 배포합니다. 수동 재배포가 필요하면 **Actions → Deploy → Run workflow**를 사용하세요.

Netlify나 별도 서버에 배포하려면 [배포 문서](docs/INSTALL.md#deployment)를 참고하세요.

## 7. 변경 사항 점검하기

일반적인 콘텐츠 수정은 로컬 미리보기로 확인하면 됩니다. 저장소 자체에 기여하거나 플러그인 설정을 바꾸는 경우에는 변경 범위에 맞추어 다음 검사를 실행합니다.

```bash
npm ci
npm run lint:prettier
npm run lint:style-contract
bundle exec jekyll build --baseurl /al-folio
```

플러그인 변경 후에는 두 등록 위치가 일치하는지 확인하고 업그레이드 감사를 실행하는 것이 좋습니다.

```bash
bundle exec al-folio upgrade audit
bundle exec al-folio upgrade overrides audit
bundle exec al-folio upgrade report
```

## 문제 해결과 추가 문서

- [5분 빠른 시작](docs/QUICKSTART.md)
- [설치 및 배포](docs/INSTALL.md)
- [사용자 정의](docs/CUSTOMIZE.md)
- [문제 해결](docs/TROUBLESHOOTING.md)
- [자주 묻는 질문](docs/FAQ.md)
- [검색 엔진 최적화](docs/SEO.md)
- [사용 사례 모음](docs/SHOWCASE.md)

## 라이선스

al-folio는 [MIT License](LICENSE)에 따라 사용할 수 있는 오픈 소스 프로젝트입니다.
