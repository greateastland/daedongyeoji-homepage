# 대동여지 홈페이지

레퍼런스 확인용 원페이지 홈페이지. 빌드 도구 없는 순수 HTML/CSS/JS.

## 로컬에서 미리보기

`index.html` 파일을 더블클릭해서 브라우저로 바로 열면 됩니다. 서버 설치가 필요 없습니다.

## 무료로 인터넷에 게재하는 방법 (GitHub + Vercel)

두 서비스 모두 완전 무료이며, 신용카드 등록도 필요 없습니다.

### 1단계. GitHub 계정 만들기
1. https://github.com 접속 → Sign up → 이메일/비밀번호로 가입 (무료)

### 2단계. 이 프로젝트를 GitHub에 올리기 (저장소 만들기)
1. GitHub에서 우측 상단 `+` → `New repository` 클릭
2. Repository name: `daedongyeoji-homepage` 입력 → `Create repository`
3. 이 프로젝트 폴더에서 아래 명령어를 순서대로 실행 (터미널에서):

```bash
git remote add origin https://github.com/사용자명/daedongyeoji-homepage.git
git branch -M main
git push -u origin main
```

(`사용자명` 자리에 본인 GitHub 아이디를 넣으세요. 처음 push할 때 GitHub 로그인 창이 뜨면 로그인하면 됩니다.)

### 3단계. Vercel 계정 만들고 자동 배포 연결
1. https://vercel.com 접속 → `Sign Up` → **Continue with GitHub** 선택 (1~2단계에서 만든 GitHub 계정으로 바로 가입, 별도 비밀번호 불필요)
2. 가입 후 `Add New...` → `Project` 클릭
3. 방금 만든 `daedongyeoji-homepage` 저장소를 목록에서 찾아 `Import` 클릭
4. 설정은 기본값 그대로 두고 `Deploy` 클릭 (Framework Preset: `Other` 로 자동 인식됨, 빌드 명령어 없음)
5. 30초~1분 후 배포 완료 — `daedongyeoji-homepage.vercel.app` 같은 무료 주소가 발급됩니다

### 4단계. 이후 내용을 수정할 때
1. Claude에게 수정을 요청하면 `index.html`/`style.css`/`script.js` 파일이 로컬에서 수정되고 git commit까지 완료됩니다
2. 터미널에서 아래 명령어만 실행하면 자동으로 재배포됩니다:

```bash
git push
```

3. Vercel이 GitHub에 새 커밋이 올라온 것을 감지해서 30초~1분 내로 자동으로 사이트에 반영합니다. 별도로 Vercel에 들어갈 필요가 없습니다.

## 커스텀 도메인을 나중에 연결하고 싶다면
Vercel 프로젝트의 `Settings` → `Domains`에서 구매한 도메인(예: `daedongyeoji.co.kr`)을 추가하면 됩니다. 도메인 구입은 가비아, 후이즈 등에서 연 1~3만원 수준입니다. 지금은 필요 없으며, 필요할 때 언제든 연결할 수 있습니다.
