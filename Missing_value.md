시계역 데이터 결측치 대체 방법

1. 결측치 데이터 종류

   1. 완전 무작위 결측(MCAR: Missing Completely At Random)
      - 변수의 종류, 값과 상관없이 비슷한 분포로 누락된 데이터
   2. 무작위 결측(MAR: Missing at Random)
      - 어떤 특정 변수에 대하여 데이터가 누락되는 경우
   3. 비무작위 결측(MNAR: Missing Not At Random)
      - 누락되는 부분들이 무작위로 누락되는 것이 아닌 누락된 변수의 값이 누락된 이유와 관련이 있는 경우
      - 대부분의 시계열 데이터에 해당

   > 1, 2번의 경우 무작위로 데이터가 누락되어 있어 결측치를 제거한 후 데이터를 사용하는 것이 좋고, 3번의 경우 무작위적인 결측이 아니기 때문에 가능한 한 결측치를 보간 혹은 대치한 데이터를 사용하는 것이 바람직

​	

2. 결측치 처리 동향

   1. 통계적 기법

      - 단순 대입법: 결측치 앞뒤 값에 대한 평균, 중앙값 등 기초 통계값으로 대치 > 표준오차가 과소 추정되는 단점 
      - 확률 대치법
        - Hot-deck: 비슷한 성향을 가진 케이스의 값으로 결측을 대치
        - Nearest-Neighbor 등 제안 > 대부분 추정량 표준 오차 계산이 어려운 단점

      이와 같은 단일변량 신호에 대한 결측치처리 기법보다 발전된 형태로 다변량 변수에더 적용이 가능한 다중 대치법이 등장

      - MICE(Multiple Imputation using Chained Equations): 결측치 별 복원 모델에 따라 대치

   2. 행렬기반 기법

      - TRMF(Temperal Regularized Matrix Factorization):  데이터 기반 시간 학습 및 예측하는 시간 정규 화된 행렬 분해 프레임워크.  결측값이 있 는 고차원 시계열 데이터에 매우 적합. 시간적 종속성을 모델 링할 뿐만 아니라 데이터 기반 종속성 특징도 학습 하여 우수한 결과를 도출
      - PSMF(Probabilistic Sequential Matrix Factorization): 고차원 시계열로 구성된 시변량 및 비정상성 데이터 세트를 분해하기 위한 방법.  일반적인 미분 가능한 비선형 부분적 공간 모델을 보정하고 추정하는 결측치 처리 방법

   3. 회귀분석 기법

      - 선형 회귀분석: 전체적인 데이터 흐름을 고 려하지 못한다는 단점이 존재 하여

      `시계열 모형`이 대두

      - 자기회귀모형(AR: Autoregressive)
        - ARIMA(Autoregressive Integrated Moving Average): 과거 데이터와 그의 추세를 반영하여 나타낼 수 있는 모형. 기본적인 시계열 데이터 예측에 사용되며 선형, 비선형 모두 적용 가능.
        - LATC(Low-Rank AutoRegressive Tensor Completion):  다변량 시계열 데이터를 3차원의 텐서 형태로 변환하여 AR 모델을 적용하는 것으로 텐서 형태로 변환할 때, 시간, 계절성, 다변량 변수 다음과 같은 3가지의 기준으로 고려

   4. RNN기반 기법

      - M-RNN(Multi-directional Recurrent Neural Network): 데이터 스트림 내의 보간과 데이터 스트림을 패턴의 특성에 관한 가정을 통해 추정
      - GUR-D
      - BRITS(Bidirectional Recurrent Imputation for Time Series): 특정 분포의 가정 없이 결측값을 대치하기 위하여 동적 시스템을 양방향 RNN으로 조정. 이 방법은 여러 개의 상관된 결측값 처리가 가능하고, 비선형 역학을 기본으로 하는 시계열로 일반화가 가능. 또한, 데이터 기반 대 치를 제공하며 누락 데이터가 있는 일반 상황에 적용 가능하다.
      - SSIM(Sequence-to-Sequence Imputation Model):  과거와 미래의 정보를 모두 활용할 수 있도록 장단기 메모리 네트워크를 사용한다. 또한, 슬라이딩 윈도우 알고리즘으로 데이터양에 비해 많은 수의 훈련 샘플을 생성하기 때문에 적은 데이터 셋으로도 사용 가능한 알고리즘
      - ODE-RNN:  상미분 방정식(Ordinary Differential Equation)에 의해 RNN의 은닉 계수의 관계를 도출하여 학습하는 모델
      - Latent ODE:  VAE(Variational AutoEncoder)를 기반으로 하여 ODE-RNN을 인코더 구조로 ODE를 디코더 구조로 이용하여 처리하는 방법

      > 불규칙적으로 샘플링된 데이터도 ODERNN과 Latent ODE는 적용 가능하며 클래식 RNN 기반 모델보다 뛰어난 성능을 보여준다.

      - CDSA(Cross-Dimensional Self-Attention): 다변량의 지리적인 위치가 지정된 시계열 데이터 처리의 경우 적합한 모델(ex, 대기질 데이터).  위치의 정보가 결합된 시계열의 데이터의 경우 더 효과 적인 결측값 대치 혹은 예측 성능을 보임.

   5.  GAN 기법

      - GAIN(Generative Adversarial Imputation Networks)
      - GRUI-GAN: 기존에 RNN기반 딥러닝 기 반 결측치 처리 방법 중 제안된 GRU-D 구조를 약 간 변형하여 GAN의 구조에 결합한 기술. 모델을 학습하는 데 많은 시간이 소요되며 임 의의 노이즈가 입력으로 들어가기에 정확도가 안 정적이지 못한 것으로 보일 수 있어 실용적이지 못 하다는 단점이 존재
      - E2GAN(End-to-End): 생성자(G)에 GRUI를 기반한 오토인코더 구조를 채택하여 구성하여 제안
      - NAOMI(Non-Autoregressive Multiresolution Sequence Imputation): 이전 값과 미래 값 모두를 활용한 비자기 회귀 모델. 결측치 대치 작 업 시에는 미래값과 과거값 모두를 이용하여 양방향 모델을 훈련.
      - Bi-GAN(Bi-directional GAN): 생성적 적대 신경망을 양방향으로 반복적으로 진행하는 네트워크

