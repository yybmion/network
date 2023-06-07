### LS Example - Proclem Definition 

![image](https://github.com/yybmion/network/assets/113106136/cd349c58-6553-486a-83b9-0330362e6b93)

2. u와 직접 연결되어 있으면 cost 적고 연결 안되어있으면 무한대 / cost 중에 가장 작은 값을 다음 기준 노드로 삼는다.
3. 그래서 x를 추가하고 똑같이 진행한다. 근데 여기서 중요한 점은 예를 들어 w까지 가는 cost를 구할때
원래 u에서 w까지 가는 값은 5였다. 하지만 x를 거쳐서 가는 방법은 4이므로 최소 cost를 갱신해준다. 나머지는 똑같다.
추가로 최종 최소 cost값이 2로 서로 같게 나왔을때는 어느 노드나 선택해도 상관이 없다.
![image](https://github.com/yybmion/network/assets/113106136/29ca77d4-5aba-4649-8bf2-651c4aee2a63)
4. 계속해서 같은 방법으로 진행해준다.
![image](https://github.com/yybmion/network/assets/113106136/6d311eec-319a-4951-b1fe-c069e93473ab)
5. 모든 노드가 들어가면 종료
![image](https://github.com/yybmion/network/assets/113106136/1defb73a-4d46-4a91-90c6-da9d9b50edbc)
6. 이 최소 노드 값을 가지고 라우팅 테이블을 만드는 것이다.
![image](https://github.com/yybmion/network/assets/113106136/36eeee13-3f58-4625-a859-db9f69752866)
7. 따라서 결론적으로 라우팅 테이블을 보면 v가 목적지면 그냥 v한테 보내면 되고, w,x,y,z가 목적지면 u기준 x한테 next hop하면 된다.
8. 최소 거리니까!!
![image](https://github.com/yybmion/network/assets/113106136/bf99c94f-2ad4-4ff1-9527-ea1237a1a5e1)

___

복잡도 = 단계를 거칠 때 마다 세야하는 노드가 하나씩 줄어든다.
그러므로 n^2+n/2 이므로 결국 복잡도는 n^2이다.

![image](https://github.com/yybmion/network/assets/113106136/ebfc664a-c5c8-4bcd-ac13-244ff0aa7400)

___

LS는 노드들이 전체 네트워크의 정보를 알고있다는 전제하에 이루어짐.
그러면 ls가 동작하려면 전체 네트워크의 정보를 가져와야하는데 , 어떻게 가져올까?

**FLODDING**
![image](https://github.com/yybmion/network/assets/113106136/d0d468d2-f008-4926-aef8-66c688055000)

그림에서도 A가 가지고 있는 정보를 다른 노드들에게 전달을 해야 ls를 동작시킬 수 있다.

방법
1.A가 자기가 가지고 있는 정보를 인접 노드들에게 전달
2.B,C,D는 받은 쪽만 제외하고 다른쪽으로 또 보낸다. 반복
3.그러면 네트워크의 모든 노드들에게 정보전달 가능
4.근데 단계가 지날수록 정보 전달 개수가 많아져서 TTL IP HEADER에 3이라 지정하면 저 값이 0이되면 더이상 값을 전달하지 않게 설정한다.
![image](https://github.com/yybmion/network/assets/113106136/9a5ed4aa-c437-47b3-b78c-12a847e4419c)

___

### LS ROUTING ALGORITHM - EXAMPLE 2
![image](https://github.com/yybmion/network/assets/113106136/fbff97ed-7e9f-4e9b-bad8-1d38956d2173)

C노드에서 인접한 B,F,G는 각각 2 2 5 COST 이 중 최소 B선택
B를 N에 추가하고 N노드에 이미 C가 있으므로 삭제 후 A,E나타냄
이중 F가 최소 COST이므로 선택
![image](https://github.com/yybmion/network/assets/113106136/852e4ec4-daba-4e4e-8799-219ed76fc986)

값을 추가 할때 전의 값과 비교하여 더 COST가 작은 노드를 선택하고 아닌 노드는 삭제 
E를 보면 3이 더 작으므로 6 E노드는 삭제

반복
![image](https://github.com/yybmion/network/assets/113106136/a51b4502-c079-420d-aa2d-6d5a8253b77c)

결과
![image](https://github.com/yybmion/network/assets/113106136/c48e9a44-dd81-433e-a11d-b0d2ea41a647)

___

### DISTANCE-VECTOR ROUTING ALGORITHM

LS와 다른 점은 모든 정보를 다 알고있는 것이 아닌 서로 인접한 노드끼리만 서로의 정보를 알고있다.
distributed!!
![image](https://github.com/yybmion/network/assets/113106136/749dbea0-1cbf-4a9a-87e3-6d6fe011c3b3)

다음 그림의 뜻은 source 노드가 x이고 y까지 간다는 뜻
x는 v들 끼리 인접해 있어 정보를 알 수 있고, v들은 y까지의 정보를 알고있다. 그래서 v들이 y까지릐 정보를
알아서 자기들한테 저장하고 c(x,v)를 거기에 더한것 중 최소값이 최소 cost가 될것이다.
![image](https://github.com/yybmion/network/assets/113106136/a132ffa7-5db0-4898-9f3e-faa764972e86)
___

그림을 보면 x와 인접한 노드는 v,u,w 이고, y와는 떨어져있다. 그러므로 v,w,u를 통해 y와의 최소 거리르 구하고
x와 u,v,w의 값 , u,v,w와 y의 값을 더한 값중 최소값을 구한다.
보면 1+3으로 u를 거쳐서 가는게 최소값이다. 그러므로 x의 next hop은 u인것을 알 수 있다.

![image](https://github.com/yybmion/network/assets/113106136/b754ee3d-e0ee-4586-b10d-d3fe499776ec)

distance and vector에서
distance는 destination까지 cost이고
vector은 destination이다.
![image](https://github.com/yybmion/network/assets/113106136/e40d36df-8d62-4c89-b0a7-71d269fa1e3e)

___
STEP1.
![image](https://github.com/yybmion/network/assets/113106136/b242e3a0-4b0f-4bd3-aaec-a199cad33ff3)

STEP2.
![image](https://github.com/yybmion/network/assets/113106136/c0685a07-feb5-415b-8a0b-cd13d704d714)

STEP3.
본격적으로 알고리즘을 적용해보면 X에서 Z DEST를 보자.
원래 처음에는 X에서 Z는 7로 표시했다. 그리고 이후 인접한 Y와 Z에 대한 정보를 받았더니 Y도 Z에 대한 정보를 가지고 있는것이다!!
그래서 오 그런 길이 있었구나!! 그럼 Y를 통해 Z로 가봐야겠다. COST가 얼마니? 2+1 3이야!!
오 뭐야 7보다 작은 값이네 !! 더 빠른 길로 가야겠다!!
![image](https://github.com/yybmion/network/assets/113106136/cdac42ad-f7ec-4582-baa5-f88d960900c4)

![image](https://github.com/yybmion/network/assets/113106136/020e7a22-c122-4508-908a-e912335e452a)

![image](https://github.com/yybmion/network/assets/113106136/b67fe2d8-7136-4f8c-ac34-a50d5102701e)
 
___

![image](https://github.com/yybmion/network/assets/113106136/8d668cab-a3c1-49fe-8bbf-a60e8ef44a1c)

변경되지 않은 테이블도 있음!!
그래서 다음 스텝은 변경된 테이블만 정보를 주는 것이다. 받는거는 다 받음!!

![image](https://github.com/yybmion/network/assets/113106136/a230584c-d516-48bb-bcd2-dc5adc78d6f7)

그래서 반복하다가 변경되는 테이블이 없으면 자연스럽게 STOP

![image](https://github.com/yybmion/network/assets/113106136/c4bb1265-8299-434b-bc26-d8839059b980)
 
___

노드 하나가 더 추가되었을 떄!!

![image](https://github.com/yybmion/network/assets/113106136/7177c567-cc23-45d1-9412-e9aca46d9423)

45분 참고!

___
LS VS DV
![image](https://github.com/yybmion/network/assets/113106136/57c9c4cf-c1e2-4618-bd4a-ff62eb035488)

중요한 부분만 보면 LS는 네트워크를 GLOBAL하게 다 알아야한다. 근데 DV는 인접한 노드의 정보만 알면 된다.
그리고 TRAFFIC AMOUNT에서 LS는 FLOODING식으로 정보를 전달하므로 엄청나게 많은 TRAFFIC이 흘러야한다.
DV는 인접 라우터들만 정보가 흘러야하므로 LESS다.  
근데 ROUNTING TABLE이 나오는데 걸리는 시간은 LS가 빠르다.

___
### RIP (ROUTING INFORMATION PROTOCOL)

AUTONOMOUS SYSTEM이란?
동일한 관리체계에서 운영되는 라우터와 네트워크의 집합
 
네트워크를 효과적으로 하기 위해 이 시스템에 대해서도 IP ADDRESS처럼 AS NUMBER을 부여(IANA)
IGP = 내부 (AS 내에서 적용되는 라우팅 프로토콜)
EGP = 외부
![image](https://github.com/yybmion/network/assets/113106136/1e351513-f658-4fb8-a5f1-7cf3d8881361)

하나의 AS내에서는 하나의 IGP 라우팅 프로토콜이 사용된다.
![image](https://github.com/yybmion/network/assets/113106136/41426eb4-1e38-4627-9dd0-9b943b15cdde)

![image](https://github.com/yybmion/network/assets/113106136/47db5b4e-9adf-4715-acf5-3a96f8dc44dd)

그러면 어째서 EGP는 프로토콜이 BGP하나인가 하면 AS들간에 하는 것은 만약 선택지들 중 여러가지를 놓게 되면
여기는 A인데 저기는 B를 쓴다고 하면 서로의 라우팅을 교환하는데 일치성이 깨지기 떄문이다.

___

## RIP OVERVIEW
![image](https://github.com/yybmion/network/assets/113106136/9df65b82-f217-417a-abf9-9a4bbea9b83d)

RIP daemon(route-d)은 Application에서 동작한다.
UDP사용
![image](https://github.com/yybmion/network/assets/113106136/0a4ddf54-86bc-4a38-abdb-81c8c6bfdf6a)

___

주기적으로 매 30초 정보를 보냄
UPDATE는 짧은 거리 갱신될때
새로운 목적지가 있을떄
어디까지 가는 DISTANCE가 있었는데 그 정보를 알려준 라우터가 변경된 정보를 보내왔을떄
업데이트 됨.
![image](https://github.com/yybmion/network/assets/113106136/758069a5-ca01-447a-990e-e286d3aff6cc)
 

![image](https://github.com/yybmion/network/assets/113106136/c890b0b9-387b-426c-94b0-a462d731c392)

___

### OSPF
특징은 앞서 본 RIP와 다르게 TCP,UDP를 사용하지 않고 바로 IP에서 사용

![image](https://github.com/yybmion/network/assets/113106136/371e47e4-cec2-4476-97e9-38c2e87ba841)

![image](https://github.com/yybmion/network/assets/113106136/b8f83692-c2c8-4c02-a3b4-2a90df66b2a1)

___

![image](https://github.com/yybmion/network/assets/113106136/723c569e-84a2-4c5f-92af-f984b8894492)

TWO-WAY-STATE
는 다음 그림을 나타내
![image](https://github.com/yybmion/network/assets/113106136/cafe5499-8f1b-49e8-bc96-0c70acd53890)

그리고 FLOODING을 통해 모든 노드들이 동일한 LINK STATE DATABASE를 가짐

이거를 가지고 LINK STATE ROUTING ALGORITM이 돌아서 SPF 만들고 FORWARDING(ROUTUING) TABLE 만든다. 
여기까지 하면 과정 중 FULL STATE다.
  
OSPF는 근데 HIERARCHY 가지고 있다. 그 이유는 RIP는 인접한 노드들끼리 정보를 주고 받아서 한 곳에서 문제가 터지면
전체에게 영향을 끼친다. 그러면 라우팅의 왜곡이 생긴다. 
근데 FLODDING 하는게 모든 전체 네트워크가 아니라 특정 AREA에게만 보내므로 이는 문제가 생겼을 시에 한 AREA에만
발생하기 때문에 HIERARCHY를 구성해서 조금 더 효과적으로 운영할 수 있다.



 
