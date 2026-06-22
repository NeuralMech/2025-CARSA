# 2025 CARSA Autonomous Driving Control Study

대학생 자율주행 대회 준비 과정에서 수행한 상위 제어 및 신경망 기반 조향 제어 실험을 정리한 저장소입니다.

이 프로젝트에서는 차량의 주행 상태와 경로 정보를 입력으로 받아 조향각을 출력하는 End-to-End steering controller를 실험했습니다. MATLAB/Simulink와 IPG CarMaker 환경을 사용했으며, PID 제어 기반 주행 데이터를 기준 데이터로 활용했습니다.

## Project Context

대회 준비 과정에서 저는 상위 제어 영역의 조향 제어 모델링을 담당했습니다.

초기에는 단일 주행 조건에서 생성된 데이터로 인공신경망 기반 조향 모델을 학습했습니다. 그러나 특정 주행 조건에 과하게 맞춰져 실제 closed-loop 주행에서 조향이 불안정하고 비정상적인 거동을 보였습니다.

이를 개선하기 위해 CarMaker 시뮬레이션에서 주행 속도 조건을 확장하여 학습 데이터를 다시 구성했습니다. 목적은 단일 조건에 대한 과적합을 줄이고, 속도 변화에 따른 조향 모델의 일반화 가능성을 확인하는 것이었습니다.

## Simulation-based Data Expansion

기존 데이터는 샘플 수는 많았지만, 하나의 주행 사례를 고밀도로 샘플링한 것에 가까웠습니다. 따라서 데이터의 양은 충분해 보이더라도, 다양한 주행 조건을 포함하지 못해 모델의 일반화 성능이 제한된다고 판단했습니다.

이를 개선하기 위해 IPG CarMaker 시뮬레이션에서 PID 제어기를 기준 제어기로 사용하고, 동일한 제어 gain 조건에서 주행 속도를 변화시키며 학습 데이터를 다시 구성했습니다.

구체적으로 30 km/h부터 37 km/h까지 1 km/h 간격의 총 8개 속도 조건을 생성하여, 기존 단일 속도 데이터셋보다 더 넓은 주행 조건을 포함하도록 했습니다.

## Model Input and Output

**Inputs**

- Vehicle longitudinal velocity (`vx`)
- Target path deviation or waypoint-related information (`Waypoint_y`)
- Vehicle/path state variables used in the Simulink and CarMaker environment

**Output**

- Steering angle command (`Wheel_angle`)

발표자료 기준 최종 학습 데이터셋은 약 210,148개의 샘플로 구성되었으며, 입력은 41차원, 출력은 1차원으로 설정했습니다.

## Tools

- MATLAB
- Simulink
- IPG CarMaker
- Neural Network Fitting
- PID-based reference driving data

## What I Tried

- MATLAB/Simulink와 IPG CarMaker 기반 주행 시뮬레이션 구성
- PID 제어기를 기준으로 조향 학습용 reference data 생성
- 단일 속도 조건 데이터로 학습한 신경망 조향 모델의 일반화 한계 확인
- 30-37 km/h 범위에서 1 km/h 간격으로 총 8개 속도 시나리오 생성
- 차량 속도 및 경로 정보를 입력으로 받아 조향각을 출력하는 neural network fitting 수행
- 속도 조건 확장이 lateral error 및 주행 안정성에 미치는 영향 검토

## Results and Observations

속도 조건을 확장한 데이터셋을 사용하면서 단일 조건 데이터만 사용했을 때보다 모델의 일반화 가능성을 더 명확히 검토할 수 있었습니다.

그러나 테스트 주행 결과, 특정 구간에서 약 0.1 m 수준의 lateral error가 발생했습니다. 이 오차는 해당 테스트 조건에서는 큰 문제로 이어지지 않았지만, 더 높은 속도나 급격한 주행 환경에서는 시스템 안정성에 영향을 줄 수 있는 잠재적 문제로 판단했습니다.

또한 동일한 PID gain 조건에서 속도만 변화시켜 데이터를 생성했기 때문에, 다양한 경로 형상, 외란, 조향 상황을 충분히 포함한 데이터셋이라고 보기는 어렵습니다.

## Important Note

본 프로젝트에서 사용한 데이터 확장은 엄밀한 의미의 일반적인 data augmentation이라기보다, CarMaker 시뮬레이션에서 속도 조건을 다양화하여 학습 데이터셋의 범위를 넓힌 접근입니다.

즉, 노이즈 추가, 회전/변환, synthetic perturbation과 같은 일반적인 증강 기법을 적용한 것이 아니라, 서로 다른 속도 조건의 PID 기준 주행 데이터를 추가로 생성하여 training data coverage를 넓히는 방식이었습니다.

또한 이 End-to-End steering controller는 최종 대회 주행에는 사용되지 않았습니다. 본 저장소는 최종 적용 모델의 성과라기보다, 신경망 기반 조향 제어기를 실험하면서 데이터 다양성, 일반화 성능, closed-loop 검증의 중요성을 확인한 기록입니다.

## Repository Contents

- `25.05.18_심층 학습 제어기 기반 횡방향 오차 _LHJ.pptx`  
  신경망 기반 조향/횡방향 제어 실험 초기 발표자료입니다.

- `25.07.01_NNF_improvement_LHJ.pptx`  
  속도 조건 확장을 통한 학습 데이터셋 개선 및 일반화 성능 검토를 정리한 발표자료입니다.

## Lessons Learned

- 단일 주행 조건에서 학습한 신경망 제어기는 특정 경로와 상태에 과적합되기 쉽습니다.
- 샘플 수가 많더라도 데이터가 하나의 조건에 편중되어 있으면 일반화 성능을 확보하기 어렵습니다.
- 자율주행 제어 모델은 단순한 예측 오차뿐 아니라 closed-loop 주행 안정성으로 평가해야 합니다.
- 시뮬레이션 기반 데이터 확장은 유용하지만, 속도 조건만 바꾸는 방식으로는 충분하지 않습니다.
- 대회 적용을 위해서는 신경망 기반 제어기와 기존 제어기 간의 안정성, 복구 가능성, 검증 기준을 명확히 비교해야 합니다.

## Future Work

- 다양한 속도 조건과 경로 형상을 포함한 데이터셋 구성
- Pure Pursuit, Stanley, MPC 등 기존 제어기와 비교
- Lateral error, heading error, steering smoothness 등 closed-loop 지표 기반 평가
- 외란 조건과 센서 노이즈를 포함한 robustness 검증
- 데이터 기반 조향 제어기와 안정성 보장 제어기의 hybrid 구조 검토
