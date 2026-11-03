# XR 한국어 학습 게임 설문 — Vercel 배포 안내

구성: 화면(설문·대시보드)은 Vercel, 응답 저장은 Google Apps Script → Google Sheets

| 파일 | 역할 |
|---|---|
| index.html | 설문 (주소 뒤에 `?id=P01&order=AB`) |
| dashboard.html | 실시간 대시보드 (주소 뒤에 `?key=대시보드키`) |
| config.js | Apps Script 웹 앱 주소(API_URL)를 넣는 곳 — **여기만 수정** |
| vercel.json | 깔끔한 주소(/dashboard), 캐시 끄기, 검색엔진 노출 막기 |

## 1. Apps Script (저장 API) 준비
1. 응답 시트 → 확장 프로그램 → Apps Script → `Code.gs` 내용을 새 파일로 교체 → 저장
2. 함수 `setup` 실행 (처음 한 번, 권한 허용)
3. 배포 → 새 배포 → 유형: 웹 앱 / 실행 사용자: 나 / 액세스 권한: **모든 사용자** → 배포
   - 이미 배포했다면: 배포 관리 → 수정(연필) → 버전: 새 버전 → 배포 (주소 유지)
4. 웹 앱 URL(`https://script.google.com/macros/s/…/exec`) 복사
   - 확인: 브라우저에서 `…/exec?action=ping` → `{"ok":true}`

## 2. config.js 수정
`API_URL: ''` 따옴표 안에 위 /exec 주소를 붙여 넣고 저장

## 3. Vercel 배포 (웹에서만, 권장)
1. GitHub에서 새 저장소 만들기 (Private 가능) → "uploading an existing file" → 이 폴더의 파일 4개(+README) 올리기 → Commit
2. vercel.com → GitHub로 로그인 → Add New → Project → 저장소 Import
3. Framework Preset: **Other**, 나머지는 그대로 → Deploy
4. 주소
   - 설문: `https://프로젝트이름.vercel.app/?id=P01&order=AB`
   - 대시보드: `https://프로젝트이름.vercel.app/dashboard?key=대시보드키`

이후 파일을 고쳐 GitHub에 다시 올리면 Vercel이 자동으로 다시 배포합니다.

### (대안) 명령줄
이 폴더에서 `npx vercel` → 로그인·질문에 답하기 → `npx vercel --prod`

## 주의
- 대시보드 키는 저장소 파일에 적지 마세요. config.js와 HTML은 누구나 볼 수 있습니다. 키는 Apps Script의 `DASHBOARD_KEY`에만 있고, 대시보드 주소로만 전달합니다.
- Apps Script 쪽 코드를 고치면 반드시 "새 버전"으로 다시 배포해야 반영됩니다. "새 배포"를 만들면 주소가 바뀌므로 config.js도 바꿔야 합니다.
