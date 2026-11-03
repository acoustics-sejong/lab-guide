# Git·GitHub 처음 쓰는 학생을 위한 안내

Git을 한 번도 써 보지 않은 학생을 위한 단계별 안내입니다. 세종대학교 음향 연구실(`acoustics-sejong`)의 모든 프로젝트에 공통으로 적용됩니다. 프로젝트별 규칙(데이터 위치, 실험 기록, 담당 팀)은 각 프로젝트 저장소의 README와 CONTRIBUTING(참여 안내)을 따릅니다. Windows PC와 VS Code를 기준으로 합니다.

Google Colab에서 노트북으로 작업한다면 [Colab·Gemini 작업 안내](colab-guide.md)도 함께 봅니다.

처음에는 1~4절을 순서대로 한 번 따라 하고, 이후에는 5절 "하루 작업 흐름"과 8절 "자주 겪는 문제"를 필요할 때 찾아봅니다.

## 1. 먼저 알아 둘 개념

| 용어 | 뜻 |
|---|---|
| Git | 내 PC에서 파일의 변경 이력을 기록하는 프로그램 |
| GitHub | Git 저장소를 인터넷에 올려 여러 사람이 함께 쓰는 서비스 |
| 저장소 (repository) | 이력이 함께 관리되는 프로젝트 폴더. 연구실 저장소는 `acoustics-sejong/저장소이름` 형식 (예: `acoustics-sejong/pe-pinn`) |
| clone | GitHub의 저장소를 내 PC로 처음 복사하는 것 (한 번만) |
| commit | 변경 내용을 하나의 기록으로 저장하는 것. 저장 시점·작성자·설명이 남음 |
| branch | `main`과 분리된 작업 공간. 내 작업이 끝날 때까지 다른 사람의 코드에 영향을 주지 않음 |
| push | 내 PC의 commit을 GitHub로 올리는 것 |
| pull | GitHub의 새 commit을 내 PC로 받아오는 것 |
| Pull Request (PR) | 내 branch를 `main`에 합쳐 달라고 요청하고 검토받는 GitHub 화면 |

흐름을 그림으로 보면 다음과 같습니다.

```text
GitHub (원격 저장소)       main ──────────●──────────●───
                                 ↑ pull   ↑ PR 병합
내 PC  (로컬 저장소)        branch ──●──●──┘
                                commit commit → push
```

파일을 저장(Ctrl+S)하는 것과 commit은 다릅니다. 파일을 저장해도 commit하지 않으면 이력에 남지 않고, commit해도 push하지 않으면 다른 사람은 볼 수 없습니다.

## 2. 프로그램 설치

아래 순서대로 설치합니다. VS Code를 먼저 설치해야 Git 설치 과정에서 VS Code를 편집기로 고를 수 있습니다.

1. **VS Code**: <https://code.visualstudio.com/> 에서 설치합니다. 설치 후 왼쪽 확장(Extensions) 메뉴에서 **Python**과 **Jupyter** 확장을 설치합니다.
2. **Git for Windows**: <https://git-scm.com/downloads/win> 에서 설치합니다. 대부분 기본값으로 두고, 두 화면만 바꿉니다.
   - *Choosing the default editor used by Git*: **Use Visual Studio Code as Git's default editor**
   - *Adjusting the name of the initial branch*: **Override the default branch name** 선택 후 `main`
3. **Python**: 버전은 프로젝트 README에 적힌 버전을 따릅니다. python.org 설치 프로그램을 쓸 경우 첫 화면에서 **Add python.exe to PATH**를 체크합니다.

설치 확인: VS Code를 열고 메뉴 **Terminal → New Terminal**을 연 뒤 다음을 입력합니다. 버전 번호가 나오면 됩니다.

```bash
git --version
python --version
```

> 명령어 대신 버튼으로 작업하고 싶다면 [GitHub Desktop](https://desktop.github.com/download/)을 써도 됩니다. 이 문서의 명령어와 GitHub Desktop 버튼의 대응은 7절에 정리했습니다. 다만 막혔을 때 도움을 받기 쉽도록 명령어 방식을 한 번은 익혀 두기를 권합니다.

## 3. GitHub 계정과 저장소 권한

1. GitHub 계정 생성, 2단계 인증(2FA) 설정
2. Settings → Emails에서 **Keep my email addresses private**를 켜고 `...@users.noreply.github.com` 주소 복사
3. 책임자에게 GitHub 사용자명 전달 → 메일로 온 `acoustics-sejong` 조직 초대 **수락**

초대를 수락한 뒤, 초대받은 계정으로 로그인한 브라우저에서 프로젝트 저장소 주소(예: `https://github.com/acoustics-sejong/pe-pinn`)가 열리는지 확인합니다. 열리지 않으면 아직 저장소 권한이 없는 것이므로(clone할 때 `Repository not found` 오류) 책임자에게 알립니다. 책임자가 프로젝트 팀 배정과 팀의 저장소 권한을 확인합니다.

## 4. 저장소를 내 PC로 받기 (처음 한 번)

VS Code 터미널에서 실행합니다. 아래는 `pe-pinn` 저장소의 예시이며, 두 곳의 `pe-pinn`을 내 프로젝트 저장소 이름으로 바꿉니다.

```bash
mkdir C:/src
git clone https://github.com/acoustics-sejong/pe-pinn.git C:/src/pe-pinn
```

- OneDrive·바탕화면·다운로드 폴더, 한글이나 공백이 들어간 경로는 피합니다.
- `mkdir`에서 이미 있다는 오류가 나면 무시하고 다음 줄을 실행합니다.

처음 실행하면 GitHub 로그인 창이 뜹니다. **Sign in with your browser**를 눌러 브라우저에서 로그인하고 승인합니다. 이후에는 로그인 정보가 Windows에 저장되어 다시 묻지 않습니다.

받은 폴더를 VS Code로 엽니다: **File → Open Folder → `C:\src\pe-pinn`** (내 저장소 폴더). 이후 모든 명령은 이 폴더가 열린 상태의 터미널에서 실행합니다.

커밋 작성자 정보를 설정합니다. 이메일은 3절에서 복사한 noreply 주소입니다.

```bash
git config user.name "홍길동"
git config user.email "12345678+gildong@users.noreply.github.com"
git config user.email
```

마지막 명령이 방금 입력한 주소를 출력하면 됩니다.

Python 가상환경을 만듭니다. 프로젝트의 `.gitignore`에 `.venv/`가 들어 있어야 GitHub에 올라가지 않습니다. 없으면 책임자에게 알립니다.

```bash
python -m venv .venv
```

`requirements.txt`가 있으면 패키지를 설치합니다. 가상환경의 Python을 직접 지정하므로 활성화 여부와 관계없이 `.venv`에 설치됩니다.

```bash
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

- VS Code에서 환경을 두 곳에서 고릅니다. `.py` 파일은 Ctrl+Shift+P → **Python: Select Interpreter**에서 `.venv`를 고릅니다. `.ipynb` 노트북은 노트북 오른쪽 위 **Select Kernel**에서도 같은 `.venv`인지 확인합니다. 인터프리터만 바꾸면 기존 노트북은 다른 커널을 계속 써서, 설치한 패키지를 불러오지 못할 수 있습니다.
- 터미널에서 가상환경을 켜려면 `.venv\Scripts\activate`를 실행합니다. 줄 앞에 `(.venv)`가 붙으면 켜진 것입니다.
- PowerShell에서 "스크립트를 실행할 수 없으므로" 오류가 나면 활성화하지 않아도 됩니다. 대신 가상환경의 Python을 직접 지정합니다. 패키지 설치는 `.\.venv\Scripts\python.exe -m pip install 패키지`, 스크립트 실행은 `.\.venv\Scripts\python.exe scripts\파일.py`처럼 씁니다. 그냥 `pip install`을 실행하면 다른 Python에 설치될 수 있습니다.

데이터는 프로젝트 README에 적힌 위치(보통 프로젝트 MS Teams 채널 파일)에서 받아 저장소의 `data\` 폴더에 둡니다. 이 폴더도 `.gitignore`로 GitHub에 올라가지 않게 되어 있는지 확인합니다.

## 5. 하루 작업 흐름

예시: 깊이 인덱스 버그를 고치는 작업. 파일 이름은 예시이므로 내 프로젝트의 파일로 바꿔 읽습니다.

### ① 시작: `main`을 최신으로

```bash
git switch main
git pull
```

### ② 작업용 branch 만들기

이름은 영어 소문자와 `-`로, 무슨 작업인지 알 수 있게 짓습니다.

```bash
git switch -c fix-depth-index
```

VS Code 왼쪽 아래 상태 표시줄에 현재 branch 이름이 보입니다. 코드를 고치기 전에 `main`이 아닌지 확인합니다.

### ③ 코드 수정 후 상태 확인

```bash
git status
```

```text
On branch fix-depth-index
Changes not staged for commit:
        modified:   src/grid.py

Untracked files:
        scripts/plot_depth.py
```

- `modified`: 기존 파일이 바뀜
- `Untracked files`: Git이 아직 모르는 새 파일
- 여기에 `data/`, `.pt` 같은 파일이 보이면 안 됩니다. 보이면 commit하지 말고 질문합니다.

무엇이 바뀌었는지 줄 단위로 보려면 `git diff`를 입력하거나, VS Code 왼쪽 **Source Control**(가지 모양 아이콘)에서 파일을 클릭합니다. `git diff` 화면은 `q`를 눌러 빠져나옵니다.

### ④ commit

commit에 넣을 파일을 고른 뒤(`add`) 기록합니다. 먼저 인덱스 수정만 commit합니다.

```bash
git add src/grid.py
git commit -m "깊이 인덱스를 격자 간격 기준으로 계산하도록 수정"
```

그림 스크립트는 성격이 다른 변경이므로 별도 commit으로 남깁니다.

```bash
git add scripts/plot_depth.py
git commit -m "깊이 인덱스 확인용 단면 그림 스크립트 추가"
```

- 파일을 하나씩 지정하는 습관을 들입니다. `git add .`은 `git status`로 목록을 확인한 뒤에만 씁니다.
- 성격이 다른 수정(인덱스 수정, residual 수정, 그림 스크립트 추가)은 위처럼 commit을 나눕니다.
- 작업 중간에 자주 commit해도 됩니다. commit은 push하기 전까지 내 PC에만 있습니다.
- Jupyter 노트북은 commit 전에 출력을 지웁니다(노트북 상단 **Clear All Outputs** → 저장). 출력이 남아 있으면 파일이 커지고 변경 내용을 비교하기 어렵습니다.

### ⑤ push

처음 push할 때만 `-u origin branch이름`을 붙입니다.

```bash
git push -u origin fix-depth-index
```

이후 같은 branch에서는 `git push`만 입력합니다.

### ⑥ Pull Request 열기

1. GitHub 저장소 페이지에 노란 띠로 **Compare & pull request** 버튼이 뜹니다. 누릅니다. (버튼이 없으면 **Pull requests → New pull request**에서 `base: main` ← `compare: fix-depth-index`를 고릅니다.)
2. 제목과 템플릿 항목(변경 목적, 확인 방법, 결과 링크 등)을 채웁니다. 관련 이슈가 있으면 본문에 `Closes #12`처럼 씁니다.
3. 오른쪽 **Reviewers**에서 검토할 참여자 한 명을 지정하고 **Create pull request**를 누릅니다.
4. 리뷰 의견이 오면 같은 branch에서 코드를 고치고 다시 commit → `git push` 합니다. PR에 자동으로 반영되므로 새 PR을 만들지 않습니다.
5. 승인(Approve)을 받으면 **Merge pull request → Confirm merge**, 이어서 **Delete branch**를 누릅니다.

### ⑦ 마무리: 내 PC 정리

```bash
git switch main
git pull
git branch -d fix-depth-index
```

다음 작업은 다시 ①부터 시작합니다.

## 6. 다른 사람의 PR 검토하기

1. PR의 **Files changed** 탭에서 바뀐 줄을 봅니다.
2. 의견이 있는 줄 왼쪽의 **+** 를 눌러 댓글을 남깁니다.
3. 오른쪽 위 **Review changes**에서 하나를 고르고 제출합니다.
   - **Comment**: 질문이나 의견만 남김
   - **Approve**: 병합해도 됨
   - **Request changes**: 고친 뒤 다시 확인해야 함

확인할 점: PR의 "확인 방법"대로 실제로 확인했는지, 데이터·가중치·개인 경로가 들어가지 않았는지, 실험 결과 링크가 있는지.

코드를 직접 실행해 보고 싶다면 PR branch의 GitHub 최신 commit을 받아와 확인합니다.

```bash
git fetch origin
git switch --detach origin/fix-depth-index
git log -1 --oneline
```

- 마지막 명령이 보여 주는 commit이 PR의 **Commits** 탭 맨 아래(가장 최근) commit과 같은지 확인합니다.
- 이 상태(detached)에서는 commit하지 않습니다. 의견은 PR 댓글로 남깁니다.
- 작성자가 수정한 뒤 다시 검토할 때도 위 세 줄을 다시 실행합니다. 예전에 받아 둔 로컬 branch로 확인하면 수정 전 코드를 실행하게 됩니다.

검토가 끝나면 돌아옵니다.

```bash
git switch main
```

## 7. VS Code·GitHub Desktop에서 같은 작업

| 명령어 | VS Code (Source Control 패널) | GitHub Desktop |
|---|---|---|
| `git pull` | Source Control 패널 위쪽 **⋯ → Pull** | **Fetch origin → Pull origin** |
| `git switch -c 이름` | 상태 표시줄 branch 이름 클릭 → Create new branch | **Current branch → New branch** |
| `git status`, `git diff` | Changes 목록, 파일 클릭 | 왼쪽 Changes 목록 |
| `git add 파일` | 파일 옆 **+** (Stage Changes) | 파일 왼쪽 체크박스 |
| `git commit -m "..."` | 위쪽 메시지 칸 입력 → **Commit** | 왼쪽 아래 Summary 입력 → **Commit to 이름** |
| `git push` | **Publish Branch** / **Sync Changes** | **Publish branch** / **Push origin** |

상태 표시줄의 동기화(↻) 버튼과 **Sync Changes**는 받아오기와 올리기를 한 번에 합니다. `main`에서 실수로 commit한 상태라면 그 commit이 GitHub `main`에 올라가므로, 최신 코드만 받을 때는 **Pull**을 씁니다.

VS Code에서 Changes 목록 위의 **Commit** 버튼을 바로 누르면 "모든 변경을 stage할까요?"라고 묻습니다. 목록을 확인하지 않았다면 **Yes**를 누르지 않습니다.

## 8. 자주 겪는 문제

### commit할 때 `Please tell me who you are`

4절의 `git config user.name`, `git config user.email`을 저장소 폴더 안에서 실행합니다.

### commit 메시지 창(VS Code 탭)이 열리고 멈춤

`-m "메시지"` 없이 `git commit`만 입력한 경우입니다. 열린 탭 첫 줄에 메시지를 쓰고 저장한 뒤 탭을 닫으면 commit됩니다. 검은 화면에 `~` 표시가 가득한 편집기(vim)가 열렸다면 `Esc`를 누르고 `:q!` → Enter로 빠져나온 뒤 `-m`을 붙여 다시 실행합니다.

### `LF will be replaced by CRLF` 경고

Windows 줄바꿈 변환 안내입니다. 무시해도 됩니다.

### push할 때 `rejected ... (fetch first)` 또는 `non-fast-forward`

GitHub의 같은 branch에 내 PC에 없는 commit이 있다는 뜻입니다. 먼저 지금 어느 branch에 있는지 확인합니다.

```bash
git branch --show-current
```

- **`main`이 나오면** 아무것도 더 실행하지 말고, 아래 ["실수로 `main`에서 작업했음"](#실수로-main에서-작업했음)의 "commit했지만 push하지 않았다면" 절차를 따릅니다. `main`에 직접 올리면 PR 검토를 건너뛰게 됩니다.
- **내 작업 branch가 나오면** GitHub의 변경을 받아온 뒤 다시 push합니다.

  ```bash
  git pull
  git push
  ```

  `git pull`에서 충돌이 나면 아래 "충돌" 절차로 해결한 뒤 push합니다.

`--force`로 해결하지 않습니다.

### `Repository not found` 또는 `Permission denied`

순서대로 확인합니다.

1. 조직 초대를 수락했는지 확인합니다.
2. 초대받은 계정으로 로그인한 브라우저에서 프로젝트 저장소 주소가 열리는지 확인합니다. **열리지 않으면** 저장소 권한 문제이므로 책임자에게 알립니다(프로젝트 팀 배정과 팀의 저장소 권한 확인). 이 경우 아래 3번은 하지 않습니다.
3. 브라우저에서는 열리는데 Git에서만 오류가 나면, Git이 다른 GitHub 계정으로 로그인되어 있을 수 있습니다. Windows 검색 → **자격 증명 관리자 → Windows 자격 증명**에서 `git:https://github.com` 항목을 제거한 뒤 다시 시도하면 로그인 창이 뜹니다.

### 충돌 (`CONFLICT ... Merge conflict in 파일`)

같은 줄을 다른 사람과 내가 다르게 고친 경우입니다. 해당 파일을 열면 다음과 같이 표시됩니다.

```text
<<<<<<< HEAD
dz = depth / nz
=======
dz = depth / (nz - 1)
>>>>>>> main
```

1. `<<<<<<<`와 `=======` 사이가 내 내용, `=======`와 `>>>>>>>` 사이가 받아온 내용입니다. VS Code에서는 위에 **Accept Current / Accept Incoming / Accept Both** 버튼이 뜹니다.
2. 올바른 코드만 남기고 표시 줄(`<<<<<<<`, `=======`, `>>>>>>>`)을 모두 지운 뒤 저장합니다. 어느 쪽이 맞는지 모르면 상대방에게 묻습니다.
3. 해결한 파일을 add하고 commit합니다.

   ```bash
   git add src/grid.py
   git commit -m "main 병합 충돌 해결"
   ```

진행 중인 병합을 취소하고 이전 상태로 돌아가려면 `git merge --abort`를 실행합니다.

### 실수로 `main`에서 작업했음

**아직 commit하지 않았다면**: 고친 내용을 그대로 둔 채 새 branch를 만들면 변경이 새 branch로 옮겨집니다. `already exists` 오류가 나면 다른 이름으로 다시 실행합니다.

```bash
git switch -c rescue-main-work
```

**commit했지만 push하지 않았다면**: 두 단계로 나누어 진행합니다. 1단계가 성공한 것을 확인하기 전에는 2단계를 실행하지 않습니다.

시작 전에 `git status`에 commit하지 않은 변경이 없어야 합니다. 있으면 먼저 commit합니다.

1단계: 작업을 새 branch로 옮기고 확인합니다.

```bash
git switch -c rescue-main-work
```

`already exists` 등 오류가 나면 **여기서 멈추고** 질문합니다. 성공했다면 다음 두 명령으로 확인합니다.

```bash
git branch --show-current
git log -1 --oneline
```

첫 명령이 `rescue-main-work`를, 두 번째 명령이 내가 마지막으로 한 commit 메시지를 보여 주면 작업이 새 branch에 안전하게 있는 것입니다.

2단계: 1단계 확인이 끝난 뒤에만, `rescue-main-work`에 머문 채로 로컬 `main`을 GitHub 상태에 맞춥니다. 먼저 GitHub의 최신 상태를 받아옵니다.

```bash
git fetch origin
```

오류 없이 끝났을 때만 다음 명령을 실행합니다.

```bash
git branch -f main origin/main
```

작업 파일과 `rescue-main-work`는 그대로 두고 로컬 `main`만 옮깁니다. 이어서 그 자리에서 `git push -u origin rescue-main-work`로 올리고 PR을 엽니다.

**이미 `main`에 push했다면**: 더 이상 고치지 말고 책임자에게 알립니다.

### 파일 변경을 버리고 마지막 commit 상태로 되돌리고 싶음

```bash
git restore src/grid.py
```

되돌린 내용은 복구할 수 없으니 파일 이름을 확인하고 실행합니다.

### add한 파일을 commit 목록에서 빼고 싶음

```bash
git restore --staged runs/result.csv
```

파일은 그대로 남고 commit 대상에서만 빠집니다.

### 방금 commit의 메시지를 고치고 싶음 (push 전)

```bash
git commit --amend -m "새 메시지"
```

이미 push한 commit에는 쓰지 않습니다.

### 데이터·가중치·비밀번호를 commit했음

- **push 전**이면 책임자나 익숙한 참여자에게 화면을 보여 주고 함께 되돌립니다.
- **push 후**면 바로 책임자에게 알립니다. 파일을 지우는 commit을 추가해도 이력에는 남아 있습니다. 비밀번호·토큰이었다면 해당 서비스에서 즉시 바꿉니다.

### 그래도 모르겠을 때

폴더를 지우고 다시 clone하지 않습니다. 다음 두 가지를 프로젝트 MS Teams 채널에 올리고 질문합니다.

```bash
git status
git log --oneline -5
```

무엇을 하려다 어떤 명령에서 어떤 오류가 났는지(오류 메시지 전체)를 함께 적습니다.

## 9. 명령어 요약

| 하고 싶은 일 | 명령 |
|---|---|
| 현재 상태 보기 | `git status` |
| 바뀐 내용 보기 | `git diff` |
| 최근 commit 목록 | `git log --oneline -10` |
| `main` 최신화 | `git switch main` → `git pull` |
| 새 branch 만들고 이동 | `git switch -c 이름` |
| 있는 branch로 이동 | `git switch 이름` |
| branch 목록 | `git branch` |
| commit | `git add 파일` → `git commit -m "메시지"` |
| 처음 push | `git push -u origin 이름` |
| 이후 push | `git push` |
| 작업 중 `main` 변경 반영 | `git switch main` → `git pull` → `git switch 이름` → `git merge main` |
| 병합 끝난 branch 삭제 | `git branch -d 이름` |

## 10. 더 공부할 자료

처음이라면 위에서부터 차례로 권합니다.

| 자료 | 내용 |
|---|---|
| [GitHub Skills: Introduction to GitHub](https://github.com/skills/introduction-to-github) | GitHub 안에서 branch·commit·PR을 직접 해 보는 실습 (영어, 1시간 이내) |
| [Learn Git Branching (한국어)](https://learngitbranching.js.org/?locale=ko) | branch와 병합을 그림으로 보며 연습하는 웹 게임 |
| [GitHub 문서: GitHub 및 Git 정보 (한국어)](https://docs.github.com/ko/get-started/start-your-journey/about-github-and-git) | GitHub 공식 입문 문서 |
| [VS Code: Source Control](https://code.visualstudio.com/docs/sourcecontrol/overview) | VS Code의 Git 기능 설명 (영어) |
| [VS Code: Python 시작하기](https://code.visualstudio.com/docs/python/python-tutorial) | VS Code에서 Python·가상환경 설정 (영어) |
| [Python 가상환경 튜토리얼 (한국어)](https://docs.python.org/ko/3/tutorial/venv.html) | `venv` 공식 설명 |
| [GitHub 문서: Pull Request 만들기 (한국어)](https://docs.github.com/ko/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) | PR 화면 상세 설명 |
| [GitHub 문서: 명령줄로 병합 충돌 해결 (한국어)](https://docs.github.com/ko/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line) | 충돌 해결 절차 |
| [GitHub Desktop 문서 (한국어)](https://docs.github.com/ko/desktop) | GitHub Desktop 사용법 |
| [Git 치트 시트 (PDF)](https://education.github.com/git-cheat-sheet-education.pdf) | 한 장짜리 명령어 요약 |
| [Pro Git (한국어)](https://git-scm.com/book/ko/v2) | Git 공식 교재. 1~3장이면 충분 |
| [Oh Shit, Git!? (한국어)](https://ohshitgit.com/ko) | 자주 하는 실수와 되돌리는 방법 |
