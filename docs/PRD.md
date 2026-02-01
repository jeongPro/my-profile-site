# PRD: Personal Profile & Team Growth Platform
> Version 1.1 | Last Updated: 2026-02-01

## 1. 프로젝트 개요 (Overview)
이 프로젝트는 단순한 자기소개 사이트를 넘어, **개인의 전문성(Branding)**과 **팀의 성장(Team Growth)**을 동시에 도모하는 하이브리드 플랫폼입니다.
기획자이자 개발자인 사용자의 정체성을 반영하여, "보여주는 것"과 "함께 성장하는 것"을 목표로 합니다.

---

## 2. 주요 타겟 유저 (Personas)
| Persona | 설명 | 주요 니즈 |
|---------|------|----------|
| **Host (나)** | 관리자. 이력을 보여주고, 학습 콘텐츠를 관리 | 콘텐츠 작성, 팀원 학습 현황 모니터링 |
| **Guest - Recruiter** | 외부 채용 담당자/리더 | 기술 스택, 프로젝트 경험, 문제 해결 능력 확인 |
| **Guest - Team Member** | 팀원 (화이트리스트 등록) | 지식 학습, 토론 참여, 과제 완료 |

---

## 3. MVP 단계 정의 (Phased Roadmap)

| Phase | 범위 | 예상 기간 |
|-------|------|----------|
| **Phase 1 (MVP)** | Home, About, Projects, 공개 Blog | 2주 |
| **Phase 2** | 팀원 인증 + Private 글 + Read Check | 1주 |
| **Phase 3** | Curriculum/Series + Quiz + Dashboard | 2주 |
| **Phase 4** | Gamification (Badge, Streak) + Notification | 1주 |

---

## 4. 핵심 기능 상세 (Detailed Features)

### A. 차별화된 프로필 (Personal Branding)
> "단순 나열이 아닌, 문제 해결 역량을 보여주는 프로필"

1.  **Interactive Timeline Resume**
    *   경력을 타임라인 인터랙션으로 구현.
    *   클릭 시 기술 스택 + 핵심 성과(Key Achievement) 확장.
2.  **Project Case Studies (Portfolio)**
    *   Case Study 형식: `배경` → `문제` → `해결책` → `결과(Impact)`.
    *   "기획자 의도" / "개발자 구현" 탭 분리 가능.
3.  **Live Tech Stack**
    *   기술 아이콘 클릭 → 해당 기술 관련 블로그 글/프로젝트 연동.

### B. 지식 공유 및 학습 관리 (Team Learning & LMS)
> "공부를 시키고 싶은 의도를 담은 기능"

1.  **Knowledge Base (블로그/위키)**
    *   공개(Public) / 팀 전용(Private) 글 구분.
    *   카테고리, 태그, 검색 기능.
2.  **Curriculum / Series (학습 로드맵)**
    *   순서가 있는 시리즈 기능 (예: "온보딩 코스 1~5강").
    *   이전 챕터 미완료 시 다음 챕터 잠금 옵션.
3.  **Assignment & Read Check (과제 및 확인)**
    *   **"다 읽었어요" 버튼**: 관리자 대시보드에서 확인 가능.
    *   **Micro-Quiz**: 글 하단 퀴즈, 정답자만 '완료' 처리.
4.  **Discussion Threads**
    *   각 글마다 댓글/토론 기능.

### C. 게이미피케이션 & 동기부여 (Gamification) - Phase 4
1.  **Contributor Badge**: 댓글/오타 수정 시 뱃지 부여.
2.  **Study Streak**: 연속 학습 일수 시각화.
3.  **Leaderboard**: 팀원 학습 순위 (선택적).

### D. 알림 (Notification) - Phase 4
*   새 글 등록 시 이메일/Slack 알림.
*   과제 미완료 리마인더.

---

## 5. 권한 매트릭스 (Permission Matrix)

| 기능 | Admin | Member | Guest |
|------|:-----:|:------:|:-----:|
| 글 작성/수정/삭제 | ✅ | ❌ | ❌ |
| Public 글 열람 | ✅ | ✅ | ✅ |
| Private 글 열람 | ✅ | ✅ | ❌ |
| 댓글 작성 | ✅ | ✅ | ❌ |
| 퀴즈 응시 | ✅ | ✅ | ❌ |
| 대시보드 열람 | ✅ | ❌ | ❌ |
| 팀원 관리 | ✅ | ❌ | ❌ |

---

## 6. 핵심 데이터 모델 (Data Schema)

```
User {
  id: string (PK)
  email: string (unique)
  name: string
  role: enum ['admin', 'member', 'guest']
  avatarUrl: string?
  createdAt: timestamp
}

Post {
  id: string (PK)
  title: string
  slug: string (unique)
  content: text (MDX)
  excerpt: string
  isPublic: boolean (default: true)
  seriesId: string? (FK → Series)
  seriesOrder: number?
  tags: string[]
  authorId: string (FK → User)
  publishedAt: timestamp
  updatedAt: timestamp
}

Series {
  id: string (PK)
  title: string
  description: string
  thumbnailUrl: string?
  createdAt: timestamp
}

Progress {
  id: string (PK)
  userId: string (FK → User)
  postId: string (FK → Post)
  isRead: boolean
  quizScore: number?
  completedAt: timestamp?
}

Quiz {
  id: string (PK)
  postId: string (FK → Post)
  question: string
  options: string[] (객관식)
  answer: string
}

Comment {
  id: string (PK)
  postId: string (FK → Post)
  authorId: string (FK → User)
  content: text
  createdAt: timestamp
}
```

---

## 7. 기술 스택 & 인프라 (무료 Tier 기반)

### 🏗️ Framework & Build
| 구분 | 선택 | 이유 |
|------|------|------|
| **Framework** | **Next.js 14 (App Router)** | SSG/SSR 혼합, SEO 최적화, Vercel 무료 배포 |
| **Styling** | **Tailwind CSS** | 빠른 개발, 작은 번들 사이즈 |
| **Content** | **MDX** | Markdown + React 컴포넌트 (퀴즈 등) |
| **Package Manager** | pnpm | 빠른 설치, 효율적 디스크 사용 |

### 🗄️ Backend & Database (무료)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Auth** | **Supabase Auth** | 50,000 MAU 무료, GitHub/Google OAuth |
| **Database** | **Supabase (PostgreSQL)** | 500MB 무료, Row Level Security |
| **대안** | Firebase | Firestore 1GB, Auth 무료 |

### 🌐 Hosting & CDN (무료)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Hosting** | **Vercel** | 무제한 배포, 100GB 대역폭/월, 커스텀 도메인 |
| **대안** | Netlify | 100GB 대역폭/월 |
| **대안** | Cloudflare Pages | 무제한 대역폭, 500 배포/월 |

### 🔍 Search (옵션)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Search** | **Algolia** | 10,000 검색/월, 10,000 레코드 무료 |
| **대안** | 자체 구현 | Supabase Full-text Search |

### 📧 Notification (옵션)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Email** | **Resend** | 3,000 이메일/월 무료 |
| **대안** | SendGrid | 100 이메일/일 무료 |

### 📊 Analytics (옵션)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Analytics** | **Vercel Analytics** | 2,500 이벤트/월 (또는 무료 Web Vitals) |
| **대안** | Google Analytics | 무제한 무료 |
| **대안** | Plausible | 오픈소스 셀프호스팅 |

### 🖼️ Media Storage (옵션)
| 구분 | 선택 | 무료 Tier |
|------|------|----------|
| **Image** | **Cloudinary** | 25GB 저장, 25GB 대역폭/월 |
| **대안** | Supabase Storage | 1GB 무료 |

---

## 8. 비기능 요구사항 (Non-Functional Requirements)

| 항목 | 기준 |
|------|------|
| **Lighthouse Performance** | 90+ |
| **First Contentful Paint** | < 1.5s |
| **반응형** | Mobile First (320px ~ 1920px) |
| **SEO** | 메타 태그, OG 이미지, sitemap.xml, robots.txt |
| **접근성** | WCAG 2.1 AA 준수 (시맨틱 HTML, aria 속성) |
| **보안** | HTTPS 필수, CSP 헤더, XSS/CSRF 방지 |
| **브라우저 지원** | Chrome, Safari, Firefox, Edge (최신 2개 버전) |

---

## 9. 메뉴 구조 (Sitemap)

```
/                       → HOME (인트로 + 최근 활동)
/about                  → ABOUT (인터랙티브 이력서)
/projects               → PROJECTS (케이스 스터디 목록)
/projects/[slug]        → PROJECT 상세
/blog                   → BLOG (공개 글 목록)
/blog/[slug]            → 글 상세 (+ 퀴즈, 댓글)
/series                 → SERIES 목록 (커리큘럼)
/series/[slug]          → SERIES 상세 (챕터 목록)
/login                  → 로그인 (OAuth)
/team (Protected)       → TEAM 메인
/team/dashboard         → 관리자 대시보드
/team/members           → 팀원 관리
```

---

## 10. 성공 지표 (Success Metrics)

| 지표 | 목표 |
|------|------|
| 팀원 온보딩 콘텐츠 완료율 | 80% 이상 |
| 평균 세션 시간 | 3분 이상 |
| Lighthouse 점수 | Performance 90+, SEO 100 |
| 신규 팀원 온보딩 시간 단축 | 기존 대비 30% 감소 |

---

## 11. 향후 확장 고려사항

*   다국어 지원 (i18n) - 영어 버전
*   PDF Export (이력서 다운로드)
*   RSS Feed
*   오프라인 지원 (PWA)
