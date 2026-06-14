# {PROJECT_NAME} 기술 스택 및 환경

작성일: 2026-06-14  
상태: 초안

## 1. 문서 목적

이 문서는 RallyOn MVP 구현에 사용할 기술 스택, 배포 환경, Supabase 사용 범위, 인증/데이터 접근 원칙을 정리한다.

제품 요구사항은 `PRD_RallyOn_MVP_v1.0.md`를 기준으로 하고, 이 문서는 구현 환경과 기술 결정의 기준으로 사용한다.

## 2. 우선 기술 방향

- Backend as a Service는 Supabase를 우선 사용한다.
- Supabase로 처리 가능한 영역은 별도 백엔드 서버를 만들지 않는다.
- 프론트엔드는 Next.js 또는 React SPA 중 하나로 결정한다.
- Next.js를 선택하면 Vercel 배포를 우선 고려한다.
- React SPA를 선택하면 Netlify 배포를 우선 고려한다.

## 3. Supabase 사용 범위

### 포함

- Postgres DB
- Row Level Security
- Auth
- Storage
- Realtime Presence

### MVP 제한

- 채팅 메시지 실시간 수신은 3초 polling으로 처리한다.
- 온라인 상태만 Supabase Realtime Presence로 처리한다.
- 읽음 표시, 타이핑 표시, 푸시 알림은 MVP에서 제외한다.
- Edge Function은 인증 설계나 보안상 필요한 경우에만 사용한다.

## 4. 인증 설계 주의사항

RallyOn의 제품 UX는 닉네임 + 4자리 PIN 기반 간편 로그인이다. 이 방식은 Supabase Auth의 기본 이메일/비밀번호, OAuth 로그인 모델과 직접 일치하지 않는다.

따라서 로그인 단위 기능 구현 전 다음 설계를 먼저 확정해야 한다.

- Supabase Auth 사용자와 `profiles` 테이블을 어떻게 매핑할지
- 닉네임 중복 금지 정책을 어떤 테이블 제약으로 보장할지
- PIN hash를 어디에 저장할지
- PIN 검증을 어떤 서버 신뢰 영역에서 처리할지
- 로그인 성공 후 Supabase 세션을 어떻게 발급할지
- 자동 로그인 토큰을 Supabase 세션으로 대체할지 별도 토큰을 둘지

금지:

- PIN 원문 저장
- 클라이언트에서 PIN hash 조회 후 직접 비교
- RLS 없이 사용자/모임/채팅 테이블을 public API에 노출
- `service_role` key를 브라우저에 노출

## 5. 채팅 구현 방향

- 메시지 원본은 Supabase Postgres에 저장한다.
- 메시지 조회는 `meetupId` 기준 페이지네이션 API로 처리한다.
- 새 메시지 반영은 MVP에서 3초 polling으로 처리한다.
- 메시지 작성은 모임 참여자만 가능하다.
- 메시지 조회는 모임 참여자만 가능하다.
- 접근 제어는 Supabase RLS로 보장한다.
- 온라인 상태는 Supabase Realtime Presence로 처리한다.

## 6. 배포 환경 후보

### Next.js 선택 시

- 배포: Vercel
- 서버 렌더링, 라우팅, 서버 액션 또는 Route Handler 활용 가능
- Supabase SSR 세션 처리가 필요하다

### React SPA 선택 시

- 배포: Netlify
- 클라이언트 중심 구조
- 서버 신뢰 영역이 필요한 기능은 Supabase Edge Function 또는 별도 API가 필요하다

## 7. 환경 변수 원칙

- 브라우저에 노출 가능한 Supabase publishable key만 public env로 둔다.
- secret key, service role key는 절대 public env에 두지 않는다.
- 배포 환경별 env 이름과 용도를 문서화한다.

예상 env:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`

`SUPABASE_SERVICE_ROLE_KEY`는 서버 신뢰 영역에서만 사용한다.

## 8. 추후 확정 필요

- Next.js와 React SPA 중 최종 프론트엔드 프레임워크
- Supabase Auth 기반 닉네임 + PIN 인증 구현 방식
- Edge Function 사용 여부
- Storage 사용 범위
- 지도/장소 검색 API 선택
- 로컬 개발 환경과 Supabase local CLI 사용 여부
