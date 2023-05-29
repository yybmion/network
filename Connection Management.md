 ![image](https://github.com/yybmion/network/assets/113106136/e11f77dc-67b1-4708-a4fb-b4946725e8bb)

우리는 이 두부분을 이제 볼 것이다.

**Connection**
![image](https://github.com/yybmion/network/assets/113106136/5d56fd19-4dfa-42d3-9a17-444eba1de4e4)

clinet는 closed상태 server은 항상 열려있어야하므로 Listen상태

처음 client가 보낼때 seq는 랜덤으로 부여한다.

ack-bit는 1 ack는 x+1
알겠다 잘받았다~~~

syn-bit=1 seq=y 내 메시지에도 답장좀 해줄래??
y또한 랜덤

그럼 또 ack-bit =1 , ack = y+1 보내면 client는 extablished 상태가 됨

서버도 ack 받으면 established상태가 됨

___

왜 근데 랜덤 부여??

![image](https://github.com/yybmion/network/assets/113106136/48220836-f482-4203-9a4f-d48dfe2ef7eb)

___

![image](https://github.com/yybmion/network/assets/113106136/c20587c9-e0e3-4279-8a88-d067ced8b8f2)

syn계속 보내서 버퍼를 다 차게 만들어 NORMAL HOST가 접속하지 못하게 만듬
DOS
___

**Closing**

![image](https://github.com/yybmion/network/assets/113106136/85887483-8db1-46a2-b557-e84589f6e90a)

닫을때의 seq number은 마지막 데이터 전송의 ack 넘버를 사용한다.
같은 ack의 seq을 보내니 아~~ 얘 이제 연결 끊으려고 하는구나~

특징은 아직 데이터를 보낼 수 있는 구간이 존재
seq=n에서 n은 클라이언트가 마지막으로 보낸 데이터의 ack num
time wait후 MSL*2의 값을 기다린 후 CLOSED상태로 간다. (CLIENT)

하지만 SERVER는 그냥 끝내자고 한 응답만 CLIENT에게 받으면 바로 끝낸다.

___

MAXIMIM SEGMENT LIFETIME (MSL)

![image](https://github.com/yybmion/network/assets/113106136/16ebe1ff-1c1b-4522-a988-987e7434b9a3)

___

TCP CONNECTION은 SENDER와 RECIEVER 간에 ARQ를 위한 POSITIVE ACK을 위한 관계를 설정하는것이다.
물리적인 CONNECTION은 아니다.

![image](https://github.com/yybmion/network/assets/113106136/1b3db579-50b4-4251-9c6f-06eb98472b67)

___

### TCP CONGESTION CONTROL

- CIRCUIT SWITCHING VS PACKET SWITCHING (CONGESTION이 어떻게 발생할까?

한번 연결되면 
- NO TRANSMISSION DELAY
- NO DELAY VARIATION 딜레이가 변하지 않
- NO LOSS 데이터 손실 X

![image](https://github.com/yybmion/network/assets/113106136/efa08468-9b5e-4872-97b6-99728f4aeb2f)

![image](https://github.com/yybmion/network/assets/113106136/5c6ca224-f68c-4040-8a1f-da40f7bf80b5)

___

PACKET

![image](https://github.com/yybmion/network/assets/113106136/fc279d57-9c95-4737-93a3-42563e87aae0)

CIRCUIT은 DEDICATE한데 PACKET은 SHARED LINK를 사용한다.

혼자만 사용하는 것이 아닌 다 같이 길을 사용한다. 그래서 딜레이가 길어지거나 패킷이 손실되면 
CONGESTION이 발생한다.

즉, SHAERD LINK로 STORE AND FORWARD 방식으로 다른 라우터에게 전달
한 라우터가 순간적으로 많은 데이터를 받으면 버퍼에 가득 차서 오버헤드가 발생
데이터 손실이 될 수 있음

![image](https://github.com/yybmion/network/assets/113106136/4310fd50-fad1-4ba8-9dc4-1e294a2546e1)

___

![image](https://github.com/yybmion/network/assets/113106136/b5a0dd31-b71a-4c51-a709-2b6c9b4e7e33)

___
![image](https://github.com/yybmion/network/assets/113106136/8bcd00e9-ee8f-4fef-9350-c934af3ac13c)

___

![image](https://github.com/yybmion/network/assets/113106136/611cb455-0647-4e40-8fe8-73fcd9abf098)

QUEUEING DELAY 데이터가 버퍼에 들어가서 자기 차례가 되면 나가게 되는데 그 사이 딜레이 시간
RPOCSSING DELAY == TRANSMISSION TIME (패킷이 전부 빠져나가는 시간)
PROPAGATION DELAY는 선을 따라서 다음 라우터까지 전달되는 시간

다른것은 예측이 가능하지만 queueing delay는 예측이 힘들다.
따라서 congestion에서 가장 영향을 많이 주는것이 queueing delay다.

___

throughput??

내가 100개의 메시지를 보냈는데 여기서 성공적으로 몇개의 메시지가 도착했냐의 비율

![image](https://github.com/yybmion/network/assets/113106136/c17a7016-f822-418a-bb02-80514af2ee7e)

(이상적인 그림)

___
![image](https://github.com/yybmion/network/assets/113106136/edd977f7-aefb-4a65-9e88-9acae403068e)

(실제 그림)

___

![image](https://github.com/yybmion/network/assets/113106136/ca7a72cb-98da-40f3-97bc-d526390e9e5f)

패킷이 많아지면 버퍼 오버플로우가 발생 그럼 타임아웃이 일어나고 재전송이 증가한다.
그러면 혼잡이 증폭되고 더 많은 타임아웃이 발생 더 많은 재전송 발
그러면서 계속 증폭되며 congestion collapse 발생
 
![image](https://github.com/yybmion/network/assets/113106136/4269b1a2-51d2-453b-a72a-b62bce9ec039)

___

congestion control 방법 2가지

![image](https://github.com/yybmion/network/assets/113106136/b747b07e-0400-4a4c-bde1-2aaa07274d97)

___

- congestion을 미리 예방
- implicit control
- 그래서 데이터 양 줄임
![image](https://github.com/yybmion/network/assets/113106136/92e5ee35-eefb-485c-94bb-ff972e66da79)

Flow control은 rwnd!! receiver가 주가 되어 제어함
Congestion control은 source(sender)가 주가 되어 제어함

**flow control**
![image](https://github.com/yybmion/network/assets/113106136/c67fdbc2-c4e8-42a2-9b94-a8993c8fe8da)

**congestion control**
![image](https://github.com/yybmion/network/assets/113106136/2a7569ec-287e-45eb-9dcf-fcf1a26251ad)
(cwnd)
즉, sender가 보내고 여기에 대해 ack이 오는 정보를 보며 (rtt,timeout...) 보낼 데이터 양을 정함

___

최종적으로 window size는 
rwnd, cwnd의 최소값보다 작거나 같게 정한다.





 








 
