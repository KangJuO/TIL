# 1. **CI**

- Continuous Integration (지속적 통합)
- 빌드, 테스트 과정을 자동화
- 애플리케이션에 변경하거나 추가한 소스 코드를 기존의 프로젝트와 통합하여 자동으로 빌드, 테스트하는 과정
- 대표적으로 사용되는 TOOL =>Jeckins, Travis CI 등등

## **1. Work Flow**

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5be45a90-1827-4bc5-ace2-d3db72ff12d2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211029T123304Z&X-Amz-Expires=86400&X-Amz-Signature=150ee73a9a9cc1c0a98c00a8c6d0c4cebe12f5f7c85ed1ac1c6f324e1433df28&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="600" height="300"></img>

```
1. git등 형상관리 툴을 사용하여 ggithub 등의 관리 시스템에 통합
2. 통합된 코드에 대하여 자동으로 빌드, 테스트 진행
3. 카카오톡, 메일 등을 통해 통합 결과 확인 -> 수정
```

---

## **2. 장점**

- 개발의 편의성 증가
- 변경된 코드에 대한 즉각적 비드백과 검증 가능
- 소스코드의 통합과 검증에 들어가는 시간이 단축됨

---

# 2. **CD**

- Continuous Delivery, Deployment
- Delivery : 지속적 제공, 릴리즈 준비 후 검증 후 수동으로 배포
- Deploymant : 지속적 배포, 릴리즈 준비 후 자동으로 배포
- 최종단계의 자동화 여부에 따라 Delivery, Deployment로 구분

## **1. 장점**

- 개발자 입장에서 배포보다는 개발에 더욱 신경 쓸 수 있도록 해줌
