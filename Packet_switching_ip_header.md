### Data plane (network layer)

![image](https://github.com/yybmion/network/assets/113106136/00805812-97e3-4705-8942-8d497db076ad)

Address를 가지고 있는 end system에게 네트워크를 통해서 packet을 전달하는것
network layer의 역할 

application layer, transport layer은
end system에만 존재했는데

network layer은 source end system, routers, destination end system 모두 존

![image](https://github.com/yybmion/network/assets/113106136/a91d7345-9ea9-42ce-b7ad-dc70539ef18f)

___

routing은 각각의 패킷의 목적지를 정해준다.
이것은 routing algorithm을 통해 테이블을 만들어준다.

forwarding은 routing에서 만들어지는 routing table을 이용해 패킷을 
다음 라우터에게 보내는 작업이다.

![image](https://github.com/yybmion/network/assets/113106136/16321fd9-ef8a-4bb6-9a2d-2f4f431180df)

___

![image](https://github.com/yybmion/network/assets/113106136/dd9152f2-acfc-4bdb-8e6f-228e4959a48a)

___
circuit switching
이미 목적지까지 connection 다 맺어둠
dedicated!
하지만 낭비되는 부분이 있음

___

packet switching

![image](https://github.com/yybmion/network/assets/113106136/63872555-b829-4c2c-a11d-71e6989d7be4)

routing table을 통해 버퍼에 있는 packet들을 차례로 다음 router로 보낸다.
이를 store and forward라 하고 
shared 되기 떄문에 낭비가 거의 없다. 

___

패킷 스위칭 전달 방법 2가지
![image](https://github.com/yybmion/network/assets/113106136/4730839c-1ddb-4d8d-b202-6ee2ece5dbc8)

하지만 데이터그램을 사용함

___
vc circuit switching 

shared 하지만 destination미리 다 설정함
그래서 순서가 뒤 바뀔리 없고 loss도 없음

![image](https://github.com/yybmion/network/assets/113106136/d80fa64e-897c-4a09-b6f0-196e3c7cf696)

![image](https://github.com/yybmion/network/assets/113106136/f8cd7060-f08c-4405-86eb-5349672f0d53)

destination address를 확인할 필요없이 빠르게 forwarding  하기 위해 vci 사
___

vc forwarding TABLE

![image](https://github.com/yybmion/network/assets/113106136/c36ac849-88a0-4158-b3c2-9e09ec3b4319)

그냥 VC 는 처음부터 끝까지 TABLE이 채워져있음 그거 보고 따라 가는거임
원래부터 DESTINATION 이 정해져있는 상태이니까
이 그림에서는 A->B 만 보는거

패킷을 보내기 전에 DESTINATNION까지 경로를 맺는것을 SIGNALING이라 한다. 
매우 복잡한 기술이다. ATM SIGANLING을 이용하여 잘 만들어졌지만 너무 지체하는 바람에 DATAGRAM PACKET SWITCHING에게 이기지 못한다.

___

![image](https://github.com/yybmion/network/assets/113106136/c5c0666e-a8f3-483f-925a-81407d32c0c7)


NO CALL SETUP 즉 SIGNALING을 하지 않는다. 

PACKET FORWARDING USING DESTINATION HOS ADDRESS
UNREEALABLE 한대도 사용되는 이유는 뭘까?
SIGNALING의 복잡성 때문
![image](https://github.com/yybmion/network/assets/113106136/d1ce6ea2-165c-491b-8109-4797fde0b3a0)

___

CONTROL PLANE에서 ROUTING PROTOCOL이 서로 정보를 주고 받고 네트워크의 정보를 받아 들이면서 ROUTING ALGORITHM을 돌리면은 ROUTER들이 독립적으로 FORWARDING TABLE을 만든다.

![image](https://github.com/yybmion/network/assets/113106136/b46452d9-7032-428e-bd6b-3790f8cc7400)

___

![image](https://github.com/yybmion/network/assets/113106136/35563221-be2f-476e-9e90-2bdf76c32707)
___

![image](https://github.com/yybmion/network/assets/113106136/3ba71390-5767-4b62-9f2e-c2827853d15f)

___

SDN (SOFTWARE DEFINED NETWORKING)
![image](https://github.com/yybmion/network/assets/113106136/76507685-1258-47ff-990f-05d63645cc8d)

= CONTROL PLANE과 DATA PLANE을 정말 물리적으로 완전히 분리해보자
___

![image](https://github.com/yybmion/network/assets/113106136/87e57b6a-c891-4dd1-b494-68dd14a1b226)

![image](https://github.com/yybmion/network/assets/113106136/eebccdb7-84f7-47a1-bfad-73026cf0f9f0)

> https://suyeon96.tistory.com/48

___

### THE INTERNET PROTOCOL (IP)

 VERSION == IPV4, IPV6 등등 벼젼 적어주기
 HEADER LENGTH == OPTION은 없을 수도 있을 수도있는데 거기까지의 LENGTH
 
DATAGRAM LENGTH는 DATA까지 포함한 LENGTH (BYTES) 단위

TIME-TO-LIVE == DATAGRAM PACKET SITCHING은 FORWARDING 하므 다음라우터 다음라우터 하다보면 목적지를 못찾을 수 있 , 라우터를 거져 갈 수록 -1 씩 되는데 목적지를 못찾아가면 결국 0 가 됨 그래서 좀비패킷을 없애기 위한 것이다.

![image](https://github.com/yybmion/network/assets/113106136/048687b9-f93b-4f87-bc4a-e705198c3d6f)

___

DATAFRAM에서 TYPE OF SERVICE 
APPLCIATION에 따라 뭐가 중요한지 보고 결정한다.

![image](https://github.com/yybmion/network/assets/113106136/1d49ccb1-1479-47f4-b5e9-0b8c4c28c7b6)

근데 IPV4에서는 이게 거의 활용안함!!!











