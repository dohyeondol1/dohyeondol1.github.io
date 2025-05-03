# Hux Blog 템플릿

### 👉 [내 블로그는 여기에서 확인하세요](http://huxpro.github.io)

---

### “Page Build Warning” 이메일을 받은 경우

Jekyll이 3.0.x로 업그레이드되면서 기존 `pygments` 코드 하이라이터가 더 이상 지원되지 않고, `rouge`만 지원됩니다. 따라서 `_config.yml` 파일에서 `highlighter: rouge`로 변경해 주세요.  
또한 `gems: [jekyll-paginate]`도 `_config.yml`에 추가해야 합니다.

그리고 로컬 Jekyll 환경도 업데이트해야 합니다.

- `jekyll server`를 사용하는 경우:
  ```bash
  gem update jekyll
  gem update github-pages
  ```

- `bundle exec jekyll server`를 사용하는 경우:
  ```bash
  bundle update
  ```

참고:  
- [Using Jekyll with Pages](https://help.github.com/articles/using-jekyll-with-pages/)  
- [2.x에서 3.x로 업그레이드](http://jekyllrb.com/docs/upgrading/2-to-3/)

---

## 템플릿에 대하여 (베타)

내 블로그 저장소 `huxpro.github.io`는 자주 수정되고 커밋도 다양하므로, 안정적인 템플릿 버전으로 `huxblog-boilerplate`를 제공합니다.  
해당 템플릿을 Fork하거나 zip으로 다운로드해 수정하세요.

```bash
git clone git@github.com:Huxpro/huxblog-boilerplate.git
```

👉 **[템플릿 미리 보기](http://huangxuan.me/huxblog-boilerplate/)**

---

## 버전 특징

### 최신 버전 (V1.5.2)

- 내 정보 관련 문서 삭제가 귀찮다면, `Boilerplate` 템플릿을 사용해 쉽게 시작 가능
- `-apple-system`이 폰트에 추가되어 macOS/iOS의 San Francisco 폰트를 예쁘게 표시
- 코드 줄바꿈 문제 해결 → 가로 스크롤로 대체 (참고: [이슈#15](https://github.com/Huxpro/huxpro.github.io/issues/15))

---

## 사용법 안내

### 환경 설정

Jekyll 설치 후 다음 명령어로 로컬 서버 실행:

```bash
jekyll serve
# 또는
jekyll serve --watch
```

Jekyll 공식 문서에서는 bundler 사용 권장:  
https://help.github.com/articles/using-jekyll-with-pages/#installing-jekyll

---

### 시작하기

`_config.yml` 파일을 열어 기본 설정을 수정하세요:

```yaml
# 사이트 설정
title: Hux Blog
SEOTitle: Hux Blog
description: "Cool Blog"

# SNS 설정
github_username: huxpro
weibo_username: huxpro

# 빌드 설정
# paginate: 10
```

Jekyll 공식사이트: https://jekyllrb.com  
한글 사이트: http://jekyllcn.com

---

### 글 작성

글은 `_posts/` 폴더 안에 Markdown 파일로 작성합니다. 예시는 다음과 같습니다:

```yaml
---
layout: post
title: "Hello 2015"
subtitle: "Hello World, Hello Blog"
date: 2015-01-29 12:00:00
author: "Hux"
header-img: "img/post-bg-2015.jpg"
tags:
    - Life
---
```

`Rake`를 이용하면 새 글 템플릿을 쉽게 생성할 수 있습니다:

```bash
rake post title="Hello 2015" subtitle="Hello World, Hello Blog"
```

---

### 사이드바 설정

`_config.yml`에서 설정:

```yaml
sidebar: true
sidebar-about-description: "자기소개 간단히 작성"
sidebar-avatar: /img/avatar-hux.jpg
```

---

### 미니 자기소개

사이드바 하단에 SNS 링크들이 나타납니다. 반응형으로 화면 작아지면 아래로 이동합니다.

---

### 추천 태그

홈페이지나 글 상단에 인기 태그 표시:

```yaml
featured-tags: true
featured-condition-size: 1
```

---

### 친구 링크

`_config.yml`에서 설정:

```yaml
friends: [
    {
        title: "Foo Blog",
        href: "http://foo.github.io/"
    },
    {
        title: "Bar Blog",
        href: "http://bar.github.io"
    }
]
```

---

### HTML5 프레젠테이션 지원

Reveal.js, Impress.js, Slides 등 외부 프레젠테이션 HTML 삽입 가능:

```yaml
---
layout: keynote
iframe: "http://your-site.com/presentation"
---
```

---

### 댓글 설정

Disqus 또는 Duoshuo(多说) 지원:

```yaml
duoshuo_username: your_duoshuo_id
# 또는
disqus_username: your_disqus_id
```

Duoshuo 공유 비활성화:
```yaml
duoshuo_share: false
```

---

### 웹 분석 도구

```yaml
ba_track_id: 4cc1f2d8f3067386cc5cdb626a202900
ga_track_id: 'UA-49627206-1'
ga_domain: huangxuan.me
```

---

### 고급 사용자 설정

Grunt 사용 가능:

```bash
grunt          # 기본 빌드
grunt watch    # 실시간 감시
```

레이아웃 커스터마이징은 `_include/`, `_layouts/` 수정 + [Liquid 문법](https://github.com/Shopify/liquid/wiki) 학습 권장

---

### 제목 배경 이미지

글마다 다른 헤더 이미지 사용 가능. 어두운 계열 이미지 권장(글자색이 흰색이기 때문).

---

### SEO Title 설정

사이트 상단 제목과 검색/공유용 제목을 분리할 수 있습니다. 예:  
사이트 제목은 “Hux Blog”지만 검색 시 “황현의 블로그 | Hux Blog”로 표시하고 싶을 때 SEO title 활용.

---

## 감사의 말

1. 이 템플릿은 [startbootstrap-clean-blog-jekyll](https://github.com/IronSummitMedia/startbootstrap-clean-blog-jekyll)에서 포크하였습니다.
2. [@BrucZhaoR](https://github.com/BruceZhaoR)의 한글 번역에 감사드립니다.
3. Jekyll, GitHub Pages, Bootstrap에 감사드립니다!