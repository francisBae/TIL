# AWS 서버리스 백엔드 구성

참고 강의 : [아마존 클라우드 무료계정으로 시작하는 서버리스 애플리케이션 프로젝트](https://www.inflearn.com/course/AWS-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4-%EC%9B%B9%EC%95%B1/dashboard)

<br>

서버리스가 추구하고자 하는 바

개발자가 신경 쓰던 OS, server, network, maintenance 등을 aws에 일임하고 개발자는 코드와 품질에 집중할 수 있도록 하는 것

<br>

## 서버리스 백엔드 만들기

<br>

<img src="https://d1.awsstatic.com/diagrams/Serverless_Architecture.5434f715486a0bdd5786cd1c084cd96efa82438f.png" width="450px">

이미지 출처 : [서버리스 웹 애플리케이션 구축](https://aws.amazon.com/ko/getting-started/hands-on/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)

<br>

### 1. Aws의 lambda function 생성

- 생성 시 실행역할 설정
  - 권한 미생성한 상태라면 ‘기본 Lambda 권한을 가진 새 역할 생성’
  - 기존 생성 역할 있을 때는 ‘기존 역할 사용’ 선택
  - Cloudwatch 서비스에서 호출 이력 확인

<br>

### 2. DB 연결(DynamoDB)

- aws에서 DynamoDB 서비스 검색 후 ‘테이블 만들기’
- 테이블 생성 후 테스트를 위한 데이터 (항목) 만들기
- Lambda function을 dynamo db에 맞게 구현 (하기 링크 참조)
- https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html
  - => 각 언어 별 sdk 참고하여 호출하기
  - 호출 시 api version 명시하는 것이 좋음 (명시하지 않는 경우 버전 차이로 에러 발생하기도 함)
- 최초 호출 시 호출은 성공해도 500코드(에러) 리턴될 수 있음
  - => IAM 으로 접근 권한 생성해줄 것

<br>

### 3. IAM(Identity and Access Management) 접근권한 생성

- 최초 lambda 함수 생성 시 ‘기본 Lambda 권한을 가진 새 역할 생성’ 선택 시 자동으로 역할 만들어짐. 그러나 충분한 역할 권한이 없어 기능 호출 시 에러 발생 가능하다. (역할 추가해야 함)
- Amazon CloudWatch Logs에 로그를 업로드할 수 있는 권한이 필요함(CloudWatch 서비스의 로그 그룹에서 로그 확인 가능)
- IAM 서비스 - 역할에서 역할 선택 시 해당 역할에 매핑된 정책(사용 가능한 정책 권한들)이 표시되어 있음
- 기존 정책 선택해도 되고, 보안성 등을 높이기 위해 임의 정책 생성 가능
- 정책 - 정책 생성 통해 필요한 권한들 등록
- ARN은 테이블로 이동 (DynamoDB 서비스)하여 테이블-{테이블명} 클릭해서 확인
- 역할을 만들고 (사용할 서비스 : Lambda 선택) 권한 정책은 위에서 만든 정책 연결
- Lambda 함수로 가서 기존 역할을 새로 만든 역할로 대체

<br>

### 4. API Gateway

- API의 구조를 정의한다 (host, path, method, body, header 등)
- aws의 API Gateway 서비스 검색 후 시작 버튼 클릭.
- REST API, 새 API 선택, 엔드포인트는 서울지역만 쓰면 ‘지역’, 전세계 대상이면 ‘최적화된 엣지’, 사내망 등이면 ‘프라이빗’
- 리소스 메뉴에서 작업-리소스 생성 (디렉토리 구조처럼 리소스 밑에 리소스 생성 가능)
- 리소스 메뉴에서 작업-메소드 생성하고 dropdown 메뉴에서 get, put 등 선택
- 통합유형 : Lambda 함수
- ‘Lambda 프록시 통합 사용’ 체크 (API 가 받은 정보 그대로 이벤트 객체 전달). 체크 해제 시 api로 들어온 것을 매핑해서 전달하는 작업 진행해야 함
- Lambda 함수는 신규 생성한 함수 매핑
- 테스트 진행 후 API 배포 진행
- 작업-API배포 (배포 후 호스트 url 획득 가능)
- 스테이지 - CloudWatch 관련 설정 시 CloudWatch Log 역할이 셋팅되어 있어야 한다는 에러 발생
  - IAM에서 API 게이트웨이를 위한 역할 생성 (사용할 서비스 : API Gateway)
  - aws에서 제공하는 로그 관련 정책 매핑
  - API Gateway 설정 시 역할 ARN 필요 (IAM - 생성한 API 게이트웨이의 역할 ARN 복사해서 API 게이트웨이 설정에 붙여넣기)
