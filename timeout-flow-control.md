Transmission time 
Round trip time

Network 상황에 따라 RTT는 달라질수 있다. (가변)

___

 TCP 타임아웃 값을 어떻게 설정해야 할까?
 너무 짧게 잡으면 불필요한 재전송을 할 수 있고,
 너무 길게 잡으면 파일이 왔지만 무조건 타임아웃 시간까지 기다리기때문에
 낭비가 일어날 수 있다.
 
 ___
 
 EWMA 을 적용!
  
 Sample RTT 는 실질적으로 측정된 값
 Estimated RTT는 EWMA가 적용된 값 
 
 ___
![image](https://github.com/yybmion/network/assets/113106136/60fd3ffb-12c3-447b-9738-d9f4dfdbead6)
  
![image](https://github.com/yybmion/network/assets/113106136/12fc1073-65bf-4f96-a5a2-1223bd635897)

 ![image](https://github.com/yybmion/network/assets/113106136/b885fed4-bb75-4c87-b094-a7f3f1f16493)

___

즉, 알파가 작아지면 EWMA 에 가까운 값이 되고 MORE SMOOTHED
알파가 커질수록 SAMPLE RTT에 가까워진다.
따라서 가장 추천되는 값은 0.125이다.
___

어? 근데 이 ESTIMATED값을 쓰고, 현재(SIMPLE)와 차이가 많이 난다면 좀 불안할것이다.
그래서 안전장치를 추가한다.

DevRTT

Safety margin
sample RTT - estimated RTT 이 값이 크면 변동폭이 크다는 것이다.

![image](https://github.com/yybmion/network/assets/113106136/f11ae556-c5e9-48ed-ad10-361e9be46920)

___

### TCP RELIABLE DATA TRANSFER

- TCP rdt services
![image](https://github.com/yybmion/network/assets/113106136/f8122d65-3158-4f37-966f-ebd17e86a10c)

![image](https://github.com/yybmion/network/assets/113106136/616113ce-f3f0-42c9-bfa9-71e4606be35e)

1. data가 너무 크면 segmentation해서 sequence number 생성
2. window로 segment 보냄
3. 타이머 구동 ( 가장 오래된 unACKed segment에)

타임아웃된 segment만 다시 보냄!
터이머를 다시 구동시킬 때는 binary exponential backoff algorithm을 이용하여
이 공식으로 segment의 타임아웃 값을 정한다.
___

Retransmission Scenarios #1
(그냥 ACK이 안옴)
![image](https://github.com/yybmion/network/assets/113106136/6d8a7719-dc5b-471b-87a7-f37e7e654fd5)

Retransmission Scenarious #2
tcp는 타임아웃 된것만 다시 보냄 (GBN과 다른점)
duplicate segment 즉, 전에 받았던 것이므로 버림

![image](https://github.com/yybmion/network/assets/113106136/92fdfe99-cae2-400c-9b15-5b4c5d7f126b)
 
Retransmission Scenarious #3
아니 timeout 전에 cumulative ack이 하나라도 오면 그 전에 segment ACK이 오지 않더라도
다시 보낼 필요가 없으며 그다음 SEGMENT를 보내면 된다!!

![image](https://github.com/yybmion/network/assets/113106136/a7483b57-acb1-4d51-9442-5cf9bc8feba4)

___

TCP에서 SENDER을 보았다 그런데 RECEIVER은 시나리오가 4가지가 있다.

Delayed ACK 하는 이유?
- ACK 패킷 전송 횟수를 줄이기 위해(오버헤드) Delayed ACK 라는 방식을 사용한다.
1.
![image](https://github.com/yybmion/network/assets/113106136/9d820c0f-55b3-4320-990e-f6d49e73a66a)

2. 
![image](https://github.com/yybmion/network/assets/113106136/8633dff5-9f29-4056-9aa5-718146ca1681)

3. 
![image](https://github.com/yybmion/network/assets/113106136/960ffd0f-0442-43ac-92d8-484e5aee298a)

Duplicate ACK : 같은 ack을 받음
TCP에는 buffer가 존재해 110 Seq을 저장할 수 있다.(GBN과 다른점) (gap 발생)
하지만 ack은 100을 보낸다.

4.
![image](https://github.com/yybmion/network/assets/113106136/86fc7a68-57e0-40b4-95ee-c31f3f0c7960)

___

### FAST RETRNSMIT

타임아웃은 상대적으로 종종 길어서 긴 딜레이가 생기기 마련이다.
그리고 DUPLICATE ACK도 그만큼 많이 생기고
이는 모두 네트워크 상황에 따라 달라질 수 있다.
어느 상황에서는 타임아웃이 걸릴때 까지 기다리지 않고 FAST RETRANSMIT를 할 필요가 있다.

___
![image](https://github.com/yybmion/network/assets/113106136/92921aac-5075-48b6-9569-855033c65679)

SEQ 100을 못받아서 ACK=100을 계속 보낸다. 근데 DUPLICATE ACK을 100번만 받는 것을 보면
그 이후의 SEQ는 잘 받았다는 것을 뜻한다.

즉 굳이 타임아웃을 기다릴 필요가 없다. 
3번의 DUPLICATED ACK을 받으면 그 즉시 받지 못한 것에 대한 SEQ을 보낸다.

___

### FLOW CONTROL

![image](https://github.com/yybmion/network/assets/113106136/b75a1802-df0f-4a79-bf2d-a86162da994e)

SENDER은 RECEIVER의 상황을 알지 못한다. 
SENDER은 WINDOW 범위 내에 SEGMENT를 계속해서 보낸다. 하지만 RECIEVE BUFFER는 모두 차고 만다.
이 상황을 알리 없는 SENDER는 계속 보내고 결국 OVERFLOW가 발생한다.

이 상황을 정리해주는 것이 Flow Control

![image](https://github.com/yybmion/network/assets/113106136/bf2acb27-9daf-489e-8a98-592c650bd573)

___

![image](https://github.com/yybmion/network/assets/113106136/fdfb4699-3fe2-4392-bb49-9d0a6fa066e5)

buffered data는 차있는 데이터

rwnd가 아직 받을 수 있는 데이터 공간
그래서 ack을 보낼때 rwnd를 같이 보내준다.
나 공간 이만큼 남아있어~~~

___

![image](https://github.com/yybmion/network/assets/113106136/61f2f305-afb3-45a0-855e-dad3d58f96ba)

그래서 지금까지는 window size가 고정된 줄 알았지만
rwnd보다 작거나 같게 만들어야한다는 것을 알 수 있다.

![image](https://github.com/yybmion/network/assets/113106136/89035203-966b-4453-858c-ea4220cca5e0)




 
