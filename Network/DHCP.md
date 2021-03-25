## DHCP (Dynamic Host Configuration Protocol)

: 호스트 IP 구성 관리를 단순화하는 IP 표준

### 고정 IP와 동적 IP

- 고정 IP  
 : 사용자가 수동으로 IP 주소가 있는 장치의 IP 주소(서브넷 마스크, default 게이트웨이, DNS server 등 포함)를 할당하는 IP

- 동적 IP  
 :  DHCP 서버로부터 자동으로 IP 주소를 설정하는 IP


### DHCP란?
- 동적 호스트 설정 통신 규약 (Transport layer용 프로토콜: UDP)
- VLAN 내부에서 Private IP를 동적으로 할당시킴
- DHCP Server에 설정된 Scope 내의 IP를 자동으로 Client에게 할당


### Scope
- ``DHCP server``는 ``Scope`` 내에서 IP 주소를 할당한다.  
- Scope는 Start IP address, End IP address를 통해 결정

### Lease
- DHCP server는 IP 주소를 컴퓨터에게 **임대**를 한다.  
-> 서버 범위 내에서 IP 주소가 부족하지 않도록 하기위함
  
  ![image](https://user-images.githubusercontent.com/59992230/112402990-e099ad80-8d50-11eb-86db-0f3615358f7a.png)

- 임대기간 (Lease Time)  
  : 주어진 IP 주소가 디바이스에 유효하는 일정 시간
- 임대 요청/갱신(Renewal)
  : 임대 기간의 절반이 지나면 임대 갱신 요청을 해야한다.
- 리바인딩(Rebinding)  
  : 할당받은 DHCP Sever와의 갱신 최대 한계를 의미한다. 갱신 실패시 타 DHCP Sever에 연장을 갱신해야한다.
- 재연결(Relocated)  
  : 예기치 못한 재부팅시에 부팅전 설정 재배당

- ipconfig /all에서 확인 가능
  ![image](https://user-images.githubusercontent.com/59992230/112415203-cb2f7e00-8d66-11eb-8386-fddf758b6fcb.png)

### DHCP 절차

![image](https://user-images.githubusercontent.com/59992230/112403622-1b501580-8d52-11eb-8d74-d8649a3b02de.png)

1. DHCP Discover (발견)  
 : Client는 DHCP Server를 찾는 요청을 Boardcast로 보낸다.
    - Client -> Server
    - Broadcast Message (LAN 상에서 DHCP Server를 찾는 요청)
    - Parameter: Client MAC

2. DHCP Offer (제공)  
  : DHCP Server들은 해당 브로드캐스트에 대한 응답을 자신의 정보, 할당할 수 있는 네트워크 정보와 함께 응답한다.
   - Server -> Client
   - Broadcast or Unicast (Broadcast Flag에 따라 다름)
   - Parameter: 
  Client MAC, DHCP Sever의 존재(IP), 
  IP 정보(IP, Subnet Mask, Router, DNS, IP Lease Time)
   - ICMP Packet  
     >Offer을 보내기 전, 할당할 IP 주소가 기존 IP 주소와 충돌하는지 확인하고자 ICMP Echo Packet을 보낸다.  
    패킷을 받지 못할 경우, IP 주소를 사용하지 않는다는 의미로 IP 주소 사용 가능
   - Offer 메세지 후  16s 내로 응답을 받지 못한다면, IP 주소가 다른 Client에 할당될 수 있음

3. DHCP Request (요청)
  : 하나의 DHCP Server를 선택하고, 해당 서버에게 기기가 사용할 네트워크 정보를 요청한다.
    - Client -> Sever
    - Broadcast
    - Parameter: 
  Client MAC, Request IP,  DHCP IP

4. DHCP Ack (수락)
  : DHCP Server가 기기에게 네트워크 정보를 전달해주는 메시지
   - Server -> Client
   - Broadcast or Unicast (Broadcast Flag에 따라 다름)
   - Parameter: 
  Client MAC, DHCP Sever의 존재(IP), 
  IP 정보(IP, Subnet Mask, Router, DNS, IP Lease Time)
   - ARP Packet
    > Client는 ACK를 수신한 후 다른 장치가 해당 IP 주소를 사용하고 있는지 확인하기 위해 ARP Packet을 Boardcast한다.  
    응답 수신 X -> IP 주소 사용 가능  
    응답 수신시 -> DHCP Server에 decline Message, 새 IP 주소를 응답받는다.



### Address Reservation

- 그렇다면 무조건 IP를 자동으로 할당받아야할까? No!

- 특별한 기기(Router, Printer ...) 등은 DHCP 설정의 Address Reservation을 이용하면 고정 IP처럼 계속해서 할당받고자하는 특정 IP를 기기의 Mac address를 이용해 지정해줄 수 있다.

### DHCP 장단점
 - 장점
   - COST 절약  
  (사용중인 Device에만 IP 할당, 고정IP에 비해 IP 절약)
   - 효율적인 네트워크 관리   
  (IP망 설계 변경 자유로움, 네트워크 설정 바뀌면 DHCP만 변경)
 - 단점
   - 초기 요청 Boardcast 트래픽 유발   
  (VLAN 설정범위 내 모든 단말에 전송, 네트워크 성능 저하)
   - Device off시 Lease Time까지 해당 IP를 할당 X
   - DHCP Sever에 전적으로 의존  
   (DHCP Sever 오작동시 IP를 할당X)






### reference
- https://www.youtube.com/watch?v=e6-TaH5bkjo
- https://ko.wikipedia.org/wiki/%EB%8F%99%EC%A0%81_%ED%98%B8%EC%8A%A4%ED%8A%B8_%EA%B5%AC%EC%84%B1_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C
- https://www.netmanias.com/ko/post/blog/5348/dhcp-ip-allocation-network-protocol/understanding-the-basic-operations-of-dhcp
- http://www.ktword.co.kr/abbr_view.php?m_temp1=1607
- https://support.huawei.com/enterprise/en/doc/EDOC1100126851/5cef90ad/how-a-dhcp-server-allocates-network-parameters-to-new-dhcp-clients



