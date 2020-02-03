이 포스트의 내용은 https://towardsdatascience.com의 '[Illustrated Guide to Recurrent Neural Networks](https://towardsdatascience.com/illustrated-guide-to-recurrent-neural-networks-79e5eb8049c9)'을 기반으로 정리한 글입니다. 개인적인 공부차원에서 작성한 것임을 참고하시기 바랍니다.

# RNN(Recurrent Neural Network)

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img001.gif" style="zoom:200%;" />

개인적인 생각으로 RNN의 컨셉은 **"이전의 '기억'으로 판단하고 행동한다"** 인 것같다.

여기에 두 문장을 보자

> The clouds are in the [  ***sky***  ] 

> I grew up in French...I speak fluent [  ***French***  ]



문장 속 [ ]안에 단어가 비어있다고 생각해보자. The clouds are in the [    ]에서 The, clouds, are, in, the 등을 통해서 ***sky***라는 단어를 유추할 수 있다. 또한 I speak fluent [    ]에서 ***French***를 유추하려면 앞의 문장인 I grew up in French가 필요하다. 이처럼 RNN은 데이터를 순서대로 기억하고 그것을 토대로 다음 데이터를 예측하는 방식의 알고리즘이다.  

RNN이 흔히 사용되고 있는 분야는 음성인식(Speech recognition), 번역(Language translation), 주가 예측(Stock predictions) 등이 사용될 뿐만 아니라 영상 내용 분석 등에도 사용이 된다.



- *음성인식(Speech recognition)*
- *번역(Language translation)*
- *주가 예측(Stock predictions)* 
- *영상 내용 분석(Image recognition)*



위의 문장에서 봤듯이 문장에는 순서가 존재한다. 그 순서를 토대로 다음에 올 단어를 예측한다. 이처럼 순서가 있는 데이터를 **시계열 데이터(Sequence Data)**라고 한다. RNN이 학습을 하기 위해서는 시계열 데이터가 반드시 필요하다.





### 시계열 데이터(Sequence Data)

위에서 예시를 들었던 문장 대신 다른 예시를 들어보겠다.

바로 앞에 공이 하나 있다. 이 공의 움직임을 실시간으로 스냅샷 촬영을 해보자.





![](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img002.png)



공이 어느 방향으로 움직일 지를 예측할 것인데 위의 그림만 봐서는 공의 움직임을 예측할 수가 없다. 하지만 이동된 공의 위치를 공의 위치를 연속해서 여러 장의 스냅샷을 찍으면 더 나은 예측을 할 수 있는 충분한 정보를 갖게 되어 공이 오른쪽으로 움직일 것이라는 것을 예측할 수 있다.

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img003.gif" style="zoom:200%;" />

다시 한 번 정의하자면 **시계열 데이터(Sequence Data)**는 특정한 순서로 나열된 데이터이다. 



### RNN

딥러닝을 처음 배울 때 Input Layer / Hidden Layer / Output Layer로 이루어져 있다고 배운다. 이러한 방식을 Feed-Forward Neural Network라고 한다. Feed-Forward Neural Network방식으로 시계열 데이터를 처리할 수는 있지만 전체 시퀀스를  하나의 데이터 포인트로 변환해야 한다.

![Feed - Forward Neural Network](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img004.png	)



아래의 그림은 이전의 인풋이 다시 Hidden Layer로 들어가 학습을 하는 RNN을 나타내는 그림이다.

![RNN](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img005.gif)

챗봇(Chatbot)을 예시로 들어보자. 챗봇은 사용자가 입력한 텍스트를 기반으로 사용자의 의도를 분류할 수 있다.

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img006.gif" style="zoom:200%;" />



위의 그림에서 사용자가 어떤 텍스트를 챗봇에게 말한 뒤 챗봇이 'Asking for the weather'이라는 사용자의 의도를 파악하였다. 이것이 어떻게 일어나는지 살펴보자. 

사용자가 ***What time is it?*** 이라는 문장을 입력하였다. RNN은 이 문장을 한 단어씩 순서대로 처리할 것이다. 



<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img007.gif" style="zoom:150%;" />



첫 번째 단계에서 RNN이 단어 ***What*** 부터 인코딩(encoding)을 하고 출력한다. 

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img008.gif" style="zoom:200%;" />



다음 단계에서는 RNN은 이전에 입력했던 ***What*** 과 ***time*** 을 함께 처리한다.

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img009.gif" style="zoom:200%;" />



이렇게 문장이 끝이 날 때까지 같은 과정을 반복한다. 

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img010.gif" style="zoom:200%;" />



최종 아웃풋이 만들어지면 이것을 Feed-Forward Neural Network에 전달하여 사용자의 의도를 추출한다.

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img011.gif" style="zoom:200%;" />





### RNN의 한계

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img012.png" style="zoom:200%;" />

위의 시계열 데이터인 ***What*** 부터 ***?*** 까지 단어를 학습시킬수록 색의 영역이 점점 줄어들고 있음을 확인할 수 있다. 이 뜻은 RNN이 더 많은 단계를 처리함에 따라 이전 단계에서 정보를 유지하기 힘들다는 뜻이다. 맨 왼쪽인 ***What*** 과 ***time*** 은 원에서 거의 없는거나 마찬가지다. 이러한 것을 보완하기 위해 만든것이 **LSTM** 이다.



[LSTM]()