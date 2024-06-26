### 작성자 : 박민지
<br>

# DNS
> `Domain Name System`<br>
> www.google.co.kr 을 입력하면 쉽게 구글에 접속할 수 있다. 하지만 실제 웹서버에 접속하기 위해서는 이런 문자열 주소가 아니라 컴퓨터가 알아들을 수 있는 IP주소가 필요하다. 그래서 **DNS** 시스템을 도입해 우리가 사용하는 **문자열의 인터넷 주소를 IP주소로 변환**하여 컴퓨터가 처리할 수 있도록 한다.

*domain name : 인터넷 호스트에 대한 하나의 식별자(사람의 입장)<br>
*IP address : 컴퓨터에서 네트워크 장치들이 서로를 인식하고 통신하기 위해 사용하는 특수 번호

### Q. www.google.co.kr 을 검색하면 무슨 일이 일어날까?
[도메인 주소가 IP로 변환되는 과정]

<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/602f791d-c576-44ee-b07a-955f7240e724">
<br>

#### `"브라우저에 도메인 입력 → DNS서버에 IP주소 요청 → 수신한 IP주소에 해당하는 웹서버에 접속"`

#### 1. 사용자가 웹브라우저 검색창에 `www.google.co.kr`을 입력한다 
#### 2. 도메인 주소에 상응하는 IP주소를 찾기 위해 `캐싱된 DNS 기록을 확인`한다
   - 브라우저 캐시 -> 로컬 캐시(OS 캐시) -> 라우터 캐시 -> ISP캐시 순으로 확인한다
   - 사용자가 해당 도메인을 검색하면 바로 제공 가능하다 (없는 경우 다음 단계 진행)
#### 3. ISP의 DNS 서버는 IP주소를 찾기 위해 `DNS query`를 시작한다
   - DNS 쿼리는 IP주소를 찾을 때까지 반복한다 (recursive search)
#### 4. DNS는 웹브라우저가 `요청한 사이트의 IP주소를 응답`한다
#### 5. IP주소를 받으면 `TCP 통신`을 시작한다
   - 3-way handshaking
#### 6. `HTTP 프로토콜로 요청`한다
   - TCP 연결에 성공하면 브라우저는 **GET request**를 통해 www.google.co.kr의 웹페이지를 요청한다
   - 요청한 메세지는 TCP 프로토콜을 사용해 인터넷을 거쳐 요청했던 IP주소의 컴퓨터로 전송된다
#### 7. `WAS와 DB`에서 우선 웹페이지 작업을 처리한다
   - 웹서버 혼자서 모든 로직을 처리하고 데이터를 관리하게되면 서버 과부하 위험이 있다
   - 서버의 일을 돕는 조력자의 역할이 바로 `WAS`
#### 8. WAS에서의 작업 처리 결과를 웹서버로 전송한다
#### 9. 웹서버는 웹브라우저에게 html 문서 결과를 응답한다
   - 검색된 웹 페이지 데이터는 또 다시 HTTP 프로토콜을 사용하여 HTTP 응답 메세지를 생성한다
   - TCP 프로토콜을 사용해 인터넷을 거쳐 원래의 컴퓨터로 전송된다
#### 10. 웹브라우저는 html 컨텐츠를 출력하여 사용자에게 제공한다 
   - 도착한 HTTP response 메세지는 HTTP프로토콜을 사용해 웹 페이지 데이터로 변환된다
   - 웹 브라우저에 의해 출력된다
<br>

##### [2번 단계 추가 설명]
<img width="500" alt="image" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/6bb71f9b-e048-4bb4-b9d4-7c03807c22b0">

>과거에는 컴퓨터마다 host.txt 파일을 가지고 있어 모든 컴퓨터의 host name과 IP주소가 저장되어 있었다. 하지만 90년대 초반 web 서비스 사용자의 폭발적인 증가로 인터넷에 연결된 host 숫자가 크게 늘어났다. 이로인해 네트워크 트래픽이 증가하고 host name을 짓기 어려워지는 문제가 생겨나면서 `분산 DB`를 이용하게 되었다.

##### [7번 단계 추가 설명]
<img width="500" alt="image" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/efd01a87-2cf9-4ca1-ad6e-9105da942802">

> - 서버는 요청에 필요한 로직이나 DB 연동을 위해 WAS(Web Application Server)에 요청한다. WAS는 DB에서 데이터 정보를 받아오고, 이렇게 연동하여 얻은 데이터를 다시 서버에 반환한다.
> - 보통 web server는 정적 데이터(html, css, js, 이미지), was는 동적데이터(ex 로그인)를 처리한다

##### [Web Server와 WAS]
- Web server가 필요한 이유
   - 클라이언트는 먼저 html 문서를 받고, 필요한 이미지 파일을 다시 서버로 요청해 받아온다
   - web server를 통해 애플리케이션 서버까지 가지 않고도 정적인 파일을 제공받을 수 있다
- WAS가 필요한 이유
   - web server만으로는 자원이 부족하기 때문에 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓을 수 없다
   - WAS로 요청이 들어올때마다 DB와 로직을 처리해 결과물을 만들어 제공한다

##### `*WAS가 모든 일을 처리한다면?`
- WAS가 정적 컨텐츠 요청까지 처리하면 **부하가 커지고 시간이 지연되면서 수행 속도가 느려진다**
- 사용자의 페이지 노출 시간이 늘어나는 문제가 발생할 수 있다
- 따라서 **단순한 정적 컨텐츠는 웹 서버에게 맡겨 기능을 분리하고 서버 부하를 방지한다**
<br>

### Q. DNS 조회 과정
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/413bff35-6259-428b-b467-1ec3179ac59f">

- 브라우저가 요청한 IP주소를 확인하는 과정이다
- **DNS는 Domain name과 IP address를 매핑해주는 서버다**
- DNS query : [`Root` DNS] -> [`.com` DNS] -> [`.naver` DNS] -> [`.www` DNS]
<br>

##### [조회 과정]
1. **Local DNS**에 요청한다
   - 'domain IP 주소 알고 있니?'
   - 캐시 미스가 발생하면 다음 단계 진행(DNS 서버 요청)
2. **Root DNS** 서버에 요청한다
   - 'TLD(top-level domain, 최상위 DNS server)가 알고있을거야'
3. **Authoritative DNS**(책임) 서버에 요청한다
   - 해당 IP주소를 알려준다
4. local dns는 받아온 정보를 사용자에게 알려준다 (최종)
5. 자신의 dns record에 저장하여 후에 같은 요청이 들어오면 신속한 처리가 가능하도록 한다

###### *DNS record : dns에 받은 요청을 어떻게 처리할 것인지에 대한 정보
###### *요청을 보내는 DNS는 재귀적으로 요청을 보내기 때문에 DNS 리쿼서라 지칭한다
###### *요청을 받는 DNS를 네임서버라 지칭한다
<br>

##### [왜 재귀적으로 조회할까?]
- 도메인의 계층 구조에 따라 DNS 서버도 계층화 되어있기 때문이다
- 도메인의 가장 최상단(.com, .kr ...)을 담당하는 Root DNS 서버는 전 세계 13개 뿐이다

##### [DNS query 조회 이후]
- 웹 서버의 IP주소를 알게 되었다
- HTTP request를 위해 TCP 통신을 연결한다 (3-way handshaking)
- TCP 연결 성공시 socket을 통해 HTTP request를 보내고, 이에 대한 response로 웹 페이지 정보를 받아 사용자의 PC로 출력한다
<br>

### Q. DNS 서버를 사용하는 이유
- IP 주소를 외울 필요 없이 기억하기 쉬운 domain name(문자 주소)을 사용할 수 있다
- 사용자가 새로운 IP주소를 기억할 필요 없이 웹사이트가 IP 주소를 변경할 수 있다
  - ex) www.naver.com 의 IP주소가 변경되어도 사용자는 똑같은 www.naver.com 주소로 서비스를 이용할 수 있다

*loopback IP : 본인 PC의 IP(localhost)로 DNS를 통하지 않고도 바로 자신의 PC 연결이 가능하다

***
[DNS 질문]
### Q. DNS 쿼리를 통해 얻어진 IP는 어디를 가리키고 있나요?
- DNS 쿼리를 통해 얻어진 IP 주소는 해당 도메인 이름을 가지고 있는 웹 서버의 IP 주소를 가리킨다
- 클라이언트는 해당 IP주소를 이용해 실제 서버와 통신을 수행하게 된다.
<br>

### Q. DNS 쿼리 과정에서 손실이 발생한다면, 어떻게 처리하나요?
- 클라이언트는 일정 시간동안 DNS서버로부터 응답을 받지 못하면 요청을 재시도한다
- UPD를 사용하는 경우 손실처리를 응용 계층에서 수행한다 (신뢰성 보장X)
- TCP를 사용하는 경우 TCP 자체의 재전송 매커니즘이 손실된 패킷을 처리한다
<br>

### Q. 캐싱된 DNS 쿼리가 잘못 될 수도 있습니다. 이 경우, 어떻게 에러를 보정할 수 있나요?
- 원인
   - **도메인의 IP 주소가 변경되는 경우** 변경된 정보가 캐시에 반영되지 않았기 때문이다
   - 캐사된 dns record의 **TTL(time to live)이 만료되지 않았을 때**(= 해당 기간에는 정보가 유효하다) 이전의 캐시 정보가 계속 사용될 수 있기 때문이다
- 해결
   - TTL 설정 : TTL이 만료되면 캐시된 레코드를 폐기하고 새로운 쿼리를 수행한다
   - DNS 캐시 비우기 : 특정 상황에서 캐시된 dns record를 무효화하거나 삭제해 새로운 쿼리를 유도한다
   - DNS 서버 재구성 : DNS 서버 설정을 조정한다
