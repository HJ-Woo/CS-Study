## DNS Server

<br>

### Domain Server의 도입
<br>

 > *‘개방된 인터넷 상의 모든 컴퓨터는 전화번호와 마찬가지로 숫자로 된 고유한 주소를 가지고 있습니다. 이 일련의 숫자로 된 주소를 우리는 **IP**(Internet Protocol)라 부릅니다. 그러나 사람들이 이 숫자로 된 주소를 기억하기는 어렵습니다.*  
 >
 > *인터넷 상에서 주어진 위치를 보다 쉽게 찾기 위해 **도메인 네임 시스템(Domain Name System, 이하 DNS)** 이 개발되었습니다. DNS는 IP주소를 영문과 숫자 조합으로 구성되어 기억하기 쉬운 고유한 **‘도메인 이름(Domain Name)’** 으로 변경하여 줍니다.*  
>
 > *익숙한 문자열(도메인 이름)을 IP 주소에 결부하여 인터넷 사용자들이 웹사이트 주소와 이메일 주소를 보다 쉽게 기억할 수 있도록 만들어 줍니다. 예를 들어 www.HJ.com에서 HJ.com은 인터넷 주소의 한 부분으로 **‘도메인 이름’** 이라 불립니다.*  
 >
> *‘www.’ 부분은 당신의 웹브라우저에게 지금 당신이 HJ.com이라는 도메인 이름으로 운영되는World Wide Web 인터페이스를 찾고 있다고 알려줍니다. DNS는 또한 당신이 보내는 이메일이 정확히 수신자에게 도착하도록 합니다. 길고 어려운 숫자로 된 IP 주소를 입력할 필요 없이 www.HJ.com을 치면 원하는 웹사이트에 정확히 도달하도록 해줍니다.*
>  
 >*도메인 이름은 웹사이트가 다른 ‘호스트 컴퓨터(host computer)’나 ‘서버(server)’로 이동했을 때에도 변하지 않습니다. 왜냐하면 DNS는 도메인 이름이 변경된 호스트 컴퓨터의 새로운 IP 주소를 찾도록 해 주기 때문입니다. 이는 마치 집주소나 사업장 주소가 바뀐다고 하더라도 가정이나 회사명이 바뀌지 않는 이치와 같습니다.*
  *-ICANN 국제 인터넷주소 관리기구*

<br>

--- 
<br>

### 호스트 이름, 도메인 이름
<br>

- 호스트 이름은 **로컬 호스트 이름**과 , 도메인 이름인 **Network** 에 주어진 이름의 조합

  > ex) 호스트 이름 www.HJ.com 에서 www는 world wide web(웹서버)의 로컬 호스트 이름, HJ.com은 도메인 이름  
    호스트 이름 mail.cs.com의 로컬 호스트 이름은 mail(메일서버), cs.com는 도메인 이름  
  
  <br>

- 도메인 이름은 DNS 규칙과 절차에 따라 형성  
 <br>

   ![image](https://user-images.githubusercontent.com/59992230/112504566-4298f800-8dcf-11eb-808a-16463414e6a3.png)

  <br>

   - 도메인 이름 공간은 도메인 이름을 트리 형태로 구성한 것 (root -> leaf)
      > 참고: 로컬 호스트 이름을 leaf label에도 사용O
   - 각 DNS zone은 하나의 권한있는 네임서버(authoritative name server)에 의해 관리 (한 네임서버가 여러개 존 관리 O)

   - 한 개 이상의 부분(레이블)로 이루어지고, 점으로 구분하여 붙여쓴다.
   - 가장 오른쪽 레이블은 최상위 도메인 (org)
   - 계층구조는 왼쪽 <- 오른쪽으로 구성
   - wikipedia는 org의 서브도메인, en은 wikipedia의 서브도메인 


<br><br>

### 호스트 이름 -> IP 주소

0. 브라우저로 접근시 
   -  **브라우저 캐시**로부터 확인을 먼저 시도하기 때문에 이전에 접속했던 주소는 기존의 IP 주소로 정상 접근할 수 있다.

1. 로컬 hosts file
   - hosts 
    > ```
    > 127.0.0.1 localhost
    > ::1 localhost
    > 10.10.10.10 www.HJ.com
    > ```
    > 호스트 파일을 변경 후 www.HJ.com 접속 시도할 경우 해당 IP로 접속하게 된다.  

2. Recursive DNS resolver (by ISP)
  ![image](https://user-images.githubusercontent.com/59992230/112514936-28fcae00-8dd9-11eb-8200-afb824437f84.png)

  - DNS resovler(해석기)는 DNS 권한있는 이름서버에 연결
  - 루트 권한에서부터 시작해서 요청되는 DNS 이름에 대한 응답을 제공할 수 있는 권한에 도달할 떄까지 계층 구조의 하위 영역에 대한 책임을 위힘하는 참조
    > 즉, "www.HJ.com"을 예시로 들자면,  
    > 1) .com에 위임
    > 2) HJ.com에 위임
    > 3) 마지막으로 www.HJ.com에 위임하여 이에 대한 주소 레코드를 제공

<br>
<br>
<br>
<br>
<br>

### reference
- [가비아 - Dmain](https://library.gabia.com/contents/domain/4005/)
- [Wiki-domain](https://en.wikipedia.org/wiki/Hostname#:~:text=3%20Example-,Internet%20hostnames,the%20domain%20name%20wikipedia.org)
- [Akanami - DNS Resolver](https://developer.akamai.com/blog/2018/05/10/introducing-new-whoami-tool-dns-resolver-information)
- [How your broswer finds websites](https://scotch.io/tutorials/dns-explained-how-your-browser-finds-websites)