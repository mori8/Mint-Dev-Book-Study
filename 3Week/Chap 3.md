Chap 3 분류
===

3.1 MNIST
---
MNIST 데이터셋: 손으로 숫자를 쓴 70000개의 작은 숫자 이미지들

3.2 이진 분류기 훈련
---
이진 분류기를 사용하여 특정 숫자임과 특정 숫자가 아님을 분류한다.  
ex) 5-감지기 : '5'와 '5아님' 두 개의 클래스를 구분할 수 있는 감지기.  
이 과정에서 분류 모델을 하나 선택해서 훈련시켜본다.  

3.3 성능 측정
---
정확도는 분류기의 성능 측정 지표로는 선호하지 않는다. 특히 불균형한 데이터셋을 다룰 때 더 그렇다.  
분류기의 성능을 평가할 때는 __오차 행렬__ 을 사용하면 성능을 더 정확하게 판단할 수 있다.    
오차 행렬의 행은 실제 클래스를 나타내고 열은 예측한 클래스를 나타낸다.  
데이터를 총 네가지의 종류로 나눌수 있는데, 5-감지기를 예로 들 때  
먼저 '5가 아닌 이미지' 중에서 '5 아님'으로 분류된 데이터를 __진짜 음성__ 이라고하고, '5'로 분류된 데이터를 __거짓 양성__ 이라고 한다.  
또 '5 이미지' 중에서 '5 아님'으로 분류된 데이터를 __거짓 음성__ 이라고 하고, '5'로 분류된 데이터를 __진짜 양성__ 이라고 한다.  

오차 행렬이 많은 정보를 제공해주지만 가끔 더 요약된 지표가 필요할 때도 있다.  
양성 예측의 정확도를 사용하는 경우도 꽤 있는데, 이를 분류기의 정밀도라고 한다.  

정밀도 = 진짜 양성의 수 / (진짜 양성의 수 + 거짓 양성의 수)  [ex) 5라고 분류된 것중 5라고 맞게 분류된 비율]  
=> 정밀도의 경우 확실한 양성 샘플 하나만 예측하면 간단히 완벽한 정밀도를 얻을 수 있지만, 이는 분류기가 다른 모든 양성 샘플을 무시하기 때문에 그리 유용하지 않다.  [5가 아닌 이미지중에서 5라고 분류된 것은 고려 안하므로]  

따라서 정밀도는 재현율이하는 또 다른 지표와 같이 사용하는 것이 일반적이다.  
재현율은 분류기가 정확하게 감지한 양성 샘플의 비율로, 민감도 혹은 진짜 양성 비율이라고도 한다.  

재현율 = (진짜 양성의 수) / (진짜 양성의 수 + 거짓 음성의 수)  [ex) 5인 이미지를 분류한 것중 5라고 맞게 분류된 데이터의 비율]  

* 정밀도와 재현율이 비슷한 분류기에서는 F1점수가 높지만 상황에 따라 정밀도가 중요할 수도 있고 재현율이 중요할 수도 있다.  
예를 들어 '5가 아닌' 이미지가 '5'로 분류되는 경우가 있더라도 '5'의 이미지는 모두 '5'로 분류되었으면 좋겠다면 정밀도가 낮더라도 재현율이 높은것이 좋다.  
반대로 '5가 아닌'이미지를 최대한 걸러내고 '5'인 이미지만 '5'로 분류되게 하고 싶다면 재현율이 낮더라도 정밀도를 높게 하는 것이 더 좋다.  

위와 같은 예처럼 재현율이 높아지게 되면 필연적으로 정밀도가 낮아질수 밖에 없게되고, 반대도 마찬가지이다. 이를 정밀도/재현율 트레이드 오프라고 한다.  

결정 임계값이라는 변수를 통해 재현율과 정밀도를 조절할 수가 있는데, 정밀도가 급격하게 낮아지는 시점을 기준으로 임계값을 설정하는 것이 가장 좋다.  

ROC(진짜양성비율 / 거짓양성비율)곡선과 AUC(곡선 아래의 면적)을 이용하면 분류기의 성능을 더 좋게 만들수 있다.  


3.4 다중 분류
---
이진 분류가 두 개의 클래스를 구별하는 반면 다중 분류기는 둘 이상의 클래스를 구별할 수 있다.  
일부 알고리즈은 여러 개의 클래스를 직접 처리할 수 있는 반면, 다른 알고리즘은 이진 분류만 가능하다. 하지만 이진 분류기를 여러 개 사용해 다중 클래스를 분류하는 기법도 많다.   

예를 들어 특정 숫자 하나만 구분하는 숫자별 이진 분류기 10개를 훈련시켜 클래스가 10개인 숫자 이미지 분류 시스템을 만들 수 있다. 이미지를 분류할 때 각 분류기의 결정 점수 중에서 가장 높은 것을 클래스로 설정하면 된다.(OvR전략)  
혹은 0과 1 구별, 0과 2 구별, 1과 2 구별 등과 같이 각 숫자의 조합마다 이진 분류기를 훈련시킬 수도 있다.(OvO 전략) 즉 이땐 클래스가 N개라면 N*(N-1)/2개의 분류기가 필요하게 된다.  
  또 이미지 하나를 분류하려면 분류기를 모두 통과시켜서 가장 많이 양성으로 분류된 클래스를 선택한다. 이와 같은 방법의 장점은 각 분류기의 훈련에 구별할 두 클래스에 해당하는 샘플만 필요하다는 것이다.  
 훈련 세트에 민감한 일부 알고리즘은 OvO를 선호하는 반면, 대부분의 이진 분류 알고리즘에서는 OvR을 선호한다.  
 
 3.5 에러 분석
 ---
 SGDClassifier를 사용하여 분류하면 클래스마다 픽셀에 가중치를 할당하고 새로운 이미지에 대해 단순히 픽셀 강도의 가중치 합을 클래스의 점수로 계산하기 때문에 3과 5같은 몇 개의 픽셀만 다른 경우 정확한 분류를 못한다.  
 분류기는 이미지의 위치나 회전 방향에 매우 민감하므로 3과 5의 에러를 줄이는 한 가지 방법은 이미지를 중앙에 위치시키고 회전되어 있지 않도록 전처리 하는 것이다.  
 
 3.6 다중 레이블 분류, 다중 출력 분류
 ---
 분류기가 샘플마다 여러 개의 클래스를 출력해야 할 때도 있는데, 이처럼 여러 개의 이진 꼬리표를 출력하는 분류 시스템을 __다중 레이블 분류__ 라고 한다.  
 다중 레이블 분류에서 한 레이블이 다중 클래스가 될 수 있도록 일반화 한 것을 __다중 출력 분류__ 라고 한다.
 