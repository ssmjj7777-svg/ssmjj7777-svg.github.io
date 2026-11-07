# 프로젝트 기록 허브 (PROJECT_HUB)

> 수업/과제 진행 상황을 남기는 허브 문서입니다.
> 집에서 새 Kiro 대화를 시작할 때 **이 파일을 먼저 읽어** 이어서 작업하세요.

## 프로젝트 개요
- **학생**: 송민준 (ssmjj7777@kookmin.ac.kr)
- **사이트**: 자기소개 + 패션 스타일 선호도 투표
- **저장소**: https://github.com/ssmjj7777-svg/ssmjj7777-svg.github.io
- **GitHub Pages**: https://ssmjj7777-svg.github.io
- **구현한 Supabase 기능**: 콘텐츠 선호도 조사(패션 스타일 투표)
- **배포 대상(과제 조건)**: Netlify (GitHub 저장소 연결 방식)

## 기술 구성
- 순수 정적 사이트 (빌드 과정 없음), 단일 `index.html`
- Supabase JS v2 (ESM, `https://esm.sh/@supabase/supabase-js@2`)
- Supabase Project URL: `https://tvocyynoanuwqodiahek.supabase.co`
- Supabase publishable(anon) key 사용 — 브라우저 공개용 키. 보안은 RLS로 처리.
- **service_role / secret 키는 코드에 절대 포함하지 않음** (요구사항 준수)

## Supabase 데이터베이스
- 테이블: `public.style_votes`
  - `id` bigint identity PK
  - `style` text (`vintage_rockstar` | `minimal` | `street`)
  - `created_at` timestamptz default now()
- RLS: 활성화됨
  - anon: INSERT 허용 (style 값 체크), SELECT 허용
  - UPDATE / DELETE 정책 없음 → 불가 (의도된 제한)
- 집계: 별도 RPC 함수 없이 **클라이언트에서 집계** (테이블 SELECT 후 JS로 count)

## ✅ 완료한 것
- [x] 자기소개 사이트 (기존 디자인/애니메이션 유지)
- [x] 국민대 엠블럼(SVG) + 등장/스크롤 애니메이션
- [x] 패션 스타일 투표 UI (3개 선택지, 막대그래프, 득표수, 총합)
- [x] Supabase 연동: 투표 INSERT + 결과 SELECT 집계
- [x] 실시간 구독(postgres_changes)으로 다른 기기 투표 자동 반영
- [x] Supabase 테이블 + RLS 생성 (SQL Editor에서 Success 확인)
- [x] 정적 검증: index.html에 모든 연동 코드 포함 확인
- [x] `netlify.toml` 추가 (정적 사이트 배포 설정)

## ⚠️ 막힌 점 / 주의
- 로컬 Python/Node가 없어서 로컬 서버 실행 테스트는 생략함.
  → 실제 Supabase 저장/조회 동작은 **배포본(브라우저)에서 테스트**해야 함.
- GitHub Pages는 `index.html`을 이미 서빙 중. Netlify는 추가 배포 대상.

## ▶️ 다음에 할 작업 (순서대로)
1. 변경사항 GitHub에 커밋/푸시
2. Netlify에 로그인 → "Add new site" → "Import an existing project"
   → GitHub 저장소 `ssmjj7777-svg.github.io` 선택
   → 빌드 명령 없음 / publish 디렉터리 `.` (netlify.toml이 자동 인식)
   → Deploy
3. Netlify URL 접속 → 투표해 보기
4. 다른 기기(또는 휴대폰/시크릿창)에서 같은 URL 접속 → 같은 투표 수 보이는지 확인
5. Supabase 대시보드 → Table Editor → `style_votes` 에 행이 쌓이는지 확인
6. 재배포(또는 커밋 후 자동 배포)해도 투표 데이터 유지되는지 확인
7. Slack #5-과제제출 채널에 Netlify 링크 게시 (마감: 11/9(월) 00:00 KST)

## 참고 링크
- Notion 가이드: https://harsh-draw-fdf.notion.site/40c51d3491048042b298d03692493382
- Gamma 가이드: https://gamma.app/docs/h61oythrf4wb2lx
