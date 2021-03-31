## 캐시 메모리 (Cache Memory)

### 지역성의 원칙(Principle of locality)
---
<br>

> 도서관에서 자료조사를 할 때 책들을 뽑아 와서 책상에 앉아있는 상황을 생각해보자.  
> 무작정 책을 가져온다면, 찾으려는 내용이 없어서 다시 일어나서 서가에서 책을 가져올 것이다.  
> 그렇지만 관련 내용이 담긴 책 목록을 가져온다면 원하는 내용이 책 안에 있을 확률이 높아지고, 왔다갔다할 시간을 절약하게 된다.
>
> 이러한 원리는 아주 작은 메모리만큼 빠르게 접근할 수 잇는 큰 메모리를 갖고 있는 듯한 현상을 만들어낼 수 있다.

- **시간적 지역성**(temporal locality)  
  : 어떤 데이터가 참조되면 곧바로 다시 참조될 가능성이 높다는 원칙
- **공간적 지역성** (spatial locality)  
  : 하나의 데이터가 참조되면 곧바로 그 주위 데이터가 참조될 가능성이 높다는 원칙

<br>

### 메모리 계층구조
---
<br>
  
![image](https://user-images.githubusercontent.com/59992230/113066479-8cca1100-91f5-11eb-9cb0-751116e2a180.png)

 - 계층의 하위로 내려갈수록 메모리의 크기와 접근 시간이 증가한다.
   - 사용자에게 가장 빠른 메모리가 갖고 있는 접근속도를 제공하면서 저렴한 기술로 최대한 큰 메모리 용량을 제공하는 것이 목표

 - 메인 메모리는 **DRAM**으로 구성, 캐시에는 **SRAM**이 사용된다. 
   - 속도 : DRAM < SRAM (빠름)
   - 가격 : DRAM < SRAM (비쌈)
   - SRAM(정적메모리)  
     - 저장 비트 상태값 유지를 위해선 전류를 계속 유지해주어야함
     - 리프레시 사이클 필요 X
    - DRAM(동적메모리)
      - 셀에 기억되는 값이 전하로 축전기(Capacitor)에 저장
      - 리프레시 필요
  - 플래시 메모리  
    : 전기적으로 지울 수 있고 다시 기록할 수 있는 비휘발성이며, 프로그래밍이 가능한 ROM의 한 일종
     - 쓰기는 비트를 마모시키므로 덜 사용된 블록에 재사상(remapping)해서 쓰기를 분산시킴 (마모균등화, wear leveling)
     - 우리가 흔히 알고 있는 **SSD**는 NAND Flash, Controller, DRAM으로 구성
      
  - 디스크 메모리 (하드디스크, **HDD**)
    : 원판의 집합으로 구성, 읽기/쓰기 헤드인 암(arm)이 존재
      - 각 표면은 트랙(track)이라는 동심원으로 구성
      - 트랙은 정보를 담는 섹터(sector)로 나뉨
      - 물리적 탐색 시간이 병목을 일으킬 수 있음

<br>

![image](https://user-images.githubusercontent.com/59992230/113069006-665aa480-91fa-11eb-9884-b8f079b7b68a.png)

- 데이터는 상위 계층이 하위 계층의 부분집합이고, 가장 낮은 계층에는 모든 데이터가 다 저장되어 있다.
- 데이터는 인접한 계층 사이에서만 한 번에 복사가 된다.
  - 두 계층 간 정보의 최소 단위를 **블록(block)** 혹은 **라인(line)** 이라고 부른다.
  - 요구한 데이터가 상위 계층의 블록에 있을 때 **적중(hit)** 이라고 부르고, 상위 계층에서 찾을 수 없다면 **실패(miss)** 라고 부른다.
 

<br>

### 작동 방식
---
<br>

![image](https://user-images.githubusercontent.com/59992230/113073310-d1f53f80-9203-11eb-9e3d-ecb555c5db07.png)

- CPU(processor)가 필요로하는 데이터는 한 워드 단위로 요청하며, 캐시로부터 메모리에 사상되는 단위는 블록으로 살펴본다.


![image](https://user-images.githubusercontent.com/59992230/113072117-36fb6600-9201-11eb-8e29-b97d988feed5.png)

- 캐시에는 존재하지 않는 데이터 Xn을 요청했을 때, 이는 실패(miss)를 발생시키고, Xn을 메모리부터 캐시로 가져온다.
 > 1. 데이터가 캐시 내에 있는지는 어떻게 알 수 있을까?  
 > 2. 캐시 내에 있는 데이터는 어떻게 찾아낼 수 있을까?  
 > -> **메모리 주소**에 기반을 두고 할당하면 된다.

  - **태그(tag)**  
  : 특정 계층에서 해당 블록이 요청한 워드와 일치하는지를 알려주는 주소 정보를 담고있는 필드

- 워드(word), 블록(block), 라인(line)
  
  ![image](https://user-images.githubusercontent.com/59992230/113074789-b17ab480-9206-11eb-8414-026e5bb76612.png)
  ![image](https://user-images.githubusercontent.com/59992230/113074799-b5a6d200-9206-11eb-97ca-eab30c70318c.png)

  - 블록 : 메모리를 개념적으로 분할한 단위
  - 라인 : 캐시를 개념적으로 분할한 단위
  - 워드 : 하나의 번지에 저장되는 데이터 단위
  - 위 그림에서 64단위 메인 메모리가 있고, 한 블럭당 4 워드를 저장한다고 보면 된다. 이 때 가질 수 있는 블럭의 수는 16가지가 될 것이다. 

<br>

1. **직접 사상(direct mappping)**

- 각 메모리의 위치가 캐시 내의 정확히 한 인덱스에만 사상되는 캐시 구조

   ![image](https://user-images.githubusercontent.com/59992230/113072476-12ec5480-9202-11eb-9553-6a0a1326701e.png)

  - ```
    (블록 주소) modulo (캐시 내에  존재하는 전체 캐시 블록수)
    ```
  - 캐시 내에 8개의 워드가 존재시 주소 X는 ``X modulo 8``에 사상, 하위비트들(
    <img src = "https://user-images.githubusercontent.com/59992230/113072711-afaef200-9202-11eb-910a-c1b853225d32.png" width = "70">
)이 캐시의 **인덱스(슬롯)**  로 사용

  - 메모리의 주소 형식 ![image](https://user-images.githubusercontent.com/59992230/113073340-de799800-9203-11eb-91ec-402236268763.png)

  - 태그가 다른 두 개 이상의 단어가 반복하여 참조되면 적중률이 상당히 떨어지는 단점 발생

- ![image](https://user-images.githubusercontent.com/59992230/113074081-56948d80-9205-11eb-9c62-da481d502793.png)

   > **CPU: ``00001`` Word 참조**  
   > 현재 캐시는 8개 워드 존재, ``mod 8``, index는 하위 세자리 ``001``  
   > 1.  캐시 주소가 ``001``인 곳 찾고, 태그 정보인 ``00``를 비교 -> **cold miss**
   > 2. 주기억장치의 ``00001`` 블럭을 참조하여 워드를 획득
   > 3. 캐시의 ``001`` 라인으로 mapping
   > 4. 워드 ``1234``를 CPU에게 전달

- ![image](https://user-images.githubusercontent.com/59992230/113074134-775ce300-9205-11eb-93fe-28452777c6a9.png)
 
  > **CPU: ``10001`` Word 참조**
  > 1. 캐시주소 ``001``인 곳 찾고, 태그 정보인 ``10`` 비교 -> **conflict miss**  
  > 2. ~4. 위와 마찬가지의 과정을 거침

2. **연관사상(Associative mapping)**

- 각 메모리의 블록이 캐시의 임의의 인덱스에 적재할 수 있는 캐시 구조

  ![image](https://user-images.githubusercontent.com/59992230/113075425-dc193d00-9207-11eb-8d33-f1ba874b0e21.png)


  - 메모리의 주소 형식 ![image](https://user-images.githubusercontent.com/59992230/113075491-010db000-9208-11eb-963a-0ffd48911e2e.png)
  - 융통성이 있으나 모든 슬롯에 대해 태그가 일치하는지 검사해야하므로 검사 시간이 오래걸림
  - 모든 검색 후 원하는 데이터가 없을 시 **miss**
  - 메모리에서 원하는 데이터 인출 -> 캐시 교체 알고리즘 필요
  - 빠른 검색을 위해 associative-mapped memory 사용

3. **집합 연관 사상(Set-associative mapping)**
 - 직접 사상 + 연관 사상
 - 두 개의 집합을 갖는 캐시 구조

    <!-- ![image](https://user-images.githubusercontent.com/59992230/113076281-a37a6300-9209-11eb-9bbb-8e76be8d20c0.png)

  - 
  - 같은 집합 내에서는 연관 사상 방식 적용

  - ![image](https://user-images.githubusercontent.com/59992230/113076388-e76d6800-9209-11eb-8d1b-459673a5277e.png) -->


<br>
<br>

### reference
- [wiki - Memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy)
- [Samsung - Disk](https://www.samsungsemiconstory.com/464?category=537531)
- [SRAM, DRAM](https://koodev.wordpress.com/2016/01/11/sram%EA%B3%BC-dram-%ED%95%98%EB%93%9C%EC%9B%A8%EC%96%B4-%EC%9D%B4%ED%95%B4/#:~:text=RAM%EC%97%90%EB%8A%94%20%ED%81%AC%EA%B2%8C%20SRAM%EA%B3%BC,%EA%B0%80%EA%B2%A9%EC%9D%B4%20%EC%8B%B8%EB%8B%A4%EA%B3%A0%20%EC%95%8C%EB%A0%A4%EC%A0%B8%EC%9E%88%EB%8B%A4.)
- [플래시 메모리](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9E%98%EC%8B%9C_%EB%A9%94%EB%AA%A8%EB%A6%AC.)
- [KOCW ppt](http://elearning.kocw.net/document/lec/2011_2/dunksung/YouGyunA/13.pdf)
