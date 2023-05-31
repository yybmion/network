### NAT

인터넷은 DE FACTO STANDARD를 따르면서 여러가지 새로운 것들을 많이 만들면서 실제 네트워크에 적용시키려고 할때
이런 Public망에다가 바로 적용시켜버리면은 Public망이 영향을 받을 수 있다.
그래서 이런 Pirvate domain을 두어서 Private domain에서만 테스트해보고 안정화 되었을 경우에 Public domain에 나올 수 있도록 함

![image](https://github.com/yybmion/network/assets/113106136/edeb2d8f-8598-4e28-9542-96c9ee7a5e45)

___
NAT
> https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-NAT-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80

하나의 Public IP address가 여러 다른 Private IP Address를 매핑함
![image](https://github.com/yybmion/network/assets/113106136/f4c72dad-45e6-427b-868e-6a48aabde648)

![image](https://github.com/yybmion/network/assets/113106136/21d38b86-b105-4377-b906-d356a61b77a2)

___

LAN 이 PRIVATE 부분 / WAN이 PUBLIC 부분
쉽게 말해 NAT은 PRIVATE 부분과 PUBLIC 부분을 테이블을 이용해 매핑을 통해 하나의 PUBLIC IP ADDRESS를 이용해
수많은 PRIVATE 단말들을 PUBLIC 가 같이 SERVICE(패킷 교환) 가능!

근데 문제가 있다. PRIVATE이 CLIENT고 PUBLIC이 SERVER면 서버는 항상 켜져있고 클라이언트는 원할 때 접속이 가능해 
문제가 없지만 만약 PUBLIC에 CLIENT가 있고, PRIVATE에 SERVER가 있다. 그러면 PUBLIC 쪽에 있는 CLIENT가 PRIVATE의 
서버에 접근할 방법 이 없다. 이걸 지원하는 것이 **STUN**.
![image](https://github.com/yybmion/network/assets/113106136/1b076d95-5d04-4001-bb7a-3e5bb47816fd)

___
### IPv6
IPV4의 유일한 단점인 ADDRESS 길이를 해소해줄 수 있는것이 IPv6.
어떠한 옵션을 처리하려고 하면은 하드웨어적으로 처리하는 것이 아닌 소프트웨어적으로 처리를 하기 때문에
시스템이 느려질 수 있다. 그래서 그 값을 고정시키고 스피드를 높혀보자!
QoS를 확실하게 해보자!
이 모든것이 IPv6에서의 변화 동기이다.
![image](https://github.com/yybmion/network/assets/113106136/90738d3c-dfa7-4003-aeb6-9d126c579db7)

___
차이점

헤더가 고정되어있다.
NETWORK ADDRESS가 HIERARCHY로 표현되어있다.
ANYCAST 추가.
QOS 고려한다.

![image](https://github.com/yybmion/network/assets/113106136/f776fea9-d4f6-4d72-b994-440222ccf415)

___

### IPv6 Address

![image](https://github.com/yybmion/network/assets/113106136/9822f974-9d05-482b-9ec5-f53bfbfa3a57)

**ANYCAST**
 
 SERVER1 , SERVER2, SERVER3 가 동일한 서비스를 제공하는 서버라고 하자, 그럼 라우터는 ANYCAST에 대한 정보를 
 그룹으로 가지고 있다. 그럼 CLIENT가 GROUP ADDRESS ANYCAST를 라우터에 제공 이렇게 접속 후 
 여러개의 서버들 중에 CLIENT에 가장 적합한 서버 하나를 선택을 해서 패킷을 전달한다. 가장 가까운 서버에서 응답하면
 빠르게 CLIENT가 받을 수 있고, 분산도 되어 네트워크가 효율적으로 운영가능하다. 
 
![image](https://github.com/yybmion/network/assets/113106136/eae460e0-b495-43ee-bcaf-6e5732b308f5)

___

HIERARCHY(IPv6)
![image](https://github.com/yybmion/network/assets/113106136/665b20b8-01ae-4a50-b88a-48217fd01b9c)

이런식으로 HIERARCHY를 제공한다!
물론 여기도 NETWORK / HOST 나누어져있음.
TOP / MIDDLE / LOW
![image](https://github.com/yybmion/network/assets/113106136/0a504a05-63ba-41b8-80c7-22bd91e484a3)
 
___

HOST ADDRESS 부여 방법??

DHCP도 가능 BUT!
AUTO-CONFIGURED 사용! 그냥 48BIT라 64BIT안에 부조건 들어가므로 그냥 특별한 조치 없이 바로 할당 받을 수 있다.
그래서 특별한 경우가 아니면 이걸 사용하고 아니면 랜덤이거나 DHCP 등을 사용!

![image](https://github.com/yybmion/network/assets/113106136/d6350193-ff20-49ac-97af-96e8db9ac80a)

___
### DATAGRAM FORMAT
![image](https://github.com/yybmion/network/assets/113106136/df4fbfbb-7bec-4206-a936-879b23f0dab1)

기본 헤더는 40 BYTE
먼저 40BYTE 베이스 헤더 부분을 살펴보자

그림에서 보듯이 SOURCE ADDRESS와 DESTINATION ADDRESS 각각 16BYES씩 이므로 나머지 부분이 8BYTES를 할당받게 된다.
VERSION = 6
TRAFFIC CLASS = TOS IPv4와 비슷한 부분 하지만 IPv4에서는 거의 없는 취급이였음.
Flow label = IPv4에서는 품질에 대한 부분이 없었는데 이런것을 추가적으로 QOS를 통해 보장해주는것이 flow label 아직 정확히 어떻게 정의하는지는 모른다.
Payload length = data(sdu)까지 포함한 length
Next hdr = 뒤에 있는 Extension header를 지정해준다.
![image](https://github.com/yybmion/network/assets/113106136/6ed6ba02-a133-41ab-aef6-28cf92aac887)

IPv4 VS IPv6
![image](https://github.com/yybmion/network/assets/113106136/c4977e8a-3da6-489f-97fc-4b72dd078b2c)
___
Ipv6에서 fragmentation이라고 하는것 패킷이 여러개로 늘어나니 라우터도(그 전에 하나 처리 하던것을 fragmentation 하면 각각을 다해주어야하고)
end system도 이게 다 올때까지 기다려야 한다. Reassembly. 네트워크도 힘들고 단말도 힘든게 fragmentatio
그래서 IPv6에서는 FRAGMENTATION을 안하는것을 DEFAULT로 한다. 만약 해야 된다면 EXTENSIONAL HEADER에 
OPTIONAL하게 들어간다. 그래서 보면 HEADER LENGTH 가 있는 이유(ipv4에서)가 fragmentation을 default 로 제공해주는데 
ipv6에서는 optional하게 제공한다.

___
기존 IPV4에서는 OPTION에 들어가는 부분을 EXTENSION에 처리하게 해놨는데. 이렇게 하면 EXTENSION HEADER들은
TYPE / LENGTH / VALUE 항상 3가지를 가진다. 모두 길이가 고정되어 있으므로 다들 하드웨어적으로 처리가 가능하여
스피드를 극복할 수 있다.
![image](https://github.com/yybmion/network/assets/113106136/effca8d9-6515-426c-a2df-ea7d230c0746)

___

IPV4에서 IPV6로 가는 것은 FLAG DAY로 하지 않는다. FLAG DAY는 어느 한 시점에서 IPV4에서 IPV6로 모두 바꿀거에요~
하는것이다. 그것이 아니라 서로 공존하며 서서히~~ 바뀌는 것이다.
![image](https://github.com/yybmion/network/assets/113106136/ec0355a0-3ee1-4e73-998c-2df54019a0b4)

라우터에 IPV4,IPV6 두가지 기능을 갖게 하는것!!
DUAL STACK

라우터가 IPV6면 IPV6로 받고 IPV4면 IPV4로 받는 상황에 따라 변함 어짜피 둘다 가지고 있어
![image](https://github.com/yybmion/network/assets/113106136/26875df0-5c8b-4f09-9b0a-9214ef7af17a)
단점은 IPV6에는FLOW가 있는데 이게 4로 넘어가고 다시 6로 넘어가면 정보가 없어짐!!
Tunneling APPROACH 

이건 마치 터널을 뚫는건데 가운데 터널이 IPV4이고 IPV6가 IPV4 라우터로 갔을때 정보를 다 변화시키지 않고 
IPV6정보에 IPV4 헤더를 추가로 붙이는 방식이다. 그러면IPV6 내용은 그대로 유지가 되면서 IPV4로 간다.

![image](https://github.com/yybmion/network/assets/113106136/adf7ec67-9d2e-4e24-b46f-7ecb0493d3b5)
___

**CONTROL PLANE**

  ROUTING PROTOCOL들이 다른 PROTOCOL과 상호작용하면서 네트워크에 대한 정보 TOPOLOGY DATABASE정보를 가져온다.
그러면 이 정보를 통해서 ROUTING ALGORITHM을 통해 라우터들이 독립적으로 테이블을 만듬.   
DATA PLANE에서는 이 정보를 가지고 FORWRADING을 한다. 이는 DESTINATION ADDRESS가 꼭 필요한데,
때문에 전 장에서 ADDRESS 얘기가 많았던 이유이다.
![image](https://github.com/yybmion/network/assets/113106136/832a806b-4eff-4b38-8caf-7a5e3239e4bd)

___

### ROUTING ALGORITHM 
크게 네트워크에 대한 전체 정보를 다 가지고서 테이블을 만드느냐  -> LS
일부 정보를 통해 계속 업데이트 받으면(언젠가 모든 정보를 받을것임) 이런식으로 점진적으로 테이블 만들거냐 -> DV

![image](https://github.com/yybmion/network/assets/113106136/5cfa4da3-ad2b-48ee-bd77-170752820aee)

![image](https://github.com/yybmion/network/assets/113106136/c01b2870-0c12-4e12-8a6e-0bdfa8583442)

___
첫번째 LINK-STATE!
![image](https://github.com/yybmion/network/assets/113106136/129aaafa-7ce5-4af1-ae77-3249b9f768ce)

GLOBAL INFORMATION 사용

하나의 노드에서 다른 노드에서 갈때 최소의 COST의 경로를 찾아라!

붙어있는 애는 COST 나오지만 떨어져있는 애는 무한대 (더한값)
D(v) = 거쳐가는 모든 노드들의 cost 합한 값.
p(v) = 바로 직전 노드.
N' = 기준 노드들이 설정될 떄마다 N'이 설정되고 모두 N'되면 알고리즘 종료.
___

![image](https://github.com/yybmion/network/assets/113106136/de8bf83d-ba75-4801-a220-617cdf14f182)






  


