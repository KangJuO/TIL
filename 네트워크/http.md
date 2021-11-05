## 1. 정의

---

- HTTP(Hyper Text Transfer Protocal) 서버/클라이언트 모델을 따라 데이터(이미지, html 텍스트 파일, 동영상, 음성파일)를 주고 받기 위한 프로토콜
- HTTP는 어플리케이션 레벨의 프로토콜로 TCP/IP 위에서 작동함
- HTTP는 상태를 가지고 있지 않는 Stateless 단방향 프로토콜이며 Method, Path, Version, Header, Body 등으로 구성됨

---

## 2. 트랜젝션

- 하나의 작업을 수행하기 위한 요청명령과 응답결과로 구성된 정형화 된 HTTP 메세지

---

### 1. 요청

- Method : 보통 클라이언트가 수행하고자 하는 동작을 정의한 GET, POST, OPTIONS, HEAT 등등
- Path : 프로토콜(http://), 도메인 리소스의 URL
- Headers : 서버에 대한 추가 정보를 전달하는 선택적 헤더

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/43ca9be4-bcda-40d0-bfd9-68d0e34b58c4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T134155Z&X-Amz-Expires=86400&X-Amz-Signature=158f4b9e83c4004b52cd45a11cf81beac3a99475e98d18ca434458f38964fc0f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

### 2. 응답

- Status Code : 상태코드, 요청의 성공여부와 설명을 나타냄
- Headers : 요청헤더와 비슷
- Body : 선택사항으로 가져온 리소스가 포함됨

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e2b75a8a-9e73-4ffc-a1a2-42aa6dbc18a7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T134212Z&X-Amz-Expires=86400&X-Amz-Signature=72a12c42738ee5b78882a6962da2df1a033ccb0048896366dd3def8f21aae079&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

## 3. Method

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f5f9a8e7-5bc8-4389-831a-c5e698ad2c24/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T134226Z&X-Amz-Expires=86400&X-Amz-Signature=21d1d91eb4a117330affb7ee7b7caf31a062f351954f4ef30da03b71618c2216&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 안전한 메서드 (Safe Method)
  - HTTP는 안전한 메서드라고 불리는 메서드의 집합을 정의
  - `GET`, `HEAD`는 안전하다고 정의할 수 있는데, HTTP 요청의 결과로 서버에 어떤 작용도 없기 때문
  - 안전한 메서드의 목적은, 서버에 영향을 줄 수 있는 안전하지 않은 메서드가 사용될 때 사용자에게 알려줄 수 있는 HTTP 애플리케이션을 만들도록 하는 것에 있음
- GET
  - 가장 흔하게 쓰이는 메서드
  - 주로 서버에게 리소스를 달라고 요청하기 위해 쓰임
- HEAD
  - 정확히 `GET`처럼 행동하지만, 서버는 응답으로 헤더만을 돌려줌. 엔터티 본문은 반환되지 않음
    - 리소스를 가져오지 않고도 타입 등을 알아낼 수 있음
    - 응답의 상태 코드를 통해 개체가 존재하는지 확인 가능
    - 헤더를 확인하여 리소스가 변경됐는지 검사할 수 있음
  - HTTP/1.1 준수를 위해서는 `HEAD` 메서드가 반드시 구현되어 있어야 함
- PUT
  - 서버가 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만들거나, 이미 URL이 존재한다면 본문을 사용해서 교체
  - 콘텐츠를 변경할 수 있게 해줌; 사용자에게 로그인 요구
- PATCH
  - **HTTP PATCH** 메소드는 리소스의 부분적인 수정을 할 때에 사용됩니다.
- POST
  - 서버에 입력 데이터를 전송하기 위해 설계
  - HTML 폼을 지원하기 위해 흔히 사용되며, 채워진 폼에 담긴 데이터는 서버로 전송되고 서버는 이를 모아 필요로 하는 곳에 보냄
- TRACE
  - 클라이언트에게 자신의 요청이 서버에 도달했을 때 어떻게 보이게 되는지 알려줌
  - 목적지 서버에서 ‘루프백(loopback)’ 진단을 함, 서버는 요청 메시지를 본문에 넣어 `TRACE` 응답을 되돌려줌
  - 주로 진단할 때나 프락시/타 애플리케이션 요청에 어떤 영향을 미치는지 확인하고자 할 때 사용
  - 서버의 엔터티 본문을 리턴받지 않고 서버가 받은 요청이 그대로 엔터티 본문에 들어 있음
- OPTIONS
  - 서버에게 여러 가지 종류의 지원 범위에 대해 물어봄; 특정 리소스에 어떤 메서드가 지원되는지
- DELETE
  - 서버에게 요청 URL로 지정한 리소스를 삭제할 것을 요청
  - 단, 삭제 수행 보장은 못함, 서버가 클라이언트에게 알리지 않고 요청 무시하는 것을 허용하기 때문
