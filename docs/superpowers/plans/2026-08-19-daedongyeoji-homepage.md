# 대동여지 홈페이지 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 대동여지(광고대행사)의 레퍼런스 확인용 원페이지 정적 홈페이지(index.html + style.css + script.js)를 빌드 도구 없이 완성하고, 무료 배포(GitHub+Vercel) 가이드를 README로 제공한다.

**Architecture:** 단일 HTML 파일에 7개 섹션(nav, hero, about, services, portfolio, clients, contact/footer)을 순서대로 배치하는 원페이지 스크롤 구조. CSS는 다크(hero, about, contact)와 라이트(services, portfolio, clients) 두 톤을 골드(#c9a86a) 포인트 컬러로 연결한다. JS는 모바일 메뉴 토글 하나만 담당.

**Tech Stack:** 순수 HTML5 / CSS3 / Vanilla JS. 빌드 도구, 프레임워크, 패키지 매니저, 외부 CDN 의존 없음.

## Global Constraints

- 빌드 도구/프레임워크/백엔드 없음 — 정적 파일 3개(index.html, style.css, script.js)로만 구성
- 문의는 `mailto:`/`tel:` 링크만 사용, 폼/백엔드 금지
- 반응형 브레이크포인트는 768px 하나만 사용 (모바일 1열, PC 다열)
- 다크 톤: Hero·회사소개·Contact 섹션 / 라이트 톤: 사업영역·레퍼런스·클라이언트 섹션 — 골드(`#c9a86a`)를 공통 포인트 컬러로 유지
- 모든 카피는 `docs/250625_●[대동여지] Credential.pdf` 원문 내용에 근거 (수치/문구 임의 변경 금지)
- SEO, 애널리틱스, 커스텀 도메인, CMS는 범위 밖 — 추가하지 않음
- 웹폰트는 시스템 폰트 스택만 사용 (외부 폰트 CDN 금지 — 정적 호스팅에서 안정성 우선)

---

### Task 1: HTML 스켈레톤 + Hero + 회사소개 섹션

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: `<!-- SECTIONS_END -->` 마커 — Task 3, 4, 5가 이 마커 앞에 섹션을 삽입한다. `id="navLinks"`, `id="navToggle"` — Task 6의 script.js가 참조하는 DOM id.

- [ ] **Step 1: index.html 작성**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>대동여지 | Comm. Solution Agency</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<header class="nav">
  <div class="nav-inner">
    <a href="#hero" class="logo">대동여지</a>
    <nav class="nav-links" id="navLinks">
      <a href="#about">회사소개</a>
      <a href="#services">사업영역</a>
      <a href="#portfolio">레퍼런스</a>
      <a href="#clients">클라이언트</a>
      <a href="#contact">문의</a>
    </nav>
    <button class="nav-toggle" id="navToggle" aria-label="메뉴 열기">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>

<section id="hero" class="hero">
  <div class="hero-inner">
    <p class="hero-eyebrow">Comm. Solution Agency</p>
    <h1 class="hero-title">대동여지</h1>
    <p class="hero-tagline">Beyond Communications,<br>We Forge Custom-made Solutions.</p>
  </div>
</section>

<section id="about" class="about">
  <div class="section-inner">
    <p class="section-eyebrow">Our Origin</p>
    <div class="about-grid">
      <p class="about-text">
        대동여지도는 지리정보 제공이라는 수준에서 나아가
        지리학적, 사회적, 예술적 역량 등이 집약된 혁신적인 종합 정보전달 매체였으며
        제작기법과 표현에 있어서 창의성과 완성도가 극대화된 작품입니다.
      </p>
      <p class="about-text">
        '대동여지'는 대동여지도에서 모티브를 받은 社名으로,
        독창적인 시각과 차별화된 아이디어를 위한 부단한 노력은 물론
        클라이언트에게 꼭 필요한 솔루션을 제공하겠다는 의지를 담고 있습니다.
      </p>
    </div>
    <div class="mission">
      <p class="section-eyebrow">Mission</p>
      <p class="mission-title">Beyond Communications, We Forge Custom-made Solutions.</p>
      <p class="mission-sub">우리는 커뮤니케이션을 넘어 문제해결을 위한 최적화된 솔루션을 지향합니다.</p>
    </div>
  </div>
</section>

<!-- SECTIONS_END -->

<script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: 파일 내용 검증**

Run: `grep -c "SECTIONS_END" index.html`
Expected: `1`

Run: `grep -c "navToggle" index.html`
Expected: `2` (버튼 id + 참조 없음이면 1이어도 무방, 최소 1 이상)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add HTML skeleton with hero and about sections"
```

---

### Task 2: CSS 파운데이션 (변수, 리셋, nav, hero, about 다크 스타일, 반응형 기초)

**Files:**
- Create: `style.css`

**Interfaces:**
- Consumes: Task 1의 `index.html` 클래스명 (`.nav`, `.hero`, `.about` 등)
- Produces: `/* STYLES_END */` 마커 — Task 3, 4, 5가 이 마커 앞에 CSS를 추가한다. CSS 변수 `--dark-bg`, `--dark-bg-alt`, `--gold`, `--white`, `--light-bg`, `--light-bg-alt`, `--text-dark`, `--text-muted`, `--border`, `--max-width` — 이후 모든 CSS 작업에서 재사용. `.section-inner`, `.section-eyebrow`, `.section-title` 공통 클래스 — 모든 섹션이 재사용.

- [ ] **Step 1: style.css 작성**

```css
* { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior: smooth; }
body {
  font-family: -apple-system, BlinkMacSystemFont, "Malgun Gothic", "Apple SD Gothic Neo", sans-serif;
  color: var(--text-dark);
  background: var(--light-bg);
  line-height: 1.6;
}
a { text-decoration:none; color:inherit; }
ul { list-style:none; }
img { max-width:100%; display:block; }

:root {
  --dark-bg: #1b120c;
  --dark-bg-alt: #2a1c12;
  --gold: #c9a86a;
  --white: #f5f1ea;
  --light-bg: #faf8f5;
  --light-bg-alt: #f1ede6;
  --text-dark: #2b2620;
  --text-muted: #6b6258;
  --border: #ddd6ca;
  --max-width: 1200px;
}

.section-inner { max-width: var(--max-width); margin:0 auto; padding: 80px 24px; }
.section-eyebrow { color: var(--gold); letter-spacing:.08em; text-transform:uppercase; font-size:.85rem; margin-bottom:16px; }
.section-title { font-size: clamp(1.6rem,3vw,2.2rem); margin-bottom:48px; color: var(--text-dark); }

/* Nav */
.nav { position: sticky; top:0; z-index:100; background: rgba(27,18,12,0.92); backdrop-filter: blur(6px); }
.nav-inner { max-width: var(--max-width); margin:0 auto; padding: 16px 24px; display:flex; align-items:center; justify-content:space-between; }
.logo { color: var(--white); font-size:1.25rem; font-weight:700; letter-spacing:.05em; }
.nav-links { display:flex; gap:32px; }
.nav-links a { color: var(--white); font-size:.95rem; opacity:.85; transition: opacity .2s; }
.nav-links a:hover { opacity:1; color: var(--gold); }
.nav-toggle { display:none; flex-direction:column; gap:5px; background:none; border:none; cursor:pointer; padding:8px; }
.nav-toggle span { width:22px; height:2px; background: var(--white); }

/* Hero */
.hero {
  min-height: 92vh; display:flex; align-items:center; justify-content:center; text-align:center;
  background: radial-gradient(ellipse at center, var(--dark-bg-alt) 0%, var(--dark-bg) 70%);
  color: var(--white); position:relative; overflow:hidden;
}
.hero::before {
  content:""; position:absolute; inset:0;
  background-image:
    repeating-linear-gradient(45deg, rgba(201,168,106,0.05) 0 2px, transparent 2px 40px),
    repeating-linear-gradient(-45deg, rgba(201,168,106,0.04) 0 2px, transparent 2px 60px);
  pointer-events:none;
}
.hero-inner { position:relative; z-index:1; padding: 24px; }
.hero-eyebrow { color: var(--gold); letter-spacing:.2em; text-transform:uppercase; margin-bottom:20px; font-size:.9rem; }
.hero-title { font-size: clamp(3rem, 10vw, 6rem); font-weight:800; letter-spacing:.02em; margin-bottom:24px; }
.hero-tagline { font-size: clamp(1.1rem, 2.5vw, 1.5rem); color: var(--white); opacity:.9; }

/* About (dark) */
.about { background: var(--dark-bg); color: var(--white); }
.about .section-title { color: var(--white); }
.about-grid { display:grid; grid-template-columns: 1fr 1fr; gap:40px; margin-bottom:64px; }
.about-text { opacity:.85; }
.mission { border-top: 1px solid rgba(255,255,255,0.15); padding-top:48px; }
.mission-title { font-size: clamp(1.4rem,3vw,2rem); font-weight:700; margin: 16px 0; }
.mission-sub { opacity:.8; }

@media (max-width: 768px) {
  .nav-links {
    position:absolute; top:100%; left:0; right:0; flex-direction:column;
    background: var(--dark-bg); padding:16px 24px; gap:16px; display:none;
  }
  .nav-links.open { display:flex; }
  .nav-toggle { display:flex; }
  .about-grid { grid-template-columns: 1fr; }
}

/* STYLES_END */
```

- [ ] **Step 2: 브라우저로 렌더링 확인**

`preview_start`로 `index.html`을 파일 경로(`file:///` + 절대경로)로 열고 `read_page` 또는 `get_page_text`로 "대동여지", "Beyond Communications", "대동여지도는" 텍스트가 보이는지 확인한다.

Expected: 다크 배경의 히어로 섹션과 회사소개 섹션이 렌더링되고 위 텍스트가 모두 나타남.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "Add CSS foundation: variables, nav, hero, about dark theme"
```

---

### Task 3: 사업영역 5개 섹션 (라이트 톤)

**Files:**
- Modify: `index.html` (Task 1의 `<!-- SECTIONS_END -->` 마커 앞에 삽입)
- Modify: `style.css` (Task 2의 `/* STYLES_END */` 마커 앞에 삽입)

**Interfaces:**
- Consumes: Task 2의 CSS 변수, `.section-inner`, `.section-eyebrow`, `.section-title`
- Produces: `#services` 섹션 — Task 8 검증에서 참조

- [ ] **Step 1: index.html에 사업영역 섹션 삽입**

`<!-- SECTIONS_END -->`를 아래로 교체:

```html
<section id="services" class="services">
  <div class="section-inner">
    <p class="section-eyebrow">주요 사업 제안</p>
    <h2 class="section-title">주요 제공 솔루션</h2>
    <div class="service-grid">
      <article class="service-card">
        <h3>브랜드전략 및 광고제작</h3>
        <p class="service-quote">"성공 캠페인으로 증명합니다"</p>
        <p>BTL / Event Promotion / PR 연동까지 아우르는, 성공 사례로 증명하는 전략과 Creative 솔루션을 제공합니다.</p>
      </article>
      <article class="service-card">
        <h3>매체구매대행 (Media Buying Rep)</h3>
        <p class="service-quote">Special Offer</p>
        <p>훌륭한 매체구매 결과는 기본, 클라이언트/기 광고대행사에게 기존 대비 약 3~5%+@ 수준의 추가 Benefit(광고집행 물량 증대)을 실현합니다.</p>
      </article>
      <article class="service-card">
        <h3>New Media — HESTIA</h3>
        <p class="service-quote">완전히 색다르게, Media styling</p>
        <p>서울 강남/홍대 지역을 순환 운행하는 LED On-Road 빌보드. 1x1mm 미세 LED를 4mm pitch로 밀집한 FHD급 화질과 Solar Visual Enhancer로 주야 관계없이 강력한 퍼포먼스를 제공합니다.</p>
      </article>
      <article class="service-card">
        <h3>OOH 옥외매체</h3>
        <p class="service-quote">제안부터 실행까지 빈틈없이</p>
        <p>도산대로/강남대로 등 서울 Digital Sign Board, 지하철 1~4호선 58개 역사 847개 광고면 스크린도어, 53개 역사 100개소 지하철 출구매체(Metro Gate)까지 서울 도심 및 지하철 고객 접점을 커버합니다.</p>
      </article>
      <article class="service-card">
        <h3>약국 디스플레이 매체</h3>
        <p class="service-quote">국내 최초, 약국 유일 영상광고 매체</p>
        <p>건강에 대한 관여도가 최고조인 순간에 노출 가능한 국내 유일의 약국 Display Media. 전국 1,100대, 일 11만회 View, 일 204,600명에게 도달합니다.</p>
      </article>
    </div>
  </div>
</section>

<!-- SECTIONS_END -->
```

- [ ] **Step 2: style.css에 사업영역 스타일 삽입**

`/* STYLES_END */`를 아래로 교체:

```css
.services { background: var(--light-bg); }
.service-grid { display:grid; grid-template-columns: repeat(3, 1fr); gap:24px; }
.service-card { background: var(--white); border:1px solid var(--border); border-radius:12px; padding:32px; }
.service-card h3 { font-size:1.15rem; margin-bottom:8px; }
.service-quote { color: var(--gold); font-weight:600; font-size:.9rem; margin-bottom:12px; }
.service-card p:not(.service-quote) { color: var(--text-muted); font-size:.95rem; }

@media (max-width:768px) {
  .service-grid { grid-template-columns: 1fr; }
}

/* STYLES_END */
```

- [ ] **Step 3: 검증**

Run: `grep -c "약국 디스플레이" index.html`
Expected: `1`

브라우저에서 사업영역 5개 카드가 3열(PC) / 1열(768px 이하)로 렌더링되는지 `resize_window` preset `desktop`과 `mobile`로 각각 확인.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "Add services section with 5 business areas"
```

---

### Task 4: 대표 레퍼런스 포트폴리오 그리드

**Files:**
- Modify: `index.html`
- Modify: `style.css`

**Interfaces:**
- Consumes: Task 2/3의 CSS 변수 및 공통 클래스
- Produces: `#portfolio` 섹션 — Task 8 검증에서 참조

- [ ] **Step 1: index.html에 포트폴리오 섹션 삽입**

`<!-- SECTIONS_END -->`를 아래로 교체:

```html
<section id="portfolio" class="portfolio">
  <div class="section-inner">
    <p class="section-eyebrow">Representative Case</p>
    <h2 class="section-title">대표 레퍼런스</h2>
    <div class="portfolio-grid">
      <article class="portfolio-card">
        <h3>대웅제약 우루사</h3>
        <p>'피로=간=우루사' 인식상의 연결고리 구축, 대한민국 대표 리브랜딩 사례 (전년 대비 약 167% 매출)</p>
      </article>
      <article class="portfolio-card">
        <h3>유한양행 해피홈</h3>
        <p>에프킬라·홈키파 양사 MS 85% 독과점 시장에서 3자 구도로 시장 재정립 (전년대비 약 139% 매출)</p>
      </article>
      <article class="portfolio-card">
        <h3>한국인삼공사 정관장</h3>
        <p>기존 '홍삼=활력' 중심 이미지에서 '홍삼=면역력'으로 기대 효능을 확대 (전년대비 약 120% 매출)</p>
      </article>
      <article class="portfolio-card">
        <h3>이니스프리</h3>
        <p>자연주의 브랜드 컨셉과 밀레니얼 세대의 '경험을 공유'하는 특징을 Big Idea로 하는 연중 캠페인 진행</p>
      </article>
      <article class="portfolio-card">
        <h3>LG전자</h3>
        <p>김치냉장고 김치톡톡 / 얼음정수기냉장고 캠페인</p>
      </article>
      <article class="portfolio-card">
        <h3>KT</h3>
        <p>001 / 기업전용 LTE / Y Teen 캠페인</p>
      </article>
      <article class="portfolio-card">
        <h3>기아자동차</h3>
        <p>SORENTO / SOUL 캠페인</p>
      </article>
      <article class="portfolio-card">
        <h3>데상트코리아</h3>
        <p>DESCENT 브랜드 캠페인</p>
      </article>
      <article class="portfolio-card">
        <h3>신한은행</h3>
        <p>기업 캠페인 및 브랜드 커뮤니케이션</p>
      </article>
      <article class="portfolio-card">
        <h3>대상 청정원</h3>
        <p>순창 고추장 등 브랜드 캠페인</p>
      </article>
    </div>
  </div>
</section>

<!-- SECTIONS_END -->
```

- [ ] **Step 2: style.css에 포트폴리오 스타일 삽입**

`/* STYLES_END */`를 아래로 교체:

```css
.portfolio { background: var(--light-bg-alt); }
.portfolio-grid { display:grid; grid-template-columns: repeat(auto-fit, minmax(260px,1fr)); gap:20px; }
.portfolio-card { background: var(--white); border:1px solid var(--border); border-radius:10px; padding:24px; }
.portfolio-card h3 { font-size:1.05rem; margin-bottom:8px; color: var(--text-dark); }
.portfolio-card p { color: var(--text-muted); font-size:.9rem; }

/* STYLES_END */
```

- [ ] **Step 3: 검증**

Run: `grep -c "portfolio-card" index.html`
Expected: `10`

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "Add portfolio section with representative cases"
```

---

### Task 5: 클라이언트 리스트 + Contact + Footer

**Files:**
- Modify: `index.html`
- Modify: `style.css`

**Interfaces:**
- Consumes: Task 2의 CSS 변수 및 공통 클래스
- Produces: `#clients`, `#contact` 섹션, `<footer>` — Task 8 검증에서 참조

- [ ] **Step 1: index.html에 클라이언트/Contact/Footer 삽입**

`<!-- SECTIONS_END -->`를 아래로 교체 (마커 자체는 제거하고 `<script>` 태그 앞으로 이동):

```html
<section id="clients" class="clients">
  <div class="section-inner">
    <p class="section-eyebrow">Client / Brand</p>
    <h2 class="section-title">클라이언트</h2>
    <ul class="clients-list">
      <li>LG전자</li><li>엔케이맥스</li><li>이니스프리</li><li>유한양행</li><li>YES24</li>
      <li>KT</li><li>기아자동차</li><li>대웅제약</li><li>한국인삼공사</li><li>데상트코리아</li>
      <li>올림푸스코리아</li><li>SK케미칼</li><li>금호아시아나그룹</li><li>대우건설</li>
      <li>대상 청정원</li><li>아벤트코리아</li><li>한성종합건설</li><li>신한은행</li><li>페리노</li>
    </ul>
  </div>
</section>

<section id="contact" class="contact">
  <div class="section-inner">
    <p class="section-eyebrow">Contact</p>
    <h2 class="section-title">문의하기</h2>
    <div class="contact-card">
      <p class="contact-name">신정균 이사</p>
      <p class="contact-career">BBDO코리아, 원더랩 / Inter PlayGround / 휘닉스커뮤니케이션즈 / 웰콤퍼블리시스월드와이드 / 상암커뮤니케이션즈 / 솔트커뮤니케이션즈</p>
      <p class="contact-cert">IAA Diploma (국제광고인협회/한국방송광고공사) · 브랜드전문가 고급과정 이수 (산업자원부 산업정책연구원)</p>
      <div class="contact-buttons">
        <a class="btn btn-primary" href="mailto:great.east.land@gmail.com">이메일 보내기</a>
        <a class="btn btn-outline" href="tel:010-8940-8595">전화 문의 010-8940-8595</a>
      </div>
    </div>
  </div>
</section>

<footer class="footer">
  <p>대동여지 · Comm. Solution Agency</p>
  <p>&copy; 2026 Daedongyeoji. All rights reserved.</p>
</footer>
```

- [ ] **Step 2: style.css에 클라이언트/Contact/Footer 스타일 삽입**

`/* STYLES_END */`를 아래로 교체 (이번이 CSS 마지막 블록이므로 마커는 제거):

```css
.clients { background: var(--light-bg); }
.clients-list { display:flex; flex-wrap:wrap; gap:12px 16px; }
.clients-list li { background: var(--white); border:1px solid var(--border); border-radius:999px; padding:8px 18px; font-size:.9rem; color: var(--text-muted); }

.contact { background: var(--dark-bg); color: var(--white); }
.contact .section-title { color: var(--white); }
.contact-card { background: var(--dark-bg-alt); border-radius:16px; padding:40px; max-width:640px; }
.contact-name { font-size:1.3rem; font-weight:700; margin-bottom:12px; }
.contact-career, .contact-cert { color: rgba(245,241,234,0.75); font-size:.9rem; margin-bottom:8px; }
.contact-buttons { display:flex; gap:16px; margin-top:24px; flex-wrap:wrap; }
.btn { padding:14px 28px; border-radius:8px; font-weight:600; font-size:.95rem; }
.btn-primary { background: var(--gold); color: var(--dark-bg); }
.btn-outline { border:1px solid var(--gold); color: var(--gold); }

.footer { background: var(--dark-bg); color: rgba(245,241,234,0.6); text-align:center; padding:32px 24px; font-size:.85rem; border-top:1px solid rgba(255,255,255,0.1); }
.footer p { margin-bottom:4px; }

@media (max-width:768px) {
  .contact-buttons { flex-direction:column; }
  .contact-card { padding:24px; }
}
```

- [ ] **Step 3: 검증**

Run: `grep -c "great.east.land@gmail.com" index.html`
Expected: `1`

Run: `grep -c "SECTIONS_END" index.html`
Expected: `0` (모든 마커가 실제 콘텐츠로 교체됨)

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "Add clients, contact, and footer sections"
```

---

### Task 6: script.js (모바일 메뉴 토글) + 최종 반응형 점검

**Files:**
- Create: `script.js`

**Interfaces:**
- Consumes: Task 1의 `id="navToggle"`, `id="navLinks"`, Task 2의 `.nav-links.open` CSS 클래스

- [ ] **Step 1: script.js 작성**

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const toggle = document.getElementById('navToggle');
  const links = document.getElementById('navLinks');

  toggle.addEventListener('click', () => {
    links.classList.toggle('open');
  });

  links.querySelectorAll('a').forEach((link) => {
    link.addEventListener('click', () => links.classList.remove('open'));
  });
});
```

- [ ] **Step 2: 브라우저로 모바일 메뉴 동작 확인**

`resize_window` preset `mobile`(375x812)로 전환 후 페이지를 새로고침(load-time gate 재실행). `nav-toggle` 버튼을 `computer` `left_click`으로 클릭 → `.nav-links`에 `open` 클래스가 붙어 링크 5개(회사소개/사업영역/레퍼런스/클라이언트/문의)가 보이는지 `read_page`로 확인.

Expected: 클릭 전에는 링크가 숨겨져 있고, 클릭 후 5개 링크가 모두 나타남. 링크 클릭 시 메뉴가 다시 닫힘.

- [ ] **Step 3: Commit**

```bash
git add script.js
git commit -m "Add mobile nav toggle script"
```

---

### Task 7: README.md — 무료 배포 가이드

**Files:**
- Create: `README.md`

- [ ] **Step 1: README.md 작성**

```markdown
# 대동여지 홈페이지

레퍼런스 확인용 원페이지 홈페이지. 빌드 도구 없는 순수 HTML/CSS/JS.

## 로컬에서 미리보기

`index.html` 파일을 더블클릭해서 브라우저로 바로 열면 됩니다. 서버 설치가 필요 없습니다.

## 무료로 인터넷에 게재하는 방법 (GitHub + Vercel)

두 서비스 모두 완전 무료이며, 신용카드 등록도 필요 없습니다.

### 1단계. GitHub 계정 만들기
1. https://github.com 접속 → Sign up → 이메일/비밀번호로 가입 (무료)

### 2단계. 이 프로젝트를 GitHub에 올리기 (저장소 만들기)
1. GitHub에서 우측 상단 `+` → `New repository` 클릭
2. Repository name: `daedongyeoji-homepage` 입력 → `Create repository`
3. 이 프로젝트 폴더에서 아래 명령어를 순서대로 실행 (터미널에서):

\`\`\`bash
git remote add origin https://github.com/사용자명/daedongyeoji-homepage.git
git branch -M main
git push -u origin main
\`\`\`

(`사용자명` 자리에 본인 GitHub 아이디를 넣으세요. 처음 push할 때 GitHub 로그인 창이 뜨면 로그인하면 됩니다.)

### 3단계. Vercel 계정 만들고 자동 배포 연결
1. https://vercel.com 접속 → `Sign Up` → **Continue with GitHub** 선택 (1~2단계에서 만든 GitHub 계정으로 바로 가입, 별도 비밀번호 불필요)
2. 가입 후 `Add New...` → `Project` 클릭
3. 방금 만든 `daedongyeoji-homepage` 저장소를 목록에서 찾아 `Import` 클릭
4. 설정은 기본값 그대로 두고 `Deploy` 클릭 (Framework Preset: `Other` 로 자동 인식됨, 빌드 명령어 없음)
5. 30초~1분 후 배포 완료 — `daedongyeoji-homepage.vercel.app` 같은 무료 주소가 발급됩니다

### 4단계. 이후 내용을 수정할 때
1. Claude에게 수정을 요청하면 `index.html`/`style.css`/`script.js` 파일이 로컬에서 수정되고 git commit까지 완료됩니다
2. 터미널에서 아래 명령어만 실행하면 자동으로 재배포됩니다:

\`\`\`bash
git push
\`\`\`

3. Vercel이 GitHub에 새 커밋이 올라온 것을 감지해서 30초~1분 내로 자동으로 사이트에 반영합니다. 별도로 Vercel에 들어갈 필요가 없습니다.

## 커스텀 도메인을 나중에 연결하고 싶다면
Vercel 프로젝트의 `Settings` → `Domains`에서 구매한 도메인(예: `daedongyeoji.co.kr`)을 추가하면 됩니다. 도메인 구입은 가비아, 후이즈 등에서 연 1~3만원 수준입니다. 지금은 필요 없으며, 필요할 때 언제든 연결할 수 있습니다.
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "Add free deployment guide (GitHub + Vercel)"
```

---

### Task 8: 최종 통합 검증

**Files:**
- (변경 없음 — 기존 3개 파일 통합 확인)

- [ ] **Step 1: 데스크톱 뷰 전체 확인**

`preview_start`로 `index.html`을 열고 `resize_window` preset `desktop`(1280x800)으로 설정. `get_page_text`로 아래 문구가 모두 페이지에 존재하는지 확인:
- "대동여지"
- "Beyond Communications"
- "약국 디스플레이 매체"
- "대웅제약 우루사"
- "great.east.land@gmail.com"

Expected: 5개 문구 모두 텍스트에 포함됨.

- [ ] **Step 2: 모바일 뷰 확인**

`resize_window` preset `mobile`(375x812)로 전환 후 새로고침. `read_page`로 `.service-grid`, `.portfolio-grid`가 1열로 쌓였는지(각 카드가 세로로 나열) 확인하고, 네비게이션이 `nav-toggle` 버튼만 보이고 링크는 숨겨져 있는지 확인.

Expected: 카드가 1열, 데스크톱 네비 링크 숨김, 토글 버튼만 노출.

- [ ] **Step 3: 콘솔 에러 확인**

`read_console_messages`로 에러(`onlyErrors: true`) 확인.

Expected: 에러 0건.

- [ ] **Step 4: 최종 git 상태 확인 및 커밋**

```bash
git status
```

Expected: `working tree clean` (Task 1~7에서 이미 모두 커밋됨). 커밋되지 않은 변경사항이 있다면:

```bash
git add -A
git commit -m "Final integration check"
```
