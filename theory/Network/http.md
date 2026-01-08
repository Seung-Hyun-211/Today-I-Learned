# [HTTP](https://ko.wikipedia.org/wiki/HTTP)

## HTTP요청

### 시작줄

구성 : 메서드 URL HTTP버전<br>
예) GET /images/logo.gif HTTP/1.1


|메서드|기능|
|-|-|
|GET|특정 데이터를 요청|
|POST|데이터를 서버로 전송|
|PUT|서버에 저장된 데이터를 변경 요청|
|DELETE|서버에 저장된 데이터를 삭제 요청|
|PATCH|서버에 저장된 데이터의 일부 변경|
|HEAD|GET과 유사, 바디를 제외하고 헤더 정보만 응답|
|OPTION|메서드나 특정 리소스에 대한 옵션 확인|
|TRACE|요청을 그대로 돌려받아 디버깅 혹은 측정에 사용|

### URI URL
URI는 URL과 URN 을 포함하는 상위 개념이다.<br>
파일 test/abc.html을 찾을 때 URL은 경로 "/test/abc.html"를 통해 abc.html를 전달하고<br>
URI는 "/qwer"이라는 식별자를 통해 abc.html을 전달하는 것이다.

- URI : Uniform Resource Identifier<br>
  자원을 문자열을 통해 식별함

- URL : Uniform Resource Locator<br>
  자원을 위치통해 식별함

### HTTP 버전

주로 1.1이 사용됨

---

### 헤더

클라 <-> 서버 간 데이터를 주고받을 때 필요한 추가정보를 제공

|헤더|설명|
|-|-|
|Host|URL에서 호스트 주소와 포트를 설정|
|User-Agent|운영체제, 웹 브라우저의 버전 등의 클라이언트 정보|
|Cookie|이전에 받은 데이터를 쿠키에 포함|
|Content-Type|요청할때 보내는 데이터의 형식|
|Content-Length|데이터의 크기|

---

### 바디

서버로 요청을 보낼때 필요한 데이터<br>
Content-Type에 따라 달라진다.

대표적으로 application/x-www-form-urlencoded, application/json, multipart/form-data 등이 있다.


## HTTP응답

### 시작줄

구성 : HTTP버전 상태코드 상태문구<br>
예) HTTP/1.1 200 OK

상태 코드와 문구는 정해져 있으며 백의 자릿수로 범주를 구분해 처리내용을 확인할 수 있다.
|코드범주|메시지|설명|
|-|-|-|
|10x|Informational(정보)|정보교환|
|20x|Success(성공)|데이터 전송이 성공적으로 이루어짐, 이해됨, 수락됨|
|30x|Redirection(방향 바뀜)|자료의 위치가 바뀜|
|40x|Client Error(클라 오류)|클라이언트측의 오류, 주소나 요청이 잘못됨|
|50x|Server Error(서버 오류)|서버측에 오류가 발생함|

---

### 헤더

|헤더|설명|
|-|-|
|Servre|서버에 대한 정보(보안을 위해선 전송되지 않도록 하는것이 좋다.)|
|Content-Type|응답할때 보내는 데이터의 형식|
|Content-Length|데이터의 크기|
|Location|300번대 응답에서 새로운 URL을 알림|
|Set-Cookie|웹브라우저에 쿠키를 저장하도록 지시|

---

### 바디

클라가 요청한 웹 페이지, 파일등을 포함<br>
데이터 형식에 따라 헤더의 Content-Type이 달라짐

|타입|설명|
|-|-|
|text/html|html문서를 의미|
|applicaiton/json|json형식의 데이터|
|image/png|png형식의 이미지|
|text/css|css스타일 시트|