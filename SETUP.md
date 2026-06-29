# 🛠️ 설정 가이드 (SETUP.md)

GitHub Pages로 공유 투표를 운영하는 방법을 단계별로 안내합니다.  
약 5분이면 완료됩니다.

---

## 1단계 — 레포지토리 만들기

1. GitHub에서 **New repository** 클릭
2. Repository name: 원하는 이름 (예: `lunch-vote`)
3. **Public** 으로 설정 *(Private이어도 되나, Pages 무료 플랜은 Public 권장)*
4. **Add a README file** 체크 → **Create repository**

---

## 2단계 — 파일 업로드

다음 3개 파일을 레포지토리에 업로드합니다.

| 파일 | 설명 |
|------|------|
| `index.html` | 투표 페이지 |
| `config.js` | 토큰·레포 설정 (아래에서 수정) |
| `SETUP.md` | 이 파일 |

> **업로드 방법:** 레포 메인 페이지 → **Add file → Upload files** → 드래그 앤 드롭

---

## 3단계 — 투표 기록용 Issue 만들기

투표 데이터는 GitHub Issue의 댓글에 저장됩니다.

1. 레포지토리의 **Issues** 탭 클릭
2. **New issue** 클릭
3. 제목: `🗳️ 점심 투표 기록` (아무 내용이나 가능)
4. **Submit new issue** 클릭
5. 생성된 Issue URL에서 번호 확인  
   예: `https://github.com/your-name/lunch-vote/issues/`**`1`** → 번호는 `1`

---

## 4단계 — Personal Access Token 발급

1. GitHub 우측 상단 프로필 → **Settings**
2. 왼쪽 메뉴 맨 아래 **Developer settings** → **Personal access tokens → Fine-grained tokens**
3. **Generate new token** 클릭
4. 설정:
   - **Token name**: `lunch-vote`
   - **Expiration**: 원하는 기간 (예: 7 Days)
   - **Repository access**: `Only select repositories` → 방금 만든 레포 선택
   - **Permissions** → `Issues`: **Read and write**
5. **Generate token** 클릭 → 토큰 복사 (다시 볼 수 없음!)

---

## 5단계 — config.js 수정

`config.js` 파일을 열어 4개 값을 채웁니다.

```js
const GITHUB_TOKEN  = "github_pat_XXXX...";  // 4단계에서 복사한 토큰
const REPO_OWNER    = "your-username";        // GitHub 유저명
const REPO_NAME     = "lunch-vote";           // 레포 이름
const ISSUE_NUMBER  = "1";                    // 3단계에서 확인한 번호
```

수정 후 GitHub에 파일을 **다시 업로드(덮어쓰기)** 합니다.

---

## 6단계 — GitHub Pages 활성화

1. 레포 → **Settings** → 왼쪽 메뉴 **Pages**
2. **Branch**: `main` (또는 `master`) / 폴더: `/ (root)` 선택
3. **Save** 클릭
4. 잠시 후 `https://your-username.github.io/lunch-vote/` 접속 확인

---

## ⚠️ 보안 주의사항

- `config.js`에 넣은 GitHub Token은 **레포가 Public이면 누구나 볼 수 있습니다.**
- 이 토큰은 해당 레포의 **Issues 권한만** 가지므로 최소 권한으로 제한되어 있습니다.
- **코드 저장소(소스코드, 민감 정보)와 분리된 전용 레포**를 사용하는 것을 권장합니다.
- 투표가 끝나면 토큰을 **Revoke(폐기)** 해 주세요.
  - Settings → Developer settings → Fine-grained tokens → Delete

---

## 🔄 투표 초기화 방법

- **내 투표만 취소**: 페이지 하단 `내 투표 취소` 버튼
- **전체 초기화**: GitHub Issue로 이동 → 모든 댓글 삭제

---

## 💡 동작 방식

```
투표 버튼 클릭
   ↓
GitHub Issue에 내 닉네임 + 선택 식당 목록을 댓글로 저장 (JSON)
   ↓
페이지 로드 / 30초마다 댓글 목록을 읽어 집계
   ↓
실시간 결과 표시
```

댓글 1개 = 참가자 1명의 투표 전체.  
같은 사람이 다시 투표하면 기존 댓글을 **수정**합니다 (중복 없음).
