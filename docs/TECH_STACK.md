# {PROJECT_NAME} 기술 스택 및 환경

작성일: TODO
상태: 초안

## 0. 문서 목적

이 문서는 구현에 사용할 기술 스택, 배포 환경, 백엔드/BaaS 사용 범위, 인증·데이터 접근 원칙을 정의한다. 제품 요구사항은 `PRD.md`를 기준으로 하고, 이 문서는 구현 환경과 기술 결정의 기준으로 사용한다.

AI 에이전트는 코드 생성 전 이 문서와 `.rules/coding-standards.mdc`의 스택 표를 확인한다. 두 문서가 어긋나면 `package.json`을 최종 기준으로 삼고 문서를 갱신한다.

## 1. 우선 기술 방향

- TODO: 프레임워크 선택과 근거를 작성한다. (예: Next.js App Router / React SPA + Vite)
- TODO: 백엔드 전략을 작성한다. (예: BaaS 우선, 별도 API 서버, BFF)
- TODO: 배포 대상을 작성한다. (예: Vercel, Netlify, 자체 인프라)

## 2. 프론트엔드 스택

| 영역 | 선택 | 버전 | 비고 |
| --- | --- | --- | --- |
| 프레임워크 | TODO (예: Next.js App Router) | TODO | |
| 언어 | TypeScript | TODO | strict 모드 |
| 스타일 | TODO (예: Tailwind CSS + CVA) | TODO | |
| 서버 상태 | TODO (예: TanStack Query) | TODO | |
| 클라이언트 상태 | TODO (예: Jotai) | TODO | |
| 폼·검증 | TODO (예: react-hook-form + Zod) | TODO | |
| 테스트 | TODO (예: Vitest + RTL + MSW / Playwright) | TODO | |
| 패키지 매니저 | TODO (예: pnpm) | TODO | Node 버전 명시 |

## 3. 백엔드 / BaaS 사용 범위

- TODO: 사용하는 서비스와 포함 기능을 작성한다. (예: Supabase — Postgres, RLS, Auth, Storage, Realtime)
- TODO: MVP에서 의도적으로 제외하거나 단순화한 항목을 작성한다. (예: 실시간 수신은 polling으로 대체)
- TODO: 서버 신뢰 영역(Route Handler, Edge Function, BFF)이 필요한 기능을 명시한다.

## 4. 인증·데이터 접근 원칙

- TODO: 인증 방식과 세션 발급 흐름을 작성한다.
- TODO: 사용자 테이블과 인증 계정의 매핑 전략을 작성한다.

공통 금지 사항:

- 비밀번호·PIN 등 크리덴셜 원문 저장
- 클라이언트에서 크리덴셜 hash 조회 후 직접 비교
- 접근 제어(RLS, 권한 검사) 없이 데이터를 public API에 노출
- secret key(`service_role` 등)를 브라우저에 노출

## 5. 배포 환경

- TODO: 배포 플랫폼, 환경 구분(dev/staging/prod), 도메인 전략을 작성한다.
- TODO: 환경 변수 관리 방식을 작성한다. 브라우저 노출 변수(`NEXT_PUBLIC_*` 등)와 서버 전용 변수를 구분해 나열한다.

## 6. 기술 결정 기록

큰 기술 결정(프레임워크 교체, 상태 관리 변경 등)은 결정 일자·근거·대안과 함께 아래에 누적하거나 별도 ADR 문서로 관리한다.

| 일자 | 결정 | 근거 |
| --- | --- | --- |
| TODO | TODO | TODO |
