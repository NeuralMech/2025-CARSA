# 2025 CARSA Autonomous Driving Control Study

대학생 자율주행 대회 준비 과정에서 수행한 상위 제어 및 신경망 기반 조향 제어 실험을 정리한 저장소입니다.

이 프로젝트에서는 차량의 주행 상태와 경로 정보를 입력으로 받아 조향각을 출력하는 End-to-End steering controller를 실험했습니다. MATLAB/Simulink와 IPG CarMaker 환경을 사용했으며, PID 제어 기반 주행 데이터를 기준 데이터로 활용했습니다.

## Project Context

대회 준비 과정에서 저는 상위 제어 영역의 조향 제어 모델링을 담당했습니다.

초기에는 한 바퀴 주행 데이터만을 이용해 인공신경망을 피팅했으나, 특정 구간에 과하게 맞춰져 실제 주행 시 조향이 불안정하고 비정상적인 거동을 보였습니다.

이를 개선하기 위해 PID 제어기를 기준으로 생성한 5바퀴 주행 데이터를 사용하여 학습 데이터 범위를 확장했습니다. 이를 통해 단일 lap 데이터에 대한 과적합 문제를 완화하고, 주행 경로 전반에 대한 모델의 일반화 가능성을 확인하고자 했습니다.

## Model Input and Output

**Inputs**

- Vehicle longitudinal velocity (`vx`)
- Yaw rate
- Waypoint-related path information
- Vehicle/path state variables used in the Simulink and CarMaker environment

**Output**

- Steering angle command

## Tools

- MATLAB
- Simulink
- IPG CarMaker
- Neural Network Fitting

## What I Tried

- PID controller 기준 주행 데이터 생성
- 1 lap 데이터 기반 steering neural network fitting
- 단일 lap 학습 시 발생하는 비정상 조향 문제 확인
- 5 laps 기준 데이터로 학습 범위 확장
- 입력 변수와 조향각 출력 간 관계 학습 실험
- CarMaker 환경에서 조향 제어 거동 검토

## Important Note

이 프로젝트에서 수행한 데이터 확장은 엄밀한 의미의 data augmentation이라기보다, PID 기준 주행 데이터를 여러 lap으로 확장하여 학습 데이터의 coverage를 넓힌 접근입니다.

또한 이 End-to-End steering controller는 최종 대회 주행에는 사용되지 않았습니다. 본 저장소는 대회 준비 과정에서 수행한 실험, 문제점, 개선 시도, 그리고 한계를 정리하기 위한 기록입니다.

## Repository Contents

- `25.05.18_심층 학습 제어기 기반 횡방향 오차 _LHJ.pptx`  
  신경망 기반 조향/횡방향 제어 실험 초기 발표자료입니다.

- `25.07.01_NNF_improvement_LHJ.pptx`  
  학습 데이터 확장 및 모델 개선 시도를 정리한 발표자료입니다.

## Lessons Learned

- 단일 주행 조건에서 학습한 신경망 제어기는 특정 경로와 상태에 과적합되기 쉽습니다.
- 자율주행 제어 모델은 예측 성능뿐 아니라 closed-loop 주행 안정성으로 평가해야 합니다.
- 데이터 수를 늘리는 것만으로는 충분하지 않으며, 다양한 주행 조건과 경로, 속도, 외란 조건을 포함한 데이터 설계가 필요합니다.
- 대회 적용을 위해서는 신경망 기반 제어기와 기존 제어기 간의 안정성, 복구 가능성, 검증 기준을 명확히 비교해야 합니다.

## Future Work

- 다양한 속도 조건과 경로 형상을 포함한 데이터셋 구성
- Pure Pursuit, Stanley, MPC 등 기존 제어기와 비교
- 조향각 예측 오차뿐 아니라 closed-loop tracking error 평가
- 데이터 기반 조향 제어기와 안정성 보장 제어기의 hybrid 구조 검토
