## Database > RDS for MySQL > 릴리스 노트

### 2023. 11. 16.

#### 기능 추가 및 개선

* DB 인스턴스 강제 승격 기능 추가
* 이벤트 구독 시 알림 유형을 선택할 수 있도록 개선
* API v3.0 추가 및 변경
  * DB 인스턴스 백업 후 내보내기 API 추가

### 2023. 10. 31.

#### 기능 추가 및 개선

* 고가용성 구성 및 읽기 복제본 추가 시 읽기 복제본의 백업도 사용하여 생성할 수 있도록 개선
* 기존에 생성된 MySQL 5.7.33 버전 이상의 고가용성 인스턴스도 인증 플러그인과 TLS 옵션을 사용할 수 있도록 활성화 기능 추가
* MySQL 8.0.33, MySQL 8.0.34 버전 추가
* API v3.0 추가 및 변경
    * 복원될 마지막 쿼리 조회 API 추가
    * DB 인스턴스 수정하기 API 요청에 `dbVersion`, `useDummy` 필드 추가
    * DB 인스턴스 상세 보기 API 응답에 `needToApplyParameterGroup`, `needMigration`, `supportDbVersionUpgrade` 필드 추가

### 2023. 09. 26.

#### 기능 추가 및 개선

* DB 엔진 버전 업그레이드 기능 추가
    * MySQL 5.7에서 MySQL 8.0으로 DB 엔진 버전 업그레이드 시 호환성 확인을 위한 사전 점검 기능 지원
    * 더미 DB 인스턴스를 사용한 DB 엔진 버전 업그레이드 지원

### 2023. 08. 29.

#### 신규 상품 출시

- TOAST Relational Database Service (RDS) 는 Relational Database 를 클라우드 환경에서 제공하는 상품입니다.
- 복잡한 설정 없이 Relational Database 사용할 수 있습니다.
