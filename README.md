# 픽셀 비례 랠리 — 깃허브 페이지 + 파이어베이스 온라인판 설치 방법

6-2 4단원 「비례식과 비례배분」 **단원 복습용 픽셀 카트 경주**예요(2~5명 동시 경주). 게임 화면은 **깃허브 페이지(GitHub Pages)** 에 올리고, 실시간 경주·기록은 **파이어베이스 Realtime Database** 로 주고받아요. 둘 다 무료 계정으로 돼요.

## ⚡ 눈싸움·숨바꼭질 게임을 이미 설치했다면 (5분)
같은 파이어베이스 프로젝트를 그대로 써도 돼요. 이 게임은 데이터베이스의 `snow/kart/` 아래에 따로 저장해서 **다른 게임과 방·기록이 섞이지 않아요**(규칙도 바꾸지 않아도 돼요).
1. 이미 쓰던 저장소의 `firebase-config.js` 를 이 폴더의 `firebase-config.js` 자리에 **그대로 복사**해요.
2. 아래 5~6단계처럼 **새 저장소**(예: `pixel-rally`)를 만들어 이 폴더 파일을 올리고 깃허브 페이지를 켜요.

## 폴더에 든 파일
| 파일 | 하는 일 |
|---|---|
| `index.html` | 게임 화면 (고치지 않아도 돼요) |
| `firebase-config.js` | **선생님이 채우는 파일** — 파이어베이스 연결 정보와 선생님 비밀번호 |
| `lib/` | 파이어베이스 9.23.0(Apache-2.0) 도구와 라이선스 |
| `database.rules.json` | 데이터베이스 규칙(3단계에서 붙여넣기) |
| `README.md` | 이 설명 |

---

## 처음 설치한다면
### 1. 파이어베이스 프로젝트 만들기
1. https://console.firebase.google.com 에 구글 계정으로 들어가요.
2. **프로젝트 추가(만들기)** → 이름 예: `math-604` → (구글 애널리틱스는 꺼도 돼요) → 만들기.

### 2. Realtime Database 만들기
1. 왼쪽 메뉴 **빌드(Build) → Realtime Database** → **데이터베이스 만들기**.
2. 위치는 가까운 **싱가포르(asia-southeast1)** 를 골라요.
3. 보안 규칙은 **잠금 모드**로 시작해요(다음 단계에서 바꿔요).

### 3. 규칙 넣기
Realtime Database의 **규칙** 탭에서 내용을 모두 지우고 `database.rules.json` 내용을 붙여넣은 뒤 **게시**해요.
```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "snow": { ".read": true, ".write": true }
  }
}
```
- 게임 자료는 모두 `snow` 아래(이 게임은 `snow/kart`)에만 저장돼요.
- ⚠️ 로그인 없이 쓰는 게임이라, 게임 주소를 아는 사람은 `snow` 아래 자료를 읽고 쓸 수 있어요. 주소는 우리 반에만 알려 주세요.

### 4. 웹 앱 등록하고 설정값 복사하기
1. 왼쪽 위 **⚙️ → 프로젝트 설정** → 아래 **내 앱**에서 **웹(`</>`)** 아이콘.
2. 앱 닉네임(예: `픽셀랠리`) → **앱 등록** (Firebase 호스팅은 체크하지 않아요).
3. 나오는 `const firebaseConfig = { ... }` 안의 값(apiKey, authDomain, databaseURL, projectId …)을 복사해요. **databaseURL이 꼭 있어야 해요.**
4. `firebase-config.js` 를 메모장으로 열어 `window.FIREBASE_CONFIG = { ... }` 안을 복사한 값으로 바꾸고, 맨 아래 `window.TEACHER_PIN = '4826';` 처럼 선생님 비밀번호를 정해 저장해요.

### 5. 깃허브 저장소에 올리기
1. https://github.com 로그인 → 오른쪽 위 **+ → New repository**.
2. 이름 예: `pixel-rally` · **Public** → **Create repository**.
3. **uploading an existing file**(또는 **Add file → Upload files**)을 눌러 **`index.html`, `firebase-config.js`, `lib` 폴더(통째로), `database.rules.json`, `README.md`** 를 끌어다 놓고 **Commit changes**.
   - 올린 뒤 `lib` 폴더 안에 파일 3개(`firebase-app-compat.js`, `firebase-database-compat.js`, `firebase.LICENSE.txt`)가 있는지 확인해요.

### 6. 깃허브 페이지 켜기
1. 저장소 **Settings → Pages** → **Source: Deploy from a branch**, **Branch: main / (root)** → **Save**.
2. 1~3분 뒤 나오는 주소 **`https://내아이디.github.io/pixel-rally/`** 가 학생에게 나눠 줄 게임 주소예요.
3. 첫 화면에 "⚠️ 온라인 경주 준비가 덜 됐어요"가 보이면 `firebase-config.js` 값을 다시 확인해 주세요.

---

## 수업에서 쓰는 법
1. 학생은 **이름**과 **반 코드**(예: `옥정초604`)를 쓰고 **🏁 친구와 온라인 경주** → **🎲 빠른 매칭**(또는 방 만들기 → 방 번호 알려 주기).
2. **5명이 모이면 바로**, 2~4명이면 **2명이 모인 때부터 25초 뒤 자동** 출발. 방장(👑)은 2명 이상이면 **▶ 지금 출발**.
3. 한 판은 **3바퀴**(바퀴마다 문제 관문 4곳 = 문제 12개). 1등이 들어오면 30초 뒤 끝나요. 보통 4~6분 걸려요.
4. 결과 화면에 순위·기록·맞힌 문제·점수와 **틀린 문제 풀이**가 나와요. 반별 순위(1등 횟수·최고 점수)는 첫 화면에 보여요.
- 남는 학생은 **🤖 컴퓨터와 연습**(컴퓨터 1·2·4명, 실력 3단계, 트랙 선택).

## 조작
| | 컴퓨터(키보드) | 태블릿·휴대폰 |
|---|---|---|
| 방향 | ← → (A·D) | 왼쪽 아래 ◀ ▶ |
| 브레이크·뒤로 | ↓ (S) | 오른쪽 아래 ▼ |
| 아이템 | 스페이스 | 🎁 버튼 |
| 4지선다 답 | 숫자 1~4 또는 클릭 | 보기 누르기 |
| 숫자 입력 답 | 숫자·소수점 키 → Enter | 화면 숫자판 → 확인 |
- 차는 저절로 앞으로 가요. 문제 관문에서는 저절로 멈춰요.

## 선생님 메뉴(첫 화면 아래)
- 비밀번호는 `firebase-config.js` 의 `TEACHER_PIN`.
- 📥 경주 기록 내려받기(엑셀용 CSV) · 경주 기록 초기화(지우기 전에 CSV가 먼저 내려받아져요) · 지난 게임 방 정리(수업이 끝난 뒤).

## 문제가 생기면
- "20초 안에 파이어베이스 응답이 없어요" → databaseURL 확인, 3번 규칙을 **게시**했는지 확인
- "permission denied" → 3번 규칙이 게시되지 않은 상태
- 학교 인터넷에서 `*.firebasedatabase.app` 주소가 막혀 있으면 온라인 경주가 안 돼요(컴퓨터와 연습은 돼요).
- 나중에 게임 파일을 고치면 저장소에서 **`index.html`만** 새 파일로 바꿔 올리면 돼요(`firebase-config.js`는 그대로).
