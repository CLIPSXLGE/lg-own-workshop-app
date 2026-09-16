# LG전자 자체 교안 — 배포용

LG전자에서 전달한 3개 HTML 교안을 GitHub + Vercel로 배포하기 위한 폴더입니다.

## 구성

| 파일 | 역할 |
|---|---|
| `index.html` | 메인 포털 (센터 전사원 레이스 포털) — 사이트 루트 |
| `고객가치혁신부문_지향점.html` | 포털에서 "우리의 지향점" 클릭 시 연결되는 페이지 |
| `code_artifact.html` | 포털에서 "원팀 워밍업 게임 챌린지" 클릭 시 연결되는 페이지 |
| `assets/` | 원본 HTML 안에 base64로 박혀 있던 이미지·동영상·폰트를 추출한 파일들 |

## 왜 원본 그대로 올리지 않았나요?

원본 파일은 이미지/동영상이 전부 base64 문자열로 파일 안에 통째로 들어 있어서
`02. 센터_전사원_레이스_포털.html` 한 파일만 153MB나 됐습니다.
GitHub은 100MB가 넘는 파일을 기본적으로 거부하고, Vercel 무료(Hobby) 플랜은
큰 파일을 다루는 Git LFS를 지원하지 않아서 그대로는 배포가 불가능했습니다.

그래서 base64 데이터를 실제 파일로 뽑아 `assets/` 폴더에 저장하고,
HTML 안에서는 그 파일 경로만 참조하도록 바꿨습니다. **화면에 보이는 결과물은
원본과 100% 동일**하며, 내용이나 디자인은 전혀 손대지 않았습니다.

- `02. 센터_전사원_레이스_포털.html` (153MB) → `index.html` (2.2MB) + assets
- `03. code_artifact.html` (64MB) → `code_artifact.html` (0.2MB) + assets
- `01. 고객가치혁신부문_지향점.html` (8MB) → `고객가치혁신부문_지향점.html` (0.06MB) + assets

원본 3개 파일은 상위 폴더(`02. LG전자 자체 교안/`)에 그대로 보존되어 있습니다.

## 알아둘 점

- 포털 페이지의 "2026 센터 전사원 교육 운영 계획" 링크(`26년_전사원과정_운영계획_웹리포트.html`)는
  기본적으로 숨겨져 있고(`display:none`) 원본 전달 파일에도 대상 파일이 없어 배포본에도 포함하지 않았습니다.
  필요하면 해당 HTML 파일을 받아서 이 폴더에 추가해주세요.
- "관리자 인증" 화면에서 지정하는 동영상/엑셀 경로(`D:\...`)는 발표자 PC 로컬 파일을 가리키도록
  설계된 기능입니다(발표자가 직접 파일을 선택). 서버에 파일을 올리는 방식이 아니므로 그대로 두었습니다.

## 배포 방법 (GitHub → Vercel)

1. GitHub에서 새 저장소 생성 (예: `lg-workshop-own-content`)
2. 이 폴더에서:
   ```bash
   git init
   git add .
   git commit -m "Initial deploy: LG전자 자체 교안"
   git branch -M main
   git remote add origin https://github.com/<계정>/<저장소>.git
   git push -u origin main
   ```
3. Vercel 대시보드 → **Add New… → Project → Import Git Repository** 로 방금 만든 저장소 선택
   (빌드 설정 건드릴 필요 없음, 정적 사이트로 자동 인식)
4. 배포 완료 후 나오는 주소가 고정 링크입니다. 이후 내용 수정 시:
   ```bash
   git add .
   git commit -m "수정 내용"
   git push
   ```
   → Vercel이 자동으로 같은 주소에 재배포합니다.
