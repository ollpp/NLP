# # README

## ==== FLOW ====
### 1. 라벨 확보 과정
- 제시된 dataset.csv 파일을 test_set으로 생각한다.
- 구글링과 selenium을 통해 크롤링을 통해 train_set을 확보한다.
- test_set :10000개 // train_set:40000개 구성
- 제시된 dataset.csv 파일을 보면 게임리뷰와 영화리뷰가 다수인것을 확인할 수 있다.
- 구글링을 통한 영화 리뷰 20000개 확보
- selenium을 통한 게임 리뷰 20000개 확보
- train_set을 기반으로 학습을 진행하고 model을 통해 test_set의 라벨을 채운다.


### 2. 데이터 전처리
- 한글 전처리를 진행하기 위해 숫자, 영어, 특수기호 등을 정규표현식으로 제거한다.
- 한글만 남은 문장을 konlpy를 통해 불용어 제거, 어간 추출을 진행한다.
- 각 리뷰에서 뽑은 bag-of-words 중 상위 1000개의 어휘로 학습을 진행한다.


### 3. 신경망 구성
- word2Vec에 기초하여 bag-of-words를 vector화 시켰다.
- 기울기 손실이 적은 ReLu 함수를 활성 함수로 2번 구성한다.
- 마지막층은 sigmoid 함수를 사용한다.
- epoch은 10으로 두어 10번 반복하며 batch-size는 512를 주었다.
- 생성된 model을 기본으로 예측할 때, 최종 확률이 
  0.5보다 크면 1(영화), 작으면 0(그외) 데이터로 정의하였다.




## ==== Source ====
### 1. crwaling.py
- 가장 많이 보였던 게임(메이플 스토리) 리뷰 데이터를 수집
- 메이플 스토리 홈페이지의 게시판에서 리뷰 수집
- 수집한 데이터를 GameRV 폴더안에 csv형식으로 저장


### 2. run.py
- model을 통한 라벨링 실행 파일
- 소스코드를 실행하면 다음 3가지를 입력해주어야 한다.
1) 예측하고자 하는 데이터 파일
   Dataset 폴더에 위치한 라벨링이 필요한 데이터 파일의 이름을 적는다.(.csv 확장자 제외)
   csv의 형식이 dataset.csv와 동일한 행렬 구성과 열이름을 가져야 한다.(리뷰column 명 반드시 'txt')
2) 사용할 모델 파일명
   Model 폴더에 위치한 사전에 학습된 파일명을 적어준다.
   확장자는 제외하고 적어준다.
   사전 학습된 파일명 : first_Model
3) 사용할 토큰 파일명
   사전에 학습을 통해 토큰화 시켜놓은 JSon 파일명을 적어준다.
   확장자는 제외하고 적어준다.
   사전 토큰 Json 파일명 : train
   

### 3. movieNlp.py
- 데이터 전처리, 신경망 구성, 데이터 라벨링에 대한 메소드를 정의
- 각 메소드에 대한 주석 참고


