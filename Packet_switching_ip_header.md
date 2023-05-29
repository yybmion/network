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

routing table을 통해 버퍼에 있는 packet들을 차례로 다음 router로 보낸다.
이를 store and forward라 하고 
shared 되기 떄문에 낭비가 거의 없다. 

