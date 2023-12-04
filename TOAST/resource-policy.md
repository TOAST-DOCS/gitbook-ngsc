## NHN Cloud > 리소스 제공 정책 
NHN Cloud는 모든 고객에게 안정적인 서비스를 제공하고, 의도치 않은 리소스 생성으로 인해 발생할 수 있는 지출 등으로부터 사용자를 보호하기 위해 리소스 제공 정책을 아래와 같이 적용합니다.

### 조직/프로젝트 리소스 제공 정책
조직의 리소스는 결제 수단을 등록한 회원을 기준으로 계산되며, 프로젝트는 조직을 기준으로 계산됩니다.

|리소스 | 제공 기준 | 제공량 | 
|----|----|----|
|조직	| 결제 수단을 등록한 회원당 |3개|
|프로젝트	 | 조직당 |5개|

### 인프라 서비스 리소스 제공 정책 
리소스 사용량은 프로젝트별로 계산되며, 리전별로 구분하여 정책이 적용됩니다. 

|리소스 | 제공 기준 | 제공량 | 
|----|----|----|
|CPU	| 프로젝트당 |100vCore|
|메모리	 | 프로젝트당 |256GB|
| 키 페어 | 프로젝트당 | 100개 |
|Block Storage| 프로젝트당 |10TB|
|볼륨당 최대 크기| volume당 |2048GB| 
|Floating IP | 프로젝트당 |50개|
|VPC | 프로젝트당 |3개|
|서브넷 | VPC당 |10개|
|라우팅 테이블 | VPC당 |10개|
|라우트 | 라우팅 테이블당 |10개|
|정적 라우트 | 서브넷당 | 20개 |
|리전 피어링 | VPC당 |10개| 
|프로젝트 피어링 | 프로젝트당 |10개| 
|인터넷 게이트웨이 | 프로젝트당	|3개|
|NAT 게이트웨이 | 프로젝트당 | 3개 | 
|VPN 게이트웨이(Site-to-Site VPN) | VPC당 | 1개 | 
|VPN 게이트웨이(Site-to-Site VPN) 연결 | 서브넷당 | 1개 | 
|서비스 게이트웨이 | VPC당 | 10개 | 
| Network Interface | 프로젝트당 | 500개 | 
| Network ACL | 프로젝트당 | 10개 | 
| Network ACL 정책 | 프로젝트당 | 100개 | 
| Network ACL 바인딩 | 프로젝트당 | 100개 | 
|Load Balancer | 프로젝트당 |10개|
|IP 접근 제어 그룹	| 프로젝트당   |10개|
|IP 접근 제어 대상 | IP 접근 제어 그룹당	|1000개|

### NHN Kubernetes Service(NKS) 리소스 제공 정책
리소스 사용량은 프로젝트별로 계산되며, 리전별로 구분하여 정책이 적용됩니다.

|리소스 | 제공 기준 | 제공량 | 
|----|----|----|
|클러스터	| 프로젝트당 |3개|
|워커 노드 그룹	 | 클러스터당 |3개(기본 워커 노드 그룹 포함)|
|워커 노드 수	 | 워커 노드 그룹당 |10개|

### DNS Plus 서비스 리소스 제공 정책
리소스 사용량은 프로젝트별로 계산됩니다.

#### DNS
|리소스 | 제공 기준 | 제공량 | 
|----|----|----|
|레코드 세트	| DNS Zone당 |5,000개|

#### GSLB
|리소스 | 제공 기준 | 제공량 | 
|----|----|----|
|GSLB	| 프로젝트당 | 20개|
|Pool	| 프로젝트당 | 20개 |
|Pool   | GSLB당    | 16개 |
|엔드포인트 | 프로젝트당 | 20개 |
|엔드포인트 | Pool당 | 5개 |
|헬스 체크	| 프로젝트당 | 5개 |

### KakaoTalk Bizmessage 서비스 리소스 제공 정책
| 리소스 | 제공 기준 | 제공량 | 
| --- | --- | --- |
| 알림톡 발송 건수 | 카카오톡 채널당 1일 | 1,000건 | 
| 친구톡 발송 건수 | 카카오톡 채널당 1일 | 1,000건 | 

### API Gateway 서비스 리소스 제공 정책 
리소스 사용량은 프로젝트별로 계산되며, 리전별로 구분하여 정책이 적용됩니다.

| 리소스 | 제공 기준 | 제공량 |
| --- | :---: | :---: |
| API Gateway 서비스 | 프로젝트당 | 10개 |
| 스테이지 | API Gateway 서비스당 | 10개 |
| 리소스 메서드 | API Gateway 서비스당 | 100개 |

### Log & Crash Search 서비스 리소스 제공 정책 
리소스 사용량은 프로젝트별로 계산됩니다.

| 리소스 | 제공 기준 | 제공량 |
| --- | :---: | :---: |
| 로그(일반 로그, 크래시 로그) 건 수 | 1일 | 20,000,000 건 |
| 로그(일반 로그, 크래시 로그) 크기 | 1건 | 8MB |

### 리소스 제공량 증설 요청  
제공 정책의 사용량 증설을 원하는 경우 NHN Cloud 고객 센터 [1:1문의](https://www.toast.com/kr/support/inquiry)로 문의하시면 됩니다. 
요청 시 원하는 항목과 양을 기재하시면 상담이 수월하게 이루어질 수 있습니다. 

요청 후 처리되기까지는 2~5일 정도 소요되므로 실제 필요한 시점보다 미리 신청하시는 것을 권장합니다.