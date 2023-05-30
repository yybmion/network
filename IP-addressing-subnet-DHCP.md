TCP에서는 APLLICATION에서 내려온 데이터가 DATA LINK LAYER에서 동작하는 NETWORK INTERFACE CARD
가 보낼 수 있는 최대 사이즈 MTU사이즈보다 커지게 되면 보낼 수 없으니 FRAGMENTATION해야한다.

SEGMENTATION VS FRAGMENTATION
> https://hongcana.tistory.com/63

HEADER
> https://code-lab1.tistory.com/32

![image](https://github.com/yybmion/network/assets/113106136/a24b84c1-95a0-478d-961d-b8b1291f8b43)

16-BIT IDENTIFIER 나중에 REASSEMBLY할때 ID를 보고 해야함 (같은 ID만 ASSENMBLY함)
FRAG : 0이면 FRAGMENTATION 해도 됨! 1은 하지 마라!
MORE : 0이면 마지막 FRAMENT다! 1이면 첫번쨰나 중간에 끼여있는 FRAGMENT다!
13-BIT FRAGMENTATION OFFSET : 뒤에서 REASSEMBLY할때 순서를 맞추기 위해

___
![image](https://github.com/yybmion/network/assets/113106136/e1543b80-375b-4c82-a22b-38be0bec6ec5)

HEADER는 아무런 옵션이 없으면 20 BYTES 기본
OFFSET 8로 나눈 이유는 8BYTES로 나타내기 때문

각각이 라우팅 포워드를 하며 최종 DESTINATION에서(ID,OFFSET보고) REASSEMBLY한다.
___
Q.
![image](https://github.com/yybmion/network/assets/113106136/db6babc6-5108-4a91-9aaf-8e08904b9c1a)
각각의 FRAGMENT들의 FIELD가 어떻게 바뀔까? 생각해보자

___

### IPv4 Addressing

![image](https://github.com/yybmion/network/assets/113106136/9a1dfcc5-51b5-40b9-83bb-ff0cf65fb707)

IPV4 == "DOTTED DECIMAL NOTATION"

___

![image](https://github.com/yybmion/network/assets/113106136/71c9773d-f778-49a1-9d6d-1750f783dcfa)

네트워크 인터페이스 카드 (NIC)는 네트워크 끼리 연결 할 수 있도록 컴퓨터에 설치된다.
그래서 하나의 NIC에는 하나의 IP ADDRESS가 부여된다.
따라서 하나의 라우터에는 IN 과 OUT이 있으므로 여러가지 NIC 가 존재하고 여러  IP ADDRESS 가 있는 것을 알 수 있다.
___

![image](https://github.com/yybmion/network/assets/113106136/df21c66a-0e39-45f3-8b2d-f8c957f5f928)
 
IP ADDRESS는 NETWORK PART와 HOST PART 두 부분으로 나누어져 있다.

NETWORK PART(INTERNET에서 겹치지 않기)는 우리 전화번호의  +82-031-219 이런 지역을 나타내는 고유 번호라고 말할 수 있다.
HOST PART(NETWORK에서 겹치지 않기)는 그 외의 부분이다. 

왜 이렇게 나누었을까? 라우터를 효과적으로 작동시키기 위해
![image](https://github.com/yybmion/network/assets/113106136/820f5bc9-686d-4cd4-90dd-0894ffd9e16a)

이건 질문이다. **분명 NETWORK ADDRESS만 사용하는것이 ROUTING TABLE의 사이즈를 줄여주어서 IP ADDRESS대신 사용한다고 했다.
하지만 IP ADDRESS는 고유한데 왜 개수가 많아지는지 모르겠다.**

일단 여기서는 NETWORK ADDRESS 부분만 나타내어 그 부분에 대한 모든 SUBNET(공통된 NETWORK PART를 가지고 있음) 컴퓨터들이 서로 전기적신호를 알아서 보내주어서
맞는 부분에게 보내주어 효율을 높혀준다고 하였다.

___
![image](https://github.com/yybmion/network/assets/113106136/d4a959aa-e959-4bc0-be5a-ff6d24227171)

이 부분이 무슨 뜻이냐면 예를 들어 CLASS C를 보자
CLASS C는 NETWORK부분이 3 부분 HOST 부분이 1부분이다.
이는 NETWORK부분이 길어 NETWORK를 많이 만들 수 있지만 하나의 네트워크가 수용할 수 있는 HOST개수는 작아지는 것이다.

![image](https://github.com/yybmion/network/assets/113106136/f98bacc7-ef7e-47ac-ba25-3a0e564f6481)

___
클래스에 따라 사용하는 방식이 다르다.
유니캐스트(Unicast), 브로드캐스트(Broadcast), 멀티캐스트(Multicast)란?
> https://sbs1621.tistory.com/40

___

IP ADDRESS를 부여하는 방식?
IANA 가 한 집에 고유한,남은 NETWORK ADDRESS를 부여하면 그 집에서는 뒤에 HOST부분이 중복되지 않게 관리한다.
그럼 UNIQUE 해짐

PULBIC IP, PRIVATE IP 차이가 뭐야!!!!
> https://hstory0208.tistory.com/40

![image](https://github.com/yybmion/network/assets/113106136/19faf97b-4595-4b4a-8f03-d3fcfe0c04d0)

> 위 철수와 영희의 경우를 살펴보자.
철수의 노트북이 사용하는 IP는 192.168.0.1
영희의 노트북이 사용하는 IP도 192.168.0.1 로 같다.
하지만 전혀 다르다.
왜냐하면, 각기 다른 공인 IP 철수 집 (178.123.32.11) ,영희 집 (178.123.41.9)로 구성된 사설망(네트워크) 안에서 내부적으로 사용되는 고유한 IP이기 때문에 다른 내부 네트워크와 IP가 겹쳐도 상관이없다.
이처럼 사설망이라는 개념을 통해 같은 아이피 번호를 중복해서 사용 가능 할 수 있어, IPv4의 주소 절약이 가능하다.

___

그렇다면 PRIVATE IP ADDRESS는 클래스가 있는데 살표보자.
![image](https://github.com/yybmion/network/assets/113106136/f8443ed9-3502-4664-b94c-5abee571ca04)

___

원래는 PRIVATE IP ADDRESS는 외부(PUBLIC)에 접근할 수 없지만
그것을 가능하게 해주는 것이 NAT(NETWORK ADDRESS TRANSLATION)
___
![image](https://github.com/yybmion/network/assets/113106136/783454be-9573-412c-9b56-d06eaccf09a3)
-2 하는 이유

___
### SUBNET AND SUBNETTING
 
1. 하나의 라우터에 인터페이스를 통해서는 하나의 서브넷이 구성이 된다.
2. 각각의 서브넷에 있는 인터페이스들 동일한 NETWORK ADDRESS를 갖는다.
3. 하나의 서브넷 내에서 하나의 인터페이스가 시그널을 만들어서 보내면 이 시그널은 이 서브넷 내에 있는 모든 인터페이스들에게 다 전달이 된다.

라우터 인터페이스를 통해서 각각의 서브넷이 만들어진다.

![image](https://github.com/yybmion/network/assets/113106136/6e5a7563-bf0a-4520-9a7f-2a60ae5af6ee)

즉, 6개 SUBNET이다.

___

SUBNET MASK??
![image](https://github.com/yybmion/network/assets/113106136/2c7a6bdb-3f3b-4d34-9c2a-2a41c378f599)

IP ADDRESS는 NETWORK ADDRESS와 HOST ADDRESS 부분으로 나뉜다. SUBNET에서의 공통된 ADDRESS는
32 BIT 중에 몇 비트에 해당하는가에 대해 나타낸것.

___

SUBNET ADDRESS는 HOST ADDRESS부분은 0 으로 나타내고 NETWORK ADDRESS 부분을 표현하고 /(슬래쉬)로 표현한다.
 
![image](https://github.com/yybmion/network/assets/113106136/841d183c-6137-4235-a5c9-f000f34f0189)

1. /24이므로 SUBNET MASK는 마지막 8BIT뺴고 다 1로 나타내고 두개를 AND 해줌
2. 그걸 나타내고 나서 마지막 슬래쉬도 잊지 말자.

___

SUBNETTING???

아주대학교를 예를 들면 학생수가 1만명은 넘으므로 클래스 C로는 NETWORK 개수가 부족할 수 있어 B를 선택하는데
하나가 연결하려고 하면 연쇄적으로 SUBNET에 있는 컴퓨터 모두가 시그널을 보낸다(동일한 NETWORK ADDRESS).그러면 부하가 심해질 수 있다.
그래서 65000개는 좀 많은데... 조금 줄이면 안될까???
해서 나온것이 SUBNETTING.
256개는 적고 65000개는 너무 많다!!!

![image](https://github.com/yybmion/network/assets/113106136/0478942d-1630-4834-9fcf-1cea6d01ce32)

서브넷에 대한 부분을 HOST부분에서 더 할당한다.

여러부분으로 나누어 여기면 저기로 가고 여기면 저기로 가고 등등...

![image](https://github.com/yybmion/network/assets/113106136/23acdada-3a37-448e-b1c0-2c0cdce78b36)

___

![image](https://github.com/yybmion/network/assets/113106136/26641279-f1f0-4ab5-be9f-41f5c373478d)

 원래 있는 부분에 b BIT만큼 확장시키면 내부적으로 구분할 수 있는 SUBNET의 개수는 
 2^b 개수다. 각각의 subnet마다 가질 수 있는 ip 개수는 2^r-2개.
 
 그래서 결론적으로 subnetting은 나한테 부여된 network address가 있는데.
 
그냥 NETWORK ADDRESS 를 가지고 HOST에 하나의 SUBNET으로 연결시키려고 하니까 비효율적이다.
내부적으로 또다른 SUBNET을 만들어서 하고자 하는게 SUBNETTING.
___

Q.
![image](https://github.com/yybmion/network/assets/113106136/adbf3110-62bd-4b00-a06c-3af3217fff82)

![image](https://github.com/yybmion/network/assets/113106136/6fb9dcdd-bd9f-4826-b114-2613287d8435)

![image](https://github.com/yybmion/network/assets/113106136/ac7c823a-1749-4288-a5c5-3e2738327f82)

왜 최대 빌려올 수 있는 SUBNETTING이 6임?

___

### CIDR

![image](https://github.com/yybmion/network/assets/113106136/e3bf9b83-e648-45fb-8cee-dd2a9814a2f4)

클래스를 구분하지 않고 NETWORK PORTION이 임의로 32BIT중에 가질 수 있게 만듬
그래서 CIDR은 임의로 NETWORK PORTION을 가질 수 있어서 반드시 SUBNET MASK를 표현해 줘야한다.
표시안하면 모름 절대.
 
___

![image](https://github.com/yybmion/network/assets/113106136/f4fc1942-5d80-4ee5-9fe6-7552f00673f9)

연속되어 있는 NETWORK ADDRESS를 하나의 형태 묶는것이 "ADDRESS AGGREGATION"

![image](https://github.com/yybmion/network/assets/113106136/1bfabbf9-73ec-44c6-9652-336cbf538516)

보면 원래 모든 네트워크들이 나한테 패킷을 보내주세요 하고 정보를 보낼 것이다.
그럼 라우터는 이걸 받아가지고 라우터 테이블을 만들것이다. (너무 사이즈가 커짐)

근데 CIDR한다?? 공동된 부분 따서 NETWORK ADDRESS로 나타내어 보냄 그래서 테이블 묶은거 하나만 가지고 있음.
이건 클래스를 구분하지 않기 때문에 가능한 것이다.
![image](https://github.com/yybmion/network/assets/113106136/6479f811-e5aa-4e90-af3f-12e5c5d5f44d)

___

![image](https://github.com/yybmion/network/assets/113106136/9fc034fe-0b91-4a76-9038-e25eb5386d37)

___

### DHCP

Host가 계속해서 네트워크를 잡아먹고 있으면 다른 host가 사용하지 못하는 경우가 발생한다.
host는 잠도 자므로 네트워크를 사용하지 않는 경우도 있다. 그래서 (공유)dynamic 하게 설정하면 좋을것같다.

![image](https://github.com/yybmion/network/assets/113106136/654c1791-1687-49cc-9186-28b34d047afc)










