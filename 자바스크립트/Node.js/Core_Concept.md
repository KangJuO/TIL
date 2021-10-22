![nodelogo](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b7af66aa-1879-4f47-8bfb-93c9dec80443/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T092112Z&X-Amz-Expires=86400&X-Amz-Signature=38010910198ab3987ebf501ad5318883a314d82707b41661b8caa24928d16aba&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

---

# **1. Node.js 정의**

- Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임, 이벤트 기반의 플랫폼
- 런타임 이란?
  - 프로그래밍 언어가 구동되는 환경
  - 예전에는 JavaScript 런타임이 브라우저만 존재했었음

---

# **2. Core Concept**

## **1. Blocking, Non-Blocking, Synchronous, Asynchronous**

### **1.용어**

- **Blocking**
  - 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 다른 작업이 끝날때까지 기다렸다가 자신의 작업을 시작하는것
      <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9c0ad23b-c0d0-4c65-885e-63f75df40c06/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T124154Z&X-Amz-Expires=86400&X-Amz-Signature=b9307b957beb1e9221a8deb0cc403d493eb972bf133063bec1714ce3e032a8b6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="300" height="300"></div>

---

- **Non-Blocking**
  - 다른 주체의 작업의 시작에 상관없이 자신의 작업을 하는것
      <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5560f788-4fc0-4e10-bf85-76cd7d68ab6d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T124234Z&X-Amz-Expires=86400&X-Amz-Signature=fe9e3b8945b95ba82e991d8be432ab7b6a6eb596153039489fd0a4c994d1eef8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="300" height="300"></div>

---

- **Synchronous(동기)**
  - 작업을 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작
  - 다른 주체의 작업 완료 후 반환을 기다리거나 반환 되더라도 작업 완료 여부를 확인 해야함  
    → 리턴값으로 어떤 작업을 수행 해야함.
    <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9ba4054d-490c-4985-b13f-338a8bbd3052/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T124255Z&X-Amz-Expires=86400&X-Amz-Signature=710ff96b6d690ce5ed9865b5dfb5e71e615de95888e10495e1fc96edfad63935&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="300" height="300"></div>

---

- **Asynchronous(비동기)**
  - 시작, 종료가 일치하지 않으며, 끝나는 동시에 시작하지 않음
  - 다른 주체가 작업을 시작할때 callback을 전달 해당 주체가 작업이 끝나면 callback을 실행,  
    자신이 다른 주체의 작업 완료 여부에 관심이 없음  
    → 리턴 값이 없을 수도 있음(callback으로 처리하기 때문) 있어도 처리 할 수도 안 할 수도 있음
    <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6bbd5cee-4edb-49f0-8be6-7292f13d8165/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T124502Z&X-Amz-Expires=86400&X-Amz-Signature=42a4f7702b901b4efb9e1f0daff17a1a7d3396d8f7d9bdbe1b5b442adb2442c7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="300" height="300"></div>

---

### **2. Blocking VS Non-Blocking, Synchronous VS Asynchronous**

- Blocking VS Non-Blocking
  - 다른 주체가 작업할 때 자신의 제어권에 초점을 둠  
    있다면 - Non-Blocking(**바로 반환되어 자신의 일을 수행**)  
    없다면 - Blocking(**반환을 기다려야함**)
- Synchronous VS Asynchronous
  - 다른 주체의 작업완료 여부에 초점을 둠  
    완료 여부가 중요 - Synchronous(**결과값으로 뭔가를 실행해야함**)  
    안 중요 - Asynchronous(**결과값으로 뭘 할 수도 있지만, 안할수도 있고, 내 할 일 할수도 있음**)

---

### **3. Node.js 는?**

- Node.js는 Non-Blocking, Asynchronous 방식
  - Node.js 표준 라이브러리의 모든 **I/O(시스템 디스크나 네트워크와 상호작용하는 것) 메서드는 논블로킹인 비동기 방식을 제공하고 콜백 함수를 받음**.  
    일부 메서드는 같은 작업을 하는 블로킹 메서드도 가지는데 이름 마지막에 Sync가 붙음. (공식 문서)

---

## **2. Node.js 이벤트 루프**

### **1. 용어**

- 프로세스, 스레드
  - **프로세스** : 운영체제에서 실행 중인 하나의 어플리케이션
    (ex : 크롬 브라우저를 두개 실행 → 두개의 프로세스 생성)
  - **스레드** : 한 가지 작업을 실행하기 위한 **코드 흐름의 단위**  
  (ex : 프로세스 하나에 스레드가 두개 → 코드가 실행되는 흐름이 두개)
  <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ce139fcd-d255-4afb-bae8-6864d9d79b91/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T125728Z&X-Amz-Expires=86400&X-Amz-Signature=99c13ffa4228c188f9439e533d352683745f70d5f21c58672d25faede16d0df1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="700" height="300"></div>

---

- 메모리 힙(Memory Heap), 콜 스택(Call Stack)
  - 메모리 힙 : 참조타입(객체 등등) 데이터가 저장됨
  - 콜 스택 : 원시타입(숫자 등) 데이터가 저장됨  
  실행 컨텍스트를 통해 변수이름저장, 스코프 체인, THIS관리, 코드실행순서 관리 등등을 함
    <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/45070265-ca8a-418e-b61f-fc229e09b0c2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T125931Z&X-Amz-Expires=86400&X-Amz-Signature=013c888f8dcb838c152efd283bcf71869c03bb78b7e5e8230c215ebddbe7c0a1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="400" height="350"></div>

---

- libuv 라이브러리
  - 비동기 작업을 처리하는 라이브러리

---

### **2. 이벤트 루프**

> **이벤트 루프의 사용**

- Javascript - 싱글 스레드 프로그래밍 언어 → 하나의 콜 스택(한번에 하나의 작업만 할 수 있음)
- Node.js는 Non-Blocking, Asynchronous 방식 → 여러 개의 처리를 같이 할수 있음
- 아래의 코드를 노드를 통해 작동시키면 결과는 다음과 같음

```js
function main() {
  console.log("맥북");

  setTimeout(function () {
    console.log("프로");
  }, 2000);

  console.log("새로나옴");
}

main();
```

```js
// 출력
"맥북";
"새로나옴";
"프로";
```

- setTimeout 함수가 사라졌다가 콜백 함수인 console.log("프로")가 콜스택에 나타나게 됨
    <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4ca3ff74-12dc-4c1e-9b47-26a633e15976/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB_%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8_2021-10-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.50.18.mov.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T130527Z&X-Amz-Expires=86400&X-Amz-Signature=b25d92e333a2fefb28a7856d2ac419c2e1b3976e1dccb25735b5d65b6811a3fa&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2592%25E1%2585%25AA%25E1%2584%2586%25E1%2585%25A7%25E1%2586%25AB%2520%25E1%2584%2580%25E1%2585%25B5%25E1%2584%2585%25E1%2585%25A9%25E1%2586%25A8%25202021-10-20%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25205.50.18.mov.gif%22" width="400" height="350"></img>

- setTimeout은 어디론가 가서 함수가 실행이된 후, 콜백 함수가 콜스택에 나타나게 됨
- Javascript는 분명히 싱글 스레드인데 함수가 동시에 처리가 되는 모습을 보여줌
- 동시성에 대한 처리 → 자바스크립트 엔진을 구동하는 환경, 즉 브라우저나 Node.js가 담당

---

> **Node의 비동기 처리 과정**

- 전체 처리과정
    <div><img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7d280b2e-d832-4350-8795-28a3fab4244c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T130828Z&X-Amz-Expires=86400&X-Amz-Signature=fcac2341f8285c2f9063e3654e39e191c15845e08e61f620af840d0bb66205f1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="800" height="350"></img>
- 비동기 처리가 필요하게 될 경우 Node api를 통해 libuv 라이브러리에서 제공하는 비동기 처리를 하게됨
- 이때 libuv 라이브러리에서 제공하는게 **이벤트 루프**
- 이벤트 루프는 여러 단계(Event Queue)가 있고, 각각의 단계별로 설계된 역할을 수행함!  
  (그 외 외부 API호출(DB 데이터 호출 등)은 커널 혹은 자신의 워커 스레드가 수행함)
- **이 이벤트 루프가 싱글 스레드이고 실제로 Javascript를 처리할 수 있는 메인 스레드라서 Node.js를 싱글스레드 라고한다.**
- libuv는 커널에서 어떤 비동기 처리를 지원하는지 알고 있기 때문에 그런 작업을 받으면 커널의 비동기 함수들을 호출함. 작업이 완료되면 시스템 콜을 libuv에 전달하고 다시 이벤트 루프에 콜백으로 등록됨.
- 커널이 지원하지 않는 작업들은 워커 쓰레드가 수행하게 되고 이때 libuv의 스레드가 사용된다.
