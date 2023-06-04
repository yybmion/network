### SDN DATA PLANE - GENERALIZED FORWARDING
1. DATA PLANE
2. CONTROL PLANE
3. MANAGEMENT PLANE

CONTROLLER 를 중심으로 아래 쪽 SOUTHBOUND API/OPEN FLOW(둘의 상호작용을 지원)를 통해 밑에 SDN SWITCH와 정보를 교환 
![image](https://github.com/yybmion/network/assets/113106136/fded9311-ad49-49bc-9c58-9088e18746d5)

___
TCP로 CONNECTION 맺혀있음.
![image](https://github.com/yybmion/network/assets/113106136/70767955-3e4f-42ab-8e81-add379cbb647)

___

OPENFLOW  CONTROLLER와 상호작용 하면서 원래는 ROUTING TABLE이 만들어졌다면
지금은 FLOW TABLE이 만들어짐.
FLOW TABLE은 RULE/ACTION/STATS로 구성 
RULE은 LINK LAYER/MPLS LAYER/NETWORK LAYER/TRANSPORT LAYER에 대한 정보를 나타냄.
이런 RULE에 매칭되는 부분을 찾아서 어떻게 처리할건가가 ACTION부분.
ACTION에 대한 통계를 나타냄. 이를 CONTROLLER에게 제공을 하면서 
계속해서 업데이트를 할수있게 함.
![image](https://github.com/yybmion/network/assets/113106136/a8a12905-50a8-4d7f-bf4d-e5af74991ee0)

___

일단 패킷이 도착하면 패킷 헤더에 정보를 보고 MATCH되는 것이 있나 확인한다. 
MATCH되는 것이 있으면 그다음 ACTION에 맞춰서 처리함.

쉽게 말하자면 RULE에서 적혀있는 DATALINK 부터 APPLICATION LAYER에 매칭 되는 것들을 가지고 
ROUTER역할도 시키고 SWITCH 역할도 시키고 FIREWALL, NAT 역할 등등 다 시킬 수 있다. 그래서 GENERALIZED
![image](https://github.com/yybmion/network/assets/113106136/48dacf42-e4f7-4270-9542-7f29fdd075d7)

ROUTER로 동작시키려면??
![image](https://github.com/yybmion/network/assets/113106136/f0200321-8ed8-4f6b-b0e0-bf1eb4bd0168)

FIREWALL 동작시키려면??
![image](https://github.com/yybmion/network/assets/113106136/1c10edab-bbb4-4e20-997a-72721448eb03)

![image](https://github.com/yybmion/network/assets/113106136/585c0134-c478-43e4-8962-1cbe839bfa23)

그래서 예전에는 무슨 기능을 하려면 하나하나 다 필요했는데 이제는 그럴필요가 없다. 

___

FLOW를 보자면 , MATCH 되는것이 있으면 그냥 ACTION 하면 되는것이고
없으면 작기가 직접 처리를 못하고 CONTROLLER에게 알려주어 어떻게 처리할지 알려줘! 한다. 
그래서 CONTROLLER가 RULE을 만들어주면은 거기에 맞추어 FORWARDING이 이루어진다. 
![image](https://github.com/yybmion/network/assets/113106136/3cb30a3c-fd99-4f4e-a460-5031606b45c6)

___

1. 패킷을 SWITCH한테 보냈는데 FLOW TABLE을 보니 MATCH되는게 없어! 처리를 못해서 CONTROLLER에게 보
2. CONTROLLER는 FORWARDING RULE정하고 관련된 SWITCH들한테 FLOW를 전달함 . 거기에 맞춰서 패킷이 FORWARDINGㅗ딤
![image](https://github.com/yybmion/network/assets/113106136/f151a9dc-b153-4603-8a61-4607a8752e0f)

___
![image](https://github.com/yybmion/network/assets/113106136/cb8fe160-9bfa-42ee-9e62-0520b69a9b4a)

FLOW1,2
CONTROLLER가 FLOW TABLE 생성.
패킷들이 라우터에 올때 TABLE(ACTION)을 보고 전달함.

그냥 뭐든것은 CONTROLLER가 다 알아서 얘가 그냥 같은 목적지라도 다른 PATH를 제공할 수 있게 해줌(지금까지는 DESTINATION이 같으면 무조건 SHORTEST PATH로 제공해줬음)
FLOW 마다 CONTROLL가능 . CONTROLLER가 SWITCH들한테 FLOW TABLE 알아서 만들어준다. 효율적으로 

___

### SDN CONTROLLER 알아보자
그러면 이거 CONTROLLER가 어떻게 만드냐????TABLE

CONTROLLER에서 3가지 부분으로 나누어져있다. 
1.SOUTHBOUND API (SWITCH와 상호작용 위해)
2. NETWORK-WIDE STATE MANAGEMENT LAYER
3. NORTHBOUND API (MANAGEMENT PLANE과 상호작용)
![image](https://github.com/yybmion/network/assets/113106136/2f6f074f-7193-4b1f-b83b-43c99ce49d5b)

___
1. 장애가 어느 한곳에 일어났다. 
2. OPENFLOW에 알려준다. CONTROLLER한테 
3. 만약 LINK 부분이 문제다 하면 LINK-STATE INFO에 정보를 전달 
4. 수정 후 동작할 APP이 필요한데 
5. NORTHBOUND API를 통해 MANAGEMENT로 보내 ROUTING을 새롭게 하기 위해 한다. 
6. 다익스트라는 모든 네트워크의 정보를 알고있어야한다. 
7. 근데 ROUTING ALGORITHM은 가지고 있지 않으니까 
8. CONTROLLER를 통해 NORTHBOUND API를 통해 LINK-STATE INFO를 통해 ACCESS를 해 
9. 정보를 가져온다. 그 정보를 가지고 계산한 결과는 FLOW TABLE에 변경이 이루어진다. 
10. CONTROLLER는 이 변경된 FLOW TABLE을 OPENFLOW를 통해 각각의 ROUTER에게 정보를 전달.
11. 이후 새로 길 찾아서 잘감 
![image](https://github.com/yybmion/network/assets/113106136/4e646fa8-1bb1-404c-85ea-94b51d15c798)

___

CONTROLLER가 하나의 PATH만 만들지 않는다. 
장애가 생길것을 대비해 BACKUP PATH도 만든다. 
![image](https://github.com/yybmion/network/assets/113106136/01cd2aaf-5bf8-4f2c-86a3-9e901b900a71)
___
### DATA LINK LAYER
데이터 링크에서의 기본 유닛인 FRAME 전달을 책임져 주는 것이 DATALINK LAYER
![image](https://github.com/yybmion/network/assets/113106136/b949ab3d-6358-4354-b045-65ed6f9474dc)

![image](https://github.com/yybmion/network/assets/113106136/bdbc83e1-23dc-4625-ac93-f42147d44629)

___

DATALINK SERVICE는 무엇이 있을까
제일 중요한 것은 MEDIA ACCESS CONTROL (MAC) 
하나의 라우터는 하나의 SUBNET을 형성하는데 여기 안에 있는 라우터는 신호를 보내면 모든 라우터 내에게 모두 보낸다. 
근데 다른 라우터도 똑같이 신호를 보내면 신호가 충돌이 날 수 있어 데이터가 섞일 수 있다.
이를 막기 위해 MAC이 존재.

ERROR는 한곳에서 ERROR가 나고 DEST에 나타나는 것이다. 
그래서 어떤 구간에서 ERROR가 발생한게 누적이 되어 DEST에 나타난다. 그래서 재전송하게 시키면은 OVERHEAD가 커져
근데 DATALINK에게 시키면은 조금 더 효과적으로 ERROR에 대처 가능.
![image](https://github.com/yybmion/network/assets/113106136/e63540cb-b2fb-4439-a660-eabb9ba58023)

___

DATALINK LAYER은 소프트웨어 부분과 하드웨어 부분을 연결시키는 부분이다. 
![image](https://github.com/yybmion/network/assets/113106136/46e1e8ac-36cd-4bc1-b42f-10e460c35ec6)
___
### ERROR DETECTION

데이터가 기본적으로 주어지면 그대로 보내는 것이 아닌
받은쪽에서 ERROR DETECTION하기 위해서 FED를 통해 ED 만듬 거이에 D를 붙인게 FRAME이다. 

이 FRAME을 받으면 아까는 DATA만 가지고 했는데 FRAME을 가지고 동일하게 FED함 
그래서 ERROR가 있으면 무시함.
그리고 에러 없으면 ED버리고 D(DATA)만 올림.
![image](https://github.com/yybmion/network/assets/113106136/9321fee6-fb47-4301-be00-e2aa5816a829)
___

ERROR CORRECTION??
ERROR를 수정할수있어야함.
먼저 데이터가 있으면 이걸 CORRECTION 하기 위한 ENCODER를 통하게 되면 이건 아까와 다르게 데이터 부분을 아이에 변형시켜버린다.
CODEWORD!
이걸 전달시키고 또 DECODER거침.
그리고 ERROR 판단.
![image](https://github.com/yybmion/network/assets/113106136/ba8879c9-8e97-40c8-acec-b49a3fab52f2)

___
![image](https://github.com/yybmion/network/assets/113106136/d62603a0-05d8-4d8b-9783-135b675a367e)

에러가 거의 발생하지 않는 곳에서 사용!
___
![image](https://github.com/yybmion/network/assets/113106136/e1652019-6f44-490a-90c6-3eec92febc9c)

___
![image](https://github.com/yybmion/network/assets/113106136/fadaf476-43bf-4c0e-a219-550817a0ca3e)

2차원 배열로 ERROR CORRECTION하면 교차하는 부분이 정확히 ERROR가 발생했다는 것을 알 수 있다.!
그부분만 고치면 됨.
___
실제로 DATALINK에서 ERROR 체크하는 방법은 CLC다.
그냥 바로 뒤에 CRC bits가 붙는것을 볼 수 있다.
우리 진짜 초반에 봤었다. 각 계층에서 뒤에 error확인 위해 뒤에 조그맣게 붙는 부분.
이 전체가 frame.
그럼 어떻게 crc encoder가지고 crc bits를 만들까.
![image](https://github.com/yybmion/network/assets/113106136/b8d49ac8-9627-4529-bc00-c2626eb01bee)

___
방법은 MODULO-2를 이용한다!
XOR은 서로 다른 입력이면 1 같으면 0

+/-RK 연산이 똑같다!! 그냥 XOR
![image](https://github.com/yybmion/network/assets/113106136/8309724d-f6e2-4f48-8558-7db480dd1e5c)

___
CRC에 들어갈 비트 어떻게 구하지??
![image](https://github.com/yybmion/network/assets/113106136/6b8f5990-b392-40c7-b751-30627793223a)

![image](https://github.com/yybmion/network/assets/113106136/e0856453-aca6-451f-9e67-3a0c5d151131)

![image](https://github.com/yybmion/network/assets/113106136/1e557c68-eed6-41f4-8406-782f0e21acc8)

___
그럼 이걸 어떻게 활용하나?

결과값이 0면 에러가 없는것이다. 
![image](https://github.com/yybmion/network/assets/113106136/415eb4af-1e83-43d1-9b2f-7c2a01ed98d1)
어딘지 모르겠지만 0이 안나오면 ERROR가 있다는 것
![image](https://github.com/yybmion/network/assets/113106136/c3b51066-ff1c-4cea-b914-3dca099adfda)
___
GENERATOR을 명시적으로 표현하기 위해 다항식을 사용한다.
0부분이 어딘지 1부분이 어딘지 한눈에 들어온다.
![image](https://github.com/yybmion/network/assets/113106136/00dd5c55-a22c-46f7-baab-773ea1273ad5)
___
### MULTIPLE ACCESS PROTOCOL











 

