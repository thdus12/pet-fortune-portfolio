<p align="center">
  <img src="https://pet-fortune.com/logo.png" alt="꼬순내 신당" width="300">
</p>

<p align="center">출생 정보 기반 AI 별자리 성격 분석 & 반려동물 운세 플랫폼</p>

<p align="center">
  <a href="https://pet-fortune.codism.co.kr">
    <img src="https://img.shields.io/badge/Live-pet--fortune.codism.co.kr-blueviolet?style=for-the-badge" alt="Live Demo">
  </a>
</p>

<br>

## 📌 프로젝트 소개

**꼬순내 신당**은 천체 위치 계산(NASA JPL Ephemeris)과 3단계 LLM 파이프라인을 활용하여, 깊이 있는 별자리 성격 분석 리포트를 생성하는 AI 콘텐츠 플랫폼입니다.

반려동물의 생년월일을 기반으로 한 성격, 전생, 궁합 분석부터, 사람의 출생 시간/장소 기반 네이탈 차트 분석까지 다양한 콘텐츠를 SSE 스트리밍으로 실시간 제공합니다.

<br>

## 🏗️ 시스템 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                      Client (Vue 3)                     │
│              pet-fortune.codism.co.kr                    │
└──────────────────────┬──────────────────────────────────┘
                       │ HTTPS
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  Spring Boot API Server                  │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌────────────────────┐    │
│  │ Security │  │  OAuth2   │  │   JWT Auth Filter  │    │
│  │  (CORS)  │  │ (Google/  │  │  (Access/Refresh/  │    │
│  │          │  │  Kakao)   │  │   Anonymous Token)  │    │
│  └──────────┘  └──────────┘  └────────────────────┘    │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │              Domain Services                      │   │
│  │  ┌──────────┐  ┌───────────┐  ┌──────────────┐  │   │
│  │  │ Astrology│  │    Pet    │  │   Payment    │  │   │
│  │  │ (성격/연애│  │ (성격/전생│  │  (Toss 연동) │  │   │
│  │  │  /궁합/  │  │  /궁합/   │  │  Star 시스템 │  │   │
│  │  │  월간운세)│  │  기분분석) │  │              │  │   │
│  │  └────┬─────┘  └────┬─────┘  └──────────────┘  │   │
│  │       │             │                            │   │
│  │       ▼             ▼                            │   │
│  │  ┌──────────────────────────────────────────┐   │   │
│  │  │         SSE Streaming Engine              │   │   │
│  │  │   (실시간 생성 진행률 + 결과 스트리밍)     │   │   │
│  │  └──────────────────┬───────────────────────┘   │   │
│  └─────────────────────┼───────────────────────────┘   │
│                        │                                │
│  ┌─────────────────────▼───────────────────────────┐   │
│  │          3-Stage LLM Pipeline (GPT-4o)           │   │
│  │                                                   │   │
│  │  Stage 1: FACT_EXTRACTOR                          │   │
│  │  → 천체 데이터에서 10개 카테고리별 행동 패턴 추출 │   │
│  │                                                   │   │
│  │  Stage 2: WRITER                                  │   │
│  │  → 패턴 뼈대를 직설적 한국어 리포트로 변환       │   │
│  │                                                   │   │
│  │  Stage 3: SURGICAL_REVIEWER                       │   │
│  │  → 모순/반복/완곡 표현 검수 및 교정               │   │
│  └───────────────────────────────────────────────────┘   │
│                                                         │
│  ┌──────────────────┐  ┌────────────────────────────┐   │
│  │  Astronomy Engine │  │       PostgreSQL           │   │
│  │  (NASA JPL 기반   │  │  (User, Report, Payment,  │   │
│  │   천체 위치 계산) │  │   Pet, Star History)       │   │
│  └──────────────────┘  └────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

<br>

## 🛠️ 기술 스택

| 구분 | 기술 |
|------|------|
| **Frontend** | Vue 3, Vite, Vue Router, Axios, Composition API |
| **Backend** | Spring Boot 3.2, Java 17, Spring Security, JPA/Hibernate |
| **Database** | PostgreSQL, QueryDSL |
| **AI/LLM** | OpenAI GPT-4o, 3-Stage Prompt Pipeline |
| **천체 계산** | Astronomy Engine (NASA JPL Ephemeris) |
| **인증** | OAuth2 (Google, Kakao), JWT (Access/Refresh/Anonymous Token) |
| **결제** | Toss Payments (토스페이먼츠) |
| **실시간** | SSE (Server-Sent Events) |
| **배포** | Docker, Nginx |
| **문서화** | Swagger (SpringDoc OpenAPI 3.0) |

<br>

## ✨ 주요 기능

### 1. 별자리 성격 분석 리포트 (10개 섹션)
출생 시간/장소 기반으로 네이탈 차트를 계산하고, 태양/달/상승/수성/금성/화성 + 12하우스 배치를 분석하여 10개 섹션의 성격 리포트를 생성합니다.

| 섹션 | 참조 행성 | 설명 |
|------|----------|------|
| 중심축 | 태양 | 인생의 핵심 욕구, 방향 |
| 혼자 있을 때 | 달 | 내면의 감정 패턴 |
| 의사결정 & 가치관 | 수성 + 금성 | 판단 방식, 가치 기준 |
| 스트레스 반응 | 달 + 화성 | 감정 트리거와 반응 |
| 인간관계 | 금성 + 7하우스 | 반복되는 관계 패턴 |
| 연애 | 금성 + 화성 | 사랑 방식과 욕망 |
| 커리어 | 토성 + 10하우스 | 일 처리 방식, 진로 |
| 강점 | 태양 + 목성 | 핵심 무기 |
| 약점 | 토성 | 반복되는 함정 |
| 조언 | 종합 | 인생 설계 방향 |

### 2. 3단계 LLM 파이프라인
단순 프롬프트 1회 호출이 아닌, 역할이 분리된 3단계 파이프라인으로 품질을 관리합니다.

```
[천체 데이터] → FACT_EXTRACTOR → [패턴 뼈대] → WRITER → [초안] → SURGICAL_REVIEWER → [최종 리포트]
                  (추출만)          (글쓰기만)         (검수만)
```

- **FACT_EXTRACTOR**: 해석하지 않음. 행성 배치에서 행동 패턴만 추출
- **WRITER**: 추출된 패턴을 직설적 한국어로 변환. 태양/달/상승 가중치(50/30/20%) 적용
- **SURGICAL_REVIEWER**: 모순, 반복, 완곡 표현을 찾아 교정. 원문 최소 수정 원칙

### 3. SSE 실시간 스트리밍
LLM 생성 과정을 SSE로 실시간 전달하여, 사용자가 리포트 생성 진행 상황을 확인할 수 있습니다.

```
Client ←──SSE──── Server
  │                  │
  │  [progress: 10%] │ ← 천체 계산 완료
  │  [progress: 30%] │ ← FACT_EXTRACTOR 완료
  │  [progress: 60%] │ ← WRITER 완료
  │  [progress: 90%] │ ← REVIEWER 완료
  │  [result: {...}]  │ ← 최종 리포트
  │  [complete]       │
```

### 4. 천체 위치 계산 (Natal Chart)
Astronomy Engine(NASA JPL Ephemeris)을 사용하여 출생 시간/장소 기반으로 정확한 천체 위치를 계산합니다.

- 태양, 달, 수성, 금성, 화성, 목성, 토성의 황도 좌표 계산
- 출생 장소(위도/경도) 기반 상승궁(ASC) 계산
- 12하우스 배치 결정
- 행성 간 각도(Aspect) 분석

### 5. 반려동물 콘텐츠
- **반려동물 별자리 성격 분석**: 생년월일 기반 10개 섹션 분석
- **전생 분석**: 반려동물의 전생 스토리 생성
- **궁합 분석**: 반려동물-보호자, 반려동물-반려동물 궁합
- **오늘의 기분**: 일일 반려동물 기분/간식 추천

### 6. 인증 & 결제
- **OAuth2 소셜 로그인**: Google, Kakao
- **JWT 토큰 체계**: Access(1H) / Refresh(7D) / Anonymous(24H) 3종 토큰
- **Toss Payments 연동**: 별(Star) 충전 패키지 결제
- **별 경제 시스템**: 프리미엄 콘텐츠 이용 시 별 차감

<br>

## 🔧 기술적 도전 & 해결

### SSE 스레드에서 SecurityContext 유실
**문제**: SSE는 별도 스레드(sseTaskExecutor)에서 실행되어, ThreadLocal 기반 Spring Security의 SecurityContext가 전파되지 않음 → userId를 가져올 수 없어 별 차감/기록 저장 실패

**해결**: Controller(메인 스레드)에서 userId를 먼저 추출하여 Request DTO에 설정한 뒤 SSE 스레드에 전달
```java
@GetMapping(value = "/report/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public SseEmitter generateReportStream(@Valid AstrologyRequest request) {
    SecurityUtil.getCurrentUserId().ifPresent(request::setUserId);
    return sseService.streamWithCallback(callback ->
        astrologyReportService.generateReportWithProgress(request, callback));
}
```

### LLM 출력 품질 제어 (태양/달/상승 가중치 불균형)
**문제**: 달+상승이 같은 별자리일 때 해당 별자리 특성이 70%로 과대평가되고, 태양이 30%로 축소됨

**해결**: 프롬프트에 가중치 균형 규칙을 명시하고, 3단계 파이프라인의 REVIEWER가 교차 검증
```
태양 = 50% (인생 방향, 핵심 판단 기준)
달   = 30% (감정 반응, 내면 패턴)
상승 = 20% (첫인상, 외피)
```

### 리포트 공유 시스템
**문제**: 인증 없이도 리포트를 공유할 수 있어야 함

**해결**: UUID 기반 shareId를 생성하여, 인증 없이 `/report/{shareId}`로 접근 가능하도록 구현

<br>

## 📊 ERD (주요 엔티티)

```
┌──────────────┐     ┌──────────────────┐     ┌─────────────────┐
│     User     │     │   UserProfile    │     │       Pet       │
├──────────────┤     ├──────────────────┤     ├─────────────────┤
│ id           │──┐  │ id               │     │ id              │
│ email        │  ├──│ user_id (FK)     │  ┌──│ user_id (FK)    │
│ nickname     │  │  │ name             │  │  │ name            │
│ stars        │  │  │ birth_date       │  │  │ birth_date      │
│ provider     │  │  │ birth_time       │  │  │ gender          │
│ provider_id  │  │  │ gender           │  │  │ pet_type        │
└──────────────┘  │  │ is_default       │  │  └─────────────────┘
                  │  └──────────────────┘  │
                  │                        │
    ┌─────────────┴───────┐    ┌───────────┴──────────────┐
    │  AstrologyReport    │    │    PetAstrologyReport     │
    ├─────────────────────┤    ├──────────────────────────┤
    │ id                  │    │ id                       │
    │ user_id (FK)        │    │ user_id (FK)             │
    │ share_id (UUID)     │    │ share_id (UUID)          │
    │ sun_sign            │    │ sun_sign                 │
    │ moon_sign           │    │ moon_sign                │
    │ rising_sign         │    │ rising_sign              │
    │ sections (JSON)     │    │ sections (JSON)          │
    │ view_count          │    │ view_count               │
    └─────────────────────┘    └──────────────────────────┘

    ┌─────────────────────┐    ┌──────────────────────────┐
    │      Payment        │    │      StarHistory         │
    ├─────────────────────┤    ├──────────────────────────┤
    │ id                  │    │ id                       │
    │ user_id (FK)        │    │ user_id (FK)             │
    │ order_id            │    │ amount                   │
    │ payment_key         │    │ type (USE/CHARGE)        │
    │ amount              │    │ description              │
    │ status              │    │ created_at               │
    └─────────────────────┘    └──────────────────────────┘
```

<br>

## 👥 Team

| 구성원 | GitHub | 역할 |
|------|------|--------|
| 배소연 | [@thdus12](https://github.com/thdus12)| 프론트엔드, 백엔드, 디자인, 기획, 프롬프트 엔지니어 |
| 주재범 | [@jaebum7396](https://github.com/jaebum7396) | 프론트엔드, 백엔드, 인프라 |

<br>

## 📁 프로젝트 구조

```
pet-fortune/          # Frontend (Vue 3)
├── src/
│   ├── views/        # 30+ 페이지 (별자리, 반려동물, 결제, 마이페이지)
│   ├── components/   # 28개 컴포넌트 (layout, fortune, input, ui, payment)
│   ├── assets/js/
│   │   ├── api/      # 13개 API 모듈
│   │   └── composables/  # 8개 Composable (useAuth, usePayment 등)
│   └── router/       # Vue Router (코드 스플리팅)

pet-fortune-api/      # Backend (Spring Boot)
├── src/main/java/com/petfortune/
│   ├── core/         # Security, JWT, SSE, OpenAI 설정
│   ├── domain/
│   │   ├── astrology/    # 별자리 분석 (Report, Love, Compatibility, Monthly)
│   │   ├── pet/          # 반려동물 (Saju, PastLife, Mood, Compatibility)
│   │   ├── user/         # 사용자 관리
│   │   ├── payment/      # Toss Payments 연동
│   │   ├── star/         # 별 경제 시스템
│   │   └── auth/         # OAuth2 + JWT 인증
│   └── resources/
│       └── application-{profile}.yml  # 환경별 설정 (local, dev, prod)
```

## 📞 Contact

- 📧 Email: petfortune8996@gmail.com
- 📸 Instagram: [@pet._.fortune](https://www.instagram.com/pet._.fortune)
  <br><br><br> 