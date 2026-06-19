# 오밥완 배포 지침

## 최초 저장소 생성 및 배포

PowerShell에서 프로젝트 루트 기준으로 실행한다.

```powershell
git init
git add index.html AGENTS.md DEPLOY.md
git commit -m "feat: initialize obabwan roulette"
git branch -M main
gh auth login
gh repo create obabwan --public --source=. --remote=origin --push
gh api --method POST repos/{owner}/obabwan/pages -f 'source[branch]=main' -f 'source[path]=/'
```

`{owner}`는 GitHub 계정명으로 바꾼다. Pages가 이미 생성되어 POST가 실패하면 다음 명령으로 설정을 갱신한다.

```powershell
gh api --method PUT repos/{owner}/obabwan/pages -f 'source[branch]=main' -f 'source[path]=/'
```

이후 배포는 다음 명령으로 처리한다.

```powershell
git add index.html AGENTS.md DEPLOY.md
git commit -m "chore: update obabwan"
git push origin main
gh api repos/{owner}/obabwan/pages
```

배포 주소: <https://dmazone.github.io/obabwan/>

## 카카오 공유 캐시 초기화

카카오 디벨로퍼스의 도구 메뉴에서 **공유 디버거**를 열어 배포 URL을 입력하고 캐시를 초기화한다.

- <https://developers.kakao.com/tool/clear/og>
- 대상 URL: `https://dmazone.github.io/obabwan/`

쿼리 문자열별 공유 미리보기가 다르면 실제 공유한 전체 URL을 각각 초기화한다. 카카오 도구 주소나 메뉴가 변경되면 카카오 디벨로퍼스의 **도구 > 공유 디버거**에서 확인한다.

## 운영 전 필수 설정

`index.html`의 `CONFIG`에 심사 완료된 광고 스크립트 URL, 쿠팡 파트너스에서 발급받은 추적 URL 템플릿, Gemini 프록시 주소를 입력한다. Gemini API 키를 정적 HTML에 직접 넣으면 누구나 열람할 수 있으므로 금지한다.

쿠팡 템플릿에는 검색어 치환 위치가 정확히 `{query}`로 포함되어야 한다. 발급받지 않은 일반 쿠팡 검색 URL은 파트너스 수익 추적 링크가 아니다.
