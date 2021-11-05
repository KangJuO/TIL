## 1. OAuth란?

- Open Authentication의 줄임말
- 유저가 다른 웹사이트 상의 자신들의 정보에 대해 웹, 어플리케이션의 접근 권한을 부여할수 있는 공통적인 수단으로 사용되는 **접근 위임**을 위한 개방형 표준

---

## 2. OAuth 2.0의 구성요소

- Client : OAuth 2.0을 사용해 서드파티 로그인 기능을 구현할 어플리케이션
- Resource Owner : 서드 파티 어플리케이션에 있는 개인정보(리소스)를 이용해 Client가 제공하는 서비스를 이용하려는 사용자
- Resource Server : 사용자의 개인정보를 가지고있는 어플리케이션 서버, Client는 Token을 이서버로 넘겨 개인정보를 응답 받을 수 있음
- Authorization Server : 권한을 부여해주는 서버, 사용자는 이 서버에서 Authorization Code를 발급 받을 수 있음. Client는 이 서버로 Authorization Code을 넘겨 Token을 받급 받을 수 있음

---

## 3. Auth 2.0의 동작 과정

- 기본 : **사용자가 인증 요청**을 하면, **Client가 Authorization Server에 인가 요청**을 하고 **Authorization Server로부터 Authorization Code를 발급**받는다. **다시 Authorization Server에 Authorization Code를 제시하여 Token을 요청**하고, **발급된 Token을 통해 Resource Server에 사용자의 데이터를 요청**한다.

---

### 1. Authorization Code Grant : 권한 부여 승인 코드

- 권한 부여 승인을 위해 자체 생성한 Authorization Code를 전달 하는 방식이고 기본이 됨
- Refresh Token 사용 가능
  → 권한 부여 승인 요청 시 response_type을 code로 지정하여 요청

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1fdfcebb-2f0c-4f55-97a8-d4bd89436e44/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T090922Z&X-Amz-Expires=86400&X-Amz-Signature=4c7ba04968b15373a710dd66a1d225573fca4eb955737b659a958b56343c095b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

### 2. Implicit Grant : 암묵적 승인

- 자격증명을 안전하게 저장하기 힘든 클라이언트(JS등 스크립트 언어를 사용한 브라우저)에게 최적화된 방식
- 권한 부여 승인 코드 없이 바로 Access Token이 발급
- Refresh Token 사용이 불가능한 방식이며, 이 방식에서 권한 서버는 client_secret를 사용해 클라이언트를 인증하지 않음 → 권한 부여 승인 요청 시 response_type을 token으로 설정

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cd5bec11-268f-443c-a6b5-68355d02061d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T090941Z&X-Amz-Expires=86400&X-Amz-Signature=2d216624f626ec6da66e742264cd9046e43be0e447cb5d0fce002df74466dc5a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

### 3. **Resource Owner Password Credentials Grant : 자원 소유자 자격 증명 승인**

- 간단하게 username, password로 Access Token을 받는 방식.
- 자신의 서비스에서 제공하는 어플리케이션일 경우에만 사용되는 인증 방식. Refresh Token의 사용도 가능.
  → 권한 서버, 리소스 서버, 클라이언트가 모두 같은 시스템에 속해 있을 때만 가능

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2e7959c2-8f6f-41c3-be52-72d5e97ce15f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T090957Z&X-Amz-Expires=86400&X-Amz-Signature=c59893bc78118683c92863b6d6981157c19cbdc4eea5b4583ded18bbfd321341&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

### 4. **Client Credentials Grant** : 클라이언트 자격증명 승인

- 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있는 경우 사용.
- 이 방식은 자격증명을 안전하게 보관할 수 있는 클라이언트에서만 사용되어야 하며, Refresh Token은 사용 불가.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a5dba5d9-cede-4d6f-8d1e-aabab55bde41/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T091025Z&X-Amz-Expires=86400&X-Amz-Signature=bb718cc23a29392558901351067afedbb18f3b0b8696bc0371c39e0c99490c23&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

## 4. OAuth 2.0의 취약점

### 1. CSRF 공격을 통한 계정 탈취

- CSFR 공격으로 피해자 계정에 공격자의 계정을 연동
- authorization code를 주고받을때 state 파라미터를 넣어 서로간에 검증을 하는데, 이 state값에 대한 검증이 누락되어있거나 미흡할경우 사용자 계정을 탈취 할 수있다. => state값에 대한 정책과 사용여부, 방법에 대해 공격 가능 여부를 확인 필요

---

### 2. Covert Redirect

- OAuth 2.0 인증과정에서 `redirect_uri` 파라미터 값에 대한 검증이 누락되거나 미흡할떄 발생하는 취약점
- 피해자에게 변조된 `redirect_uri` 를 보내 로그인을 유도하고, Authorization Code가 공격자에게 전달된다.(계정 탈취)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/441fff23-0fe6-4439-927a-3fcf1148f3fe/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211105T091041Z&X-Amz-Expires=86400&X-Amz-Signature=c727d1e188350a6b8e1bda536d859264bc011a6def57fc2e702fe886c999d711&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

## 5. 기타

> OAuth 1.0 → OAuth 2.0 ???

- OAuth 1.0의 한계
  - 웹 어플리케이션만 가능
  - 절차가 복잡하여 OAuth 구현 라이브러리를 만드는 것도 어렵고 OAuth 인증을 제공해주는 서비스 측에서도 연산에 부담이 발생하는 단점
- OAuth 2.0의 특징
  - 별도 암호화가 필요없이 HTTPS를 사용
  - Access Token에 Life-Time을 지정할 수 있도록 함. (OAuth 1.0에서는 지원X)
