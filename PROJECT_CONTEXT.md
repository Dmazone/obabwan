# 오밥완 프로젝트 컨텍스트

## 현재 상태

- 공개 사이트: <https://dmazone.github.io/obabwan/>
- GitHub 저장소: <https://github.com/Dmazone/obabwan>
- 배포 브랜치: `main` 루트, GitHub Pages legacy 방식
- 앱 구조: 단일 `index.html` 제로 빌드 웹앱
- 현재 기능 기준 커밋: `bfe47a2` (문서 전용 커밋은 이후일 수 있음)

## 구현된 기능

- `performance.now()` 델타 타임 기반 룰렛과 Offscreen Canvas 리베이킹
- 초기 고속 회전 후 무입력 시 약 7.3~9초, 최대 9.5초 내 종료
- 룰렛 클릭·터치 시 5단계 브레이크, 단계별 속도·감속력·효과음·진동 피드백
- 회전 중 메뉴 편집 패널 자리에 샌드박스 광고 슬롯 표시
- 제목 즉시 편집, 메뉴 태그 추가·삭제, 쉼표·줄바꿈·슬래시 일괄 입력
- 메뉴 최대 10개, 최소 2개
- 결과 모달, 최근 기록, Web Share/클립보드 공유
- URL `title`, `items` 동기화와 `pushState`, `popstate`, `pageshow` 복구
- 기본 상태 URL은 쿼리 없이 `/obabwan/` 유지
- Pretendard Variable 웹폰트를 본문·제목·Canvas에 적용

## 메뉴 프리셋

- 초기 메뉴 6개: 김치찌개, 돈가스, 초밥, 파스타, 제육볶음, 마라탕
- 버튼: 추천, 한식, 중식, 양식, 배달, 회식, 가볍게, 노술안주
- 각 카테고리 20개, 전체 등록 항목 140개
- 추천은 전체 풀의 중복을 제거한 뒤 6개 추첨
- 카테고리 버튼은 해당 풀에서 중복 없이 8개 추첨
- 버튼은 모바일에서도 잘리지 않도록 4열 2행 배치
- 같은 버튼을 다시 누를 때도 새 결과가 나오도록 연속 동일 선택 방지

## 외부 연동 상태

`index.html`의 `CONFIG` 연결 지점만 구현되어 있고 실제 발급값은 아직 비어 있다.

- `adfitScriptUrl`: 카카오 애드핏 심사 완료 URL 필요
- `tenpingScriptUrl`: 텐핑 공식 광고 URL 필요
- `coupangDeepLinkTemplate`: `{query}`를 포함한 쿠팡 파트너스 발급 URL 필요
- `geminiEndpoint`: API 키를 숨기는 서버리스 프록시 필요
- 정적 HTML에 API 키나 인증 토큰을 넣지 않는다.

## 변경·검증·배포

1. `index.html` 인라인 JavaScript를 `node --check`로 검사한다.
2. 로컬 HTTP 응답과 관련 기능을 검증한다.
3. 변경을 커밋하고 `origin/main`으로 푸시한다.
4. GitHub Pages 최신 빌드가 `built`가 될 때까지 확인한다.
5. 공개 URL을 Chrome에서 캐시 버스트 쿼리로 열어 기능과 콘솔 오류를 확인한다.
6. 검증 후 기본 URL과 기본 메뉴 상태로 복원해 탭을 유지한다.

## 최근 주요 결정

- 강제 광고 대기 모달은 사용하지 않는다. 광고는 회전 액션을 막지 않아야 한다.
- 일반 쿠팡 검색 URL을 제휴 딥링크로 표시하지 않는다.
- Gemini 키를 클라이언트에 노출하지 않고 프록시 방식만 허용한다.
- 사용자 입력은 `textContent`와 스크리닝을 사용하며 사용자 문자열을 `innerHTML`에 넣지 않는다.
- 프리셋은 가로 슬라이드 대신 한눈에 보이는 4열 2행을 사용한다.
