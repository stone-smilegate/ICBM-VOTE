# 🛠️ 설정 가이드

토큰은 GitHub Secrets에만 저장됩니다. `config.js`에는 공개 정보만 있습니다.

---

## 파일 구성

```
lunch-vote/
├── index.html                        # 투표 페이지
├── config.js                         # 레포 정보 (토큰 없음)
├── SETUP.md                          # 이 파일
└── .github/
    └── workflows/
        └── vote.yml                  # 투표 저장 Actions
```

---

## 1단계 — 레포에 파일 업로드

위 4개 파일(폴더 구조 포함)을 GitHub 레포에 업로드합니다.  
`.github/workflows/vote.yml` 경로가 정확해야 합니다.

---

## 2단계 — Actions 권한 확인

레포 **Settings → Actions → General → Workflow permissions**  
→ **Read and write permissions** 선택 후 Save

> 이 설정이 있어야 Actions가 Issue 댓글을 작성할 수 있습니다.

---

## 3단계 — 투표 기록용 Issue 만들기

레포 **Issues → New issue**  
제목 아무거나 (예: `점심 투표 기록`) → Submit

`config.js`의 `ISSUE_NUMBER`에 생성된 Issue 번호를 입력합니다.  
현재 설정값: **`1`** (stone-smilegate/ICBM-VOTE#1)

---

## 4단계 — GitHub Pages 활성화

레포 **Settings → Pages → Branch: main / folder: / (root) → Save**

배포 후 주소: `https://stone-smilegate.github.io/ICBM-VOTE/`

---

## 동작 방식

```
투표 클릭
  → repository_dispatch 이벤트 발생 (토큰 불필요)
  → .github/workflows/vote.yml 실행
  → GITHUB_TOKEN(자동 주입)으로 Issue 댓글 저장/수정
  → 10초 후 페이지 자동 갱신
```

- **읽기**: 공개 GitHub API (토큰 없음)
- **쓰기**: Actions의 자동 `GITHUB_TOKEN` (Secrets에 수동 저장 불필요)
- 참가자 1명 = 댓글 1개, 재투표 시 수정 (중복 없음)

---

## 전체 초기화

레포 Issue → 댓글 전체 삭제
