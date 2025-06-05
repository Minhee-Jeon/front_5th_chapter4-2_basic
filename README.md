# 바닐라 JS 프로젝트 성능 개선



- 배포 url: [front-5th-chapter4-2-basic-k0xf78l62-minheejeons-projects.vercel.app](https://vercel.com/minheejeons-projects/front-5th-chapter4-2-basic/Feh5VSM9Bsf8jVwLrc1eCRUWefez)

## 성능 개선 보고서



### 1. 개선 이유



🚨 **웹사이트 성능 측정 결과 (초기)**

- 페이지 성능 지표가 낮아 **사용자 경험(UX) 저하** 및 **검색엔진 최적화(SEO) 문제 발생**
- **LCP (Largest Contentful Paint)**: **14.71s (🔴 Poor)** → 페이지 주요 콘텐츠가 늦게 로드됨
- **Lighthouse Performance 점수**: **72% (🟠 개선 필요)**
- 리소스 최적화 부족: **이미지 최적화 미흡, CSS/JS 로딩 방식 비효율적**

📊 **초기 Lighthouse 점수**

| 카테고리           | 점수 | 상태 |
| ------------------ | ---- | ---- |
| **Performance**    | 72%  | 🟠    |
| **Accessibility**  | 82%  | 🟠    |
| **Best Practices** | 75%  | 🟠    |
| **SEO**            | 82%  | 🟠    |
| **PWA**            | 0%   | 🔴    |

📊 **초기 Core Web Vitals (2024)**

| 메트릭  | 설명                      | 측정값 | 상태   |
| ------- | ------------------------- | ------ | ------ |
| **LCP** | Largest Contentful Paint  | 14.56s | 🔴 Poor |
| **INP** | Interaction to Next Paint | N/A    | 🟢 Good |
| **CLS** | Cumulative Layout Shift   | 0.011  | 🟢 Good |

📝 **Core Web Vitals 기준값**

- 🟢 **Good**: LCP < 2.5s, INP < 200ms, CLS < 0.1
- 🟠 **Needs Improvement**: LCP < 4.0s, INP < 500ms, CLS < 0.25
- 🔴 **Poor**: LCP ≥ 4.0s, INP ≥ 500ms, CLS ≥ 0.25

📅 **측정 시간:** 2025. 6. 3. 오전 9:57:51



### 2. 개선 방법



1. LCP 최적화 (14.56s → 3.91s)

- 히어로 이미지 최적화

  - WebP/AVIF 버전 추가 (`srcset` 적용) 및 `<picture>` 태그 사용

  - `fetchpriority="high"` 추가로 로딩 우선순위 지정

  - 예시 코드:

    ```
    <picture>
      <source srcset="images/Hero_Desktop.avif" type="image/avif" />
      <source srcset="images/Hero_Desktop.webp" type="image/webp" />
      <img
        class="desktop"
        src="images/Hero_Desktop.jpg"
        alt="Hero Image"
        width="1920"
        height="1080"
        fetchpriority="high"
      />
    </picture>
    ```

    



1. CSS 로딩 최적화

- **렌더링 차단 리소스 제거** (`preconnect`, `defer` 적용)

- 비동기 CSS 로딩

  ```
  <link rel="stylesheet" href="/css/styles.css" media="print" onload="this.media='all'" />
  <noscript><link rel="stylesheet" href="/css/styles.css" /></noscript>
  ```

  



1. 쿠키 동의 스크립트 로딩 최적화

- 쿠키 동의 스크립트 초기화 방식 개선

- window.addEventListener('load', () => {...}) 적용 → 페이지 로딩 완료 후 실행

  ```
  <script
    async
    defer
    type="text/javascript"
    src="//www.freeprivacypolicy.com/public/cookie-consent/4.1.0/cookie-consent.js"
    charset="UTF-8"
  ></script>
  <script type="text/javascript" charset="UTF-8">
    window.addEventListener('load', function () {
      cookieconsent.run({
        notice_banner_type: 'simple',
        consent_type: 'express',
        palette: 'light',
        language: 'en',
        page_load_consent_levels: ['strictly-necessary'],
        notice_banner_reject_button_hide: false,
        preferences_center_close_button_hide: false,
        page_refresh_confirmation_buttons: false,
        website_name: 'Performance Course',
      });
    });
  </script>
  ```

  

1. HTML 구조 개선

- 제품 정보 섹션의 헤더 태그 계층 수정
- 기존: `<h5>` → `<h3>`, 변경: `<h3>` → `<h5>` (일관성 유지)



### 3. 개선 후 향상된 지표



📊 **초기 Lighthouse 점수**

| 카테고리       | 점수       | 상태 |
| -------------- | ---------- | ---- |
| Performance    | 72% -> 88% | 🟠    |
| Accessibility  | 82% -> 95% | 🟢    |
| Best Practices | 75% -> 71% | 🟠    |
| SEO            | 82% -> 91% | 🟢    |
| PWA            | 0% -> 0%   | 🔴    |

📊 Core Web Vitals (2024)

| 메트릭 | 설명                      | 측정값          | 상태 |
| ------ | ------------------------- | --------------- | ---- |
| LCP    | Largest Contentful Paint  | 14.71s -> 3.91s | 🟠    |
| INP    | Interaction to Next Paint | N/A             | 🟢    |
| CLS    | Cumulative Layout Shift   | 0.020           | 🟢    |

📊 **PageSpeed Insights**

<img width="516" alt="Image" src="https://github.com/user-attachments/assets/06fd3b16-72e6-4333-ab08-4673d8a1df36" />
