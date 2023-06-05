### CSMA와 CD가 합쳐진 CSMA/CD

> https://security-nanglam.tistory.com/193

지금 내가 보내려는 정보(신호)(MEDIUM)가 다른 또다른 누군가가 보내고 있지는 않은가 확인하는 것이 
CARRIER SENSE
확인해보니 누가 사용중이다. 근데 반대로 IDLE하다면 CAN TRANSMIT
기본적으로 그래서 정보를 TRANSMIT하면 ACK를 보낸다. 
만약, 캐리어가 감지되지 않았을때, 두 PC나 서버가 데이터를 동시에 보내면 이 경우를 Multiple Access(다중 접근) 이라 한다.

위 처럼 두 PC나 서버가 데이터를 동시에 보내려다 부딪치는 경우 충돌(Collision)이 발생했다고 한다.

만약, 충돌이 일어나게 된다면 데이터를 전송했던 두 PC나 서버들은 랜덤한 시간(타이머 구동) 동안 기다린 다음 다시 데이터를 전송한다.
![image](https://github.com/yybmion/network/assets/113106136/d44be5a2-4b96-48a6-86e0-74e0518a3b7a)
___

1. 먼저 어떤 STATION(HOST)가 보낼 정보가 있어서 SENSING을 하는데 결과가 BUSY가 나왔다.
2. 그러면 NON-PERSISTENT를 한다. 즉, 일정시간동안 아무것도 안한다. 시간이 지나서 다시 SENSING을 해서 IDLE하면 그때 정보를 보낸다.
3. 근데 어느정도 보냃수있는데 못보내는 낭비 부분이 생긴다. 
4. 근데 1-PERSISTENT는 지속적으로 SENSING하다가 BUSY끝나면 바로 보낸다. 
5. 근데 문제는 나뿐만 아니라 다른 STATION도 계속해서 SENSING을 하다가 BUSY끝나는 순간 같이 보내면 충동이 일어난다.
6. 그래서 이를 해결하기 위해 P-PERSISTENT는 보낼 수 있을때 확률 P에 대해 보낼건가 말건가의 확률을 정한다.
![image](https://github.com/yybmion/network/assets/113106136/bdfa2e95-284b-46a4-b3c9-75b2e585a513)

___
그래서 PERSISTENT-1을 사용한다면 CD(COLLISION DETECTION)이 필요하다.
만약 B신호가 D신호와 겹치는 즉시 보내는 것을 중단시키면 COLLISION PERIOD를 조금이라도 줄일수있따.
![image](https://github.com/yybmion/network/assets/113106136/0bf66d2e-7a19-46a5-a34f-ee76cc5a5945)
___
그래서 앞의 그림이 전체적인 메커니즘
여기서 중요한건 JAM은 충돌이 발생할때 더 이상 보내지 말라는 신호이다.
그리고 COLLISION이 발생하게 되면은 RANDOM TIME 동안에 기다리고 보낸다.
![image](https://github.com/yybmion/network/assets/113106136/6086b49d-76c0-4267-acf0-75b1f7e82ba9)
___

STATION이 MULITPLE ACCESS를 하는데 다른 STATION도 놀고있는것이 아니라 계속해서 모니터링을 한다.
그래서 만약 두 STAION 간에 충돌이 일어나면 다른 STATION이 먼저 그것을 알아차릴 수 있고
그 알아차린 STATION은 JAM신호를 보낸다. 그래서 이걸 받은 STATION은 즉시 정보전잘 중단.
그래서 다시 이후에 보내려고 한다면 RANDOM BACKOFF를 하는데 보낼려고 하는 STATION만 시간을 재조정하는 것이 아니고
모든 STATION 시간을 재조정하여 그 시간이후에 데이터를 보낼수있게 한다.
![image](https://github.com/yybmion/network/assets/113106136/65a7fbd1-6e9a-4832-ac31-f2550315f43c)
___

그럼 BINARY EXPONENTIAL BACKOFF는 어떤식으로 일어나나??
그니까 이게 충돌이 발생하면 특정 시간구간을 정해서 STATION을 분산시킨다.
근데 이것도 충돌이 일어나면 그 구간을 2배로 늘려서 더더욱 분산시킨다.
이걸 반복해서 10번째까지 2배 된후 10번쨰 부터는 그 구간에서 계속 시도 후 16번이 되면 아이에실패했다고
메시지가 온다.
![image](https://github.com/yybmion/network/assets/113106136/bd127a2a-8769-43c3-ab15-70f6d5b80eea)
___
### ARP 
![image](https://github.com/yybmion/network/assets/113106136/450d320b-3d41-476f-9650-04004e4b18cf)

IP ADDRESS VS MAC
IP ADDRESS는 레이어 3에서 동작 / 4바이트 / NETWORK ADDRESS와 HOST ADDRESS로 나뉨.
MAC은 레이어 2에서 동작 / 6바이트 / OUI와 VENDER ASSIGNED 파트로 나뉨.
서로 중복되면 안되니 그 ADDRESS를 제공해주는 곳이 IP는 IANA MAC은 IEEE.
NIC에게 부여되고 / NIC ROM에게 부여되고 
![image](https://github.com/yybmion/network/assets/113106136/8527c49e-9464-4b12-913c-ba8010032455)
![image](https://github.com/yybmion/network/assets/113106136/166c9881-464e-49ad-95c5-2c567cded364)
___
> https://jhnyang.tistory.com/403
보자, IP ADDRESS는 레이터 3에서 IP HEADER에 이 IP ADDRESS가 저장되어 있을텐데 DATA LINK에서는 ENCAPSULATION되어
정보를 건드릴 수도없고 보지도 않는다. 그러면 어떻게 다음 라우터에게 전달할수있을까?
MAC ADDRESS다!
일반적으로는 IP ADDRESS를 알고있는데 상대방의 MAC ADDRESS는 모르고 있는데 이걸 어떻게 알아올 것인가가
ADDRESS RESOLUTION PROBLEM.
![image](https://github.com/yybmion/network/assets/113106136/121adaa5-8b1c-433e-a51a-7a6fbc3af564)
___
ARP는 IP ADDRESS를 통해 MAC ADDRESS를 찾는것이고
RARP는 반대.
![image](https://github.com/yybmion/network/assets/113106136/ecc8baef-cc8b-43a4-956c-d903b1ff4107)
![image](https://github.com/yybmion/network/assets/113106136/e52a4b3d-01c7-4f75-9650-92decc5e67b8)
___
ARP는 DATA부분에 ENCAPSULATION되어서 들어간다.
DATALINK에서 ENCAPSULATION된것이 어떤 PROTOCOL에 의한것이냐는 TPYE FIELD에서 알수있다.
![image](https://github.com/yybmion/network/assets/113106136/905b517e-6fde-47f4-ba67-07d8babbec03)
![image](https://github.com/yybmion/network/assets/113106136/f69095e5-4138-4391-88d6-20529ce965c4)

___
호스트 A 호스트 C에게 FRAME을 하나 전송하고싶은데 A는 C의 IP ADDRESS만 알고있고 C의 MAC ADDRESS는 모르는 상태이다.
따라서 ARP가 동작해야한다. 
![image](https://github.com/yybmion/network/assets/113106136/dc13a7f7-7fe7-4911-aeb4-d40cace1fe71)

DEST는 모두 F로 두고 SRC는 자기 자신 MAC ADDRESS고 TYPE은 ARP TYPE이고, ARP 메시지에는 
TARGET HA는 모르니 0으로 두고 TARGET IP와 SENDER IP를 보낸다. 그러고 BROADCAST로 보낸후
TARGET IP와 맞는 부분이 나오면 MAC ADDRESS를 SENDER IP와 일치하는 HOST에게 보내준다.(이때는 UNICAST로)
![image](https://github.com/yybmion/network/assets/113106136/32f56a5d-3aed-4c56-b2a0-b568165a942d)
![image](https://github.com/yybmion/network/assets/113106136/3e88425c-b955-4429-8de6-5cdc9aa97d00) 

![image](https://github.com/yybmion/network/assets/113106136/6fcf29db-9a22-4395-94b7-fa7013040d57)

___

근데 이게 BRAODCAST라 다른 서브넷은 전달이 안됨. 라우터에서 BLOCK됨. 
![image](https://github.com/yybmion/network/assets/113106136/c50b0643-56ae-408b-be54-80839baede30)
ARP를 하기 위해 IP ADDRESS를 BROADCAST 근데 아무것도 맞는것이 없고 라우터에 BLOCK됨.
라우터는 블락되었다고 하면서 동시에 자신(라우터)의 MAC ADDRESS를 HOST에게 보내줌. 상대방의 IP ADDRESS는 알고있다고 가정.

- DEST IP ADDRESS는 목적지 갈때까지 변하지 않음. 근데 DEAT MAC ADDRESS는 변함 
![image](https://github.com/yybmion/network/assets/113106136/454f120d-517f-4f3e-81dd-3a8dd4162660)

___
### ETHERNET

LAN은 학교라든지 작은 단위 내에서 연결된 네트워크
WAN은 이런 LAN들을 넓은 지역에 거쳐서 연결을 해주는 네트워크
![image](https://github.com/yybmion/network/assets/113106136/6612eeed-32c5-4eb0-bf77-84df3dc80327)

VLAN
> https://aws-hyoh.tistory.com/75
 










  
