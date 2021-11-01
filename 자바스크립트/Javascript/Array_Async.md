## 1. 개요

- 프로젝트 진행중 배열에 비동기 처리를 해야하는 부분이 있었음
- 당시에는 그냥 "아,, for문 써야하는구나" 하고 넘어 갔는데 조금 자세히 알아보려 한다.

**[참고]** : [https://velog.io/@hanameee/배열에-비동기-작업을-실시할-때-알아두면-좋을법한-이야기들](https://velog.io/@hanameee/%EB%B0%B0%EC%97%B4%EC%97%90-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%9E%91%EC%97%85%EC%9D%84-%EC%8B%A4%EC%8B%9C%ED%95%A0-%EB%95%8C-%EC%95%8C%EC%95%84%EB%91%90%EB%A9%B4-%EC%A2%8B%EC%9D%84%EB%B2%95%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0%EB%93%A4)

- 본 게시글은 위 블로그가 주제에 대한 내용이 가장 깔끔하게 정리되어있어, 해당 게시글을 제가 이해한대로 재구성한 것입니다.

---

## 2. 배열에 비동기 처리를하면 일어나는일

---

- 아래와 같은 코드의 출력을 생각해보자

```js
function test() {
  const promiseFunction = () =>
    new Promise((resolve) => setTimeout(() => resolve("result"), 1000));

  Array(10)
    .fill(0)
    .forEach(async () => {
      const result = await promiseFunction();
      console.log(result);
    });
}

test();
```

```js
// ... 1초 후
results;
results;
results;
results;
results;
results;
results;
results;
results;
results;
```

- 구현한 로직에 의하면 1초마다 "results"가 하나씩 나와야하는데 1초후에 10개가 다 같이 나온다.
- 결국 원하는 결과를 얻어 내려면 아래와 같이 for 혹은 for...of 를 사용해야한다고 함.

```js
async function test() {
  const promiseFunction = () =>
    new Promise((resolve) => setTimeout(() => resolve("result"), 1000));

  let arr = Array(10).fill(0);
  // (2) forEach 대신 for 을 사용한다
  for (let i = 0; i < arr.length; i++) {
    const result = await promiseFunction();
    console.log(result);
  }
}

test();

//혹은

function test() {
  const promiseFunction = () =>
    new Promise((resolve) => setTimeout(() => resolve("result"), 1000));

  let array = Array(10).fill(0);
  // (1) test를 async로 감싸는 대신, for문을 async 즉시실행함수로 감싸도 된다
  (async () => {
    // (2) forEach 대신 for ... of를 사용한다
    for (let element of array) {
      const result = await promiseFunction();
      console.log(result);
    }
  })();
}

test();
```

---

## 3. 왜??? 똑같은 반복 처리를하는데 결과가 다를까

---

### 1. forEach의 동작원리

- 참고한 링크 [MDN forEach](https://developer.mozilla.org/ko/docs/Web/API/NodeList/forEach), [외국형의 블로그](https://codeburst.io/javascript-async-await-with-foreach-b6ba62bbf404) 에 의하면 이렇게 동작한다고 한다.

```js
Array.prototype.forEach = function (callback) {
  // this represents our array
  for (let index = 0; index < this.length; index++) {
    // We call the callback for each entry
    callback(this[index], index, this);
  }
};
```

- 외국형은 이렇게 말한다.
- "As you can see, the`callback` is called but **we are not waiting for it to be done** before going to the next entry of the array."
- 즉, 배열의 요소에 대한 콜백함수가 실행이되고, 콜백함수의 실행완료 여부와 상관없이(종료를 기다리지않음) 다음 요소에 대한 콜백함수를 실행시켜 버린다는 것
- 그래서 예시로 들었던 코드는 순차적으로 처리를 기다리지않고 단순하게 콜백함수만 우르르 실행이 되서 1초뒤에 결과가 쏟아져 나왔던 것

---

## 4. 배열에서 비동기 처리시 두 가지 방법

### 1. 순차처리

- 배열 요소에 대해서 **차례대로** 비동기 처리, 실행순서가 보장되어야할 때 사용함.

---

### 2. 병렬처리

- 배열 요소에 대해서 **한꺼번에** 비동기 처리, 실행 순서가 중요하지 않을 때 사용
- 순서가 중요하지 않다면, 비동기처리를 병렬로 실행시키는것이 효율적이다.
- 주로 실행 순서보다 성능이 우선시 되어야 하는곳에 사용한다.

---

### 3. 병렬처리 예시

```js
const target_url = ["ur11", "ur12", "url3"];

// 다운로드에 약 1초가 걸리는 비동기 함수라고 가정
function async_download(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(url);
      resolve();
    }, 1000);
  });
}

async function parallel(array) {
  const promises = array.map((url) => async_download(url));
  await Promise.all(promises);
  console.log("all done :)");
}

parallel(target_url);
```

- 여러 URL 각각에 어떤 요청을 하고 모든 URL에 대한 처리가 끝나면 새로운 `promises` 배열에 담음.
- `Promise.all`은 Pending 상태로 있다가 `promises`의 요소가 모두 Resolve 되기를 기다린다.
- `promises`의 요소가 모두 resolve 되면 그후에 "all done :)"이 출력된다.

---

### 4. 의문

- 그렇다면 아까 `forEach`와 차이점은 뭐지??
- 겉보기엔 불다 배열 안의 요소에 대해 병렬로 처리를하는데..

```js
const target_url = ["ur11", "ur12", "url3"];

function async_download(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(url);
      resolve();
    }, 1000);
  });
}

async function parallel(array) {
  array.forEach(async (url) => {
    await async_download(url);
  });
  // all done은 언제 찍힐까?
  await console.log("all done :)");
}

parallel(target_url);
```

- 유사한 코드지만 출력 순서가 다르다.
- 앞서, `forEach`는 순서는 관심없고 단순히 콜백함수만 실행한다고 했다.
- 이때 모든 요소에 대해 콜백함수의 실행이 끝나면 `forEach`는 모든 처리를 다 했다고 보고 함수를 종료시켜 버린다.
- `forEach`가 실행한 콜백함수들은 Pending 상태로 Resolve 되지 않았지만, 모든 요소에 대해 실행이 끝났기 때문에 `forEach`는 함수를 종료시킴
- blocking, non-blocking과 연관이 있을것같은데 다음에 추가 정리해야겠다!
- 따라서 배열의 요소들에 비동기 작업을 실시한 후 (순차든, 병렬이든), 어떤 작업을 해야 한다면 `forEach`가 아닌 `map`과 `Promise.all`을 사용하는 것이 좋다.

---

## 5. 결론

- 배열의 요소들에 비동기 작업을 실시할 때는 순차처리 와 병렬처리 중 어떤 방식이 필요한지에 따라 접근법이 달라진다.
- `forEach`를 비롯한 배열의 요소에 `callback`을 실행하는 방식인 ES5 array methods (`map`, `filter`, `reduce`...) 를 사용할 땐, `callback`이 `async` 하더라도 전체 method는 `async` 하지 않음을 유의해야 한다.
- 일반적으로 순차처리는 `for` 또는 `for...of` 문을 통해, 병렬처리는 `map` + `promise.all` 을 통해 구현할 수 있다.
- 순차처리가 꼭 필요한 상황이 아니라면, 성능 상 유리한 병렬처리를 고려하자.
