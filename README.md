# PhoSem

PIN 다이오드 불량 검출 프로젝트입니다. <br>
**MIL model과 PatchCore model**을 합친 model입니다. <br>

- **MIL 분류기:** 불량 라벨로 학습합니다.
  - 최종본은 `Phosem5.ipynb`입니다.
- **PatchCore식 이상탐지:** 정상 이미지만으로 기준을 만듭니다.
  - 최종본은 `AnomalyDetection.ipynb`입니다.

## 최종 소스 · 결과물 받기

| 구분 | 위치 | 내용 |
|---|---|---|
| 코드 | 이 저장소 | 노트북 8개 · 환경 파일 2개 (아래 [저장소 구성](#저장소-구성) 참고) |
| 결과물 | [Google Drive · 300_data](https://drive.google.com/drive/folders/1FvdcUWm50mw2Sf-dVZ6hx_DM8uKJgRQS?usp=sharing) | 메모리 뱅크(.pt), 채점 결과 CSV, 그래프, 판정 사진, 오분류 사진 |

노트북은 드라이브 경로가 `/content/drive/MyDrive/300_data`로 고정돼 있습니다. 그래서 실행 전에 준비가 필요합니다.

1. 공유 폴더를 엽니다.
2. **"내 드라이브에 바로가기 추가"** 로 내 드라이브 최상위에 `300_data` 이름 그대로 둡니다.
3. 이후 코드 수정 없이 실행할 수 있습니다.

## 사용 가이드

### 1. 소스 자료 위치 및 다운로드 하는 방법
#### 자료 위치
- 소스 코드 (노트북, 환경 파일 등 GitHub 내 README에 기재됨)
  - GitHub: https://github.com/ilfpns/PhoSem
- 모델 파일 · 학습 데이터 · 결과물
  - Google Drive 300_data: https://drive.google.com/drive/folders/1FvdcUWm50mw2Sf-dVZ6hx_DM8uKJgRQS?usp=sharing

#### 소스 코드 받기
- 명령어: `git clone https://github.com/ilfpns/PhoSem.git`
- 또는 GitHub 저장소 페이지에서 **Code → Download ZIP**

#### 모델 파일 받기
모델 파일 `phosem_patchcore_v1.pt`는 약 200 MB입니다. GitHub에는 100 MB가 넘는 파일을 올릴 수 없어서 코드와 분리해 Google Drive에 올려 두었습니다.

1. 아래 드라이브 링크를 엽니다.
   - https://drive.google.com/drive/folders/1FvdcUWm50mw2Sf-dVZ6hx_DM8uKJgRQS?usp=sharing
2. `release` 폴더로 들어갑니다.
   - `phosem_patchcore_v1.pt` 파일을 우클릭하고 **다운로드**를 누릅니다. 파일은 보통 내 컴퓨터의 "다운로드" 폴더에 저장됩니다.
3. GitHub에서 받은 `PhoSem` 폴더 안에 `weights`라는 새 폴더를 만듭니다.
   - 2번에서 받은 파일을 `weights` 폴더로 옮깁니다.

```
PhoSem/
├─ PhoSem_Inference.ipynb
└─ weights/
   └─ phosem_patchcore_v1.pt
```

이렇게 두면 `PhoSem_Inference.ipynb`가 모델 파일을 자동으로 찾아서 불러옵니다.

#### Colab에서 실행하는 경우
위 1~3번 대신, 드라이브 링크에서 `300_data` 폴더를 **"내 드라이브에 바로가기 추가"** 로 내 드라이브 최상위에 추가하면 됩니다. 노트북이 드라이브의 모델 파일을 바로 찾습니다.

### 2. 자신의 컴퓨터에 동일한 환경 구현하는 방법
개발은 Google Colab(Python 3.11, PyTorch 2.x, T4 GPU)에서 진행했습니다. 아래 두 방법 중 하나를 고르면 됩니다.

#### 방법 A. Colab에서 실행
1. 아래 링크를 엽니다. Google 계정 로그인이 필요합니다.
   - https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/PhoSem_Inference.ipynb
2. 메뉴 **런타임 → 런타임 유형 변경 → T4 GPU**를 선택합니다.
3. 1번 셀을 실행합니다. 라이브러리 버전과 장치가 출력되면 준비가 끝난 겁니다.

#### 방법 B. 내 컴퓨터에 설치 (Windows · macOS · Linux)
- Miniconda: https://docs.anaconda.com/miniconda/ 에서 내 운영체제용을 받아 설치합니다.
- Git: https://git-scm.com/downloads 에서 받아 설치합니다. Git 없이 ZIP으로 받을 것이라면 생략해도 괜찮습니다.

**1. 터미널 열기**
- Windows: 시작 메뉴에서 **Anaconda Prompt**를 엽니다.
- macOS · Linux: 터미널을 엽니다.

**2. 소스 받기**
```bash
git clone https://github.com/ilfpns/PhoSem.git
cd PhoSem
```

**3. 환경 만들기**
`PhoSem` 폴더 안의 `environment.yml`로 같은 환경을 만듭니다. 처음 한 번 설정이 필요합니다.
```bash
conda env create -f environment.yml
conda activate phosem
```

**4. 실행 확인**
```bash
jupyter lab
```

브라우저가 열리면 `PhoSem_Inference.ipynb`를 열고 1번 셀을 실행합니다. 아래처럼 버전과 장치가 출력되면 환경 구성이 끝난 겁니다.
```
Python 3.11.x / torch 2.x / torchvision 0.x / ...
실행 위치: 로컬 / 장치: CPU (또는 GPU 이름)
```

다음에 다시 실행할 때는 터미널에서 `conda activate phosem` → `jupyter lab`만 하면 됩니다.

#### 참고 사항
처음 추론할 때 ResNet50 사전학습 가중치(약 100 MB)가 인터넷에서 자동으로 내려받아집니다. 패키지 목록은 `requirements.txt`, 환경 정의는 `environment.yml`에 있습니다.

### 3. 다운로드 및 구현한 환경에서 이미지 한 장에 대하여 어떻게 추론 결과를 얻는 방법
추론은 `PhoSem_Inference.ipynb` 하나로 합니다. 1번(모델 파일 받기)과 2번(환경 구성)을 마친 상태에서 시작합니다.
`PhoSem_Inference.ipynb` 하나로 추론 셀을 모아뒀습니다.

#### 순서
1. 노트북 열기
   - Colab: https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/PhoSem_Inference.ipynb
   - 내 컴퓨터: 터미널에서 `conda activate phosem` → `jupyter lab`을 실행하고 `PhoSem_Inference.ipynb`를 엽니다.
2. 1번 셀 「환경 확인」 실행: 버전과 장치(GPU/CPU)가 출력됩니다.
3. 2번 셀 「모델 파일 준비」 실행: `weights/phosem_patchcore_v1.pt`를 자동으로 찾아 불러옵니다. 아래처럼 판정 기준이 출력되면 정상입니다.
   ```
   판정 구간: ≥ 11.601 불량 의심 / 7.950 ~ 11.601 검토 필요 / < 7.950 정상 범위
   ```

4. 3번 셀 「모델 구성」 실행: 학습 때와 같은 특징 추출기(ResNet50)를 준비합니다. 처음 한 번은 ResNet50 가중치를 내려받습니다.
5. 4번 셀 「이미지 한 장 추론」에 사진 경로를 넣고 실행합니다.
   ```python
   IMAGE_PATH = "C:/Users/내이름/Desktop/R4_2_1.PNG"
   ```

   Colab에서는 비워 두고 실행하면 파일 업로드 창이 뜹니다. 거기서 사진을 고르면 됩니다.

   테스트용 사진은 드라이브 `300_data/판정_1467/` 폴더에서 받을 수 있습니다 (예: `R4_2_1.PNG`).

#### 결과
실행하면 원본 사진과 이상 히트맵이 나란히 표시되고, 아래 결과가 출력됩니다.
```
파일          R4_2_1.PNG
점수          16.8196
판정          불량 의심
최댓값        17.2105
최대위치_y    64
최대위치_x    63
정상_백분위   100.0
시험지_순위   1 / 1468
```
히트맵 그림은 `outputs/R4_2_1_heatmap.png`로 저장됩니다. 빨간 부분일수록 정상 die와 많이 다른 곳이고, 흰 X가 가장 이상한 위치입니다.

#### 결과 읽는 법
| 항목 | 뜻 |
|---|---|
| 점수 | 사진을 11,881개 조각으로 나눠 정상 die 조각과 비교했을 때, 가장 다른 12개 조각의 평균 거리. 클수록 불량에 가깝습니다 |
| 판정 | 점수를 아래 세 구간으로 나눈 결과 |
| 최대위치 | 가장 이상한 조각의 위치 (109×109 격자 좌표, 히트맵의 흰 X) |
| 정상_백분위 | 정상 die 2,138장 중 이 사진보다 점수가 낮은 비율. 100이면 어떤 정상 die보다도 이상합니다 |
| 시험지_순위 | 채점용 1,467장 사이에 넣었을 때의 순위. 1위가 가장 불량스럽습니다 |

| 판정 | 점수 | 기준 |
|---|---|---|
| 불량 의심 | 11.60 이상 | 정상 die 점수의 상위 3% 경계 |
| 검토 필요 | 7.95 ~ 11.60 | 사람이 확인할 구간 |
| 정상 범위 | 7.95 미만 | 시험지 1,467장에서 불량 95%가 이 값 이상이었음 |

#### 여러 장을 한 번에 할 때 (선택)
5번 셀 「폴더 일괄 추론」에 폴더 경로를 넣고 실행합니다.

```python
INPUT_DIR = "C:/Users/내이름/Desktop/새배치"
```
폴더 안 사진 전부의 결과가 점수 순으로 `outputs/predictions.csv`에 저장됩니다.

#### 입력 사진 조건
- 학습에 쓴 사진과 같은 장비·배율로 찍은 die 크롭 사진이어야 합니다.
  - 3609 배치
  - 339×450
  - RGB입니다.
- 촬영 조건이 다르면 정상 die도 점수가 높게 나올 수 있습니다.
- 처리 시간은 사진 한 장에 GPU로 1~2초, CPU로 10~20초 정도입니다.

### 4. 학습모델 소스 디렉토리 구조에 대한 설명
#### GitHub 저장소 (`PhoSem` 폴더)
```
PhoSem/
├─ README.md                    프로젝트 설명 · 사용 가이드
├─ environment.yml              conda 환경 정의 (Python 3.11 + requirements.txt)
├─ requirements.txt             pip 패키지 목록
├─ PhoSem_Inference.ipynb       ★ 추론: 모델 파일을 불러와 사진 한 장 / 폴더 채점
├─ AnomalyDetection.ipynb       ★ 최종 모델 학습: 메모리 뱅크 구축 → 채점 → 평가 → 모델 파일 내보내기
├─ Phosem5.ipynb                MIL 분류기 최종본 (비교·결합용)
├─ Phosem_ver4.ipynb            MIL 개선 A~D
├─ Phosem_ver3.ipynb            MIL + 공식 정답지 채점 대응
├─ Phosem_ver2.ipynb            MIL 전환 (패치 단위, 다중 라벨, ResNet18 앙상블)
├─ Phosem_CNN.ipynb             첫 모델 (die 전체를 256×256으로 줄여 넣는 CNN)
├─ Phosem.ipynb                 데이터 준비 (압축 해제, 라벨 분리, 슬라이싱)
├─ weights/                     (직접 만듦) phosem_patchcore_v1.pt 를 넣는 곳
└─ outputs/                     (추론 시 자동 생성) 히트맵 그림 · 결과 CSV
```

#### Google Drive (`300_data` 폴더)

```
300_data/
├─ release/
│  ├─ phosem_patchcore_v1.pt        ★ 추론용 모델 파일 (메모리 뱅크 + 설정 + 판정 기준)
│  └─ phosem_patchcore_v1.json      모델 파일 정보 (설정값, 판정 기준, sha256)
├─ 남은_2142장/                     정상 뱅크를 만든 사진 (정렬 표식 4장 제외 2,138장 사용)
├─ 판정_1467/                       채점 시험지 사진 1,467장 (불량 882 / 양품 585)
├─ banks/                           메모리 뱅크 · 채점 점수 · 히트맵 · 평가 결과
├─ wrong_imgs/                      잘못 판정한 사진 (FP · FN, 원본 + 히트맵)
├─ 261008_이탐점수_3609장.csv       1,467장 판정 라벨의 출처
└─ 3609_개선_전체순위_….csv         MIL 분류기의 3,609장 순위
```

#### 최종 모델이 만들어지는 과정
```
정상 사진 2,138장 → 868×868 리사이즈 → ResNet50 layer2+layer3 특징 (1,024차원)
                 → 장당 1,000조각 = 후보 213만 개 → k-center 코어셋 10만 개 = 메모리 뱅크

새 사진 → 같은 특징 추출 (109×109 = 11,881조각) → 조각마다 뱅크와의 최근접 거리
        → 상위 12개 평균 = 점수 → 판정 구간
```

#### 학습 노트북 `AnomalyDetection.ipynb` 셀 구성
위에서부터 코드 셀 순서입니다.

| 순서 | 셀 제목 | 하는 일 | 드라이브에 남는 결과 |
|---|---|---|---|
| 1 | Library 호출 | 라이브러리 불러오기 | — |
| 2 | 정상 데이터 확인 | `남은_2142장` 사진 확인 | — |
| 3 | 특징 개수 및 메모리 뱅크 크기 확인 | 조각 수 · 뱅크 크기 계산 | — |
| 4 | 메모리 뱅크 구축 v2 | 점검용 뱅크 (2,142장 전체) | `banks/bank_868_coreset_ex0.pt` |
| 5 | Outlier 채점 | 정상 사진끼리 비교해 튀는 사진 찾기 | `banks/outlier_scores_868_coreset_ex0.csv` |
| 6~7 | 점검 · 이상 맵 겹쳐 보기 | 가장자리 · 웨이퍼 위치 확인 | — |
| 8 | 제외 목록 만들기 | 제외 후보 정리 (적용 보류) | `banks/exclusion_lists_ex0.csv` |
| 9 | 메모리 뱅크 구축 v2 (정렬 표식 4장 제외) | **최종 메모리 뱅크** | `banks/bank_868_coreset_nomark.pt` |
| 10 | 1,467장 채점 | **시험지 채점** | `banks/test1467_scores_…_nomark.csv` |
| 11 | 새 축 + MIL 결합 | AUROC · ROC/PR 곡선 | `banks/fusion_*_nomark.*` |
| 12 | 틀리게 판단한 이미지 모으기 | 오분류 사진 정리 | `wrong_imgs/` |
| 13 | 개선 실험 | 테두리 마스크 · top-k · 색 축 비교 | `banks/improve_*.csv` |
| 14 | 최종 모델 내보내기 | **모델 파일 만들기** (재현 확인 포함) | `release/phosem_patchcore_v1.pt` |

최종 모델만 다시 만들려면 1 → 2 → 3 → 9 → 10 → 14 순서로 실행합니다.

#### 모델 파일 `phosem_patchcore_v1.pt` 구성
| 항목 | 내용 |
|---|---|
| `memory_bank` | 정상 조각 특징 100,000 × 1,024 (float16) |
| `config` | 백본(ResNet50) · 사용 층 · 입력 크기(868) · 정규화 값 · 점수 방식(상위 12개 평균) |
| `thresholds` | 판정 기준 두 개 (11.60 / 7.95)와 그 근거 |
| `reference` | 정상 2,138장 · 시험지 1,467장 점수 (백분위 · 순위 계산용) |
| `verify` | 내보낼 때 다시 채점해 맞춰 본 사진 3장의 점수 |

#### 추론 노트북 `PhoSem_Inference.ipynb` 셀 구성
| 순서 | 셀 제목 | 하는 일 |
|---|---|---|
| 1 | 환경 확인 | 라이브러리 버전 · GPU/CPU 확인 |
| 2 | 모델 파일 준비 | `phosem_patchcore_v1.pt` 찾아서 불러오기 |
| 3 | 모델 구성 | 학습 때와 같은 특징 추출기 준비 |
| 4 | 이미지 한 장 추론 | 사진 한 장 → 점수 · 판정 · 히트맵 |
| 5 | 폴더 일괄 추론 (선택) | 폴더 안 사진 전부 → `outputs/predictions.csv` |


## 배경

PIN 다이오드 die에는 육안 검사로 놓치기 쉬운 미세 결함이 있습니다. 예를 들면 이물질(particle)과 스크래치(scratch)입니다. 이런 결함을 가진 채 다음 공정으로 넘어가는 경우가 있어, die 이미지를 입력으로 받아 결함 유무를 자동으로 판별하는 파이프라인을 만들었습니다.

## 핵심 아이디어

### 1. 다중 인스턴스 학습 (MIL)

die 이미지 전체에 라벨 하나를 붙여 학습하면, 결함이 없는 영역까지 "불량"으로 학습되는 노이즈가 생깁니다.

그래서 die 하나를 여러 patch 묶음으로 나눕니다. 학습할 때는 링 3개, 박스 2개, 무작위 7개로 12장을 씁니다. 그다음 가장 의심스러운 patch를 기준으로 die 단위 판정을 내립니다. 이것이 **다중 인스턴스 학습(MIL, Multiple Instance Learning)** 방식입니다.

- 결함이 실제로 있는 위치에 학습 신호가 집중됩니다.
- 백본은 ImageNet 사전학습 ResNet18입니다.
- seed 3개로 학습해 앙상블합니다.

### 2. 다중 라벨 구조

결함은 여러 유형이 동시에 나타날 수 있습니다. 그래서 단일 클래스 분류 대신 **다중 라벨(multi-label)** 출력(P · S · 불량)을 씁니다.

원인을 모르는 불량은 "불량" 칸만 학습하고 P·S 칸은 손실에서 제외합니다(마스킹). 이렇게 하면 유형별 데이터가 적어도 버틸 수 있습니다.

### 3. 이상탐지 축 (PatchCore식)

불량 라벨에는 노이즈가 있습니다. 그래서 라벨 없이 **정상 die와의 거리**로 점수를 내는 축을 더했습니다.

1. **입력:** 868×868로 정사각 보정합니다. 원본 픽셀이 세로로 1.289배 늘어나 있어서, 정사각으로 맞춰야 수광창이 원으로 복원됩니다.
2. **특징 추출:** ResNet50(ImageNet V2) layer2 + layer3을 씁니다. 3×3 이웃 평균을 낸 뒤 1,024차원으로 줄이고, 특징맵은 109×109 = 11,881조각입니다.
3. **정상 뱅크:** 정상 사진 2,138장에서 장당 1,000조각을 뽑고, greedy k-center 코어셋으로 10만 개를 고릅니다.
4. **채점:** 조각마다 뱅크와의 최근접 거리를 구하고, 상위 12개를 평균해 die 점수로 씁니다.

### 4. 두 축 결합

두 점수를 순위로 바꾸어 `w × 이상탐지 순위 + (1 − w) × MIL 순위`로 합칩니다.

## 최종 결과

**Test set:** 교수님 채점 1,467장 (불량 882 / 양품 585, 불량률 0.601)

| 방법 | AUROC [95% CI] | AP |
|---|---|---|
| MIL (`Phosem5`) | 0.596 [0.569, 0.625] | 0.746 |
| 이상탐지 (`AnomalyDetection`, 상위 12개 평균) | **0.802** [0.781, 0.823] | 0.872 |
| 두 축 결합 (w = 0.8) | 0.805 [0.782, 0.826] | 0.882 |
| (참고) 교수님 이상탐지 점수 | 0.779 [0.756, 0.802] | 0.873 |

## 저장소 구성

진행 순서대로 정리했습니다. 굵게 표시한 두 개가 최종본입니다.
노트북 이름을 누르면 Colab에서 열립니다.

| 순서 | 노트북 | 단계 | 내용 |
|---|---|---|---|
| 1 | [`Phosem.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem.ipynb) | 데이터 준비 | zip 풀기, 파일명 라벨 분리, 슬라이싱, 분류 결과를 GUI로 확인 |
| 2 | [`Phosem_CNN.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem_CNN.ipynb) | 첫 모델 | die 전체를 256×256으로 줄여 넣는 자체 CNN(`build_cnn`), recall 기준 threshold. WideResNet50 PatchCore 첫 시도(양품 뱅크, 증강 뱅크) |
| 3 | [`Phosem_ver2.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem_ver2.ipynb) | MIL 전환 | 5um 픽셀 처리, ROI·메사 검출, MIL 데이터셋(다중 라벨), 자체 MIL CNN → ResNet18 MIL, seed 3개 앙상블, FP/FN 분석, 양품 기준 이상탐지 시도 |
| 4 | [`Phosem_ver3.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem_ver3.ipynb) | 공식 채점 대응 | 260731·260721 홀드아웃 분리, 정답지 라벨 반영, 라벨 일치도 분석, 도메인 차이 진단, 정답지 채점, 3609 배치 크롭과 순위표 |
| 5 | [`Phosem_ver4.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem_ver4.ipynb) | 개선 A~D | 정답지 라벨로 학습셋 구성, 방법 8가지 비교, 3609 도메인 정합(배율·방향·위치·색), 상위 900 채점, 제출용 zip |
| 6 | **[`Phosem5.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/Phosem5.ipynb)** | **MIL 최종** | ver4 + 개선 E(numpy 2 호환, 정답지 누락 행 확인, 3609 재크롭·검은 크롭 방지), 드라이브 재연결 |
| 7 | **[`AnomalyDetection.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/AnomalyDetection.ipynb)** | **이상탐지 최종** | ResNet50 PatchCore 뱅크, Outlier 점검, 1,467장 Novelty 채점, MIL 결합, 오분류 분석, 개선 실험 |
| 8 | **[`PhoSem_Inference.ipynb`](https://colab.research.google.com/github/ilfpns/PhoSem/blob/main/PhoSem_Inference.ipynb)** | **추론** | 모델 파일(`phosem_patchcore_v1.pt`)을 불러와 사진 한 장 · 폴더 채점 → 점수 · 판정 · 히트맵 |

### `Phosem5.ipynb` 셀 위치 (MIL 최종)

셀 번호는 맨 위 Colab 배지를 0번으로 셌을 때의 순서입니다.

| 셀 | 묶음 | 하는 일 |
|---|---|---|
| 1–4 | 준비 | 라이브러리, 학습 이미지 불러오기(`MyDrive/every_define/crop`), 5um 픽셀 환산, 학습/검증 분리 |
| 5–9 | 시험지 분리 | 260731과 학습 데이터 겹침 확인(해시), 공식 채점 대상 홀드아웃, 260721 정답지 반영, 라벨 일치도, 제외 목록 |
| 10–14 | 전처리 | ROI 정렬, 밝기·대비 도메인 진단, 메사 검출, [개선 A] 정답지 라벨 학습셋, 패치 유틸 |
| 15–19 | 모델·학습 | MIL 데이터셋, 자체 MIL CNN, **ResNet18 MIL(최종 구조)**, 추론·지표, 학습 |
| 20–27 | 앙상블·저장 | seed 3개 앙상블, 조합 탐색, 선택적 판정, 최종 평가, TTA, 체크포인트 저장 |
| 28–34 | 기록·오류 분석 | requirements.txt, 데이터 목록, FP/FN 목록·격자, 라벨 검토 재계산, 집계 방식 A/B, 체크포인트 불러오기 |
| 35–40 | 공식 채점 | [개선 E-0], 정답지 채점(260731 · 260721), [개선 E-1], [개선 B-0·B] 방법 비교 |
| 41–47 | 3609 배치 | 원본 크롭, [개선 E] 재크롭, 신규 배치 채점, [개선 C] 도메인 정합, [개선 D] 상위 900, 드라이브 재연결, 제출 zip |

### `AnomalyDetection.ipynb` 셀 위치 (이상탐지 최종)

| 셀 | 하는 일 | 드라이브에 남는 결과 |
|---|---|---|
| 1 | 라이브러리 | — |
| 2 | 정상 데이터(`남은_2142장`) 확인, 드라이브 목록 | — |
| 3 | 특징 개수·뱅크 크기 계산 | — |
| 4 | 뱅크 구축 (2,142장 전체, 점검용 · `RUN_TAG="ex0"`) | `banks/bank_868_coreset_ex0.pt` |
| 5 | Outlier 채점 (자기 자신 제외, 20%마다 저장) | `banks/outlier_scores_868_coreset_ex0.csv`, `outlier_maps_…pt` |
| 6 | Outlier 점검 (가장자리, 웨이퍼 그림) | — |
| 7 | 상위 사진 + 이상 맵 겹쳐 보기 | — |
| 8 | 제외 목록 (Outlier + MIL), 교수님 지시로 적용 보류 | `banks/exclusion_lists_ex0.csv` |
| 9 | **최종 뱅크** (정렬 표식 4장 제외 · `RUN_TAG="nomark"`) | `banks/bank_868_coreset_nomark.pt` |
| 10 | **1,467장 Novelty 채점** | `banks/test1467_scores_…_nomark.csv`, `test1467_maps_…pt` |
| 11 | **MIL 결합**, AUROC·신뢰구간·작동점·w 훑기·ROC/PR | `banks/fusion_table · fusion_w_sweep · fusion_summary_nomark.csv`, `fusion_curves_nomark.png` |
| 12 | 오분류 사진 모으기 + 회차별 측정값 비교 | `wrong_imgs/` |
| 13 | 개선 실험 (테두리 마스크·top-k·블러 + 색 축, 반반 분할 검증) | `banks/improve_*.csv`, `color_feats_lab.csv` |
| 14 | **최종 모델 내보내기** (메모리 뱅크 + 설정 + 판정 기준, 재현 확인 포함) | `release/phosem_patchcore_v1.pt`, `phosem_patchcore_v1.json` |

### `PhoSem_Inference.ipynb` 셀 위치 (추론)

| 셀 | 하는 일 |
|---|---|
| 1 | 환경 확인 (라이브러리 버전 · GPU/CPU) |
| 2 | 모델 파일 준비 (`weights/` 또는 드라이브 `release/`에서 `phosem_patchcore_v1.pt` 불러오기) |
| 3 | 모델 구성 (학습 때와 같은 특징 추출기) |
| 4 | 이미지 한 장 추론 → 점수 · 판정 · 히트맵 |
| 5 | 폴더 일괄 추론 (선택) → `outputs/predictions.csv` |

## 드라이브 결과물 구성 (`300_data`)

```
300_data/
├─ 남은_2142장/                     정상 뱅크용 사진 (판정 안 받은 2,142장)
├─ 판정_1467/                       교수님 채점 대상 사진 1,467장
├─ 261008_이탐점수_3609장.csv       교수님 이상탐지 점수 + 판정 라벨 (1,467장 라벨의 출처)
├─ 3609_개선_전체순위_….csv         MIL(Phosem5) 3,609장 전체 순위
├─ 판정_1467_전체순위.csv           MIL 순위표를 1,467장만 추린 것
├─ release/                         추론용 모델 파일 (phosem_patchcore_v1.pt · .json)
├─ banks/
│  ├─ bank_868_coreset_nomark.pt    최종 메모리 뱅크 (10만 × 1,024)
│  ├─ bank_868_coreset_ex0.pt       점검용 뱅크 (정렬 표식 포함)
│  ├─ memory_bank_320x240.pt        폐기한 첫 뱅크 (세로 비율 왜곡)
│  ├─ test1467_scores_…_nomark.csv  1,467장 이상탐지 점수
│  ├─ test1467_maps_…_nomark.pt     1,467장 조각별 거리 맵 (109×109)
│  ├─ outlier_scores/maps_…_ex0     2,142장 Outlier 점수·맵
│  ├─ fusion_*_nomark.*             MIL 결합 표 · w 훑기 · 요약 · 곡선
│  ├─ improve_*.csv                 개선 실험 결과
│  ├─ color_feats_lab.csv           색 특징 캐시
│  └─ roc_patchcore_1467.png        이상탐지 ROC 곡선
└─ wrong_imgs/                      오분류 사진 (FP · FN, 원본 + 히트맵, 측정값 CSV)
```

MIL 학습 원본(`every_define/crop`)과 MIL 가중치 제출 zip은 이 공유 폴더에 들어 있지 않습니다.

## 생성 파일

개발 마무리 단계에서 새로 만든 파일과, 실행하면 생기는 파일입니다.

| 파일 | 위치 | 만드는 곳 | 내용 |
|---|---|---|---|
| `environment.yml` | 저장소 | — | conda 환경 정의 (Python 3.11 + `requirements.txt`) |
| `requirements.txt` | 저장소 | — | pip 패키지 목록 |
| `phosem_patchcore_v1.pt` | 드라이브 `300_data/release/` | `AnomalyDetection.ipynb` 마지막 셀 | 추론용 모델 파일 (메모리 뱅크 100,000 × 1,024 + 설정 + 판정 기준, 약 195 MB) |
| `phosem_patchcore_v1.json` | 드라이브 `300_data/release/` | `AnomalyDetection.ipynb` 마지막 셀 | 모델 파일 정보 (설정값 · 판정 기준 · sha256) |
| `weights/` | 내 컴퓨터의 `PhoSem` 폴더 | 사용자가 직접 만듦 | 내려받은 `phosem_patchcore_v1.pt`를 넣는 폴더 |
| `outputs/` | 노트북을 실행한 폴더 | `PhoSem_Inference.ipynb` 실행 시 자동 생성 | 히트맵 그림(`*_heatmap.png`) · 일괄 추론 결과(`predictions.csv`) |

## 재현 순서

1. 위 [최종 소스 · 결과물 받기](#최종-소스--결과물-받기)대로 `300_data` 바로가기를 만듭니다.
2. Colab 런타임을 **GPU(T4)** 로 둡니다.
3. **`Phosem5.ipynb`** 를 위에서부터 실행합니다. MIL 학습과 3,609장 채점을 하고 `3609_개선_전체순위_*.csv`를 만듭니다. 학습 원본 이미지가 필요합니다. <br>
   3.1. 개발 모델 학습은 3,609장으로 진행하였습니다.
4. **`AnomalyDetection.ipynb`** 를 실행합니다.
   - 최종 경로만 돌리려면 셀 1 → 2 → 3 → 9 → 10 → 11 → 14 순서입니다. 셀 4~8은 뱅크 점검용이고, 셀 14가 추론용 모델 파일을 만듭니다.
   - 셀 10(1,467장 채점)은 T4에서 약 23분 걸리고, 20%마다 체크포인트를 저장해서 끊겨도 이어서 돌아갑니다.
   - 셀 12~13은 결과 분석과 개선 실험입니다.

## 환경

- Google Colab, T4 GPU
- PyTorch, torchvision (ResNet18 · ResNet50 ImageNet 사전학습 가중치), scikit-learn, OpenCV, pandas
- 정확한 버전 목록은 `Phosem5.ipynb` 셀 28이 `requirements.txt`로 기록합니다
