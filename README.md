# study_http

나중에 다시 마크업 형식 + 부족한 내용 보충으로 정리할 것. <인프런의 김영한 님의 강의 보며 정리한 내용>.

http

* IP(인터넷 프로토콜)?
지정한 ip에 데이터 전달 가능
패킷이라는 통신 단위로 전송

- IP패킷 정보
출발지, 목적지, 기타...

* TCP / UDP 
- 인터넷 프로토콜 스택의 4계층
애플리케이션 계층 - HTTP, FTP
전송 계층 - TCP, UDP
인터넷 계층 -  IP
네트워크 인터페이스 계층 

- TCP특징
IP만으로는 부족했던 부분을 TCP의 개념으로 극복.
특징: 연결지향, 데이터 보증, 순서 보장 등등

TCP 3 way handshake

- UDP특징
TCP같은 특징은 아니지만 단순하고 빠름. 단, PORT가 있음. 등등

* PORT
같은 IP내에서 프로세스 구분.

예) ip는 아파트 동, port는 호수

 0 ~ 65535 할당 가능
0 ~ 1023 잘 알려진 포트
예) http: 80/ https:443 / FTP:20,21

* DNS(Domain Name System)
예) google.xx.xx 200.200.200.2

DNS서버에 도메인 명과 IP를 등록 하고, 도메인 명으로 아이피를 찾아서 알려주는 방식
IP가 변경되어도 문제 없음.

* URI(Uniform Resource Identifier)
공식문서 설명 "URI는 로케이터(Resource Locator), 이름 (Resource Name)또는 둘다 추가로 분류될 수 있다."
URI = URL(Locator) + URN(Name)

- 로케이터 : 리소스가 있는 위치를 지정
- 이름: 리소스에 이름을 부여

위치는 변할 수 있지만, 이름은 변하지 않는다. URN은 사실 잘 사용되지 않음. 따라서 URL과 URI와 같은 의미로 사용.

- URL전체문법 (예: https://www.google.com:443/search?q=hello)
- scheme://[userinfo@]host[:port][/path][?quertuy][#fragment] 
: 프로토콜 (https, http secure)
: 호스트명 (www.google.com)
: 포트번호 (443)
: 패스 (/search)
: 쿼리 파라미터  (q=hello)


* 웹 브라우저의 요청 흐름
1. DNS조회
2. 포트정보확인
3. 웹브라우저가 HTTP요청 메세지 생성
예) GET /search......

4. SOCKET라이브러리를 통해 전달
- A: TCP/IP 연결(IP, PORT)
- B: 데이터 전달

5. TCP/IP 패킷 생성, HTTP메세지 포함
6. 네트워크 인터페이스(랜 드라이버, 장비 등)에서 전송
7. 응답 메세지 받음 (http 응답 메세지.. 예를들어 결과 코드 200)
8. 웹 브라우즈 HTML랜더링 (요청 결과 보여줌)

* HTTP(Hyper Text Transfer Protocal)
- 거의 모든 데이터를 HTTP프로토콜 통신을 한다.

 HTTP/1.1 가장 많이 사용
현재, HTTP/3진행중, TCP대신 UTP사용. 성능 개선

- 기반 프로토콜
TCP: HTTP1/.1,  HTTP/2
UDT: HTTP/3

- 특징
클라이언트-서버 구조, 무상태 프로토콜(스테이스리스), 비연결, HTTP메세지, 단순하고 확장가능. 
- HTTP 메세지와 구조
2가지가 있다. 요청 메세지 응답 메세지
시작라인(start-line) - 헤더(header) - 공백라인(CRLF) - 바디(message body)

** 공식 스펙 참조
https://tools.ietf.org/html/rfc7230#section-3 
HTTP-message   = start-line
                      *( header-field CRLF )
                      CRLF
                      [ message-body ]

- 시작라인(start-line)
:request-line(요청메세지의 경우) / status-line(응답 메세지의 경우)
:요청메세지의 경우 시작라인 종류: GET POST PUT DELETE...
:응답메세지의 경우 http상태코드, 200(성공) 400(클라이언트요청오류) 500(서버내부오류)

- 헤더
:http전송에 필요한 부가정보가 포함되어 있다. Content/Type. Content-Length, 브라우저정보, 압축, 인증요청, 캐쉬정보등등..너무많음

- 메세지 바디
실제 전송할 데이터가 들어가 있다. 바이트로 표현할 수 있는 모든 데이터가 들어가있다.

- HTTP 메소드 
:GET - 리소스 조회(쿼리 파라미터, 쿼리 스트링을 통해서 전달)
:POST - 요청 데이터 처리, 주로 등록에 사용 (메세지 바디를 통해 전달)
:PUT - 리소스를 대체, 해당 리소스가 없으면 생성
:PATCH - 리소스 부분 변경
:DELETE - 리소스 삭제
HEAD: GET과 동일하지만 메세지 부분을 제외하고 상태줄과 헤더만 반환
OPTIONS, CONNECT등등 있음

- HTTP 메소드의 속성
안전(safe methods), 멱등(Idempotent methods), 캐시가능(cacheable methods)

안전: 호출해도 리소스를 변경하지 않는다.
멱등: 한 번 호출하든 여러번 호출하든 항상 똑같다.
캐쉬: 응답결과 리소스를 캐쉬해서 사용. (실제로 GET, HEAD정도만 캐쉬로 가능. POST의 경우, 고려할 것이 많아서 잘 사용하지않음.)

- Form 데이터 전송 (GET, POST만 가능)
form의 내용을 메세지 바디를 통해서 전송 (key==value, 쿼리 파라미터 형식)
:파일 전송의 경우, Content-Type: multipart/form-data; boundary=---xxx (바이너리 데이터 전송시)
:POST전송의 경우, Content-Type: application/x-www-form-urlencoded (url encoding처리)

- HTTP API 설계 (URI)
:계층 구조로 작성할 것.
:리소스와 행위를 분리. URI는 리소스만 식별! (리소스만으로 작성하는 게 이상적이지만, 실제로는 힘들다.. 동사로도 URI사용하자!)

예시1)
:회원 목록 /members -> get
:회원 등록 /members -> post (클라이언트가 리소스 URI를 모른다. id는 서버쪽에서 생성)
:회원 조회 /members/{id} -> get
:회원 수정 /members/{id} -> patch, put, post  
:회원 삭제 /members/{id} -> delete

회원등록 폼 /members/new -> get
회원등록 /members/new -> post
회원수정폼 /members/{id}/edit -> get
회원수정 /members/{id}/edit -> post
-> 리소스만으로, get post만으로는 제약이 있기 때문에 컨트롤URI를 사용(new edit delete, 동사를 사용한다). 

예시2-put기반)
:파일 목록 /files -> get
:파일 조회 /files/{filename} -> get :파일 등록 /files/{filename} -> put (클라이언트가 리소스 URI를 알고 있어야 한다. /files/abc.jpg)
:파일 삭제 /files/{filename} -> delete
:파일 대량 등록 /files -> post

https://restfulapi.net/resource-naming 참조


- HTTP 상태코드
1xx(Infomational):  요청이 수신되어 처리중
2xx(Successful): 요청 정상 처리
3xx(Redirection): 요청을 완료하려면 추가 행동이 필요함
 -> 리다이렉션: Location 헤더에 url이 포함된 경우, 거기로 이동하는 것. 
4xx(Client Error): 클라이언트 오류, 잘못된 문법으로 서버가 실행할 수 없음.
5xx:(Server Error): 서버 오류 


- HTTP 헤더 (쿠키, 세션 위주로..)
:개요
HTTP전송에 필요한 부가정보 (너무 많음)

- 쿠키
:쿠키 기본 동작

- 캐시
:캐시 기본 동작

:검증헤더와 조건부 요청1 -> if-modified-since , last modified 데이터의 최종 수정일로 다시 받을지 확인. (last modified가 검증헤더, 서버에서 작성. if-modified-since가 조건부 요청, 클라이언트 작성
)
 단점 있음.

:검증헤더와 조건부 요청2
 if-modified-since , last modified단점을 보안한 방법.
ETag(고유한 버전을 사용. 해쉬를 사용.)

- 프록시 캐시서버

- 캐시 무효화 (확실한 방법)
Cache-Control: no-cache, no-store, must-revalidate 전부 설정
Pragma: no-cache(HTTP 1.0 하위 호환)


 




 

