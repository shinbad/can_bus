# 🚗 Anomaly CAN Message Detection Using Heuristics and XGBoost

> 차량 네트워크 보안을 위한 머신러닝 기반 비정상 CAN 메시지 탐지 모델

---

## 📌 개요

자동차의 **CAN (Controller Area Network)** 통신 시스템에서 발생할 수 있는 **비정상 메시지를 머신 러닝 기법으로 탐지**하여 **자동차 보안**을 강화하는 것이 본 프로젝트의 목적입니다.

- **저자:** 김세린, 윤범헌, 조학수 (호서대학교 컴퓨터공학부)
- **논문 게재처:** 한국정보처리학회 학술대회논문집
- **논문 링크:** [논문 바로가기](https://kiss.kstudy.com/Detail/Ar?key=4096826)

---

## 🎯 배경 및 목적

현대 차량은 다양한 전자 장치들이 CAN 통신을 통해 연결됩니다. 그러나 **CAN 프로토콜은 인증 및 암호화 기능이 없어 보안에 매우 취약**합니다.

기존의 Rule 기반 탐지 기법은 새로운 공격에 대한 대응력이 낮기 때문에, 본 연구에서는 **머신러닝 기반 탐지(XGBoost)**를 적용하고, **휴리스틱 피처(TimeDiff, DataDiff)**를 추가하여 탐지 성능을 향상시켰습니다.

### ✅ 문제점 및 해결 전략

| 문제 | 해결 전략 |
|------|------------|
| CAN의 보안 취약성 | 🔧 머신러닝 기반 탐지 기법 도입 |
| Rule-based 탐지의 한계 | 🔧 데이터 기반 학습으로 유연한 대응 |
| Replay 공격 탐지 어려움 | 🔧 TimeDiff, DataDiff 휴리스틱 피처 도입 |

---

## 🔬 연구 방법

### 1. 데이터셋

- **제공처:** 고려대학교 해킹대응 기술 연구실
- **데이터셋:** Car Hacking: Attack & Defense Challenge 2021 (Rev)
- **차량 모델:** 현대 아반떼 CN7
- **공격 유형:** Flooding, Fuzzing, Replay, Spoofing
- [데이터셋 링크](http://ieee-dataport.org/open-access/car-hacking-attack-defense-challenge-2020-dataset)

### 2. 데이터 전처리

- `Data` 필드: 8바이트 → 개별 byte로 분해
- `CAN ID`: Integer 변환
- `Timestamp`: 0 base 정규화
- **추가 피처**
  - `TimeDiff`: 메시지 발생 간격
  - `DataDiff`: 이전 메시지와의 바이트 차분값

### 3. 모델 구성

- **모델:** XGBoost
- **비교 모델:**
  - `X-1`: 기본 피처만 사용
  - `X-2`: TimeDiff, DataDiff 포함
- **하이퍼파라미터 튜닝:** `n_estimators` 등

---

## 🧪 실험 결과

- **Train Set:** Pre_train_D_1 (806,390건)
- **Test Set:** Pre_train_D_2 (889,395건)
- **개발 환경:** Python, XGBoost, scikit-learn, Pandas
- **하드웨어:** Intel i5-11400F / RAM 16GB

| 모델 | 정확도(Accuracy) | 정밀도(Precision) | F1-Score |
|------|------------------|--------------------|----------|
| X-1  | 96.98%           | 80.85%             | 82.02%   |
| X-2  | **99.59%**       | **94.84%**         | **96.81%** |

> 🔍 **TimeDiff, DataDiff 도입으로 Replay 공격 탐지 성능 및 전체 정확도 대폭 향상**

---
