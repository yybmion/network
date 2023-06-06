### AIMD

RTT정상적으로 받으면서 CWND를 1,2,3,4 +로 증가함
LOSS(TIME OUT)가 일어나면 CWND를 배수로 줄여나감

![image](https://github.com/yybmion/network/assets/113106136/e0706695-065a-47de-b9ac-d5f7efd2120f)

![image](https://github.com/yybmion/network/assets/113106136/c96053ca-937a-440d-a82c-0d18917dd0f8)

___

![image](https://github.com/yybmion/network/assets/113106136/6816b405-1a31-4f85-a622-f2654ae01524)

![image](https://github.com/yybmion/network/assets/113106136/2f0cb113-86ba-45e3-a1fc-cd4c0e859204)

AIMD == SAW TOOTH BEHAVIOR

![image](https://github.com/yybmion/network/assets/113106136/3ff4698b-3c4b-4e22-b888-ae2500428a57)

절반씩 줄임 문제 생기면

___

근데 일시적인 혼잡이 생기면 반으로 줄여져서 원래 보낼 수 있는 양은 엄청 많은데
+1 +2 +3 씩 증가하니 낭비되는 것이 많다.
(비효율성)
![image](https://github.com/yybmion/network/assets/113106136/69bce3ea-e2e8-4d2f-983f-283e14e54b33)

___

그래서 나온게 Slow start

- slow start
- congestion avoidance
- fast retransmit
까지가 Tahoe  이것은 GBN 사용

추가로
- Fast Recovery 하면 Reno 이것은 tcp 사용
![image](https://github.com/yybmion/network/assets/113106136/bd86cadd-c3d4-491d-b6a1-66e0a80bea66)

___

![image](https://github.com/yybmion/network/assets/113106136/6497cf65-2e79-4bb4-bf16-f386222bc14b)
Slow start 무조건 increasment 는 1 
congestion 발생해도 반으로 줄이는 것이 아닌 1로 고정

![image](https://github.com/yybmion/network/assets/113106136/a8320598-ed6a-4b10-8d39-58c02deef414)

___

![image](https://github.com/yybmion/network/assets/113106136/cd93c6c6-3938-4169-9374-76940185c8d4)

빨리 증가하는 것을 볼 수 있다.

![image](https://github.com/yybmion/network/assets/113106136/be294b12-e027-4346-9735-fe332be78840)

근데 이것도 문제가 분명 있다.

절대 slow 하지 않다는 것이다. 
그래프를 보면 알 수 있듯이 기하급수적으로 증가한다.
그래서 기하급수적으로 증가하는 부분중에 congestion이 발생할 수 있는데
갑자기 늘리면 congestion 발생 확률이 더 늘어난다.
   
![image](https://github.com/yybmion/network/assets/113106136/63b2a0bc-a741-4307-a54a-4c3fbf6c50d9)

이를 해결하기 위해 나온것이 Threshold

![image](https://github.com/yybmion/network/assets/113106136/89f2e406-78cb-4259-8a3c-b97b39b686da)
Tahoe버전이라 GBN 사용

그래서 결국 SLOW START와 AIMD 둘다 사용

![image](https://github.com/yybmion/network/assets/113106136/7018adc2-df8f-420a-9e3c-c0782d6f5212)

SSTHRESH 정해놓고 그 전에는 SLOW START 사용하고 그 지점 넘어가면
AIMD 사용 CONGESTION 발생하면 SSTHRESH 를 CWND의 1/2로 줄이고 
CWND를 1로 설정 후 다시 시작

___

이것도 문제가 있다. 그림을 보면 seq=100이 일시적으로 네트워크가 안좋아서 못받았거나 증증
상황이 있는데 다른 seq는 잘 간 것을 보아 이 하나 때문에 3 duplicate ACK이 발생하여 
CWND를 1로 줄이는 것은 또한 낭비가 될 수 있다.
![image](https://github.com/yybmion/network/assets/113106136/a315c6ee-0335-44f1-ad0a-1917ffe9e1b2)

그래서 Fast Recovery를 사용

이는 timeout이 아닌 3 duplicate ACK에서 사용한다.

3 DUPLICATE ACK은 그 해당 seq만 못 받은 것이기 떄문이다.

![image](https://github.com/yybmion/network/assets/113106136/d96e78d8-cb71-4b0c-8526-1b65a0a6dd2f)

CWND는 1로 줄이는 것이 아니다.
![image](https://github.com/yybmion/network/assets/113106136/dcd53c7f-c6c5-44e1-9d78-9275ddd876cd)

___
**비교**

![image](https://github.com/yybmion/network/assets/113106136/6c59fdc4-9976-4573-b4e4-c4761e044440)

![image](https://github.com/yybmion/network/assets/113106136/aa4d8696-6e32-4a76-919c-029e1a801088)

![image](https://github.com/yybmion/network/assets/113106136/cb8b3651-004d-4105-ae91-269e09b195aa)

___

### TCP FAIRNESS
  
모든 CLIENT가 공평하게 데이터를 보내고 받을 수 있다.

![image](https://github.com/yybmion/network/assets/113106136/e31609ce-9bdd-45f5-bd74-bac5830a4aff)

___

### QUIC 

![image](https://github.com/yybmion/network/assets/113106136/d5d7b5e9-31f3-4aa2-84c5-ff57e7ccdcbe)



 
