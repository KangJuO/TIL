# **1. AWS CI/CD 구성**

- **본 게시물에서는 AWS CI/CD에 대한 설명 만을 다루고, 사용방법에 대해서는 다루지 않습니다!**

1. **Source 단계**: 원격 저장소에 관리되고 있는 소스 코드에 변경 사항이 일어날 경우, 이를 감지하고 다음 단계로 전달
2. **Build 단계**: Source 단계에서 전달받은 코드를 컴파일, 빌드, 테스트 함, Build 단계를 거쳐 생성된 결과물을 다음 단계로 전달
3. **Deploy 단계**: Build 단계로부터 전달받은 결과물을 실제 서비스에 반영하는 작업을 수행함

> **_파이프라인의 단계는 상황과 필요에 따라 더 세분화되거나 간소화될 수 있음_**

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8bab8057-d42e-436a-87df-72b96f93cf21/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211031T115718Z&X-Amz-Expires=86400&X-Amz-Signature=db405d8ce9ac808d1fe715c278e8f13cc5a6f32628f39632dce93d9c580044bb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22"></img>

---

## 1. **CodeCommit**

- Source 단계를 구성할 때 CodeCommit 서비스를 사용
- CodeCommit은 GitHub과 유사한 서비스를 제공하는 버전 관리 도구임
- CodeCommit과 GitHub의 차이점 :
  - CodeCommit - 소스코드의 유출이 치명적일 경우, 보안과 관련된 기능에 강점을 가짐**(과금 주의)**
  - GitHub - 사이드 프로젝트나 가볍게 작성한 소스 코드를 저장해야 할 경우

## 2. **CodeBuild**

- Build 단계에서는 CodeBuild 서비스를 사용
- CodeBuild 서비스를 통해 유닛 테스트, 컴파일, 빌드와 같은 빌드 단계에서 필수적으로 실행되어야 할 작업을 명령어를 통해 실행할 수 있음

## 3. **CodeDeploy**

- Deploy 단계를 구성할 때는 기본적으로 다양한 서비스를 이용 가능
- ex) S3 서비스를 통해 S3 버킷을 통해 업로드된 정적 웹 사이트에 변경 사항을 실시간으로 전달하고 반영

## 4. **CodePipeline**

- 각 단계를 연결하는 파이프라인을 구축할 때 CodePipeline 서비스를 사용

---

# **2. AWS에서 제공하는 서비스**

## **1. S3 Bucket**

### 1. **S3 Bucket 란?**

- 인터넷용 스토리지 서비스(저장소)
- Bucket, Object로 구성되어 있음
- Object(객체) : 데이터와 메타데이터를 구성하고 있는 저장 단위
- Bucket(버킷) : 객체를 저장하고 관리하는 역할

---

### 2. **특징**

- 많은 사용자가 접속을 해도 이를 감당하기 위해서 시스템적인 작업을 하지 않아도 된다.
- 저장할 수 있는 파일 수의 제한이 없다.
- 최소 1바이트에서 최대 5TB의 데이터를 저장하고 서비스 할 수 있다.
- 파일에 인증을 붙여서 무단으로 엑세스 하지 못하도록 할 수 있다.
- HTTP와 BitTorrent 프로토콜을 지원한다.
- REST, SOAP 인터페이스를 제공한다.
- 데이터를 여러 시설에서 중복으로 저장해 데이터의 손실이 발생할 경우 자동으로 복원한다.
- 버전관리 기능을 통해서 사용자에 의한 실수도 복원이 가능하다.
- 정보의 중요도에 따라서 보호 수준을 차등 할 수 있고, 이에 따라서 비용을 절감 할 수 있다. (RSS)

<참고 : [https://opentutorials.org/course/608/3006](https://opentutorials.org/course/608/3006) >

---

## **2. EC2 Instance**

### 1. **EC2 란?**

- 클라우드 컴퓨팅 서비스
- 아마존으로부터 한대의 컴퓨터를 임대 받는 것
- 자신이 선호하는 운영체제를 설치하고, 웹서비스를 위한 프로그램들(웹서버, 데이터베이스 등)을 설치해서 사용

---

### 2. **특징**

- 용량을 늘리거나 줄일 수 있다. (탄력성)
- 사용한만큼 지불하므로 저렴하다.
- 사용자가 인스턴스를 완전히 제어할 수 있다.
- **보안** 및 네트워크 구성, 스토리지 관리 효과적이다.

---

## **3. Elastic Load Balance**

### 1. **ELB 란?**

- 시스템에 가해지는 부하를 여러대의 시스템으로 분산시켜, 규모있는 시스템을 만들 수 있도록 해주는 단일 진입점

---

### 2. **특징**

- **트래픽 분산**
- 자동 확장
  - 분산이 필요한 어머어마한 트래픽을 혼자 다 감당함
  - Scale Out 아마존에서 스케일 알아서 조정??
- 인스턴스의 상태를 자동으로 감지해서 오류가 있는 시스템은 배제
  - 여러대의 컴퓨터에 연결(EC2) 되어있음
  - 그 중 한대가 장애가 발생하면 알아서 분산에서 제외시킴(회복 될때까지)
- 사용자 세션을 특정 인스턴스에 고정
  - 사용자가 특정 서버(인스턴스)접속시, 지속적으로 그쪽으로 접속 시킴
- **SSL 암호화 지원**
- **SSL의 경유지로 ELB를 사용하는 경우에 SSL 처리에 따른 부하를 ELB가 수용하게 된다.**
- IPv4, IPv6 지원
- CloudWatch를 통한 모니터링 가능

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1a220782-daf5-4e7f-b538-3fefc1678ca4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211031T115914Z&X-Amz-Expires=86400&X-Amz-Signature=0aad82ab7f333aabbab818fae93f9118ad702e55b7e76d93d4f891c22b72a4ce&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="500" height="300"></img>

- 참고자료
  - AWS : [https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#x-forwarded-for](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#x-forwarded-for)
  - 생활코딩 : [https://opentutorials.org/course/608/3008](https://opentutorials.org/course/608/3008)

---

## **4. CloudFront**

### 1. **CloudFront 란?**

- 정적, 동적, 실시간 웹사이트 컨텐츠를 사용자에서 효율적으로 전달하기 위한 서비스

---

### 2. **특징**

- 전 세계에 배치된 **엣지 로케이션(Edge Location)**을 통해 웹사이트에 컨텐츠가 전달되어 성능 향상이 목적임
- **엣지 로케이션**
  - AWS가 CDN 을 제공하기 위해서 만든 서비스인 CloudFront의 캐시 서버
  - 오리진에서 전 세계 곳곳의 캐시서버(엣지 로케이션)에 원본 콘텐츠를 복제해 둠
- CDN(Content Delivery Network)
  - 웹 페이지가 어디서 불려지는지, 불러오는 사용자가 어디에 서 불러오는지에 근거하여 웹페이지에 전달해주는 분산 네트워크
  - html, javascript, 이미지 파일들을 가지고 있는 내용물들을 가져오는 속도를 향상 시킬수 있음

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/dcbe802b-acef-4839-81b6-534a8863e737/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211031T120120Z&X-Amz-Expires=86400&X-Amz-Signature=05f207058876e29697f1bad5a44c394a6658631c9a2158d730cc37bfa4ed752a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22"></img>

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/93b99c31-fe3d-4c4c-900e-3d325c868684/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211031T120135Z&X-Amz-Expires=86400&X-Amz-Signature=4ff5d7cfae1a9291e2697421addf0a009da8d4d158029894169c5cb52fa0e207&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22"></img>

출처 : [https://velog.io/@songa29/AWS-CloudFront란](https://velog.io/@songa29/AWS-CloudFront%EB%9E%80)

---

### **3. 객체 무효화(삽질 경험에 근거함)**

- 문제점 : 클라이언트를 새롭게 배포(새로운 버전)했지만 변경사항이 하루뒤에 적용됨
- 위 내용을 바탕으로 생각을 해본다면
  - 기존 컨텐츠를 업데이트 했지만 파일이름 등이 기존과 같아서 엣지 로케이션에 동일한 파일로 간주되어 우리가 생각하는 배포가 이루어지지 않았다.
  - 배포를 했다고해서 무조건 원본을 엣지 로케이션에 재 복사를 하진않음
  - 하루 뒤에 적용?? → 캐시서버(엣지 로케이션)에 저장된 콘텐츠(이전 버전)는 수명이 있어 수명을 다해 사라지고 그 후에 다시 원본(새로운 버전)을 가져오는 듯!
- 해결하기위해 객체 무효화를 해주면 된다..!
  - 특정 파일만 무효화할지 전체를 무효화 할지는 선택사항

---

## **5. Route53**

### **1. Route53이란?**

- AWS에서 제공하는 DNS(Domain Name Server) 서비스

---

### **2. 용어**

- IP : 인터넷에 연결된 컴퓨터와 컴퓨터를 식별하는 주소 (전화번호)
- Domain : IP를 문자로 매핑한 것 (전화번호 주인 이름)
- DNS : Domain, Domain에 해당되는 IP를 가지고 있다가 도메인 주소에 대한 요청이 들어왔을 때 해당되는 IP를 알려주는 서버

---

### **3. 작동 순서**

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d277d581-dda7-4c22-a8ef-97e9f2573bc0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211031T120402Z&X-Amz-Expires=86400&X-Amz-Signature=4764222793f34611cc2ccd5263571844d052d503755f90e7decf077d34473ef3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="600" height="450"></img>

출처 : [https://velog.io/@kurikuri/AWS스터디5클라우드워치-Route-53](https://velog.io/@kurikuri/AWS%EC%8A%A4%ED%84%B0%EB%94%945%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C%EC%9B%8C%EC%B9%98-Route-53)

1. www.example.com로 사용자가 접속합니다.
2. www.example.com에 대한 요청은 인터넷 서비스 제공업체(ISP)가 관리하는 DNS 해석기로 라우팅됩니다.
3. ISP의 DNS 해석기는 www.example.com에 대한 요청을 DNS 루트 이름 서버에 전달합니다.
4. DNS 해석기는 www.example.com에 대한 요청을 .com 도메인의 TLD 이름 서버 중 하나에 다시 전달합니다. .com 도메인의 이름 서버는example.com 도메인과 연관된 4개의 Route 53 이름 서버의 이름을 사용하여 요청에 응답합니다.DNS 해석기는 4개의 Route 53 이름 서버를 캐시에 저장합니다. 다음에 누군가 example.com을 찾아볼 때 example.com의 이름 서버가 이미 있으므로 해석기는 3단계와 4단계를 건너뜁니다. 이름 서버는 일반적으로 2일 동안 캐시에 저장됩니다.
5. DNS 해석기는 Route 53 이름 서버 하나를 선택하여 www.example.com에 대한 요청을 해당 이름 서버에 전달합니다.
6. Route 53 이름 서버는 example.com 호스팅 영역에서 www.example.com 레코드를 찾아 웹 서버의 IP 주소 192.0.2.44 등 연관된 값을 받아 이 IP 주소를 DNS 해석 프로그램에 반환합니다.
7. DNS 해석기가 사용자에게 필요한 IP 주소를 해석해 냅니다. 해석기는 이 값을 웹 브라우저로 반환합니다
8. 웹 브라우저는 DNS 해석기로부터 얻은 IP 주소로 www.example.com에 대한 요청을 전송합니다. 여기가콘텐츠가 있는 곳, 예컨대 Amazon EC2 인스턴스 또는 웹 사이트 엔드포인트로 구성된 Amazon S3 버킷에서 실행되는 웹 서버입니다.
9. 192.0.2.44에 있는 웹 서버 또는 그 밖의 리소스는 www.example.com의 웹 페이지를 웹 브라우저에게 반환하고, 웹 브라우저는 이 페이지를 표시합니다.
