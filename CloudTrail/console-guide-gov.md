
## CloudTrail > 콘솔 사용 가이드

### 시작하기
CloudTrail은 조직을 활성화하면 기본으로 제공되는 서비스입니다. 따라서 조직을 생성했다면 별도로 처리할 작업은 없습니다.
조직을 생성한 후 서비스 선택에서 CloudTrail 서비스를 선택하시면 사용할 수 있습니다.

![cloudtrail_01](https://static.toastoven.net/prod_cloudtrail/IMG01_KO.png)

### 목록

CloudTrail 서비스를 선택하면 다음과 같은 화면이 나타납니다.

![cloudtrail_02](https://static.toastoven.net/prod_cloudtrail/IMG02_KO.png)

해당 조직 내에서 사용자 이벤트가 기록되고, 이를 확인할 수 있습니다.

### 검색 및 정렬 기능

![cloudtrail_03](https://static.toastoven.net/prod_cloudtrail/IMG03_KO.png)

1. 프로젝트를 검색합니다. 이름 단위로만 검색할 수 있습니다.
2. 소스(이벤트가 발생한 장소) 단위로 검색합니다.
    - ADMIN_CONSOLE: 관리자 콘솔에서 발생한 이벤트
    - API: 시스템 내부적으로 발생한 이벤트 
        - [참고] 배치 작업이나 예약된 작업, 알람 등의 이벤트가 기록됩니다.
    - USER_CONSOLE: 조직 내부 회원 사용자 콘솔에서 발생한 이벤트
3. 서비스 단위로 검색합니다. 
    - 서비스는 여러 개 항목을 검색할 수 있습니다.
    - [참고] 서비스는 NHN Cloud 서비스 단위입니다.
4. 이벤트 단위로 검색합니다.
    - 이벤트는 여러 개 항목을 검색할 수 있습니다.
5. 사용자를 검색합니다.
6. 요청, 응답값을 검색합니다.
7. 검색 범주를 지정합니다. 24시간, 1주일, 2개월, 3개월 단위 필터가 제공되며, 직접 지정할 수도 있습니다. 
    - [참고] 이벤트는 최대 3개월간 저장됩니다.
8. 목록 표시 단위를 지정합니다. 한 화면에 몇 개의 목록을 표시할지 20, 50, 100개 단위로 설정할 수 있습니다.

---

* 정렬 기능은 시간 항목에서만 사용할 수 있습니다.

### 상세 보기 기능

![cloudtrail_04](https://static.toastoven.net/prod_cloudtrail/IMG04_KO.png)

행을 선택하면, 해당 행이 확장되면서 상세 이벤트가 나타납니다.

요청과 응답 결과를 표시해 어떠한 요청을 시도했는지 확인할 수 있습니다.

### 로그 저장/다운로드 설정
해당 조직 내에서 기록된 사용자 이벤트 로그를 외부 Object Storage를 통해 저장/다운로드 할 수 있습니다.
**로그 저장/다운로드 설정** 버튼을 클릭한 후 대화 상자에서 사용 여부를 선택합니다.

![cloudtrail_07](https://static.toastoven.net/prod_cloudtrail/IMG07_KO.png)

* 아래부터는 [NHN Cloud Object Storage](/Storage/Object%20Storage/ko/Overview/)를 이용한 로그 저장/다운로드 방법을 설명합니다.
    * **액세스 키**, **비밀 키**는 [AWS S3 API](/Storage/Object%20Storage/ko/s3-api-guide-gov/#_1)를 이용한 **EC2 자격 증명 등록 및 조회**로 확인할 수 있습니다.
    * **버킷이름**은 로그가 저장될 Object Storage 컨테이너의 이름입니다.
    * **엔드포인트**, **리전**은 로그를 저장할 Object Storage를 관리할 정보이며, [Amazon S3 호환 API 가이드 - AWS SDK](/Storage/Object%20Storage/ko/s3-api-guide-gov#aws-sdk)에서 확인할 수 있습니다.
    * 설정이 완료되면 설정한 Object Storage에 로그가 저장됩니다.
    * 저장된 로그는 2시간~2시간반 후 Object Storage에 업로드된 이후 확인 가능합니다.
    * 3회 이상 업로드에 실패하면 저장된 인증 정보가 비활성화됩니다. **결과 수신(이메일)**에 등록된 메일로 내용이 통보됩니다.
    * **외부 로그 변조 알림**은 Object Storage 안의 로그들이 임의로 수정 또는 삭제될 때, 이를 이메일로 통보하여 주는 기능입니다.
    * 외부 로그 변조 시, **결과 수신(이메일)**에 등록된 메일로 내용이 통보됩니다.
    * **외부 로그 변조 알림**은 NHN Cloud의 Object Storage에서만 제공되는 기능입니다.
        * **테넌트ID**, **비밀번호**는 로그가 저장될 Object Storage의 **API 엔드포인트 설정**에서 확인할 수 있습니다.
        * **유저ID**의 경우 해당 Object Storage에 권한이 있는 NHN Cloud 유저의 이메일을 입력합니다.
