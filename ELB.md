# ELB

### 확장성 (Scalability)

수직적 확장

- 자원을 추가하는 방식
- t2.micro → t2.large

수평적 확장

- 노드를 추가하는 방식
- EC2 인스턴스 개수를 늘리는 것
- 사용량에 따라 즉시 리소스를 확장 및 축소를 할 수 있기에 탄력성이 있음

### 고가용성(High Availability) vs 내결함성(Fault Tolerance)

**고가용성**

- 서비스 중단을 최소화
- AWS에서 고가용성은 2개 이상의 가용영역에 애플리케이션이나 리소스를 운영하는 것
- 내결함성 설계보다 덜 복잡하고 비용이 상대적으로 적게 발생

**내결함성**

- 서비스 중단이 없음
- 애플리케이션 계속 실행 하는 것을 목표
- 구성요소에 오류가 발생해도 계속 작동하도록 중복 하드웨어 구성
- 중복된 리소스 구성으로 구성이 복잡하고 비용이 많이 발생

### Load Balancer

네트워크 트래픽을 분산하는 서비스

- 네트워크 트래픽을 EC2 인스턴스, 컨테이너, IP 주소 등 여러 대상으로 자동 분산 가능
- 애플리케이션의 가용성과 내구성을 높일 수 있음
- 로드 밸런스가 비정상 대상을 감지하면 해당 대상으로 트래픽 라우팅을 중단하고 정상대상으로만 트래픽 라우팅
- 대상이 다시 정상으로 감지되면 트래픽을 해당 대상으로 다시 라우팅 가능

ELB 종류

Application Load Balancer

- Layer 7
- HTTP, HTTPS
- HTTP Header Content를 상뇽해 라우팅 요청 처리
- 웹 애플리케이션

Network Load Balancer

- Layer 4
- TCP< UDP< TLS
- Protocol, Port Number를 사용해 라우팅 요청 처리
- 수백만의 대용량 트래픽 처리에 적합

Gateway Load Balancer

- Layer 3 - Gateway Load Balancer Endpoint
- Layer 4 - Gateway Load Balancer
- GENEVE protocol을 사용하여 encapsulation 트래픽 전송
- Transparency한 네트워크 게이트웨이를 제공하므로 보안 검사를 위한 방화벽, IPS, IDS 등의 원본 패킷의 데이터가 중요한 가상 어플라이언스에 적합

Classic Load Balancer

- Layer 4 , Layer 7
- HTTP, HTTPS, TCP, TLS
- Protocol, Port Number를
  사용해 라우팅 요청 처리
- 이전 세대 EC2-Classic 네
  트워크에서 사용

Elastic Load Balancer 구성

- Listener

  - 구성한 프로토콜 및 포트를 사용해 연결 요청을 확인하는 기능
  - 리스너에서 정의한 규칙에 따라 로드밸런서가 대상 그룹에서 대상으로 라우팅하는 방법이 결정됨
  - 클라이언트와 대상간의 연결을 위한 프로토콜 및 포트 번호로 구성

- Target Group
  - 대상(Target)의 모임
  - Target
    ✓ EC2 인스턴스
    ✓ EC2 Auto Scaling Group
    ✓ IP address
    ✓ Lambda
    ✓ Application Load Balancer

### ALB

- HTTP, HTTPS(보안 HTTP) 프로토콜을 사용하는 애플리케이션 로드 밸런싱에 사용
- 리스너 프로토콜은 HTTP, HTTPS를 사용 할 수 있음
- 웹 애플리케이션 처리에 적합
- 리스너 규칙을 기반으로 라우팅 설정이 가능함
- 데이터 전송 보안을 위한 HTTPS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함
- 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능
- Application Load Balancer를 AWS Web Application Firewall(WAF)과 통합하여 로드 밸런서 뒤쪽의 웹 애플리케이션을 웹 공격으로부터 보호 가능

Listener 규칙

host-header(호스트 헤더)

- 각 요청의 호스트 이름을 기반으로 라우팅 (예, [aaa.example.com](http://aaa.example.com/) 또는 [ddd.example.com](http://ddd.example.com/))

path-pattern(경로)

- 요청의 URL의 경로 패턴을 기반으로 라우팅 (예, [www.example.com/audio](http://www.example.com/audio) 또는 [www.example.com/video](http://www.example.com/video))

http-header(HTTP 헤더)

- 각 요청의 HTTP 헤더를 기반으로 라우팅

http-request-method(HTTP 요청 메서드)

- 각 요청의 HTTP 요청 매서드를 기반으로 라우팅

query-string(쿼리 문자열)

- 쿼리 문자열의키/값페어또는값을기반으로라우팅

source-ip(소스 IP)

- 각 요청의소스IP주소를기반으로라우팅

### Network Load Balancer

- TCP, UDP, TLS 요청을 로드밸런싱 해야 하는 경우에 사용
- 리스너 프로토콜은 TCP, TCP_UDP, UDP, TLS를 사용 할 수 있음
- 초당 수백만 개의 요청과 갑작스럽고 변동성이 높은 트래픽 패턴을 처리하도록 설계되었으며 대기 시간이 매
  우 짧음
- 고도의 성능이 요구되거나 대기 시간이 낮아야 하는 애플리케이션에 적합
- 게임등의수백만의동시사용자처리에적합
- 고정IP주소할당가능
- 클라이언트 IP 주소 전달 가능 (Source IP Preservation)
- ALB처럼 리스너 규칙 설정 없음
- 데이터 전송보안을 위한 TLS 프로토콜 사용시 SSL/TLS 인증서를 배포해야 함
- 인증서는 ACM(AWS Certificate Manager)사용 또는 클라이언트 인증서 사용 가능

### **Gateway Load Balancer**

방화벽,침입탐지및방지시스템,심층패킷검사시스템과같은가상어플라이언스를배포,확장및관리
