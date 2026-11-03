# Colab·Gemini로 작업할 때의 GitHub 관리

Google Colab에서 노트북으로 작업하고 Gemini의 도움을 받는 학생을 위한 안내입니다. 내 PC에 Git을 설치하지 않아도 GitHub 웹 화면과 Colab 메뉴만으로 작업할 수 있습니다. 용어(branch, commit, Pull Request)는 [Git·GitHub 입문 안내](git-guide.md) 1절을 참고합니다. 데이터 위치, 실험 기록 항목 등 프로젝트별 규칙은 각 프로젝트 저장소의 README와 CONTRIBUTING을 따릅니다.

## 핵심 규칙 다섯 가지

1. **노트북의 원본은 프로젝트 저장소의 `notebooks/` 폴더입니다.** Google Drive 사본의 변경은 연구실 GitHub 이력에 자동 반영되지 않습니다.
2. **GitHub에 저장할 때는 항상 내 branch를 고릅니다.** Colab 저장 창의 기본값은 `main`이므로 매번 확인합니다.
3. **노트북 하나는 한 사람만 수정합니다.** 노트북은 두 사람이 동시에 고치면 합치기가 거의 불가능합니다. 다른 사람의 노트북을 고치고 싶으면 사본을 만들거나 이슈로 요청합니다.
4. **데이터와 결과는 GitHub에 올리지 않습니다.** 프로젝트 README에 적힌 보관 위치(보통 프로젝트 MS Teams 채널 파일)에 둡니다.
5. **공식 실험은 GitHub에 저장한 노트북을 새 런타임에서 처음부터 실행한 결과만 인정합니다.** 자세한 절차는 3절에 있습니다.

## 1. 처음 한 번

### Colab에 GitHub 연결

1. [GitHub Skills 실습](https://github.com/skills/introduction-to-github)이나 [입문 안내](git-guide.md) 1·3절로 branch·PR 개념과 조직 초대 수락을 먼저 마칩니다.
2. Colab에서 **파일 → GitHub에 사본 저장**(File → Save a copy in GitHub)을 한 번 누르면 GitHub 승인 화면이 뜹니다.
3. **Include private repos**를 체크하고, 조직 목록의 `acoustics-sejong` 옆 **Request**(또는 **Grant**)를 누른 뒤 승인합니다.
4. 저장소 목록에 프로젝트 저장소가 보이지 않으면 책임자에게 알립니다. 조직 관리자가 Colab 접근을 승인해야 보입니다.

### 데이터 준비

Colab은 MS Teams 폴더를 직접 열 수 없으므로, 내 Google Drive에 작업용 사본을 둡니다.

1. **먼저 책임자에게 개인 Drive로 복사해도 되는 데이터 ID인지 확인합니다.** Drive 사본은 다른 사람에게 공유할 수 있고, 프로젝트 권한이 회수된 뒤에도 내 Drive에 남습니다. 복사가 허용되지 않은 데이터는 Colab에서 다루지 않습니다.
2. 프로젝트 데이터 보관 위치에서 해당 데이터 ID를 내려받습니다.
3. 내 Google Drive에 `프로젝트이름/data/데이터ID/` 폴더를 만들어 올립니다. Drive 폴더를 다른 사람에게 공유하지 않습니다.
4. 프로젝트 참여를 마치면 Drive 사본을 지웁니다.

Drive의 데이터는 작업용 사본입니다. 데이터를 수정하거나 새로 만들었다면 프로젝트 데이터 보관 위치에 새 데이터 ID로 올리고 책임자에게 알립니다.

## 2. 작업할 때마다

### ① GitHub에서 내 branch 만들기

1. 저장소 페이지 왼쪽 위 branch 버튼(`main` ▾)을 누릅니다.
2. 새 branch 이름(예: `pinn-fourier-features`)을 입력하고 **Create branch: … from main**을 누릅니다.

이미 진행 중인 branch가 있으면 그 branch를 계속 씁니다.

### ② 노트북 열기

- **이미 저장소에 있는 노트북**: Colab **파일 → 노트북 열기 → GitHub** 탭에서 프로젝트 저장소를 고르고, **Branch**를 내 branch로 바꾼 뒤 노트북을 엽니다.
- **새 노트북**: Colab에서 새로 만들고, 파일 이름을 영어 소문자와 `_`로 짓습니다. 예: `pinn_fourier_features.ipynb`.

### ③ 첫 설정 셀

노트북 맨 앞에 패키지 설치, Drive 연결, 경로 설정, 환경 기록을 모아 둡니다. 다른 사람이 처음부터 실행해도 돌아가야 합니다.

**셀 1: 패키지 설치.** 필요한 패키지를 버전까지 적고 주석(`#`)을 지웁니다. 설치할 패키지가 없으면 셀을 비워 둡니다.

```python
# !pip install numpy==1.26.4
```

**셀 2: 경로와 결과 폴더.**

```python
from pathlib import Path
from google.colab import drive
drive.mount('/content/drive')

PROJECT_DIR = Path('/content/drive/MyDrive/프로젝트이름')
DATA_ID = '데이터ID'
RUN_ID = '2026-11-03_사용자명_run01'   # 새 실험마다 새 ID
RESUME = False                          # 중단된 실험을 이어갈 때만 True

DATA_DIR = PROJECT_DIR / 'data' / DATA_ID
RUN_DIR = PROJECT_DIR / 'runs' / RUN_ID

if RESUME:
    assert RUN_DIR.exists(), f'이어갈 실험 폴더가 없습니다: {RUN_DIR}'
else:
    RUN_DIR.mkdir(parents=True, exist_ok=False)   # 같은 ID의 폴더가 있으면 오류
```

**셀 3: 환경 기록.** 학습을 시작하기 전에 기록하므로, 학습이 중간에 끊겨도 남습니다. 파일 이름에 시각을 붙여 이어서 실행할 때도 이전 기록을 덮지 않습니다.

```python
import shutil
from datetime import datetime
ENV_TAG = datetime.now().strftime('%Y%m%d_%H%M%S')

!python --version > {RUN_DIR}/python_version_{ENV_TAG}.txt
!pip freeze > {RUN_DIR}/requirements_{ENV_TAG}.txt
if shutil.which('nvidia-smi'):        # GPU 런타임에서만
    !nvidia-smi > {RUN_DIR}/nvidia_smi_{ENV_TAG}.txt
```

- 그림, CSV, 가중치는 모두 `RUN_DIR` 아래에 저장합니다. Colab의 `/content` 폴더는 런타임이 끝나면 지워집니다.
- 셀마다 경로를 직접 쓰지 않고 `DATA_DIR`, `RUN_DIR`만 씁니다.
- 실험 ID 형식은 프로젝트 README를 따릅니다.
- `FileExistsError`가 나면 이미 쓴 실험 ID입니다. 새 실험이면 `RUN_ID`를 바꿉니다. 이전 폴더를 지우거나 덮어쓰지 않습니다.
- **중단된 실험을 재개할 때는** 작업 branch에서 기존 `RUN_ID`를 유지하고 `RESUME = True`와 불러올 체크포인트(예: `CKPT = RUN_DIR / 'ckpt_step01000.pt'`)를 지정해 새 commit으로 저장합니다. 기존 실험 이슈에 재개용 commit ID와 체크포인트 파일명을 추가한 뒤, 그 commit을 새 런타임에서 엽니다(3절 "중단된 실험 재개"). 체크포인트는 파일 이름까지 직접 지정하고, "가장 최근 파일"을 자동으로 고르게 하지 않습니다.

### ④ GitHub에 저장 (= commit)

1. **수정 → 모든 출력 지우기**(Edit → Clear all outputs)를 누릅니다. 출력이 남아 있으면 파일이 커지고 변경 내용을 비교하기 어렵습니다.
2. **파일 → GitHub에 사본 저장**을 누릅니다.
3. 저장 창에서 다음을 확인합니다.

   | 항목 | 값 |
   |---|---|
   | Repository | 프로젝트 저장소 (`acoustics-sejong/저장소이름`) |
   | Branch | **내 branch** (`main` 아님) |
   | File path | `notebooks/파일이름.ipynb` |
   | Commit message | 무엇을 왜 바꿨는지 한 줄. 예: "Fourier Features 차수를 설정값으로 분리" |
   | Include a link to Colaboratory | 체크 해제 |

4. **확인**을 누르면 내 branch에 commit이 하나 생깁니다. 의미 있는 단위로 작업이 끝날 때마다 저장합니다.

### ⑤ Pull Request 열기

1. GitHub 저장소 페이지에 뜨는 **Compare & pull request**를 누릅니다. 버튼이 없으면 **Pull requests → New pull request**에서 `compare`를 내 branch로 고릅니다.
2. PR 템플릿을 채웁니다. 결과 그림은 프로젝트 결과 보관 위치의 링크로 넣습니다.
3. 리뷰어 한 명을 지정합니다. 리뷰 의견이 오면 같은 branch에 다시 저장(④)하면 PR에 반영됩니다.
4. 승인을 받으면 **Merge pull request** → **Delete branch**를 누릅니다.

## 3. 공식 실험 실행과 기록

노트북을 GitHub에 저장해도, 열려 있는 런타임에는 이전에 실행한 셀의 함수와 변수가 남아 있습니다. 예를 들어 residual 함수를 고쳐 저장한 뒤 그 셀을 다시 실행하지 않으면, 기록에는 새 commit이 남지만 학습에는 이전 함수가 쓰입니다. 그래서 공식 실험은 다음 순서를 지킵니다.

branch 이름으로 노트북을 열면, 기록한 뒤 그 branch에 새 commit이 생겼을 때 기록과 다른 버전을 실행하게 됩니다. 그래서 실행할 노트북은 **commit ID로 고정해서** 엽니다.

1. **저장**: `RUN_ID`를 새 실험 ID로 바꾸고 노트북을 GitHub 내 branch에 저장합니다(2절 ④).
2. **기록**: GitHub의 내 branch → **Commits**에서 방금 만든 commit(맨 위, 내 commit 메시지인지 확인) 오른쪽의 복사 버튼을 눌러 **전체 commit ID**(40자)를 복사합니다. 데이터 ID와 함께 실험 기록 이슈에 먼저 적습니다.
3. **새 런타임**: Colab **런타임 → 런타임 연결 해제 및 삭제**를 누릅니다.
4. **commit ID로 열기**: 브라우저 주소창에 다음 주소를 넣어 노트북을 엽니다. `전체commitID` 자리에 2번에서 복사한 값을 넣습니다.

   ```text
   https://colab.research.google.com/github/acoustics-sejong/저장소이름/blob/전체commitID/notebooks/파일이름.ipynb
   ```

   이 창은 실행 전용입니다. 여기서 코드를 고치거나 GitHub에 저장하지 않습니다.
5. **처음부터 실행**: **런타임 → 모두 실행**으로 첫 셀부터 순서대로 실행합니다. 중간 셀만 골라 실행하지 않습니다.
6. **결과 보관**: `RUN_DIR`의 결과를 프로젝트 결과 보관 위치의 같은 실험 ID 폴더로 복사하고, 실험 기록 이슈에 링크합니다.

**실행 중에 코드를 고쳤다면** 그 실행은 공식 결과로 쓰지 않습니다. branch로 연 노트북에서 고쳐 다시 저장하고, 새 실험 ID로 1번부터 실행합니다.

### 중단된 실험 재개

예전 commit ID로 노트북을 다시 열면 `RESUME = False`라 셀 2에서 폴더 중복 오류가 납니다. 그렇다고 열린 창에서 셀을 직접 고치면 기록한 commit과 실행한 코드가 달라집니다. 재개도 새 commit으로 진행합니다.

1. **branch에서 수정**: 작업 branch로 노트북을 열고(2절 ②) 기존 `RUN_ID`는 그대로 둔 채 `RESUME = True`와 `CKPT`(불러올 체크포인트 파일)를 지정합니다. 모델·학습 코드는 바꾸지 않습니다.
2. **저장**: GitHub 내 branch에 저장합니다(2절 ④). commit 메시지 예: "run01 재개: ckpt_step01000.pt부터".
3. **기록**: 새 실험 이슈를 만들지 않고 **기존 실험 이슈**에 재개용 전체 commit ID와 체크포인트 파일명을 추가합니다.
4. **실행**: 위 3~6번과 같이 새 런타임에서 재개용 commit ID 주소로 열고 모두 실행합니다.

이렇게 하면 최초 실행과 재개 실행을 같은 실험 이슈에서 추적할 수 있습니다. 재개 중 모델·학습 코드를 바꿔야 한다면 재개가 아니라 새 실험이므로 새 `RUN_ID`를 씁니다.

| 기록 항목 | Colab에서 확인하는 방법 |
|---|---|
| Git commit ID | 위 2번에서 복사한 전체 ID (4번 주소에 넣은 값과 같음) |
| 실행 당시 미커밋 변경 | 위 절차를 지켰다면 "없음" |
| Python·라이브러리 버전 | `RUN_DIR`의 `python_version_*.txt`, `requirements_*.txt` (셀 3) |
| 실행 장치 | 런타임 유형(T4, L4, A100 등)과 `nvidia_smi_*.txt` |

Colab 무료 런타임은 중간에 끊길 수 있습니다. 긴 학습은 일정 반복마다 가중치를 `RUN_DIR`에 저장하도록 작성합니다. 끊긴 뒤 이어서 학습했다면 실험 기록에 그 사실을 적습니다.

## 4. Gemini를 쓸 때

- **외부 AI 사용이 승인된 코드·데이터만 Gemini로 다룹니다.** 승인 범위가 불분명하면 합성 데이터와 최소 예제로 질문합니다. Google은 Colab의 생성형 AI 기능을 쓸 때 프롬프트뿐 아니라 관련 코드와 생성된 출력도 수집하고, 사람 검토자가 이를 읽을 수 있다고 안내합니다([Colab FAQ](https://research.google.com/colaboratory/faq.html)). 비공개 연구 코드나 미공개 결과는 승인 없이 Gemini에 넘기지 않습니다.
- **비밀번호, 접근 토큰, 개인정보는 어떤 경우에도 넣지 않습니다.**
- **생성된 코드를 실행하기 전에 읽습니다.** 각 줄이 무엇을 하는지 설명할 수 없으면 남기지 않습니다.
- **물리·수식 정의는 프로젝트 README를 기준으로 확인합니다.** 시간 규약과 부호, 좌표 축 순서, 단위와 정규화는 Gemini가 다른 관례로 쓰는 경우가 많습니다.
- **"돌아간다"와 "맞다"는 다릅니다.** 손실이 줄어드는 것만으로 맞다고 판단하지 않고, 프로젝트의 구현 점검 절차로 확인합니다.
- **PR 본문에 적습니다.** 예: "residual 계산 셀은 Gemini 초안을 바탕으로 작성, 부호와 배율을 README 식과 대조함".
- Gemini가 추천하는 패키지를 설치할 때는 버전을 고정해 셀 1에 모읍니다.

## 5. 자주 겪는 문제

| 증상 | 해결 |
|---|---|
| 저장 창에 저장소가 안 보임 | 1절 "Colab에 GitHub 연결"의 Include private repos와 조직 승인 확인. 안 되면 책임자에게 문의 |
| 저장 창의 Branch 목록에 내 branch가 없음 | GitHub에서 branch를 먼저 만들고(①) Colab 창을 새로고침 |
| 실수로 `main`에 저장함 | 더 저장하지 말고 책임자에게 알림 |
| 다른 사람이 같은 노트북을 수정함 | 덮어쓰지 말고 프로젝트 MS Teams 채널에서 누구의 버전을 기준으로 할지 정함 |
| Drive 사본과 GitHub 버전을 각각 수정해 내용이 달라짐 | 아래 "두 사본 합치기" 참고 |
| `pip freeze` 등에서 `No such file or directory` | 첫 설정 셀(③)을 실행하지 않아 `RUN_DIR`이 없는 경우. 첫 셀부터 다시 실행 |
| 셀 2에서 `FileExistsError` | 이미 쓴 실험 ID. 새 실험이면 `RUN_ID`를 바꾸고, 중단된 실험을 이어가는 경우에만 `RESUME = True` |
| 런타임이 끊겨 결과가 사라짐 | 결과를 `/content`가 아닌 `RUN_DIR`에 저장하도록 수정 |

### 두 사본 합치기

Drive 사본과 GitHub branch의 노트북을 둘 다 고쳤다면, 어느 한쪽이 최신이라고 할 수 없습니다. 한쪽으로 덮어쓰면 다른 쪽의 수정이 사라집니다.

1. 두 사본을 모두 그대로 둡니다.
2. GitHub에서 기존 branch를 기준으로 새 branch를 만듭니다. 예: 기존이 `pinn-fourier-features`이면 새 branch는 `pinn-fourier-features-drive`.
3. Drive 사본을 Colab에서 열고 **GitHub에 사본 저장**으로 **새 branch**의 같은 경로에 저장합니다.
4. GitHub에서 **Pull requests → New pull request**를 누르고 `base: 기존 branch` ← `compare: 새 branch`로 비교합니다. **Files changed**에서 셀별 차이를 봅니다.
5. 필요한 수정을 기존 branch의 노트북에 옮겨 저장하고, 비교용 PR은 병합하지 않고 닫습니다. 판단이 어려우면 책임자와 함께 봅니다.

## 6. Colab 다음 단계

노트북이 길어지면 모델·손실 함수처럼 다시 쓰는 코드를 `src/`의 `.py` 파일로 옮기고, 노트북은 실행과 그림만 담당하도록 정리합니다. 이 단계에서는 Colab에서 저장소 전체를 받아와야 하므로, 방법(접근 토큰 발급 또는 PC 작업 전환)을 책임자와 정합니다.

## 참고 링크

- [Colab: GitHub에서 노트북 열기·저장 예제](https://colab.research.google.com/github/googlecolab/colabtools/blob/main/notebooks/colab-github-demo.ipynb)
- [Colab: Drive 등 외부 데이터 사용](https://colab.research.google.com/notebooks/io.ipynb)
- [Colab 자주 묻는 질문 (런타임 제한, 생성형 AI 데이터 수집)](https://research.google.com/colaboratory/faq.html)
- [GitHub 문서: branch 만들기와 삭제 (한국어)](https://docs.github.com/ko/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)
- [GitHub 문서: Pull Request 만들기 (한국어)](https://docs.github.com/ko/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
