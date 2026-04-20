# data_analysis_tda

# 🔬 TDA를 활용한 백내장 유무 판별 (Cataract Detection using TDA)

## 📌 프로젝트 개요

위상적 데이터 분석(Topological Data Analysis, TDA)의 **Persistent Homology**를 활용하여
안구 이미지에서 백내장 유무를 판별하는 머신러닝 분류 모델을 개발한 프로젝트입니다.

단순한 딥러닝 접근이 아닌, **수학적 위상 구조를 특징으로 추출**하여 SVM 분류기에 적용한 것이 핵심입니다.

---

## 📁 데이터셋

- **출처**: Kaggle - Ocular Disease Recognition (안구 질환 인식 데이터셋)
- 백내장이 있는 이미지 / 없는 이미지로 구성된 이진 분류 문제
- 이미지 전처리: 회색조 변환(Grayscale) + 28×28 픽셀 리사이즈

---

## ⚙️ 방법론

### 1. 이미지 전처리 및 2D 격자화
- `im2gray`로 컬러 이미지를 단일 채널로 변환
- `imresize`로 모든 이미지를 28×28 픽셀로 통일
- Meshgrid를 이용해 픽셀 좌표 + 회색조 값을 데이터 포인트로 구성

### 2. TDA 적용 (Vietoris-Rips Complex)
- **JavaPlex** 라이브러리를 사용해 Vietoris-Rips Stream 생성
- MAX_DIMENSION=2, MAX_FILTRATION=0.2, NUM_DIVISIONS=100 설정
- Persistent Homology 계산으로 데이터의 위상적 구조(구멍, 연결 성분 등) 추출

### 3. 특징 추출
- Barcode Diagram에서 각 특징의 **생존 기간(birth~death)** 계산
- 모든 이미지에 대한 생존 기간 벡터를 결합하여 특징 벡터 형성

### 4. 머신러닝 분류 (SVM)
- 데이터 분할: 훈련 80% / 테스트 20%
- **SVM (Support Vector Machine)** 이진 분류 모델 적용
- 평가 지표: Accuracy, Precision, Recall, F1-Score, Confusion Matrix

---

## 📊 결과

| 실험 | Training Accuracy | Test Accuracy | Precision | Recall | F1-Score |
|------|:-----------------:|:-------------:|:---------:|:------:|:--------:|
| 20개 데이터 | 75% | 50% | 0.5 | 1.0 | 0.667 |
| 70개 데이터 | 71.4% | 60.7% | 1.0 | 0.353 | 0.522 |

> 데이터 크기가 증가할수록 모델 성능이 향상되는 경향 확인.

---

## 🔍 핵심 발견

- 백내장이 **있는** 이미지: Persistent Homology의 유지 기간이 **길고 복잡** → 위상적 구조가 풍부함
- 백내장이 **없는** 이미지: 유지 기간이 **짧고 단순** → 위상적 특징이 적음
- 이 차이를 SVM이 학습하여 분류 수행

---

## 🛠️ 사용 기술

- **언어**: MATLAB
- **TDA 라이브러리**: JavaPlex
- **ML 모델**: SVM (Support Vector Machine)
- **평가**: Confusion Matrix, F1-Score, Precision, Recall

---

## 🚀 향후 개선 방향

- [ ] 데이터셋 확장으로 모델 일반화 성능 향상
- [ ] Random Forest, Neural Network 등 다양한 모델 비교 실험
- [ ] Python (Gudhi, Ripser 라이브러리) 기반으로 재구현하여 재현성 향상

---

## 📚 관련 개념

- **TDA**: 데이터의 형상적 특성을 분석하는 수학적 방법론
- **Persistent Homology**: 다양한 스케일에서 데이터의 위상적 특징을 추적
- **Vietoris-Rips Complex**: 데이터 포인트 간 거리 기반으로 단체 복합체를 생성

---

## 👤 Author

**전기범 (Jeon Gibeom)**
고려대학교 일반대학원 응용수학 석사
