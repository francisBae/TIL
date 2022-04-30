# Terraform 기본

인프런 강의 [[DevOps] AWS and 테라폼: Infrastructure as Code: 초급, 입문편](https://www.inflearn.com/course/%EB%8D%B0%EB%B8%8C%EC%98%B5%EC%8A%A4-%ED%85%8C%EB%9D%BC%ED%8F%BC-aws/dashboard)

## IaC (Infrastructure as Code) : 코드로써의 인프라

Infrastructure as Code, 즉 코드로써의 인프라는 인프라를 이루는 서버, 미들웨어 그리고 서비스 등, 인프라 구성요소들을 코드를 통해 구축하는 것.
**IaC는 코드로써의 장점, 즉 작성용이성, 재사용성, 유지보수** 등의 장점을 가진다.

## Terraform by Hashicorp

테라폼은 인프라를 만들고, 변경하고, 기록하는 IaC를 위해 만들어진 도구로써, 문법이 쉬워 비교적 다루기 쉽고 사용자가 매우 많아 참고 가능한 예제가 많다. .tf 형식의 파일 형식을 가진다.

AWS, Azure, GCP 같은 퍼블릭 클라우드뿐만이 아닌 다양한 서비스들 역시 지원한다.

## Terraform 구성 요소

- provider : 테라폼으로 생성할 인프라의 종류
- resource : 테라폼으로 실제로 생성할 인프라 자원
- state : 테라폼을 통해 생성한 자원의 상태
- output : 테라폼으로 만든 자원을 변수 형태로 state에 저장하는 것
- module : 공통적으로 활용 가능한 코드를 문자 그대로 모듈 형태로 정의하는 것을 의미
- remote : 다른 경로의 state를 참조하는 것을 의미. output 변수를 불러올 때 주로 사용

## Terraform 기본 명령어

- init : 테라폼 명령어 사용을 위해 각종 설정을 진행함
- plan : 테라폼으로 작성한 코드가 실제로 어떻게 만들어질지에 대한 예측 결과를 보여줌 (사용 빈도 높음)
- apply : 테라폼 코드로 실제 인프라를 생성하는 명령어 (실제 인프라에 영향을 끼치는 명령어이므로 주의깊게 실행해야 함. 실행 전 반드시 plan을 통해 예상 결과 확인할 것!)
- import : 이미 만들어진 자원을 테라폼 state 파일로 옮겨주는 명령어 (이미 만들어진 자원을 코드로 만들고 싶을 때)
- state : 테라폼 state를 다루는 명령어. 하위 명령어로 mv, push 등의 명령어가 있음
- destroy : 생성된 자원들을 state 파일 기준으로 모두 삭제하는 명령어

init->plan->apply 순으로 실행
