## 1. 개요

- 백엔드 포지션 면접자리에서 이런저런 기술 면접을 진행하다가 면접관님이 토큰관련된 질문을 하심
- 토큰이 뭔가, 토큰은 어떻게 사용되나 이런 식의 질문..
- 그중에 답변을 어버버하고 넘어갔던 질문이 있었는데,

> **"엑세스 토큰을 재 발급해주는 리프레시 토큰이 탈취 당하면 어쩌죠??"**

- 나의 답변은 내 프로젝트에 적용했던데로,
- "db에 저장해서 요청에 들어있는 토큰하고 비교한 다음 쓰도록하면 되지 않을까요?"
- 면접관님은 그럴경우엔 문제점이 있다며 알려주셨다. (세상 친절 ㅜㅜㅜ)
- "세션-쿠키 방법에서 인증정보를 메모리, db에 저장해서 트래픽에 의한 부하가 생기는 문제를 피하기 위해서 토큰 방법을 사용하는데 저런식으로 사용하면 토큰의 장점을 하나도 못살리게 될거같다."
- 라고 하셔서 면접 복기 차원에서 **토큰의 저장위치와 내 나름대로 최적의 방법을 찾아보려한다.**

---

## 2. 토큰 인증 방식의 과정(access token + refresh token)

<img src="https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6bc2e007-9893-46e4-9b6b-2c7fa7fbe143%2FUntitled.png&blockId=75c4cc32-1969-409e-8c59-dc6030c8a62c" width="400" height="300"></img>

### 1. 과정

- 유저가 로그인 요청에 성공 (db에서 유저확인)
- access token, refresh token 발급
- 클라이언트로 두가지 토큰 전달
- 인증이 필요한경우 access token 전달
- 인증정보가 유효한지 token 검증
- 유효하면 요청 처리 및 응답
- 유효하지 않다면 토큰만료 응답을 보냄
- 클라이언트에서 access token 재발급 요청과 함께 refresh token 전달
- refresh token 검증
- 유효 → access token 재발급
- 만료 → "너 다시 로그인해"

### 2. 내가 한 짓

- 인증이 필요할때만 토큰을 전달 받고 전달해야하지만
- refresh token을 db에 따로 저장해놓고 사용한 것
- 토큰 사용의 이점을 벗어난 사용법이 되어버렸다..

### 3. 근데 다들 디비에 많이 쓰던데..

- **쓰려면 클라이언트에는 refresh token을 보내 주지말고 서버에서만 관리하게 하자**
- 두개를 다같이 보내면 결국 탈취되면 두개가 같이 탈취됨
- access token은 짧은 유효시간을 가지고 있어 탈취되도 많은 일을 못함
- refresh token 또한 access token이 없으면 별다른 일을 하지 못한다.
- 하지만 두개가 다 같이 있으면? 만료 → 재발급 → 사용 → 만료 → 재발급...
- 과정을 계속해서 할 수 있기 때문에 같이 보내는건 금지..!
- 클라이언트가 관리한다 하더라도 결국 쿠키, 로컬스토리지인데 탈취당하는건 마찬가지
- 그렇다면 그냥 차라리 내가 관리할래..!

---

## 3. 그래서 결국 어디 저장하면 좋은가...

- 많지는 않지만 여기저기 찾아본 결과 최적의 방법은 없는것같다.. ㅠㅠ
- 지금도 위의 방법인 refresh를 db에 저장해서 사용중이다...
- 절대!! 절대 절대 탈취당하면 안될 경우 그냥 access token을 유효기간을 엄청 짧게 사용하고 refresh token의 사용은 하지 말자
- 하지만 사용자의 편리성을 위해 로그인의 지속적인 요청은 꼴보기 싫다!!! 하면
- 나같으면 그냥 세션-쿠키 방식을 사용하고 세션 클러스터링을 해줌으로서 로그인 유지, 여러 세션관리,
  트래픽 분산을 통해 부하를 막아주는 방법을 고안해 볼것같다. 물론 세션 키가 쿠키를 통해 전달되기 때문에 탈취되면 답 없는건 마찬가지.

---

> 결론 : 최적의 방법은 없다. 어느쪽에 보안을 신경 쓰느냐에 따라 저장 위치가 달라질것 같다. 사용하는 환경에 따라 개발자가 항상 (그 어려운)최선의 선택을 해야하지 않을까.
