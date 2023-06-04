AS내에서 동작하는 프로토콜이 IGP
AS간 동작하는 프로토콜이 EGP
![image](https://github.com/yybmion/network/assets/113106136/32b90dcf-68aa-4691-bae8-53c305caa129)

___

VECTOR은 DESTINATION
DISTANCE는 그 DESTINATION까지 가는데 COST
근데 RIP에서는 모든 링크의 COST를 1로 설정 그래서 얼마나 많은 HOP(라우터)를 거쳐가냐를 본다.
![image](https://github.com/yybmion/network/assets/113106136/0da59f12-0dbe-42eb-b346-81b7eb51ee7f)
 
근데 BGP에서는 좀 다르게 PATH-VECTOR을 사용
STACK식으로 경로를 하나한 적어줌 나타냄. 목적지까지가는 PATH를 구체적으로 표현해주는 방법이 바로
PATH VECTOR ROUTING.

![image](https://github.com/yybmion/network/assets/113106136/c8496b04-f2d5-47f3-a3b9-2fcbaebb4c4f)

___

그렇다면 왜 BGP는 PATH-VECTOR을 사용하냐 알아보자.

1. AS내에서는 IGP ROUTING을 통해서 라우팅 정보를 주고받는다. A 라우터는 N1,N2정보를 수집해서 알고있다.
2. 그럼 AS1에는 (N1,N2)정보있고, 자신의 라우터에 대한 ADDRESS, 자기가 속한 AS에 대한 정보를 B라우터에게 전달 
3. B는 이걸 C한테 전달할 (N1,N2)에 대해서는 자신 B를 통해서 전달할 수 있는데 가는 경로는 AS2-AS1이다.
4. 그리고 N3,N4는 아까와 같이 전달한다.    
5. 그래서 C는 이걸 가지고 테이블을 만든다.

![image](https://github.com/yybmion/network/assets/113106136/8c0fa165-0558-4faf-8f2f-25de345eae73)

___
![image](https://github.com/yybmion/network/assets/113106136/95899942-439a-4449-ac4e-db2826db4492)

근데 여기서 문제가 있다. 만약 AS4가 AS1까지의 PATH가 여러가지일 경우에는 어떻게 할가?
예전에는 최소 코스트를 통해 선택을 했지만 지금은 **ADMINISTRATOR'S POLICY**를 따른다.

근데 DV그냥 쓰지 왜 PATH-VECTOR씀??
LOOP(목적지를 찾지 못하고 한 구간에서 계속 도는 것)에 대해서 확인할 수 있다.!!
서로 다른 서비스를 제공할 수 있는 네트워크를 구성해야 할때  
![image](https://github.com/yybmion/network/assets/113106136/aeb8c15f-5578-41ae-9210-26fd23f32b1b)

___

W고객의 정보를 A에게 전달 후 자기 PATH하고 해서 B와 C에게 전달한다. 그 후 B는 자기가 받은것을
다른 쪽에다가 알려주는데 이 알려주는 것도 POLICY를 통해 알려줄 수도있고 안 알려줄수도있다.
예를들어 Y나 다른쪽에서 오는 것들은 나(B)를 거쳐서 서비스를 해줘봤자 아무런 이득이 되질못한다.
그래서 (C,B,A,w) 이 PATH를 C에게 알려주지 않는다. 그래서 Y 로부터 W에 가는것은 무조건 (C,A,w) 한 PATH로 정해진다.
그래서 POLICY는 PATH-VECTOR에 FORWARDING하는 것에 대해 정책을 삼을 수 있다. 
![image](https://github.com/yybmion/network/assets/113106136/99802eda-2e14-4d18-84a8-7fb45defbc2e)

___

BGP라우터들 간에는 TCP CONNECTION을 맺어서 동작한다. 
참고로 RIP는 UDP 그리고 둘 다 APPLICATOIN에서 동작한다. 
OSPF는 네트워크 레이어에서 바로 사용하기 때문에 TRANSPORT LAYER 사용 X 
![image](https://github.com/yybmion/network/assets/113106136/897daa9d-7ae6-4641-bc9b-337a43edca8b)

___

BGP SPEAKERS = BGP 라우터와 같은 뜻. 
BGP GATEWAYS = 두개이상의 AUTONOMOUS SYSTEM 간에 연결을 제공하는 BGP SPEAKER을 BORDER GATEWAYS라 함.

BGP는 외부(eBGP)와 내부(iBGP)가 있다.
eBGP가 정보를 교환할 때는 누구를 거쳐서 교환하지 않고 무조건 직접 연결되어있어야한다.
![image](https://github.com/yybmion/network/assets/113106136/cacca804-bad9-442d-a0ca-a9c9a40cbb36)

iBGP가 필요한 이유?
BGP는 정보를 누구에게 전달하려면 무조건 직접 eBGP가 연결되어있어야한다.
근데 이 거리가 너무 멀면 이걸 설치하는데만 비용이 엄청날 것이다. (아주대-부산대)
그래서 어느 한 지점에 AS100(매우 공공함)를 놓고 라우터들 중 가장 가까운 쪽의 라우터와 연결하여 eBGP를 발동!!
iBGP를 통해 아주대 정보를 전달한다음에 상대방으로 전달(eBGP)한다.
![image](https://github.com/yybmion/network/assets/113106136/174a0e1f-d1e3-4849-90d1-92d52bb434ab)

___

![image](https://github.com/yybmion/network/assets/113106136/a84d8d49-5a53-4151-bf24-2cb1253e8f22)

X 라우터가 추가되었다고 한다면 이를 다른 애들한테 알려줘야함.
eBGP로 AS3,X 이렇게 AS2에게 전달 이 정보는 iBGP로 내부에서 서로 전달 
다음 AS1으로 갈때는 PATH 를 나타내어 전달 AS2,AS3,X 이렇게.

근데 만약 여러 경로가 있다면 모든 경로에 정보를 보내주긴 하지만 AS1의 POLICY에 의해서
선택을 하여 보내준다.
![image](https://github.com/yybmion/network/assets/113106136/08a791e9-f306-41ba-a647-76978dbe9dd6)

___

그래서 ROUTE를 선택하는 것에 여러 방법이있는데,
1. POLICY 기반
2. AS를 적게 거쳐가는 것을 쓸 수 있고 등등
![image](https://github.com/yybmion/network/assets/113106136/f2cd70f2-7748-44f5-8a80-86510551ba3e)

___

INTRA AS와 INTER AS에 차이를 보자!!

당연히 내부적으로 동작시킬 때는 모든 네트워크와 ROUTERF를 동일한ADMINISTER가 관리하니까 정책을
굳이 관리할 필요는 없고 효율성만 따진다.(INTER-AS)
근데 AS를 넘어서 갈때는 POLICY 중심으로 본다. 
![image](https://github.com/yybmion/network/assets/113106136/f47735ce-2915-42e9-b583-0857c948c3db)

___

### ICMP
![image](https://github.com/yybmion/network/assets/113106136/cebd78c1-8d72-4cc8-80c5-b2f0e152b051)
오늘날 거의 DATAGRAM-PACKET -SWITCHING을 사용하는데 이게 순서도 바뀌고 손실될 수 있어서 
안전하게 갔는지 안갔는지 확인할 방법이 필요함.

그래서 SOURCE에서 정보를 포워딩하다가 어느 한 라우터에서 문제가 터지면 (LOOP을 돌다가 TTL값이 0가 되거나)
근데 그냥 저 라우터에서 폐기 시켜버리면 SOURCE는 잘 갔는지 안갔는지 모른다. 
그래서 문제가 있는 라우터에서 SOURCE에게 문제상황을 REPORT해준다. (애초에 유일하게 아는 정보는 SRC IP다)
![image](https://github.com/yybmion/network/assets/113106136/2c6fbe42-deb6-467a-aceb-ef26cb2f25fc)

TYPE과 CODE를 보면 어떤 문제인지 알수있다. 
![image](https://github.com/yybmion/network/assets/113106136/56a49bf3-76e3-425c-9a12-42ba2d039497)

___
![image](https://github.com/yybmion/network/assets/113106136/c0faba0e-aeaf-4201-a278-2f1d4cbb43e9)

1로 지정하면 ICMP
___
이제 ICMP의 종류를 알아보자.
1. PING
살아있나 죽어있나 확인하는 것.
8,0으로 보내서 살아있으면 0,0으로 응답.
![image](https://github.com/yybmion/network/assets/113106136/cb3bc95a-cd5c-4a28-b6e9-ffd2e6bccc21)

2. TRACEROUTE
내가 DESTINATION까지 어떤 ROUTER를 거쳐갔나 확인하는 것.
DEFALUT로 UDP SGEMENT를 TTL=1로 설정하여 다음 라우터에 전송.
그러면 다음 라우터에는 TTL이 -1되고 0이므로 SRC에게 다시 11,0을 보냄.
이후 TTL=2로 다시 보냄 계속 반복해서 라우터를 추적하는것.

___
### IGMP 
![image](https://github.com/yybmion/network/assets/113106136/4c0938c5-f601-40f0-9471-3bfdf385b840)

IGMP는 하나의 SUBNET내에서만 동작한다. 
MULTICAST를 지원하는 라우터는 주기적으로 IGMP 쿼리 메시지를 보내고 MULTICAST 그룹에 JOIN한
HOST는 IGMP 리포트를 보낸다. 이게 뭔 X소린가 싶다. 알아보자.

사용자 A가 MULTICASTING 메시지를 받으려고 하면 자기 서브넷 상에서 IGMP REPORT를 먼저 보낸다. 
만약 MULTICAST 라우터가 만약에 내가 이 MEMBERSHIP에 대한  MULTICAST패킷이 요 MEMBERSHIP에 해당하는게 오면
이쪽 SUBNET에 잔달해달라!
그래서 IGMP는 이 서브넷내에 MULTICAST 그룹에 조인되어 있는 사용자가 있다 없다를 라우터에게 알려주는 
프로토콜이다. 
![image](https://github.com/yybmion/network/assets/113106136/0e959c71-3bb6-4815-8689-58d9c3ef57c6)

QQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQ
모르겠어 쿼리 주기적으로 날리는건 그냥 정보 받은 리시버가 정보받으면 바로 사라지는데 이거 계속 유지하면 안되니까
쿼리 날려서 해당하는 멤버십에 대한 것들에 대한 응답이 오질 않으면 라우터를 리스트에 없애버린다.
![image](https://github.com/yybmion/network/assets/113106136/b9de642c-97b7-4577-85dc-50428a5cf759)

___
MULTICAST ROUTING PROTOCOL이라는 것이 있는데 , MULTICAST서버로부터 전달되어서 받는구나~~
___

### SDN

CONTROL PLANE: ROUTING&MANAGEMENT
DATA PLANE: FORWARDING 
 
___

router은 vender들이 만들어서 제공하고 또 vender들이 만든 os를 쓰니까 새로운 app을 만들어서
적용하기가 힘들다. app도 vender가 만들어주는걸 갖다가 사용해야함 
그러므로 limited user rogrammability.
![image](https://github.com/yybmion/network/assets/113106136/648053a9-36e7-4156-9d6f-3a76484dbeaf)

vender에게 끌려다니며 네트워크를 운영하려다 보니까 불만이 생김.
그럼 control plane과 data plane이 logical하게 분리되어있는것을 physical하게 분리해보자.

![image](https://github.com/yybmion/network/assets/113106136/e4a96e1a-98e5-43e2-997c-d87e5a70b66b)

___

둘을 완전히 분리시켜 사용자가 직접 programmable 할 수 있게 하고 data link 부분에서는 forward만 할수 있게 만듬
기능적으로 너무 좋음.
![image](https://github.com/yybmion/network/assets/113106136/878ad761-8443-439a-9352-d9235fbd7c62)

___
![image](https://github.com/yybmion/network/assets/113106136/f25108a4-4454-4284-8e41-29bd549e6528)

___
SDN의 가장 큰 특징은 
1. CONTROL PLANE과 DATA PLANE을 완전히 구분했다. 
2. 그래서 GENERALIZED FORWARDING이 가능하게 된다.
3. 그러면서 CONTROL PLANE에서는 CONTROLLER가 존재해서 이 CONTROLLER는 모든 것을 관리할수있는 VIEW POINTER을 제공
4. 그러면서 필요한 기능들을 PROGRAMMING가능하게 둘 수 있다. 
5. ![image](https://github.com/yybmion/network/assets/113106136/9ee3d50e-1861-421f-bcb8-f93def20a056)
___

CONTROLLER와 SDN SWITCH(ROUTER)가 상호작용이 필요하다. 위쪽으로 NORTHBOUND API = MANAGEMENT PLANE
아래는 SOUTHBOUNT API 
따라서 CONTROLLER는 SOUTHBOUNT API를 통해 SWITCH들과 정보를 교환하며 FORWARDING TABLE도 만들어서 제공하고 
그러면서 이런 FORWARDING TABLE을 만들기 위해서 어떤 특정한 기능을 사용할 떄는 NORTHBOUND API를 사용해서 
MANAGEMENT PLANE기능을 활용한다. 










 
