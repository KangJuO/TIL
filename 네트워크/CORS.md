# **1. CORS란? (Cross Origin Resource Sharing )**

- 서로 다른 도메인간에 자원을 공유하는 것을 의미하며 기본적으로 차단

- URL 구조

<img src="https://user-images.githubusercontent.com/9318975/139165297-0e528dca-9129-40e4-83c5-5242a5f899eb.png" width="400"></br>

- 프로토콜의 HTTP는 80번, HTTPS는 443번 포트를 사용, 80번과 443번 포트는 생략이 가능</br>

# **2. Origin(출처)란?**

- Protocol + Host + Port 를 합친 것</br>
- `location.origin` 를 실행해서 확인</br>

## **1. 같은 출처 VS 다른 출처**

|                 URL                  |   결과    |           이유            |
| :----------------------------------: | :-------: | :-----------------------: |
|         `https://eatmeup.me`         | 같은 출처 | Protocal, Host, Port 동일 |
|     `https://eatmeup.me/recipes`     | 같은 출처 | Protocal, Host, Port 동일 |
| `https://eatmeup.me/recipe/info/487` | 같은 출처 | Protocal, Host, Port 동일 |
|         `http://eatmeup.me`          | 다른 출처 |       Protocal 다름       |
|      `https://eatmeup.me:8888`       | 다른 출처 |         Port 다름         |
|        `https://eatmeup.com`         | 다른 출처 |         Host 다름         |

## **2. Same-Origin Policy**

- Postman으로 API를 테스트하거나, 다른 서버에서 API를 호출할 때는 멀쩡히 잘 동작,</br>
- 브라우저에서 API를 호출할 때만 CORS policy 오류가 발생</br>
- XSS나 XSRF 등의 보안 취약점을 노린 공격을 방어</br>
- 외부 리소스를 사용하기 위한 SOP의 예외 조항이 CORS</br>

## **3. CORS 동작원리**

### **1. Simple request**

- 서버에게 바로 요청을 보내는 방법

<img src="https://user-images.githubusercontent.com/9318975/139169911-e380a764-f7d8-4b76-a935-7811266f832d.png" width="400">

- 서버에 API를 요청 -> 서버는 Access-Control-Allow-Origin 헤더를 포함한 응답을 브라우저에 보냄</br>
- 브라우저는 Access-Control-Allow-Origin 헤더를 확인해서 CORS 동작을 수행할지 판단</br>

### **2.Simple request 조건**

1. 요청 메서드(method)는 GET, HEAD, POST 중 하나
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안됨
3. Content-Type 헤더는 application/x-www-form-urlencoded, multipart/form-data, text/plain 중 하나를 사용해야 함

- 첫 번째 조건은 어렵지 않은 조건이지만 2번, 3번 조건은 까다로운 조건</br>
- 2번 조건은 사용자 인증에 사용되는 Authorization 헤더도 포함되지 않아 까다로운 조건</br>
- 3번 조건은 많은 REST API들이 Content-Type으로 application/json을 사용하기 때문에 지켜지기 어려운 조건</br>

### **2. Preflight request**

- 서버에 예비 요청을 보내서 안전한지 판단한 후 본 요청을 보내는 방법</br>
  <img src="https://user-images.githubusercontent.com/9318975/139170641-24d09660-bb6a-41ae-ba2a-eb811cd876f6.png" width="400">

- OPTIONS 메서드로 서버에 예비 요청을 먼저 보내고, 서버는 이 예비 요청에 대한 응답으로 Access-Control-Allow-Origin 헤더를 포함한 응답을 브라우저에 보냄
- 브라우저는 단순 요청과 동일하게 Access-Control-Allow-Origin 헤더를 확인해서 CORS 동작을 수행할지 판단

## **4. CORS 에러 해결 방법**

- Access-Control-Allow-Origin: 특정 브라우저가 리소스에 접근이 가능하도록 허용</br>
- Access-Control-Allow-Method: 특정 HTTP Method만 리소스에 접근이 가능하도록 허용</br>
- Access-Control-Expose-Headers: 자바스크립트에서 헤더에 접근할 수 있도록 허용</br>
- credentials: 쿠키 등의 인증 정보를 전달</br>

### Node.js의 Express는 cors라는 서드 파트 미들웨어 라이브러리에서 CORS 응답 헤더를 추가해 주기 때문에, 개발자가 별도의 CORS 응답 헤더를 추가해 주지 않아도 됨

- 참고
  > https://developer.mozilla.org/ko/docs/Web/HTTP/CORS</br> > https://evan-moon.github.io/2020/05/21/about-cors/</br> > https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy</br> > https://ko.wikipedia.org/wiki/동일-출처_정책</br> > https://velog.io/@yejinh/CORS-4tk536f0db</br> > https://ko.wikipedia.org/wiki/JSONP</br> > https://velog.io/@jmkim87/지긋지긋한-CORS-파헤쳐보자</br> > https://homoefficio.github.io/2015/07/21/Cross-Origin-Resource-Sharing/</br> > https://developer.mozilla.org/ko/docs/Web/API/Request/credentials</br>
