## Database > RDS for MySQL > API 가이드

| 리전        | 엔드포인트                                         |
|-----------|-----------------------------------------------|
| 한국(판교) 리전 | https://kr1-rds-mysql.api.nhncloudservice.com |
| 한국(평촌) 리전 | https://kr2-rds-mysql.api.nhncloudservice.com |
| 일본 리전     | https://jp1-rds-mysql.api.nhncloudservice.com |

## 인증 및 권한

API를 사용하려면 인증에 필요한 `User Access Key ID`와 `Secret Access Key`가 필요합니다. <b>회원 정보 > API 보안 설정</b>에서 생성할 수 있습니다.
생성된 Key는 Appkey와 함께 요청 Header에 포함해야 합니다.

| 이름                         | 종류     | 형식     | 필수 | 설명                               |
|----------------------------|--------|--------|----|----------------------------------|
| X-TC-APP-KEY               | Header | String | O  | RDS for MySQL 서비스의 Appkey        |
| X-TC-AUTHENTICATION-ID     | Header | String | O  | API 보안 설정 메뉴의 User Access Key ID |
| X-TC-AUTHENTICATION-SECRET | Header | String | O  | API 보안 설정 메뉴의 Secret Access Key  |

또한 프로젝트 멤버 역할에 따라 호출할 수 있는 API가 제한됩니다. `RDS for MySQL ADMIN`, `RDS for MySQL VIEWER`로 구분하여 권한을 부여할 수 있습니다.

* `RDS for MySQL ADMIN` 권한은 모든 기능을 사용 가능합니다.
* `RDS for MySQL VIEWER` 권한은 정보를 조회하는 기능만 사용 가능합니다.
    * DB 인스턴스를 생성, 수정, 삭제하거나, DB 인스턴스를 대상으로 하는 어떠한 기능도 사용할 수 없습니다.
    * 단, 알림 그룹과 사용자 그룹 관련된 기능은 사용 가능합니다.

API 요청 시 인증에 실패하거나 권한이 없을 경우 다음과 같은 오류가 발생합니다.

| resultCode | resultMessage | 설명          |
|------------|---------------|-------------|
| 80401      | Unauthorized  | 인증에 실패했습니다. |
| 80403      | Forbidden     | 권한이 없습니다.   |

## 응답 공통 정보

모든 API 요청에 '200 OK'로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고합니다.

#### 응답 본문
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

#### 필드
| 이름 | 형식      | 설명|
| --- |---------| --- |
|resultCode | Number  | 결과 코드<br/>- 성공: `0`<br/>- 실패: `0`이 아닌 값 |
|resultMessage | String  | 결과 메시지 |
|isSuccessful | Boolean | 성공 여부 |


## DB 엔진 유형

| DB 엔진 유형 | 생성 가능 여부 | OBS로부터 복원 가능 여부 |
| -------- | -------- | ---------------- |
| MYSQL\_V5633 | X | X |
| MYSQL\_V5715 | O | O |
| MYSQL\_V5719 | O | O |
| MYSQL\_V5726 | O | O |
| MYSQL\_V5731 | X | X |
| MYSQL\_V5733 | O | X |
| MYSQL\_V5737 | O | O |
| MYSQL\_V8018 | O | O |
| MYSQL\_V8023 | O | O |
| MYSQL\_V8028 | O | O |
| MYSQL\_V8032 | O | O |
| MYSQL\_V8033 | O | O |
| MYSQL\_V8034 | O | O |

* ENUM 타입의 dbVersion 필드에 대해 해당 값을 사용할 수 있습니다.
* 버전에 따라 생성 또는 복원이 불가능한 경우가 있을 수 있습니다.

## 프로젝트 정보

### 리전 목록 보기

```
GET /v3.0/project/regions
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                 | 종류   | 형식      | 설명                                                                         |
|--------------------|------|---------|----------------------------------------------------------------------------|
| regions            | Body | Array   | 리전 목록                                                                      |
| regions.regionCode | Body | Enum    | 리전 코드<br/>- `KR1`: 한국(판교) 리전<br/>- `KR2`: 한국(평촌) 리전<br/>- `JP1`: 일본(도쿄) 리전 |
| regions.isEnabled  | Body | Boolean | 리전의 활성화 여부                                                                 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "regions": [
        {
            "regionCode": "KR1",
            "isEnabled": true
        },
        {
            "regionCode": "KR2",
            "isEnabled": true
        },
        {
            "regionCode": "JP1",
            "isEnabled": true
        }
    ]
}
```

</details>

---

### 프로젝트 멤버 목록 보기

```
GET /v3.0/project/members
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                   | 종류   | 형식     | 설명              |
|----------------------|------|--------|-----------------|
| members              | Body | Array  | 프로젝트 멤버 목록      |
| members.memberId     | Body | UUID   | 프로젝트 멤버의 식별자    |
| members.memberName   | Body | String | 프로젝트 멤버의 이름     |
| members.emailAddress | Body | String | 프로젝트 멤버의 이메일 주소 |
| members.phoneNumber  | Body | String | 프로젝트 멤버의 전화번호   |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "members": [
        {
            "memberId": "1b1d3627-507a-49ea-8cb7-c86dfa9caa58",
            "memberName": "홍길동",
            "emailAddress": "gildong.hong@nhn.com",
            "phoneNumber": "+821012345678"
        }
    ]
}
```

</p>
</details>

---

## DB 인스턴스 사양

### DB 인스턴스 사양 목록 보기

```
GET /v3.0/db-flavors
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                     | 종류   | 형식     | 설명              |
|------------------------|------|--------|-----------------|
| dbFlavors              | Body | Array  | DB 인스턴스 사양 목록   |
| dbFlavors.dbFlavorId   | Body | UUID   | DB 인스턴스 사양의 식별자 |
| dbFlavors.dbFlavorName | Body | String | DB 인스턴스 사양 이름   |
| dbFlavors.ram          | Body | Number | 메모리 용량(MB)      |
| dbFlavors.vcpus        | Body | Number | CPU 코어 수        |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbFlavors": [
        {
            "dbFlavorId": "50be6d9c-02d6-4594-a2d4-12010eb65ec0",
            "dbFlavorName": "m2.c1m2",
            "ram": 2048,
            "vcpus": 1
        }
    ]
}
```

</p>
</details>

---

## 네트워크

### 서브넷 목록 보기

```
GET /v3.0/network/subnets
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                       | 종류   | 형식      | 설명               |
|--------------------------|------|---------|------------------|
| subnets                  | Body | Array   | 서브넷 목록           |
| subnets.subnetId         | Body | UUID    | 서브넷의 식별자         |
| subnets.subnetName       | Body | String  | 서브넷을 식별할 수 있는 이름 |
| subnets.subnetCidr       | Body | String  | 서브넷의 CIDR        |
| subnets.usingGateway     | Body | Boolean | 게이트웨이 사용 여부      |
| subnets.availableIpCount | Body | Number  | 사용 가능한 IP 수      |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "subnets": [
        {
            "subnetId": "1b2a9b23-0725-4b92-8c78-35db66b8ad9f",
            "subnetName": "Default Network",
            "subnetCidr": "192.168.0.0/24",
            "usingGateway": true,
            "availableIpCount": 240
        }
    ]
}
```

</p>
</details>

---

## DB 엔진

### DB 엔진 목록 보기

```
GET /v3.0/db-versions
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                           | 종류   | 형식      | 설명                    |
|------------------------------|------|---------|-----------------------|
| dbVersions                   | Body | Array   | DB 엔진 목록              |
| dbVersions.dbVersion         | Body | String  | DB 엔진 유형              |
| dbVersions.dbVersionName     | Body | String  | DB 엔진 이름              |
| dbVersions.restorableFromObs | Body | Boolean | 오브젝트 스토리지로부터 복원 가능 여부 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbVersions": [
        {
            "dbVersion": "MYSQL_V8028",
            "dbVersionName": "MySQL 8.0.28",
            "restorableFromObs": true
        }
    ]
}
```

</p>
</details>

---

## 스토리지

### 스토리지 타입 목록 보기

```
GET /v3.0/storage-types
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름           | 종류   | 형식    | 설명         |
|--------------|------|-------|------------|
| storageTypes | Body | Array | 스토리지 타입 목록 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "storageTypes": [
        "General SSD",
        "General HDD"
    ]
}
```

</p>
</details>

---

### 스토리지 목록 보기

```
GET /v3.0/storages
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름           | 종류   | 형식    | 설명        |
|--------------|------|-------|-----------|
| storages | Body | Array | 스토리지 목록 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "storages": [
        "General SSD",
        "General HDD"
    ]
}
```

</p>
</details>

---

## 작업 정보

### 작업 상태

| 상태명                | 설명                   |
|--------------------|----------------------|
| `READY`            | 작업이 준비 중인 경우         |
| `RUNNING`          | 작업이 진행 중인 경우         |
| `COMPLETED`        | 작업이 완료된 경우           |
| `REGISTERED`       | 작업이 등록된 경우           |
| `WAIT_TO_REGISTER` | 작업 등록 대기 중인 경우       |
| `INTERRUPTED`      | 작업 진행 중 인터럽트가 발생한 경우 |
| `CANCELED`         | 작업이 취소된 경우           |
| `FAILED`           | 작업이 실패한 경우           |
| `ERROR`            | 작업 진행 중 오류가 발생한 경우   |
| `DELETED`          | 작업이 삭제된 경우           |
| `FAIL_TO_READY`    | 작업 준비에 실패한 경우        |

### 작업 정보 상세 보기

```
GET /v3.0/jobs/{jobId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름    | 종류  | 형식   | 필수 | 설명      |
|-------|-----|------|----|---------|
| jobId | URL | UUID | O  | 작업의 식별자 |

#### 응답

| 이름                             | 종류   | 형식       | 설명                                |
|--------------------------------|------|----------|-----------------------------------|
| jobId                          | Body | UUID     | 작업의 식별자                           |
| jobStatus                      | Body | Enum     | 작업의 현재 상태                         |
| resourceRelations              | Body | Array    | 연관 리소스 목록                         |
| resourceRelations.resourceType | Body | Enum     | 연관 리소스 유형                         |
| resourceRelations.resourceId   | Body | UUID     | 연관 리소스의 식별자                       |
| createdYmdt                    | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| updatedYmdt                    | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "0ddb042c-5af6-43fb-a914-f4dd0540eb7c",
    "jobStatus": "RUNNING",
    "resourceRelations": [
        {
            "resourceType": "INSTANCE",
            "resourceId": "56b39dcf-65eb-47ec-9d4f-09f160ba2266"
        }
    ],
    "createdYmdt": "2023-02-22T20:47:12+09:00",
    "updatedYmdt": "2023-02-22T20:49:46+09:00"
}
```

</p>
</details>

---

## DB 인스턴스 그룹

### DB 인스턴스 그룹 목록 보기

```
GET /v3.0/db-instance-groups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                                 | 종류   | 형식       | 설명                                                                       |
|------------------------------------|------|----------|--------------------------------------------------------------------------|
| dbInstanceGroups                   | Body | Array    | DB 인스턴스 그룹 목록                                                            |
| dbInstanceGroups.dbInstanceGroupId | Body | UUID     | DB 인스턴스 그룹의 식별자                                                          |
| dbInstanceGroups.replicationType   | Body | Enum     | DB 인스턴스 그룹의 복제 형태<br/>- `STANDALONE`: 단일<br/>- `HIGH_AVAILABILITY`: 고가용성 |
| dbInstanceGroups.createdYmdt       | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                        |
| dbInstanceGroups.updatedYmdt       | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                        |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceGroups": [
        {
            "dbInstanceGroupId": "05de0746-89fd-49c8-94f9-9c5b1df97009",
            "replicationType": "STANDALONE",
            "createdYmdt": "2023-02-13T17:35:20+09:00",
            "updatedYmdt": "2023-02-13T17:35:20+09:00"
        }
    ]
}
```

</p>
</details>

---

### DB 인스턴스 그룹 상세 보기

```
GET /v3.0/db-instance-groups/{dbInstanceGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                | 종류  | 형식   | 필수 | 설명              |
|-------------------|-----|------|----|-----------------|
| dbInstanceGroupId | URL | UUID | O  | DB 인스턴스 그룹의 식별자 |

#### 응답

| 이름                           | 종류   | 형식       | 설명                                                                                                                                    |
|------------------------------|------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceGroupId            | Body | UUID     | DB 인스턴스 그룹의 식별자                                                                                                                       |
| replicationType              | Body | Enum     | DB 인스턴스 그룹의 복제 형태<br/>- `STANDALONE`: 단일<br/>- `HIGH_AVAILABILITY`: 고가용성                                                              |
| dbInstances                  | Body | Array    | DB 인스턴스 그룹에 속한 DB 인스턴스 목록                                                                                                             |
| dbInstances.dbInstanceId     | Body | UUID     | DB 인스턴스의 식별자                                                                                                                          |
| dbInstances.dbInstanceType   | Body | Enum     | DB 인스턴스의 역할 타입<br/>- `MASTER`: 마스터<br/>- `FAILED_MASTER`: 장애 조치된 마스터<br/>- `CANDIDATE_MASTER`: 예비 마스터<br/>- `READ_ONLY_SLAVE`: 읽기 복제본 |
| dbInstances.dbInstanceStatus | Body | Enum     | DB 인스턴스의 현재 상태                                                                                                                        |
| createdYmdt                  | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |
| updatedYmdt                  | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceGroupId": "36617a8e-0df8-4b16-b6ea-6306019e95da",
    "replicationType": "STANDALONE",
    "dbInstances": [
        {
            "dbInstanceId": "6d2db0ef-fe9b-4ed4-97b1-d97fcb4cf1b8",
            "dbInstanceType": "MASTER",
            "dbInstanceStatus": "AVAILABLE"
        }
    ],
    "createdYmdt": "2023-03-03T17:38:14+09:00",
    "updatedYmdt": "2023-03-03T17:38:14+09:00"
}
```

</p>
</details>

---

## DB 인스턴스

### DB 인스턴스 상태

| 상태                  | 설명                           |
|---------------------|------------------------------|
| `AVAILABLE`         | DB 인스턴스가 사용 가능한 경우           |
| `BEFORE_CREATE`     | DB 인스턴스가 생성 전인 경우            |
| `STORAGE_FULL`      | DB 인스턴스의 용량이 부족한 경우          |
| `FAIL_TO_CREATE`    | DB 인스턴스 생성에 실패한 경우           |
| `FAIL_TO_CONNECT`   | DB 인스턴스 연결에 실패한 경우           |
| `REPLICATION_STOP`  | DB 인스턴스의 복제가 중단된 경우          |
| `FAILOVER`          | DB 인스턴스가 고가용성 장애 조치된 경우      |
| `FAILOVER_SHUTDOWN` | DB 인스턴스가 고가용성 장애 조치되어 중단된 경우 |
| `SHUTDOWN`          | DB 인스턴스가 중지된 경우              |
| `DELETED`           | DB 인스턴스가 삭제된 경우              |

### DB 인스턴스 진행 상태

| 상태                         | 설명           |
|----------------------------|--------------|
| `APPLYING_PARAMETER_GROUP` | 파라미터 그룹 적용 중 |
| `BACKING_UP`               | 백업 중         |
| `CANCELING`                | 취소 중         |
| `CREATING`                 | 생성 중         |
| `CREATING_SCHEMA`          | DB 스키마 생성 중	 |
| `CREATING_USER`            | 사용자 생성 중	    |
| `DELETING`                 | 삭제 중         |
| `DELETING_SCHEMA`          | DB 스키마 삭제 중  |
| `DELETING_USER`            | 사용자 삭제 중     |
| `EXPORTING_BACKUP`         | 백업을 내보내는 중   |
| `FAILING_OVER`             | 장애 조치 중      |
| `MIGRATING`                | 마이그레이션 중     |
| `MODIFYING`                | 수정 중         |
| `PREPARING`                | 준비 중         |
| `PROMOTING`                | 승격 중         |
| `REBUILDING`               | 재구축 중        |
| `REPAIRING`                | 복구 중         |
| `REPLICATING`              | 복제 중         |
| `RESTARTING`               | 재시작 중        |
| `RESTARTING_FORCIBLY`      | 강제 재시작 중     |
| `RESTORING`                | 복원 중         |
| `STARTING`                 | 시작 중         |
| `STOPPING`                 | 정지 중         |
| `SYNCING_SCHEMA`           | DB 스키마 동기화 중 |
| `SYNCING_USER`             | 사용자 동기화 중	   |
| `UPDATING_USER`            | 사용자 수정 중	    |

### DB 인스턴스 목록 보기

```
GET /v3.0/db-instances
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                            | 종류   | 형식       | 설명                                                                                                                                    |
|-------------------------------|------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| dbInstances                   | Body | Array    | DB 인스턴스 목록                                                                                                                            |
| dbInstances.dbInstanceId      | Body | UUID     | DB 인스턴스의 식별자                                                                                                                          |
| dbInstances.dbInstanceGroupId | Body | UUID     | DB 인스턴스 그룹의 식별자                                                                                                                       |
| dbInstances.dbInstanceName    | Body | String   | DB 인스턴스를 식별할 수 있는 이름                                                                                                                  |
| dbInstances.description       | Body | String   | DB 인스턴스에 대한 추가 정보                                                                                                                     |
| dbInstances.dbVersion         | Body | Enum     | DB 엔진 유형                                                                                                                              |
| dbInstances.dbPort            | Body | Number   | DB 포트                                                                                                                                 |
| dbInstances.dbInstanceType    | Body | Enum     | DB 인스턴스의 역할 타입<br/>- `MASTER`: 마스터<br/>- `FAILED_MASTER`: 장애 조치된 마스터<br/>- `CANDIDATE_MASTER`: 예비 마스터<br/>- `READ_ONLY_SLAVE`: 읽기 복제본 |
| dbInstances.dbInstanceStatus  | Body | Enum     | DB 인스턴스의 현재 상태                                                                                                                        |
| dbInstances.progressStatus    | Body | Enum     | DB 인스턴스의 현재 진행 상태                                                                                                                     |
| dbInstances.createdYmdt       | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |
| dbInstances.updatedYmdt       | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstances": [
        {
            "dbInstanceId": "d067593b-1acc-4ccc-9e8a-cc72d6d79ec3",
            "dbInstanceGroupId": "51c7d080-ff36-4025-84b1-9d9d0b4fe9e0",
            "dbInstanceName": "db-instance",
            "description": null,
            "dbVersion": "MYSQL_V8028",
            "dbPort": 10000,
            "dbInstanceType": "MASTER",
            "dbInstanceStatus": "AVAILABLE",
            "progressStatus": "NONE",
            "createdYmdt": "2023-01-23T12:03:13+09:00",
            "updatedYmdt": "2023-02-02T17:20:17+09:00"
        }
    ]
}
```

</p>
</details>

---

### DB 인스턴스 상세 보기

```
GET /v3.0/db-instances/{dbInstanceId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름                          | 종류   | 형식       | 설명                                                                                                                                    |
|-----------------------------|------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId                | Body | UUID     | DB 인스턴스의 식별자                                                                                                                          |
| dbInstanceGroupId           | Body | UUID     | DB 인스턴스 그룹의 식별자                                                                                                                       |
| dbInstanceName              | Body | String   | DB 인스턴스를 식별할 수 있는 이름                                                                                                                  |
| description                 | Body | String   | DB 인스턴스에 대한 추가 정보                                                                                                                     |
| dbVersion                   | Body | Enum     | DB 엔진 유형                                                                                                                              |
| dbPort                      | Body | Number   | DB 포트                                                                                                                                 |
| dbInstanceType              | Body | Enum     | DB 인스턴스의 역할 타입<br/>- `MASTER`: 마스터<br/>- `FAILED_MASTER`: 장애 조치된 마스터<br/>- `CANDIDATE_MASTER`: 예비 마스터<br/>- `READ_ONLY_SLAVE`: 읽기 복제본 |
| dbInstanceStatus            | Body | Enum     | DB 인스턴스의 현재 상태                                                                                                                        |
| progressStatus              | Body | Enum     | DB 인스턴스의 현재 작업 진행 상태                                                                                                                  |
| dbFlavorId                  | Body | UUID     | DB 인스턴스 사양의 식별자                                                                                                                       |
| parameterGroupId            | Body | UUID     | DB 인스턴스에 적용된 파라미터 그룹의 식별자                                                                                                             |
| dbSecurityGroupIds          | Body | Array    | DB 인스턴스에 적용된 DB 보안 그룹의 식별자 목록                                                                                                         |
| useDeletionProtection       | Body | Boolean  | DB 인스턴스 삭제 보호 여부                                                                                                                      |
| supportAuthenticationPlugin | Body | Boolean  | 인증 플러그인 지원 여부                                                                                                                         |
| needToApplyParameterGroup   | Body | Boolean  | 최신 파라미터 그룹 적용 필요 여부                                                                                                                   |
| needMigration               | Body | Boolean  | 마이그레이션 필요 여부                                                                                                                          |
| supportDbVersionUpgrade     | Body | Boolean  | DB 버전 업그레이드 지원 여부                                                                                                                     |
| createdYmdt                 | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |
| updatedYmdt                 | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                     |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceId": "d067593b-1acc-4ccc-9e8a-cc72d6d79ec3",
    "dbInstanceGroupId": "51c7d080-ff36-4025-84b1-9d9d0b4fe9e0",
    "dbInstanceName": "db-instance",
    "description": null,
    "dbVersion": "MYSQL_V8028",
    "dbPort": 10000,
    "dbInstanceType": "MASTER",
    "dbInstanceStatus": "AVAILABLE",
    "progressStatus": "NONE",
    "dbFlavorId": "e9ed4ef6-78d7-46fa-ace9-32481e97f3b7",
    "parameterGroupId": "b03e8b13-de27-4d04-a488-ff5689589372",
    "dbSecurityGroupIds": ["01908c35-d2c9-4852-baf0-17f06ec42c03"],
    "useDeletionProtection": false,
    "supportAuthenticationPlugin": true,
    "needToApplyParameterGroup": false,
    "needMigration": false,
    "supportDbVersionUpgrade": true,
    "createdYmdt": "2022-11-23T12:03:13+09:00",
    "updatedYmdt": "2022-12-02T17:20:17+09:00"
}
```

</p>
</details>

---

### DB 인스턴스 생성하기

```
POST /v3.0/db-instances
```

#### 요청

| 이름                                           | 종류   | 형식      | 필수 | 설명                                                                                                                                                                                                                          |
|----------------------------------------------|------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceName                               | Body | String  | O  | DB 인스턴스를 식별할 수 있는 이름                                                                                                                                                                                                        |
| description                                  | Body | String  | X  | DB 인스턴스에 대한 추가 정보                                                                                                                                                                                                           |
| dbFlavorId                                   | Body | UUID    | O  | DB 인스턴스 사양의 식별자                                                                                                                                                                                                             |
| dbVersion                                    | Body | Enum    | O  | DB 엔진 유형                                                                                                                                                                                                                    |
| dbPort                                       | Body | Number  | O  | DB 포트<br/>- 최솟값: `3306`<br/>- 최댓값: `43306`                                                                                                                                                                                  |
| dbUserName                                   | Body | String  | O  | DB 사용자 계정명                                                                                                                                                                                                                  |
| dbPassword                                   | Body | String  | O  | DB 사용자 계정 암호<br/>- 최소 길이: `4`<br/>- 최대 길이: `16`                                                                                                                                                                             |
| parameterGroupId                             | Body | UUID    | O  | 파라미터 그룹의 식별자                                                                                                                                                                                                                |
| dbSecurityGroupIds                           | Body | Array   | X  | DB 보안 그룹의 식별자 목록                                                                                                                                                                                                            ||network|Body|Object|O|네트워크 정보 객체|
| userGroupIds                                 | Body | Array   | X  | 사용자 그룹의 식별자 목록                                                                                                                                                                                                              |
| useHighAvailability                          | Body | Boolean | X  | 고가용성 사용 여부<br/>- 기본값: `false`                                                                                                                                                                                               |
| pingInterval                                 | Body | Number  | X  | 고가용성 사용 시 Ping 간격(초)<br/>- 기본값: `3`<br/>- 최솟값: `1`<br/>- 최댓값: `600`                                                                                                                                                         |
| useDefaultNotification                       | Body | Boolean | X  | 기본 알림 사용 여부<br/>- 기본값: `false`                                                                                                                                                                                              |
| useDeletionProtection                        | Body | Boolean | X  | 삭제 보호 여부<br/>- 기본값: `false`                                                                                                                                                                                                 |
| authenticationPlugin                         | Body | Enum    | X  | 인증 플러그인<br/>- NATIVE: `mysql_native_password`<br />- SHA256: sha256_password<br />- CACHING_SHA2: caching_sha2_password                                                                                                     |
| tlsOption                                    | Body | Enum    | X  | TLS Option<br/>- NONE<br />- SSL<br />- X509                                                                                                                                                                                |
| network                                      | Body | Object  | O  | 네트워크 정보 객체                                                                                                                                                                                                                  |
| network.subnetId                             | Body | UUID    | O  | 서브넷의 식별자                                                                                                                                                                                                                    |
| network.usePublicAccess                      | Body | Boolean | X  | 외부 접속 가능  여부<br/>- 기본값: `false`                                                                                                                                                                                             |
| network.availabilityZone                     | Body | Enum    | O  | DB 인스턴스를 생성할 가용성 영역<br/>- 예시: `kr-pub-a`                                                                                                                                                                                    |
| storage                                      | Body | Object  | O  | 스토리지 정보 객체                                                                                                                                                                                                                  |    
| storage.storageType                          | Body | Enum    | O  | 데이터 스토리지 타입<br/>- 예시: `General SSD`                                                                                                                                                                                         |
| storage.storageSize                          | Body | Number  | O  | 데이터 스토리지 크기(GB)<br/>- 최솟값: `20`<br/>- 최댓값: `2048`                                                                                                                                                                           |
| backup                                       | Body | Object  | O  | 백업 정보 객체                                                                                                                                                                                                                    |
| backup.backupPeriod                          | Body | Number  | O  | 백업 보관 기간(일)<br/>- 최솟값: `0`<br/>- 최댓값: `730`                                                                                                                                                                                 |
| backup.ftwrlWaitTimeout                      | Body | Number  | X  | 쿼리 지연 대기 시간(초)<br/>- 기본값: `1800`<br/>- 최솟값: `0`<br/>- 최댓값: `21600`                                                                                                                                                          |
| backup.backupRetryCount                      | Body | Number  | X  | 백업 재시도 횟수<br/>- 기본값: `0`<br/>- 최솟값: `0`<br/>- 최댓값: `10`                                                                                                                                                                     |
| backup.replicationRegion                     | Body | Enum    | X  | 백업 복제 리전<br />- `KR1`: 한국(판교)<br/>- `KR2`: 한국(평촌)<br/>- `JP1`: 일본(도쿄)                                                                                                                                                       |
| backup.useBackupLock                         | Body | Boolean | X  | 테이블 잠금 사용 여부<br/>- 기본값: `true`                                                                                                                                                                                              |
| backup.backupSchedules                       | Body | Array   | O  | 백업 스케줄 목록                                                                                                                                                                                                                   |
| backup.backupSchedules.backupWndBgnTime      | Body | String  | O  | 백업 시작 시각<br/>- 예시: `00:00:00`                                                                                                                                                                                               |
| backup.backupSchedules.backupWndDuration     | Body | Enum    | O  | 백업 Duration<br/>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br/>- `HALF_AN_HOUR`: 30분<br/>- `ONE_HOUR`: 1시간<br/>- `ONE_HOUR_AND_HALF`: 1시간 30분<br/>- `TWO_HOURS`: 2시간<br/>- `TWO_HOURS_AND_HALF`: 2시간 30분<br/>- `THREE_HOURS`: 3시간 |
| backup.backupSchedules.backupRetryExpireTime | Body | String  | O  | 백업 재시도 만료 시각<br/>- 백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.<br/>- 예시: `01:30:00`                                                                                                                                              |

<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance",
    "description": "description",
    "dbFlavorId": "71f69bf9-3c01-4c1a-b135-bb75e93f6268",
    "dbVersion": "MYSQL_V8028",
    "dbPort": 10000,
    "dbUserName": "db-user",
    "dbPassword": "password",
    "parameterGroupId": "488bf4f5-d8f7-459b-ace6-529b606c8570",
    "dbSecurityGroupIds": [
        "b0483a3d-e8e2-46f6-9e84-d5e31b0d44f4"
    ],
    "userGroupIds": [],
    "network": {
        "subnetId": "e721a9dd-dad0-4cf0-a53b-dd654ebfc683",
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20
    },
    "backup": {
        "backupPeriod": 1,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "ONE_HOUR",
                "backupRetryExpireTime": "01:30:00"
            }
        ]
    }
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}
```

#### 요청

| 이름                 | 종류   | 형식      | 필수 | 설명                                                                        |
|--------------------|------|---------|----|---------------------------------------------------------------------------|
| dbInstanceId       | URL  | UUID    | O  | DB 인스턴스의 식별자                                                              |
| dbInstanceName     | Body | String  | X  | DB 인스턴스를 식별할 수 있는 이름                                                      |
| description        | Body | String  | X  | DB 인스턴스에 대한 추가 정보                                                         |
| dbPort             | Body | Number  | X  | DB 포트<br/>- 최솟값: `3306`<br/>- 최댓값: `43306`                                |
| dbVersion          | Body | Enum    | X  | DB 엔진 유형                                                                                                                              |
| useDummy      | Body | Boolean | X  | 단일 DB 인스턴스의 DB 버전 업그레이드 시 더미 사용 여부<br/>기본값: `false`                                         |
| dbFlavorId         | Body | UUID    | X  | DB 인스턴스 사양의 식별자                                                           |
| parameterGroupId   | Body | UUID    | X  | 파라미터 그룹의 식별자                                                              |
| dbSecurityGroupIds | Body | Array   | X  | DB 보안 그룹의 식별자 목록                                                          |
| executeBackup      | Body | Boolean | X  | 현재 시점 백업 진행 여부<br/>- 기본값: `false`                                         |
| useOnlineFailover  | Body | Boolean | X  | 장애 조치를 이용한 재시작 여부<br/>고가용성을 사용 중인 DB 인스턴스에서만 사용 가능합니다.<br/>- 기본값: `false` |

<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance2",
    "description": "description2",
    "dbPort": 10001,
    "dbSecurityGroupIds": [],
    "executeBackup": true
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 삭제하기

```
DELETE /v3.0/db-instances/{dbInstanceId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 재시작하기

```
POST /v3.0/db-instances/{dbInstanceId}/restart
```

#### 요청

| 이름                | 종류   | 형식      | 필수 | 설명                                                                        |
|-------------------|------|---------|----|---------------------------------------------------------------------------|
| dbInstanceId      | URL  | UUID    | O  | DB 인스턴스의 식별자                                                              |
| useOnlineFailover | Body | Boolean | X  | 장애 조치를 이용한 재시작 여부<br/>고가용성을 사용 중인 DB 인스턴스에서만 사용 가능합니다.<br/>- 기본값: `false` |
| executeBackup     | Body | Boolean | X  | 현재 시점 백업 진행 여부<br/>- 기본값: `false`                                         |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---
### DB 인스턴스 강제 재시작하기
```
POST /v3.0/db-instances/{dbInstanceId}/force-restart
```

#### 요청

| 이름                | 종류   | 형식      | 필수 | 설명                                                                        |
|-------------------|------|---------|----|---------------------------------------------------------------------------|
| dbInstanceId      | URL  | UUID    | O  | DB 인스턴스의 식별자                                                              |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>


---

### DB 인스턴스 시작하기

```
POST /v3.0/db-instances/{dbInstanceId}/start
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 정지하기

```
POST /v3.0/db-instances/{dbInstanceId}/stop
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 백업하기

```
POST /v3.0/db-instances/{dbInstanceId}/backup
```

#### 요청

| 이름           | 종류   | 형식     | 필수 | 설명              |
|--------------|------|--------|----|-----------------|
| dbInstanceId | URL  | UUID   | O  | DB 인스턴스의 식별자    |
| backupName   | Body | String | O  | 백업을 식별할 수 있는 이름 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 백업 후 내보내기

```
POST /v3.0/db-instances/{dbInstanceId}/backup-to-object-storage
```

#### 요청

| 이름              | 종류   | 형식     | 필수 | 설명                          |
|-----------------|------|--------|----|-----------------------------|
| dbInstanceId    | URL  | UUID   | O  | DB 인스턴스의 식별자                |
| tenantId        | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 테넌트 ID   |
| username        | Body | String | O  | NHN Cloud 회원 또는 IAM 멤버 ID   |
| password        | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 API 비밀번호 |
| targetContainer | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 컨테이너     |
| objectPath      | Body | String | O  | 컨테이너에 저장될 백업의 경로            |

<details><summary>예시</summary>
<p>

```json
{
    "tenantId": "399631c404744dbbb18ce4fa2dc71a5a",
    "username": "gildong.hong@nhn.com",
    "password": "password",
    "targetContainer": "/container",
    "objectPath": "/backups/backup_file"
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 복제하기

```
POST /v3.0/db-instances/{dbInstanceId}/replicate
```

#### 요청

| 이름                                           | 종류   | 형식      | 필수 | 설명                                                                                                                                                                                                                                                  |
|----------------------------------------------|------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId                                 | URL  | UUID    | O  | DB 인스턴스의 식별자                                                                                                                                                                                                                                        |
| dbInstanceName                               | Body | String  | O  | DB 인스턴스를 식별할 수 있는 이름                                                                                                                                                                                                                                |
| description                                  | Body | String  | X  | DB 인스턴스에 대한 추가 정보                                                                                                                                                                                                                                   |
| dbFlavorId                                   | Body | UUID    | X  | DB 인스턴스 사양의 식별자<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                                             |
| dbPort                                       | Body | Number  | X  | DB 포트<br/>- 기본값: 원본 DB 인스턴스 값<br/>- 최솟값: `3306`<br/>- 최댓값: `43306`                                                                                                                                                                                  |
| parameterGroupId                             | Body | UUID    | X  | 파라미터 그룹의 식별자<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                                                |
| dbSecurityGroupIds                           | Body | Array   | X  | DB 보안 그룹의 식별자 목록<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                                            |
| userGroupIds                                 | Body | Array   | X  | 사용자 그룹의 식별자 목록                                                                                                                                                                                                                                      |
| useDefaultNotification                   | Body | Boolean | X  | 기본 알림 사용 여부<br/>- 기본값: `false`                                                                                                                                                                                                                      |
| useDeletionProtection                        | Body | Boolean | X  | 삭제 보호 여부<br/>- 기본값: `false`                                                                                                                                                                                                                         |
| network                                      | Body | Object  | O  | 네트워크 정보 객체                                                                                                                                                                                                                                          |
| network.usePublicAccess                      | Body | Boolean | X  | 외부 접속 가능 여부<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                                                 |
| network.availabilityZone                     | Body | Enum    | O  | DB 인스턴스를 생성할 가용성 영역<br/>- 예시: `kr-pub-a`                                                                                                                                                                                                            |
| storage                                      | Body | Object  | X  | 스토리지 정보 객체                                                                                                                                                                                                                                          |    
| storage.storageSize                          | Body | Number  | X  | 데이터 스토리지 크기(GB)<br/>- 기본값: 원본 DB 인스턴스 값<br/>- 최솟값: `20`<br/>- 최댓값: `2048`                                                                                                                                                                           |
| backup                                       | Body | Object  | X  | 백업 정보 객체                                                                                                                                                                                                                                            |
| backup.backupPeriod                          | Body | Number  | X  | 백업 보관 기간(일)<br/>- 기본값: 원본 DB 인스턴스 값<br/>- 최솟값: `0`<br/>- 최댓값: `730`                                                                                                                                                                                 |
| backup.ftwrlWaitTimeout                      | Body | Number  | X  | 쿼리 지연 대기 시간(초)<br/>- 기본값: 원본 DB 인스턴스 값<br/>- 최솟값: `0`<br/>- 최댓값: `21600`                                                                                                                                                                            |
| backup.backupRetryCount                      | Body | Number  | X  | 백업 재시도 횟수<br/>- 기본값: 원본 DB 인스턴스 값<br/>- 최솟값: `0`<br/>- 최댓값: `10`                                                                                                                                                                                    |
| backup.replicationRegion                     | Body | Enum    | X  | 백업 복제 리전<br />- `KR1`: 한국(판교)<br/>- `KR2`: 한국(평촌)<br/>- `JP1`: 일본(도쿄)<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                       |
| backup.useBackupLock                         | Body | Boolean | X  | 테이블 잠금 사용 여부<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                                                |
| backup.backupSchedules                       | Body | Array   | X  | 백업 스케줄 목록                                                                                                                                                                                                                                           |
| backup.backupSchedules.backupWndBgnTime      | Body | String  | X  | 백업 시작 시각<br/>- 예시: `00:00:00`<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                                                                               |
| backup.backupSchedules.backupWndDuration     | Body | Enum    | X  | 백업 Duration<br/>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br/>- `HALF_AN_HOUR`: 30분<br/>- `ONE_HOUR`: 1시간<br/>- `ONE_HOUR_AND_HALF`: 1시간 30분<br/>- `TWO_HOURS`: 2시간<br/>- `TWO_HOURS_AND_HALF`: 2시간 30분<br/>- `THREE_HOURS`: 3시간<br/>- 기본값: 원본 DB 인스턴스 값 |
| backup.backupSchedules.backupRetryExpireTime | Body | String  | X  | 백업 재시도 만료 시각<br/>- 백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.<br/>- 예시: `01:30:00`<br/>- 기본값: 원본 DB 인스턴스 값                                                                                                                                              |

<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance-replicate",
    "description": "description",
    "dbPort": 11000,
    "network": {
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "stroageSize": 100
    }
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 인스턴스 승격하기

```
POST /v3.0/db-instances/{dbInstanceId}/promote
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 복원 정보 조회

```
GET /v3.0/db-instances/{dbInstanceId}/restoration-info
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| dbInstanceId | URL | UUID | O | DB 인스턴스의 식별자 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| oldestRestorableYmdt | Body | DateTime | 가장 오래된 복원 가능한 시각 |
| latestRestorableYmdt | Body | DateTime | 가장 최신의 복원 가능한 시각 |
| restorableBackups | Body | Array | 복원 가능한 백업 목록 |
| restorableBackups.backup | Body | Object | 백업 정보 객체 |
| restorableBackups.backup.backupId | Body | UUID | 백업의 식별자 |
| restorableBackups.backup.backupName | Body | String | 백업 이름 |
| restorableBackups.backup.useBackupLock | Body | Boolean | 테이블 잠금 사용 여부 |
| restorableBackups.backup.backupSize | Body | Number | 백업 크기 |
| restorableBackups.backup.backupType | Body | Enum | 백업 유형<br><ul><li>`AUTO`: 자동</li><li>`MANUAL`: 수동</li></ul> |
| restorableBackups.backup.backupStatus | Body | Enum | 백업 상태<br><ul><li>`BACKING_UP`: 백업 중인 경우</li><li>`COMPLETED`: 백업이 완료된 경우</li><li>`DELETING`: 백업이 삭제 중인 경우</li><li>`DELETED`: 백업이 삭제된 경우</li><li>`ERROR`: 오류가 발생한 경우</li></ul> |
| restorableBackups.backup.dbInstanceId | Body | UUID | 원본 DB 인스턴스의 식별자 |
| restorableBackups.backup.dbInstanceName | Body | String | 원본 DB 인스턴스의 이름 |
| restorableBackups.backup.dbVersion | Body | String | DB 엔진 유형 |
| restorableBackups.backup.failoverCount | Body | Number | 장애 조치 횟수 |
| restorableBackups.backup.binLogFileName | Body | String | 바이너리 로그 파일 이름 |
| restorableBackups.backup.binLogPosition | Body | Number | 바이너리 로그 파일 위치 |
| restorableBackups.backup.createdYmdt | Body | DateTime | 백업 생성 일시 |
| restorableBackups.backup.updatedYmdt | Body | DateTime | 백업 갱신 일시 |
| restorableBackups.restorableBinLogs | Body | Array | 해당 백업을 이용하여 복원 가능한 바이너리 로그 이름 목록 |



<details><summary>예시</summary>
<p>

```json
{
	"header": {
		"resultCode": 0,
		"resultMessage": "SUCCESS",
		"isSuccessful": true
	},
    "oldestRestorableYmdt": "2023-07-09T16:33:33+09:00",
	"latestRestorableYmdt": "2023-07-10T15:44:44+09:00",
	"restorableBackups": [
		{
			"backup": {
				"backupId": "145d889a-fe08-474f-8f58-bde576ff96a9",
				"backupName": "example-backup-name",
				"backupStatus": "COMPLETED",
				"dbInstanceId": "dba1be25-9429-4589-9716-7fb6daad7cb9",
				"dbInstanceName": "original-db-instance-name",
				"dbVersion": "MYSQL_V8032",
				"backupType": "MANUAL",
				"backupSize": 8299904,
				"useBackupLock": true,
				"failoverCount": 0,
				"binLogFileName": "mysql-bin.000001",
				"binLogPosition": 367916037,
				"createdYmdt": "2023-07-10T15:44:44+09:00",
				"updatedYmdt": "2023-07-10T15:46:07+09:00"
			},
			"restorableBinLogs": [
				"mysql-bin.000001"
			]
		}
	]
}
```

</p>
</details>

---

### 복원될 마지막 쿼리 조회

```
GET /v3.0/db-instances/{dbInstanceId}/restoration-info/last-query
```

#### 공통 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| dbInstanceId | URL | UUID | O | DB 인스턴스의 식별자 |
| restoreType | Query | Enum | O | 복원 타입 종류<br><ul><li>`TIMESTAMP`: 복원 가능한 시간 이내의 시간을 이용한 시점 복원 타입</li><li>`BINLOG`: 복원 가능한 바이너리 로그 위치를 이용한 시점 복원 타입</li></ul>  |

#### restoreType이 `TIMESTAMP`인 경우

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| restoreYmdt | Query | DateTime | O | DB 인스턴스 복원 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

#### restoreType이 `BINLOG`인 경우

| 이름 | 종류    | 형식 | 필수 | 설명 |
| --- |-------| --- | --- | --- |
| backupId | Query | UUID | O | 복원에 사용할 백업의 식별자 |
| binLogFileName | Query  | String | O | 복원에 사용할 바이너리 로그 이름 |
| binLogPosition | Query  | Number | O | 복원에 사용할 바이너리 로그 위치 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| executedYmdt | Body | DateTime | 쿼리 수행 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| lastQuery | Body | String | 마지막 수행 쿼리 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "executedYmdt": "2023-03-17T14:02:29+09:00",
    "lastQuery": "INSERT INTO `test`.`test`SET  @1='0123'"
}
```

</p>
</details>

---

### 복원

```
POST /v3.0/db-instances/{dbInstanceId}/restore
```

#### 공통 요청

| 이름 | 종류 | 형식 | 필수 | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| --- | --- | --- | --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId | URL | UUID | O | DB 인스턴스의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| restore | Body | Object | O | 복원 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| restore.restoreType | Body | Enum | O | 복원 타입 종류<br><ul><li>`TIMESTAMP`: 복원 가능한 시간 이내의 시간을 이용한 시점 복원 타입</li><li>`BINLOG`: 복원 가능한 바이너리 로그 위치를 이용한 시점 복원 타입</li><li>`BACKUP`: 기존에 생성한 백업을 이용한 스냅샷 복원 타입</li></ul>                                                                                                                                                                                                                                                                                              |
| dbInstanceName | Body | String | O | DB 인스턴스를 식별할 수 있는 이름                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| description | Body | String | X | DB 인스턴스에 대한 추가 정보                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| dbFlavorId | Body | UUID | O | DB 인스턴스 사양의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| dbPort | Body | Number | O | DB 포트<br><ul><li>최솟값: `3306`</li><li>최댓값: `43306`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                          |
| <span style="color:#313338">parameterGroupId</span> | Body | UUID | O | 파라미터 그룹의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| dbSecurityGroupIds | Body | Array | X | DB 보안 그룹의 식별자 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| userGroupIds | Body | Array | X | 사용자 그룹의 식별자 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| useHighAvailability | Body | Boolean | X | 고가용성 사용 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| pingInterval | Body | Number | X | 고가용성 사용 시 Ping 간격(초)<br><ul><li>기본값: `3`</li><li>최솟값: `1`</li><li>최댓값: `600`</li></ul>                                                                                                                                                                                                                                                                                                                                                                               |
| useDefaultNotification | Body | Boolean | X | 기본 알림 사용 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                        |
| network | Body | Object | O | 네트워크 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| network.subnetId | Body | UUID | O | 서브넷의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| network.usePublicAccess | Body | Boolean | X | 외부 접속 가능 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                        |
| network.availabilityZone | Body | Enum | O | DB 인스턴스를 생성할 가용성 영역<br><ul><li>예시: `kr-pub-a`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                              |
| storage | Body | Object | O | 스토리지 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| storage.storageType | Body | Enum | O | 데이터 스토리지 타입<br><ul><li>예시: `General SSD`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                   |
| storage.storageSize | Body | Number | O | 데이터 스토리지 크기(GB)<br><ul><li>최솟값: `20`</li><li>최댓값: `2048`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                   |
| backup | Body | Object | O | 백업 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| backup.backupPeriod | Body | Number | O | 백업 보관 기간(일)<br><ul><li>최솟값: `0`</li><li>최댓값: `730`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                         |
| backup.ftwrlWaitTimeout | Body | Number | X | 쿼리 지연 대기 시간(초)<br><ul><li>기본값: `1800`</li><li>최솟값: `0`</li><li>최댓값: `21600`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                |
| backup.backupRetryCount | Body | Number | X | 백업 재시도 횟수<br><ul><li>기본값: `0`</li><li>최솟값: `0`</li><li>최댓값: `10`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                           |
| backup.replicationRegion | Body | Enum | X | 백업 복제 리전<br><ul><li>`KR1`: 한국(판교)</li><li>`KR2`: 한국(평촌)</li><li>`JP1`: 일본(도쿄)</li></ul>                                                                                                                                                                                                                                                                                                                                                                              |
| backup.useBackupLock | Body | Boolean | X | 테이블 잠금 사용 여부<br><ul><li>기본값: `true`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                        |
| backup.backupSchedules | Body | Array | O | 백업 스케줄 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| backup.backupSchedules.backupWndBgnTime | Body | String | O | 백업 시작 시각<br><ul><li>예시: `00:00:00`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| backup.backupSchedules.backupWndDuration | Body | Enum | O | 백업 Duration<br>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br><ul><li>`HALF_AN_HOUR`<span style="color:#313338">: 30분</span></li><li>`ONE_HOUR`<span style="color:#313338">: 1시간</span></li><li>`ONE_HOUR_AND_HALF`<span style="color:#313338">: 1시간 30분</span></li><li>`TWO_HOURS`<span style="color:#313338">: 2시간</span></li><li>`TWO_HOURS_AND_HALF`<span style="color:#313338">: 2시간 30분</span></li><li>`THREE_HOURS`<span style="color:#313338">: 3시간</span></li></ul> |
| backup.backupSchedules.backupRetryExpireTime | Body | String | O | 백업 재시도 만료 시각<br><ul><li>백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.</li><li>예시: `01:30:00`</li></ul>                                                                                                                                                                                                                                                                                                                                                                      |
| useDeletionProtection | Body | Boolean | X | 삭제 보호 여부<br>기본값: `false`                                                                                                                                                                                                                                                                                                                                                                                                                                             |

#### Timestamp를 이용한 시점 복원 시 요청(restoreType이 `TIMESTAMP`인 경우)

| 이름 | 종류 | 형식 | 필수 | 설명                                                                                             |
| --- | --- | --- | --- |------------------------------------------------------------------------------------------------|
| restore.restoreYmdt | Body | DateTime | O | DB 인스턴스 복원 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)<br>복원 정보 조회로 조회한 가장 최신의 복원 가능한 시간 이전에 대해서만 복원이 가능하다. |


<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance",
    "description": "description",
    "dbFlavorId": "71f69bf9-3c01-4c1a-b135-bb75e93f6268",
    "dbPort": 10000,
    "dbUserName": "db-user",
    "dbPassword": "password",
    "parameterGroupId": "488bf4f5-d8f7-459b-ace6-529b606c8570",
    "dbSecurityGroupIds": [
        "b0483a3d-e8e2-46f6-9e84-d5e31b0d44f4"
    ],
    "userGroupIds": [],
    "network": {
		"subnetId": "3ae7914f-9b42-4729-b125-87417b72cf36",
		"availabilityZone": "kr-pub-a"
	},
	"storage": {
		"storageType": "General SSD",
		"storageSize": 20
	},
	"restore": {
		"restoreType": "TIMESTAMP",
		"restoreYmdt": "2023-07-10T15:44:44+09:00"
	},
	"backup": {
		"backupPeriod": 1,
		"backupSchedules": [
			{
				"backupWndBgnTime": "00:00:00",
				"backupWndDuration": "ONE_HOUR_AND_HALF",
				"backupRetryExpireTime": "01:30:00"
			}
		]
	}
}
```

</p>
</details>

#### 바이너리 로그를 이용한 시점 복원 시 요청(restoreType이 `BINLOG`인 경우)

| 이름 | 종류 | 형식     | 필수 | 설명 |
| --- | --- |--------| --- | --- |
| restore.backupId | Body | UUID   | O | 복원에 사용할 백업의 식별자 |
| restore.binLog | Body | Object | O | 바이너리 로그 정보 객체 |
| restore.binLog.binLogFileName | Body | String | O | 복원에 사용할 바이너리 로그 이름 |
| restore.binLog.binLogPosition | Body | Number | O | 복원에 사용할 바이너리 로그 위치 |

* 바이너리 로그를 이용한 시점 복원 시 기준 백업의 바이너리 로그 파일 및 위치를 기준으로 그 이후에 기록된 로그에 대해 복원이 가능합니다.


<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance",
    "description": "description",
    "dbFlavorId": "71f69bf9-3c01-4c1a-b135-bb75e93f6268",
    "dbPort": 10000,
    "dbUserName": "db-user",
    "dbPassword": "password",
    "parameterGroupId": "488bf4f5-d8f7-459b-ace6-529b606c8570",
    "dbSecurityGroupIds": [
        "b0483a3d-e8e2-46f6-9e84-d5e31b0d44f4"
    ],
    "userGroupIds": [],
    "network": {
		"subnetId": "3ae7914f-9b42-4729-b125-87417b72cf36",
		"availabilityZone": "kr-pub-a"
	},
	"storage": {
		"storageType": "General SSD",
		"storageSize": 20
	},
	"restore": {
		"restoreType": "BINLOG",
        "backupId":"3ae7914f-9b42-4729-b125-87417b72cf36",
		"binLogFileName": "mysql-bin.000001",
		"binLogPosition": 1234567
	},
	"backup": {
		"backupPeriod": 1,
		"backupSchedules": [
			{
				"backupWndBgnTime": "00:00:00",
				"backupWndDuration": "ONE_HOUR_AND_HALF",
				"backupRetryExpireTime": "01:30:00"
			}
		]
	}
}
```

</p>
</details>

#### 백업을 이용한 복원 시 요청(restoreType이 `BACKUP`인 경우)

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| restore.backupId | Body | UUID | O(restoreType이 `BACKUP`인 경우) | 복원에 사용할 백업의 식별자 |



<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance",
    "description": "description",
    "dbFlavorId": "71f69bf9-3c01-4c1a-b135-bb75e93f6268",
    "dbPort": 10000,
    "dbUserName": "db-user",
    "dbPassword": "password",
    "parameterGroupId": "488bf4f5-d8f7-459b-ace6-529b606c8570",
    "dbSecurityGroupIds": [
        "b0483a3d-e8e2-46f6-9e84-d5e31b0d44f4"
    ],
    "userGroupIds": [],
    "network": {
		"subnetId": "3ae7914f-9b42-4729-b125-87417b72cf36",
		"availabilityZone": "kr-pub-a"
	},
	"storage": {
		"storageType": "General SSD",
		"storageSize": 20
	},
	"restore": {
		"restoreType": "BACKUP",
        "backupId":"3ae7914f-9b42-4729-b125-87417b72cf36"
	},
	"backup": {
		"backupPeriod": 1,
		"backupSchedules": [
			{
				"backupWndBgnTime": "00:00:00",
				"backupWndDuration": "ONE_HOUR_AND_HALF",
				"backupRetryExpireTime": "01:30:00"
			}
		]
	}
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| jobId | Body | UUID | 요청한 작업의 식별자 |


---

### 오브젝트 스토리지로부터 복원

```
POST /v3.0/db-instances/restore-from-obs
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --- | --- | --- | --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| restore | Body | Object | O | 복원 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| restore.tenantId | Body | String | O | 백업이 저장된 오브젝트 스토리지의 테넌트 ID                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| restore.username | Body | String | O | NHN Cloud 회원 또는 IAM 멤버 ID                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| restore.password | Body | String | O | 백업이 저장된 오브젝트 스토리지의 API 비밀번호                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| restore.targetContainer | Body | String | O | 백업이 저장된 오브젝트 스토리지의 컨테이너                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| restore.objectPath | Body | String | O | 컨테이너에 저장된 백업의 경로                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| dbVersion | Body | Enum | O | DB 엔진 유형                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| dbInstanceName | Body | String | O | DB 인스턴스를 식별할 수 있는 이름                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| description | Body | String | X | DB 인스턴스에 대한 추가 정보                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| dbFlavorId | Body | UUID | O | DB 인스턴스 사양의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| dbPort | Body | Number | O | DB 포트<br><ul><li>최솟값: `3306`</li><li>최댓값: `43306`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                           |
| <span style="color:#313338">parameterGroupId</span> | Body | UUID | O | 파라미터 그룹의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| dbSecurityGroupIds | Body | Array | X | DB 보안 그룹의 식별자 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| userGroupIds | Body | Array | X | 사용자 그룹의 식별자 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| useHighAvailability | Body | Boolean | X | 고가용성 사용 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                          |
| pingInterval | Body | Number | X | 고가용성 사용 시 Ping 간격(초)<br><ul><li>기본값: `3`</li><li>최솟값: `1`</li><li>최댓값: `600`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                |
| useDefaultNotification | Body | Boolean | X | 기본 알림 사용 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| network | Body | Object | O | 네트워크 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| network.subnetId | Body | UUID | O | 서브넷의 식별자                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| network.usePublicAccess | Body | Boolean | X | 외부 접속 가능 여부<br><ul><li>기본값: `false`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| network.availabilityZone | Body | Enum | O | DB 인스턴스를 생성할 가용성 영역<br><ul><li>예시: `kr-pub-a`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                               |
| storage | Body | Object | O | 스토리지 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| storage.storageType | Body | Enum | O | 데이터 스토리지 타입<br><ul><li>예시: `General SSD`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                    |
| storage.storageSize | Body | Number | O | 데이터 스토리지 크기(GB)<br><ul><li>최솟값: `20`</li><li>최댓값: `2048`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                    |
| backup | Body | Object | O | 백업 정보 객체                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| backup.backupPeriod | Body | Number | O | 백업 보관 기간(일)<br><ul><li>최솟값: `0`</li><li>최댓값: `730`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                          |
| backup.ftwrlWaitTimeout | Body | Number | X | 쿼리 지연 대기 시간(초)<br><ul><li>기본값: `1800`</li><li>최솟값: `0`</li><li>최댓값: `21600`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                 |
| backup.backupRetryCount | Body | Number | X | 백업 재시도 횟수<br><ul><li>기본값: `0`</li><li>최솟값: `0`</li><li>최댓값: `10`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                            |
| backup.replicationRegion | Body | Enum | X | 백업 복제 리전<br><ul><li>`KR1`: 한국(판교)</li><li>`KR2`: 한국(평촌)</li><li>`JP1`: 일본(도쿄)</li></ul>                                                                                                                                                                                                                                                                                                                                                                               |
| backup.useBackupLock | Body | Boolean | X | 테이블 잠금 사용 여부<br><ul><li>기본값: `true`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| backup.backupSchedules | Body | Array | O | 백업 스케줄 목록                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| backup.backupSchedules.backupWndBgnTime | Body | String | O | 백업 시작 시각<br><ul><li>예시: `00:00:00`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                          |
| backup.backupSchedules.backupWndDuration | Body | Enum | O | 백업 Duration<br>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br><ul><li>`HALF_AN_HOUR`<span style="color:#313338">: 30분</span></li><li>`ONE_HOUR`<span style="color:#313338">: 1시간</span></li><li>`ONE_HOUR_AND_HALF`<span style="color:#313338">: 1시간 30분</span></li><li>`TWO_HOURS`<span style="color:#313338">: 2시간</span></li><li>`TWO_HOURS_AND_HALF`<span style="color:#313338">: 2시간 30분</span></li><li>`THREE_HOURS`<span style="color:#313338">: 3시간</span></li></ul> |
| backup.backupSchedules.backupRetryExpireTime | Body | String | O | 백업 재시도 만료 시각<br><ul><li>백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.</li><li>예시: `01:30:00`</li></ul>                                                                                                                                                                                                                                                                                                                                                                       |



<details><summary>예시</summary>
<p>

```json
{
    "dbInstanceName": "db-instance",
    "description": "description",
    "dbFlavorId": "71f69bf9-3c01-4c1a-b135-bb75e93f6268",
    "dbPort": 10000,
    "dbVersion": "MYSQL_V8028",
    "dbUserName": "db-user",
    "dbPassword": "password",
    "parameterGroupId": "488bf4f5-d8f7-459b-ace6-529b606c8570",
    "dbSecurityGroupIds": [
        "b0483a3d-e8e2-46f6-9e84-d5e31b0d44f4"
    ],
    "userGroupIds": [],
    "network": {
		"subnetId": "3ae7914f-9b42-4729-b125-87417b72cf36",
		"availabilityZone": "kr-pub-a"
	},
	"storage": {
		"storageType": "General SSD",
		"storageSize": 20
	},
	"restore": {
		"tenantId":"tenant-id",
        "username":"username",
        "password":"password",
        "targetContainer":"targetContainer",
        "objectPath":"objectPath"
	},
	"backup": {
		"backupPeriod": 1,
		"backupSchedules": [
			{
				"backupWndBgnTime": "00:00:00",
				"backupWndDuration": "ONE_HOUR_AND_HALF",
				"backupRetryExpireTime": "01:30:00"
			}
		]
	}
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| jobId | Body | UUID | 요청한 작업의 식별자 |


---


### DB 인스턴스 삭제 보호 설정 변경하기

```
PUT /v3.0/db-instances/{dbInstanceId}/deletion-protection
```

#### 요청

| 이름                    | 종류   | 형식      | 필수 | 설명           |
|-----------------------|------|---------|----|--------------|
| dbInstanceId          | URL  | UUID    | O  | DB 인스턴스의 식별자 |
| useDeletionProtection | Body | Boolean | O  | 삭제 보호 여부     |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 고가용성 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}/high-availability
```

#### 요청

| 이름                  | 종류   | 형식      | 필수 | 설명                                                   |
|---------------------|------|---------|----|------------------------------------------------------|
| dbInstanceId        | URL  | UUID    | O  | DB 인스턴스의 식별자                                         |
| useHighAvailability | Body | Boolean | O  | 고가용성 사용 여부                                           |
| pingInterval        | Body | Number  | X  | 고가용성 사용 시 Ping 간격(초)<br/>- 최솟값: `1`<br/>- 최댓값: `600` |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 고가용성 다시 시작하기

```
POST /v3.0/db-instances/{dbInstanceId}/high-availability/resume
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 고가용성 일시 중지하기

```
POST /v3.0/db-instances/{dbInstanceId}/high-availability/pause
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 고가용성 복구하기

```
POST /v3.0/db-instances/{dbInstanceId}/high-availability/repair
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 고가용성 분리하기

```
POST /v3.0/db-instances/{dbInstanceId}/high-availability/split
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 스토리지 정보 보기

```
GET /v3.0/db-instances/{dbInstanceId}/storage-info
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름            | 종류   | 형식     | 설명                                                                                   |
|---------------|------|--------|--------------------------------------------------------------------------------------|
| storageType   | Body | Enum   | 데이터 스토리지 타입                                                                          |
| storageSize   | Body | Number | 데이터 스토리지 크기(GB)                                                                      |
| storageStatus | Body | Enum   | 데이터 스토리지의 현재 상태<br/>- `DETACHED`: 부착되지 않음<br/>- `ATTACHED`: 부착됨<br/>- `DELETED`: 삭제됨 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "storageType": "General SSD",
    "storageSize": 20,
    "storageStatus": "ATTACHED"
}
```

</p>
</details>


---

### 스토리지 정보 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}/storage-info
```

#### 요청

| 이름                | 종류   | 형식      | 필수 | 설명                                                                        |
|-------------------|------|---------|----|---------------------------------------------------------------------------|
| dbInstanceId      | URL  | UUID    | O  | DB 인스턴스의 식별자                                                              |
| storageSize       | Body | Number  | O  | 데이터 스토리지 크기(GB)<br/>- 최솟값: 현재값<br/>- 최댓값: `2048`                          |
| useOnlineFailover | Body | Boolean | X  | 장애 조치를 이용한 재시작 여부<br/>고가용성을 사용 중인 DB 인스턴스에서만 사용 가능합니다.<br/>- 기본값: `false` |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 백업 정보 보기

```
GET /v3.0/db-instances/{dbInstanceId}/backup-info
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름                                    | 종류   | 형식      | 설명             |
|---------------------------------------|------|---------|----------------|
| backupPeriod                          | Body | Number  | 백업 보관 기간(일)    |
| ftwrlWaitTimeout                      | Body | Number  | 쿼리 지연 대기 시간(초) |
| backupRetryCount                      | Body | Number  | 백업 재시도 횟수      |
| replicationRegion                     | Body | Enum    | 백업 복제 리전       |
| useBackupLock                         | Body | Boolean | 테이블 잠금 사용 여부   |
| backupSchedules                       | Body | Array   | 백업 스케줄 목록      |
| backupSchedules.backupWndBgnTime      | Body | String  | 백업 시작 시각       |
| backupSchedules.backupWndDuration     | Body | Enum    | 백업 Duration    |
| backupSchedules.backupRetryExpireTime | Body | String  | 백업 재시도 만료 시각   |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "backupPeriod": 1,
    "ftwrlWaitTimeout": 1800,
    "backupRetryCount": 0,
    "replicationRegion": null,
    "useBackupLock": false,
    "backupSchedules": [
        {
            "backupWndBgnTime": "00:00:00",
            "backupWndDuration": "ONE_HOUR_AND_HALF",
            "backupRetryExpireTime": "01:30:00"
        }
    ]
}
```

</p>
</details>


---

### 백업 정보 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}/backup-info
```

#### 요청

| 이름                                    | 종류   | 형식      | 필수 | 설명                                                                                                                                                                                                                          |
|---------------------------------------|------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId                          | URL  | UUID    | O  | DB 인스턴스의 식별자                                                                                                                                                                                                                |
| backupPeriod                          | Body | Number  | X  | 백업 보관 기간(일)<br/>- 최솟값: `0`<br/>- 최댓값: `730`                                                                                                                                                                                 |
| ftwrlWaitTimeout                      | Body | Number  | X  | 쿼리 지연 대기 시간(초)<br/>- 최솟값: `0`<br/>- 최댓값: `21600`                                                                                                                                                                            |
| backupRetryCount                      | Body | Number  | X  | 백업 재시도 횟수<br/>- 최솟값: `0`<br/>- 최댓값: `10`                                                                                                                                                                                    |
| replicationRegion                     | Body | Enum    | X  | 백업 복제 리전<br />- `KR1`: 한국(판교)<br/>- `KR2`: 한국(평촌)<br/>- `JP1`: 일본(도쿄)                                                                                                                                                       |
| useBackupLock                         | Body | Boolean | X  | 테이블 잠금 사용 여부                                                                                                                                                                                                                |
| backupSchedules                       | Body | Array   | X  | 백업 스케줄 목록                                                                                                                                                                                                                   |
| backupSchedules.backupWndBgnTime      | Body | String  | O  | 백업 시작 시각<br/>- 예시: `00:00:00`                                                                                                                                                                                               |
| backupSchedules.backupWndDuration     | Body | Enum    | O  | 백업 Duration<br/>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br/>- `HALF_AN_HOUR`: 30분<br/>- `ONE_HOUR`: 1시간<br/>- `ONE_HOUR_AND_HALF`: 1시간 30분<br/>- `TWO_HOURS`: 2시간<br/>- `TWO_HOURS_AND_HALF`: 2시간 30분<br/>- `THREE_HOURS`: 3시간 |
| backupSchedules.backupRetryExpireTime | Body | String  | O  | 백업 재시도 만료 시각<br/>- 백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.<br/>- 예시: `01:30:00`                                                                                                                                              |

<details><summary>예시</summary>
<p>

```json
{
    "backupPeriod": 5,
    "useBackupLock": true,
    "backupSchedules": [
        {
            "backupWndBgnTime": "01:00:00",
            "backupWndDuration": "TWO_HOURS",
            "backupRetryExpireTime": "03:00:00"
        }
    ]
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 네트워크 정보 보기

```
GET /v3.0/db-instances/{dbInstanceId}/network-info
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름                     | 종류   | 형식     | 설명                                                                                                                                      |
|------------------------|------|--------|-----------------------------------------------------------------------------------------------------------------------------------------|
| availabilityZone       | Body | Enum   | DB 인스턴스를 생성할 가용성 영역                                                                                                                     |
| subnet                 | Body | Object | 서브넷 객체                                                                                                                                  |
| subnet.subnetId        | Body | UUID   | 서브넷의 식별자                                                                                                                                |
| subnet.subnetName      | Body | UUID   | 서브넷을 식별할 수 있는 이름                                                                                                                        |
| subnet.subnetCidr      | Body | UUID   | 서브넷의 CIDR                                                                                                                               |
| endPoints              | Body | Array  | 접속 정보 목록                                                                                                                                |
| endPoints.domain       | Body | String | 도메인                                                                                                                                     |
| endPoints.ipAddress    | Body | String | IP 주소                                                                                                                                   |
| endPoints.endPointType | Body | Enum   | 접속 정보 타입<br>-`EXTERNAL`: 외부 접속 도메인<br>-`INTERNAL`: 내부 접속 도메인<br>-`PUBLIC`: (Deprecated) 외부 접속 도메인<br>-`PRIVATE`: (Deprecated) 내부 접속 도메인 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "availabilityZone": "kr-pub-a",
    "subnet": {
        "subnetId": "bd453789-34ae-416c-9f78-05b9e43a46be",
        "subnetName": "Default Network",
        "subnetCidr": "192.168.0.0/16"
    },
    "endPoints": [
        {
            "domain": "ea548a78-d85f-43b4-8ddf-c88d999b9905.internal.kr1.mysql.rds.nhncloudservice.com",
            "ipAddress": "192.168.0.2",
            "endPointType": "INTERNAL"
        }
    ]
}
```

</p>
</details>

---

### 네트워크 정보 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}/network-info
```

#### 요청

| 이름              | 종류   | 형식      | 필수 | 설명           |
|-----------------|------|---------|----|--------------|
| dbInstanceId    | URL  | UUID    | O  | DB 인스턴스의 식별자 |
| usePublicAccess | Body | Boolean | O  | 외부 접속 가능  여부 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 사용자 목록 보기

```
GET /v3.0/db-instances/{dbInstanceId}/db-users
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름                           | 종류   | 형식       | 설명                                                                                                                          |
|------------------------------|------|----------|-----------------------------------------------------------------------------------------------------------------------------|
| dbUsers                      | Body | Array    | DB 사용자 목록                                                                                                                   |
| dbUsers.dbUserId             | Body | UUID     | DB 사용자의 식별자                                                                                                                 |
| dbUsers.dbUserName           | Body | String   | DB 사용자 계정 이름                                                                                                                |
| dbUsers.host                 | Body | String   | DB 사용자 계정의 호스트 이름                                                                                                           |
| dbUsers.authorityType        | Body | Enum     | DB 사용자 권한 타입<br/>- `READ`: SELECT 쿼리 수행 가능한 권한<br/>- `CRUD`: DML 쿼리 수행 가능한 권한<br/>- `DDL`: DDL 쿼리 수행 가능한 권한<br/>            |
| dbUsers.dbUserStatus         | Body | Enum     | DB 사용자의 현재 상태<br/>- `STABLE`: 생성됨<br/>- `CREATING`: 생성 중<br/>- `UPDATING`: 수정 중<br/>- `DELETING`: 삭제 중<br/>- `DELETED`: 삭제됨 |
| dbUsers.authenticationPlugin | Body | Enum     | 인증 플러그인<br/>- NATIVE: `mysql_native_password`<br />- SHA256: sha256_password<br />- CACHING_SHA2: caching_sha2_password     |
| dbUsers.tlsOption            | Body | Enum     | TLS Option<br/>- NONE<br />- SSL<br />- X509                                                                                |
| dbUsers.createdYmdt          | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                           |
| dbUsers.updatedYmdt          | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                           |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbUsers": [
        {
            "dbUserId": "4b3d530b-fd02-4d59-a620-83d019a67bbb",
            "dbUserName": "db-user",
            "host": "%",
            "authorityType": "DDL",
            "dbUserStatus": "STABLE",
            "authenticationPlugin": "NATIVE",
            "tlsOption": "NONE",
            "createdYmdt": "2023-03-17T14:02:29+09:00",
            "updatedYmdt": "2023-03-17T14:02:31+09:00"
        }
    ]
}
```

</p>
</details>

---

### DB 사용자 생성하기

```
POST /v3.0/db-instances/{dbInstanceId}/db-users
```

#### 요청

| 이름                   | 종류   | 형식     | 필수 | 설명                                                                                                                      |
|----------------------|------|--------|----|-------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId         | URL  | UUID   | O  | DB 인스턴스의 식별자                                                                                                            |
| dbUserName           | Body | String | O  | DB 사용자 계정 이름<br/>- 최소 길이: `1`<br/>- 최대 길이: `32`                                                                         |
| dbPassword           | Body | String | O  | DB 사용자 계정 암호<br/>- 최소 길이: `4`<br/>- 최대 길이: `16`                                                                         |
| host                 | Body | String | O  | DB 사용자 계정의 호스트명<br/>- 예시: `1.1.1.%`                                                                                     |
| authorityType        | Body | Enum   | O  | DB 사용자 권한 타입<br/>- `READ`: SELECT 쿼리 수행 가능한 권한<br/>- `CRUD`: DML 쿼리 수행 가능한 권한<br/>- `DDL`: DDL 쿼리 수행 가능한 권한<br/>        |
| authenticationPlugin | Body | Enum   | X  | 인증 플러그인<br/>- NATIVE: `mysql_native_password`<br />- SHA256: sha256_password<br />- CACHING_SHA2: caching_sha2_password |
| tlsOption            | Body | Enum   | X  | TLS Option<br/>- NONE<br />- SSL<br />- X509                                                                            |

> [주의]
> DB 인스턴스의 `supportAuthenticationPlugin` 값이 true인 DB 인스턴스만 `authenticationPlugin`, `tlsOption`의 값을 설정할 수 있습니다.

<details><summary>예시</summary>
<p>

```json
{
    "dbUserName": "db-user",
    "dbPassword": "password",
    "host": "1.1.1.%",
    "authorityType": "CRUD",
    "authenticationPlugin": "NATIVE",
    "tlsOption": "NONE"
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 사용자 수정하기

```
PUT /v3.0/db-instances/{dbInstanceId}/db-users/{dbUserId}
```

#### 요청

| 이름                   | 종류   | 형식     | 필수 | 설명                                                                                                                      |
|----------------------|------|--------|----|-------------------------------------------------------------------------------------------------------------------------|
| dbInstanceId         | URL  | UUID   | O  | DB 인스턴스의 식별자                                                                                                            |
| dbUserId             | URL  | UUID   | O  | DB 사용자의 식별자                                                                                                             |
| dbPassword           | Body | String | X  | DB 사용자 계정 암호<br/>- 최소 길이: `4`<br/>- 최대 길이: `16`                                                                         |
| authorityType        | Body | Enum   | X  | DB 사용자 권한 타입<br/>- `READ`: SELECT 쿼리 수행 가능한 권한<br/>- `CRUD`: DML 쿼리 수행 가능한 권한<br/>- `DDL`: DDL 쿼리 수행 가능한 권한<br/>        |
| authenticationPlugin | Body | Enum   | X  | 인증 플러그인<br/>- NATIVE: `mysql_native_password`<br />- SHA256: sha256_password<br />- CACHING_SHA2: caching_sha2_password |
| tlsOption            | Body | Enum   | X  | TLS Option<br/>- NONE<br />- SSL<br />- X509                                                                            |

> [주의]
> DB 인스턴스의 `supportAuthenticationPlugin` 값이 true인 DB 인스턴스만 `authenticationPlugin`, `tlsOption`의 값을 수정할 수 있습니다.
> `authenticationPlugin`의 값은 `dbPassword`와 동시에 수정을 해야 합니다.

<details><summary>예시</summary>
<p>

```json
{
    "authorityType": "DDL"
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 사용자 삭제하기

```
DELETE /v3.0/db-instances/{dbInstanceId}/db-users/{dbUserId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |
| dbUserId     | URL | UUID | O  | DB 사용자의 식별자  |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 스키마 목록 보기

```
GET /v3.0/db-instances/{dbInstanceId}/db-schemas
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |

#### 응답

| 이름                       | 종류   | 형식       | 설명                                                                                                   |
|--------------------------|------|----------|------------------------------------------------------------------------------------------------------|
| dbSchemas                | Body | Array    | DB 스키마 목록                                                                                            |
| dbSchemas.dbSchemaId     | Body | UUID     | DB 스키마의 식별자                                                                                          |
| dbSchemas.dbSchemaName   | Body | String   | DB 스키마 이름                                                                                            |
| dbSchemas.dbSchemaStatus | Body | Enum     | DB 스키마의 현재 상태<br/>- `STABLE`: 생성됨<br/>- `CREATING`: 생성 중<br/>- `DELETING`: 삭제 중<br/>- `DELETED`: 삭제됨 |
| dbSchemas.createdYmdt    | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                    |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSchemas": [
        {
            "dbSchemaId": "7c9a94b8-86c1-435d-8af2-82a5e9d53fd4",
            "dbSchemaName": "schema",
            "dbSchemaStatus": "STABLE",
            "createdYmdt": "2023-03-20T13:37:45+09:00"
        }
    ]
}
```

</p>
</details>

---

### DB 스키마 생성하기

```
POST /v3.0/db-instances/{dbInstanceId}/db-schemas
```

#### 요청

| 이름           | 종류   | 형식     | 필수 | 설명           |
|--------------|------|--------|----|--------------|
| dbInstanceId | URL  | UUID   | O  | DB 인스턴스의 식별자 |
| dbSchemaName | Body | String | O  | DB 스키마 이름    |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 스키마 삭제하기

```
DELETE /v3.0/db-instances/{dbInstanceId}/db-schemas/{dbSchemaId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류  | 형식   | 필수 | 설명           |
|--------------|-----|------|----|--------------|
| dbInstanceId | URL | UUID | O  | DB 인스턴스의 식별자 |
| dbSchemaId   | URL | UUID | O  | DB 스키마의 식별자  |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

## 백업

### 백업 상태

| 상태           | 설명           |
|--------------|--------------|
| `BACKING_UP` | 백업 중인 경우     |
| `COMPLETED`  | 백업이 완료된 경우   |
| `DELETING`   | 백업이 삭제 중인 경우 |
| `DELETED`    | 백업이 삭제된 경우   |
| `ERROR`      | 오류가 발생한 경우   |

### 백업 목록 조회

```
GET /v3.0/backups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름           | 종류    | 형식     | 필수 | 설명                                                       |
|--------------|-------|--------|----|----------------------------------------------------------|
| page         | Query | Number | O  | 조회할 목록의 페이지<br/>- 최솟값: `1`                               |
| size         | Query | Number | O  | 조회할 목록의 페이지 크기<br/>- 최솟값: `1`<br/>- 최댓값: `100`           |
| backupType   | Query | Enum   | X  | 백업 유형<br/>- `AUTO`: 자동<br/>- `MANUAL`:  수동<br/>- 기본값: 전체 |
| dbInstanceId | Query | UUID   | X  | 원본 DB 인스턴스의 식별자                                          |
| dbVersion    | Query | Enum   | X  | DB 엔진 유형                                                 |

#### 응답

| 이름                   | 종류   | 형식       | 설명                                |
|----------------------|------|----------|-----------------------------------|
| totalCounts          | Body | Number   | 전체 백업 목록 수                        |
| backups              | Body | Array    | 백업 목록                             |
| backups.backupId     | Body | UUID     | 백업의 식별자                           |
| backups.backupName   | Body | String   | 백업을 식별할 수 있는 이름                   |
| backups.backupStatus | Body | Enum     | 백업의 현재 상태                         |
| backups.dbInstanceId | Body | UUID     | 원본 DB 인스턴스의 식별자                   |
| backups.dbVersion    | Body | Enum     | DB 엔진 유형                          |
| backups.utilVersion  | Body | String   | 유틸리티 버전                           |
| backups.utilVersion  | Body | String   | 백업에 사용된 xtrabackup 유틸리티 버전        |
| backups.backupType   | Body | Enum     | 백업 유형                             |
| backups.backupSize   | Body | Number   | 백업의 크기(Byte)                      |
| createdYmdt          | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| updatedYmdt          | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "backups": [
        {
            "backupId": "0017f136-3e01-4530-94aa-20661afe6632",
            "backupName": "backup",
            "backupStatus": "COMPLETED",
            "dbInstanceId": "142e6ccc-3bfb-4e1e-84f7-38861284fafd",
            "dbVersion": "MYSQL_V8028",
            "utilVersion": "8.0.28",
            "backupType": "AUTO",
            "backupSize": 4996786,
            "createdYmdt": "2023-02-21T00:35:00+09:00",
            "updatedYmdt": "2023-02-22T00:35:32+09:00"
        }
    ]
}
```

</p>
</details>

---

### 백업 내보내기

```
POST /v3.0/backups/{backupId}/export
```

#### 요청

| 이름              | 종류   | 형식     | 필수 | 설명                          |
|-----------------|------|--------|----|-----------------------------|
| backupId        | URL  | UUID   | O  | 백업의 식별자                     |
| tenantId        | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 테넌트 ID   |
| username        | Body | String | O  | NHN Cloud 회원 또는 IAM 멤버 ID   |
| password        | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 API 비밀번호 |
| targetContainer | Body | String | O  | 백업이 저장될 오브젝트 스토리지의 컨테이너     |
| objectPath      | Body | String | O  | 컨테이너에 저장될 백업의 경로            |

<details><summary>예시</summary>
<p>

```json
{
    "tenantId": "399631c404744dbbb18ce4fa2dc71a5a",
    "username": "gildong.hong@nhn.com",
    "password": "password",
    "targetContainer": "/container",
    "objectPath": "/backups/backup_file"
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

> [주의]
> 수동 백업의 경우 백업이 수행된 DB 인스턴스가 존재하지 않으면, 백업을 오브젝트 스토리지로 내보낼 수 없습니다.

---

### 백업 복원하기

```
POST /v3.0/backups/{backupId}/restore
```

#### 요청

| 이름                                           | 종류   | 형식      | 필수 | 설명                                                                                                                                                                                                                          |
|----------------------------------------------|------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| backupId                                     | URL  | UUID    | O  | 백업의 식별자                                                                                                                                                                                                                     |
| dbInstanceName                               | Body | String  | O  | DB 인스턴스를 식별할 수 있는 이름                                                                                                                                                                                                        |
| description                                  | Body | String  | X  | DB 인스턴스에 대한 추가 정보                                                                                                                                                                                                           |
| dbFlavorId                                   | Body | UUID    | O  | DB 인스턴스 사양의 식별자                                                                                                                                                                                                             |
| dbPort                                       | Body | Integer | O  | DB 포트<br/>- 최솟값: `3306`<br/>- 최댓값: `43306`                                                                                                                                                                                  |
| parameterGroupId                             | Body | UUID    | O  | 파라미터 그룹의 식별자                                                                                                                                                                                                                |
| dbSecurityGroupIds                           | Body | Array   | X  | DB 보안 그룹의 식별자 목록                                                                                                                                                                                                            ||network|Body|Object|O|네트워크 정보 객체|
| userGroupIds                                 | Body | Array   | X  | 사용자 그룹의 식별자 목록                                                                                                                                                                                                              |
| useHighAvailability                          | Body | Boolean | X  | 고가용성 사용 여부<br/>- 기본값: `false`                                                                                                                                                                                               |
| pingInterval                                 | Body | Number  | X  | 고가용성 사용 시 Ping 간격(초)<br/>- 기본값: `3`<br/>- 최솟값: `1`<br/>- 최댓값: `600`                                                                                                                                                         |
| useDefaultNotification                       | Body | Boolean | X  | 기본 알림 사용 여부<br/>- 기본값: `false`                                                                                                                                                                                              |
| useDeletionProtection                        | Body | Boolean | X  | 삭제 보호 여부<br/>- 기본값: `false`                                                                                                                                                                                                 | 
| network                                      | Body | Object  | O  | 네트워크 정보 객체                                                                                                                                                                                                                  |
| network.subnetId                             | Body | UUID    | O  | 서브넷의 식별자                                                                                                                                                                                                                    |
| network.usePublicAccess                      | Body | Boolean | X  | 외부 접속 가능 여부<br/>- 기본값: `false`                                                                                                                                                                                              |
| network.availabilityZone                     | Body | Enum    | O  | DB 인스턴스를 생성할 가용성 영역<br/>- 예시: `kr-pub-a`                                                                                                                                                                                    |
| storage                                      | Body | Object  | O  | 스토리지 정보 객체                                                                                                                                                                                                                  |    
| storage.storageType                          | Body | Enum    | O  | 데이터 스토리지 타입<br/>- 예시: `General SSD`                                                                                                                                                                                         |
| storage.storageSize                          | Body | Number  | O  | 데이터 스토리지 크기(GB)<br/>- 최솟값: `20`<br/>- 최댓값: `2048`                                                                                                                                                                           |
| backup                                       | Body | Object  | O  | 백업 정보 객체                                                                                                                                                                                                                    |
| backup.backupPeriod                          | Body | Number  | O  | 백업 보관 기간(일)<br/>- 최솟값: `0`<br/>- 최댓값: `730`                                                                                                                                                                                 |
| backup.ftwrlWaitTimeout                      | Body | Number  | X  | 쿼리 지연 대기 시간(초)<br/>- 기본값: `1800`<br/>- 최솟값: `0`<br/>- 최댓값: `21600`                                                                                                                                                          |
| backup.backupRetryCount                      | Body | Number  | X  | 백업 재시도 횟수<br/>- 기본값: `0`<br/>- 최솟값: `0`<br/>- 최댓값: `10`                                                                                                                                                                     |
| backup.replicationRegion                     | Body | Enum    | X  | 백업 복제 리전<br />- `KR1`: 한국(판교)<br/>- `KR2`: 한국(평촌)<br/>- `JP1`: 일본(도쿄)                                                                                                                                                       |
| backup.useBackupLock                         | Body | Boolean | X  | 테이블 잠금 사용 여부<br/>- 기본값: `true`                                                                                                                                                                                              |
| backup.backupSchedules                       | Body | Array   | O  | 백업 스케줄 목록                                                                                                                                                                                                                   |
| backup.backupSchedules.backupWndBgnTime      | Body | String  | O  | 백업 시작 시각<br/>- 예시: `00:00:00`                                                                                                                                                                                               |
| backup.backupSchedules.backupWndDuration     | Body | Enum    | O  | 백업 Duration<br/>백업 시작 시각부터 Duration 안에 자동 백업이 실행됩니다.<br/>- `HALF_AN_HOUR`: 30분<br/>- `ONE_HOUR`: 1시간<br/>- `ONE_HOUR_AND_HALF`: 1시간 30분<br/>- `TWO_HOURS`: 2시간<br/>- `TWO_HOURS_AND_HALF`: 2시간 30분<br/>- `THREE_HOURS`: 3시간 |
| backup.backupSchedules.backupRetryExpireTime | Body | String  | O  | 백업 재시도 만료 시각<br/>- 백업 재시도 만료 시각은 백업 시작 시각 이전이거나 이후여야 합니다.<br/>- 예시: `01:30:00`                                                                                                                                              |

<details><summary>예시</summary>
<p>

```json

{
    "dbInstanceName": "db-instance-restore",
    "dbFlavorId": "50be6d9c-02d6-4594-a2d4-12010eb65ec0",
    "dbPort": 10000,
    "parameterGroupId": "132d383c-38e3-468a-a826-5e9a8fff15d0",
    "network": {
        "subnetId": "e721a9dd-dad0-4cf0-a53b-dd654ebfc683",
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20
    },
    "backup": {
        "backupPeriod": 1,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR",
                "backupRetryExpireTime": "01:30:00"
            }
        ]
    }
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### 백업 삭제하기

```
DELETE /v3.0/backups/{backupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름       | 종류  | 형식   | 필수 | 설명      |
|----------|-----|------|----|---------|
| backupId | URL | UUID | O  | 백업의 식별자 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

## DB 보안 그룹

### DB 보안 그룹 진행 상태

| 상태              | 설명           |
|-----------------|--------------|
| `NONE`          | 진행 중인 작업이 없음 |
| `CREATING_RULE` | 규칙 정책 생성 중   |
| `UPDATING_RULE` | 규칙 정책 수정 중   |
| `DELETING_RULE` | 규칙 정책 삭제 중   |

### DB 보안 그룹 목록 보기

```
GET /v3.0/db-security-groups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                                   | 종류   | 형식       | 설명                                |
|--------------------------------------|------|----------|-----------------------------------|
| dbSecurityGroups                     | Body | Array    | DB 보안 그룹 목록                       |
| dbSecurityGroups.dbSecurityGroupId   | Body | UUID     | DB 보안 그룹의 식별자                     |
| dbSecurityGroups.dbSecurityGroupName | Body | String   | DB 보안 그룹을 식별할 수 있는 이름             |
| dbSecurityGroups.description         | Body | String   | DB 보안 그룹에 대한 추가 정보                |
| dbSecurityGroups.progressStatus      | Body | Enum     | DB 보안 그룹의 현재 진행 상태                |
| dbSecurityGroups.createdYmdt         | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| dbSecurityGroups.updatedYmdt         | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSecurityGroups": [
        {
            "dbSecurityGroupId": "fe4f2aee-afbb-4c19-a5e9-eb2eab394708",
            "dbSecurityGroupName": "dbSecurityGroup",
            "description": "description",
            "progressStatus": "NONE",
            "createdYmdt": "2023-02-19T19:18:13+09:00",
            "updatedYmdt": "2022-02-19T19:18:13+09:00"
        }
    ]
}
```

</p>
</details>

---

### DB 보안 그룹 상세 보기

```
GET /v3.0/db-security-groups/{dbSecurityGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                | 종류  | 형식   | 필수 | 설명            |
|-------------------|-----|------|----|---------------|
| dbSecurityGroupId | URL | UUID | O  | DB 보안 그룹의 식별자 |

#### 응답

| 이름                  | 종류   | 형식       | 설명                                                                                                                 |
|---------------------|------|----------|--------------------------------------------------------------------------------------------------------------------|
| dbSecurityGroupId   | Body | UUID     | DB 보안 그룹의 식별자                                                                                                      |
| dbSecurityGroupName | Body | String   | DB 보안 그룹을 식별할 수 있는 이름                                                                                              |
| description         | Body | String   | DB 보안 그룹에 대한 추가 정보                                                                                                 |
| progressStatus      | Body | Enum     | DB 보안 그룹의 현재 진행 상태                                                                                                 |
| rules               | Body | Array    | DB 보안 그룹 규칙 목록                                                                                                     |
| rules.ruleId        | Body | UUID     | DB 보안 그룹 규칙의 식별자                                                                                                   |
| rules.description   | Body | String   | DB 보안 그룹 규칙에 대한 추가 정보                                                                                              |
| rules.direction     | Body | Enum     | 통신 방향<br/>- `INGRESS`: 수신<br/>- `EGRESS`: 송신                                                                       |
| rules.etherType     | Body | Enum     | Ether 타입<br/>- `IPV4`: IPv4<br/>- `IPV6`: IPv6                                                                     |
| rules.port          | Body | Object   | 포트 객체                                                                                                              |
| rules.port.portType | Body | Enum     | 포트 타입<br/>- `DB_PORT`: 각 DB 인스턴스 포트값으로 설정됩니다.<br/>- `PORT`: 지정된 포트값으로 설정됩니다.<br/>- `PORT_RANGE`: 지정된 포트 범위로 설정됩니다. |
| rules.port.minPort  | Body | Number   | 최소 포트 범위                                                                                                           |
| rules.port.maxPort  | Body | Number   | 최대 포트 범위                                                                                                           |
| rules.cidr          | Body | String   | 허용할 트래픽의 원격 소스                                                                                                     |
| rules.createdYmdt   | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                  |
| rules.updatedYmdt   | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                  |
| createdYmdt         | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                  |
| updatedYmdt         | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                  |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSecurityGroup": {
        "dbSecurityGroupId": "fe4f2aee-afbb-4c19-a5e9-eb2eab394708",
        "dbSecurityGroupName": "dbSecurityGroup",
        "description": "description",
        "progressStatus": "NONE",
        "rules": [
            {
                "ruleId": "17c88ef6-95f1-4678-84f9-fee1b22e250d",
                "description": "description",
                "direction": "INGRESS",
                "etherType": "IPV4",
                "port": {
                    "portType": "PORT_RANGE",
                    "minPort": 10000,
                    "maxPort": 10005
                },
                "cidr": "0.0.0.0/0",
                "createdYmdt": "2023-02-19T19:18:13+09:00",
                "updatedYmdt": "2023-02-19T19:18:13+09:00"
            }
        ],
        "createdYmdt": "2023-02-19T19:18:13+09:00",
        "updatedYmdt": "2023-02-19T19:18:13+09:00"
    }
}
```

</p>
</details>

---

### DB 보안 그룹 생성하기

```
POST /v3.0/db-security-groups
```

#### 요청

| 이름                  | 종류   | 형식     | 필수 | 설명                                                                                                                                                                                       |
|---------------------|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbSecurityGroupName | Body | String | O  | DB 보안 그룹을 식별할 수 있는 이름                                                                                                                                                                    |
| description         | Body | String | X  | DB 보안 그룹에 대한 추가 정보                                                                                                                                                                       |
| rules               | Body | Array  | O  | DB 보안 그룹 규칙 목록                                                                                                                                                                           |
| rules.description   | Body | String | X  | DB 보안 그룹 규칙에 대한 추가 정보                                                                                                                                                                    |
| rules.direction     | Body | Enum   | O  | 통신 방향<br/>- `INGRESS`: 수신<br/>- `EGRESS`: 송신                                                                                                                                             |
| rules.etherType     | Body | Enum   | O  | Ether 타입<br/>- `IPV4`: IPv4<br/>- `IPV6`: IPv6                                                                                                                                           |
| rules.cidr          | Body | String | O  | 허용할 트래픽의 원격 소스<br/>- 예시: `1.1.1.1/32`                                                                                                                                                    |
| rules.port          | Body | Object | O  | 포트 객체                                                                                                                                                                                    |
| rules.port.portType | Body | Enum   | O  | 포트 타입<br/>- `DB_PORT`: 각 DB 인스턴스 포트값으로 설정됩니다. `minPort`값과 `maxPort`값을 필요로 하지 않습니다.<br/>- `PORT`: 지정된 포트값으로 설정됩니다. `minPort`값과 `maxPort`값이 같아야 합니다.<br/>- `PORT_RANGE`: 지정된 포트 범위로 설정됩니다. |
| rules.port.minPort  | Body | Number | X  | 최소 포트 범위<br/>- 최솟값: 1                                                                                                                                                                    |
| rules.port.maxPort  | Body | Number | X  | 최대 포트 범위<br/>- 최댓값: 65535                                                                                                                                                                |

<details><summary>예시</summary>
<p>

```json
{
    "dbSecurityGroupName": "dbSecurityGroup",
    "description": "description",
    "rules": [
        {
            "direction": "INGRESS",
            "etherType": "IPV4",
            "port": {
                "portType": "PORT_RANGE",
                "minPort": 10000,
                "maxPort": 10005
            },
            "cidr": "0.0.0.0/0"
        }
    ]
}
```

</p>
</details>

#### 응답

| 이름                | 종류   | 형식   | 설명            |
|-------------------|------|------|---------------|
| dbSecurityGroupId | Body | UUID | DB 보안 그룹의 식별자 |

---

### DB 보안 그룹 수정하기

```
PUT /v3.0/db-security-groups/{dbSecurityGroupId}
```

#### 요청

| 이름                  | 종류   | 형식     | 필수 | 설명                    |
|---------------------|------|--------|----|-----------------------|
| dbSecurityGroupId   | URL  | UUID   | O  | DB 보안 그룹의 식별자         |
| dbSecurityGroupName | Body | String | X  | DB 보안 그룹을 식별할 수 있는 이름 |
| description         | Body | String | X  | DB 보안 그룹에 대한 추가 정보    |

<details><summary>예시</summary>
<p>

```json
{
    "dbSecurityGroupName": "dbSecurityGroup",
    "description": "description"
}
```

</p>
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>


---

### DB 보안 그룹 삭제하기

```
DELETE /v3.0/db-security-groups/{dbSecurityGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                | 종류  | 형식   | 필수 | 설명            |
|-------------------|-----|------|----|---------------|
| dbSecurityGroupId | URL | UUID | O  | DB 보안 그룹의 식별자 |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### DB 보안 그룹 규칙 생성하기

```
POST /v3.0/db-security-groups/{dbSecurityGroupId}/rules
```

#### 요청

| 이름                | 종류   | 형식     | 필수 | 설명                                                                                                                                                                                       |
|-------------------|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbSecurityGroupId | URL  | UUID   | O  | DB 보안 그룹의 식별자                                                                                                                                                                            |
| description       | Body | String | X  | DB 보안 그룹 규칙에 대한 추가 정보                                                                                                                                                                    |
| direction         | Body | Enum   | O  | 통신 방향<br/>- `INGRESS`: 수신<br/>- `EGRESS`: 송신                                                                                                                                             |
| etherType         | Body | Enum   | O  | Ether 타입<br/>- `IPV4`: IPv4<br/>- `IPV6`: IPv6                                                                                                                                           |
| port              | Body | Object | O  | 포트 객체                                                                                                                                                                                    |
| port.portType     | Body | Enum   | O  | 포트 타입<br/>- `DB_PORT`: 각 DB 인스턴스 포트값으로 설정됩니다. `minPort`값과 `maxPort`값을 필요로 하지 않습니다.<br/>- `PORT`: 지정된 포트값으로 설정됩니다. `minPort`값과 `maxPort`값이 같아야 합니다.<br/>- `PORT_RANGE`: 지정된 포트 범위로 설정됩니다. |
| port.minPort      | Body | Number | X  | 최소 포트 범위<br/>- 최솟값: 1                                                                                                                                                                    |
| port.maxPort      | Body | Number | X  | 최대 포트 범위<br/>- 최댓값: 65535                                                                                                                                                                |
| cidr              | Body | String | O  | 허용할 트래픽의 원격 소스<br/>- 예시: `1.1.1.1/32`                                                                                                                                                    |

<details><summary>예시</summary>
<p>

```json
{
    "direction": "INGRESS",
    "etherType": "IPV4",
    "port": {
        "portType": "PORT",
        "minPort": 10000,
        "maxPort": 10000
    },
    "cidr": "0.0.0.0/0"
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 보안 그룹 규칙 수정하기

```
PUT /v3.0/db-security-groups/{dbSecurityGroupId}/rules/{ruleId}
```

#### 요청

| 이름                | 종류   | 형식     | 필수 | 설명                                                                                                                                                                                       |
|-------------------|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dbSecurityGroupId | URL  | UUID   | O  | DB 보안 그룹의 식별자                                                                                                                                                                            |
| ruleId            | URL  | UUID   | O  | DB 보안 그룹 규칙의 식별자                                                                                                                                                                         |
| description       | Body | String | X  | DB 보안 그룹 규칙에 대한 추가 정보                                                                                                                                                                    |
| direction         | Body | Enum   | O  | 통신 방향<br/>- `INGRESS`: 수신<br/>- `EGRESS`: 송신                                                                                                                                             |
| etherType         | Body | Enum   | O  | Ether 타입<br/>- `IPV4`: IPv4<br/>- `IPV6`: IPv6                                                                                                                                           |
| port              | Body | Object | O  | 포트 객체                                                                                                                                                                                    |
| port.portType     | Body | Enum   | O  | 포트 타입<br/>- `DB_PORT`: 각 DB 인스턴스 포트값으로 설정됩니다. `minPort`값과 `maxPort`값을 필요로 하지 않습니다.<br/>- `PORT`: 지정된 포트값으로 설정됩니다. `minPort`값과 `maxPort`값이 같아야 합니다.<br/>- `PORT_RANGE`: 지정된 포트 범위로 설정됩니다. |
| port.minPort      | Body | Number | X  | 최소 포트 범위<br/>- 최솟값: 1                                                                                                                                                                    |
| port.maxPort      | Body | Number | X  | 최대 포트 범위<br/>- 최댓값: 65535                                                                                                                                                                |
| cidr              | Body | String | O  | 허용할 트래픽의 원격 소스<br/>- 예시: `1.1.1.1/32`                                                                                                                                                    |

<details><summary>예시</summary>
<p>

```json
{
    "direction": "INGRESS",
    "etherType": "IPV4",
    "port": {
        "portType": "DB_PORT"
    },
    "cidr": "0.0.0.0/0"
}
```

</p>
</details>

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

### DB 보안 그룹 규칙 삭제하기

```
DELETE /v3.0/db-security-groups/{dbSecurityGroupId}/rules
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                | 종류    | 형식    | 필수 | 설명                  |
|-------------------|-------|-------|----|---------------------|
| dbSecurityGroupId | URL   | UUID  | O  | DB 보안 그룹의 식별자       |
| ruleIds           | Query | Array | O  | DB 보안 그룹 규칙의 식별자 목록 |

#### 응답

| 이름    | 종류   | 형식   | 설명          |
|-------|------|------|-------------|
| jobId | Body | UUID | 요청한 작업의 식별자 |

---

## 파라미터 그룹

### 파라미터 그룹 목록 보기

```
GET /v3.0/parameter-groups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름        | 종류    | 형식   | 필수 | 설명       |
|-----------|-------|------|----|----------|
| dbVersion | Query | Enum | X  | DB 엔진 유형 |

#### 응답

| 이름                                   | 종류   | 형식       | 설명                                                                |
|--------------------------------------|------|----------|-------------------------------------------------------------------|
| parameterGroups                      | Body | Array    | 파라미터 그룹 목록                                                        |
| parameterGroups.parameterGroupId     | Body | UUID     | 파라미터 그룹의 식별자                                                      |
| parameterGroups.parameterGroupName   | Body | String   | 파라미터 그룹을 식별할 수 있는 이름                                              |
| parameterGroups.description          | Body | String   | 파라미터 그룹에 대한 추가 정보                                                 |
| parameterGroups.dbVersion            | Body | Enum     | DB 엔진 유형                                                          |
| parameterGroups.parameterGroupStatus | Body | Enum     | 파라미터 그룹의 현재 상태<br/>- `STABLE`: 적용 완료<br/>- `NEED_TO_APPLY`: 적용 필요 |
| parameterGroups.createdYmdt          | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                 |
| parameterGroups.updatedYmdt          | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "parameterGroups": [
        {
            "parameterGroupId": "404e8a89-ca4d-4fca-96c2-1518754d50b7",
            "parameterGroupName": "parameter-group",
            "description": null,
            "dbVersion": "MYSQL_V8023",
            "parameterGroupStatus": "STABLE",
            "createdYmdt": "2023-02-31T15:28:17+09:00",
            "updatedYmdt": "2023-02-31T15:28:17+09:00"
        }
    ]
}
```

</p>
</details>


---

### 파라미터 그룹 상세 보기

```
GET /v3.0/parameter-groups/{parameterGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름               | 종류  | 형식   | 필수 | 설명           |
|------------------|-----|------|----|--------------|
| parameterGroupId | URL | UUID | O  | 파라미터 그룹의 식별자 |

#### 응답

| 이름                            | 종류   | 형식       | 설명                                                                                                     |
|-------------------------------|------|----------|--------------------------------------------------------------------------------------------------------|
| parameterGroupId              | Body | UUID     | 파라미터 그룹의 식별자                                                                                           |
| parameterGroupName            | Body | String   | 파라미터 그룹을 식별할 수 있는 이름                                                                                   |
| description                   | Body | String   | 파라미터 그룹에 대한 추가 정보                                                                                      |
| dbVersion                     | Body | Enum     | DB 엔진 유형                                                                                               |
| parameterGroupStatus          | Body | Enum     | 파라미터 그룹의 현재 상태<br/>- `STABLE`: 적용 완료<br/>- `NEED_TO_APPLY`: 적용 필요                                      |
| parameters                    | Body | Array    | 파라미터 목록                                                                                                |
| parameters.parameterId        | Body | UUID     | 파라미터 식별자                                                                                               |
| parameters.parameterFileGroup | Body | Enum     | 파라미터 파일 그룹 타입<br/>- `CLIENT`: client<br/>- `MYSQL`: mysql<br/>- `MYSQLD`: mysqld                       |
| parameters.parameterName      | Body | String   | 파라미터 이름                                                                                                |
| parameters.fileParameterName  | Body | String   | 파라미터 파일 이름                                                                                             |
| parameters.value              | Body | String   | 현재 설정된 값                                                                                               |
| parameters.defaultValue       | Body | String   | 기본값                                                                                                    |
| parameters.allowedValue       | Body | String   | 허용된 값                                                                                                  |
| parameters.updateType         | Body | Enum     | 수정 타입<br/>- `VARIABLE`: 언제든 수정 가능<br/>- `CONSTANT`: 수정 불가능<br/>- `INIT_VARIABLE`: DB 인스턴스 생성 시에만 수정 가능 |
| parameters.applyType          | Body | Enum     | 적용 타입<br/>- `SESSION`: 세션 적용<br/>- `FILE`: 설정 파일 적용(재시작 필요)<br/>- `BOTH`: 전체(재시작 필요)                   |
| createdYmdt                   | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                      |
| updatedYmdt                   | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                      |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "parameterGroupId": "404e8a89-ca4d-4fca-96c2-1518754d50b7",
    "parameterGroupName": "parameter-group",
    "description": null,
    "dbVersion": "MYSQL_V8023",
    "parameterGroupStatus": "STABLE",
    "parameters": [
        {
            "parameterId": "fa040b5e-f29f-46de-8f0d-bba4cb82887a",
            "parameterFileGroup": "client",
            "parameterName": "socket",
            "fileParameterName": "socket",
            "value": "/home/tcrds/db/mysql/tmp/mysql.sock",
            "defaultValue": "/home/tcrds/db/mysql/tmp/mysql.sock",
            "allowedValue": "",
            "updateType": "CONSTANT",
            "applyType": "BOTH"
        }
    ],
    "createdYmdt": "2023-03-13T11:02:28+09:00",
    "updatedYmdt": "2023-03-13T11:02:28+09:00"
}
```

</p>
</details>


---

### 파라미터 그룹 생성하기

```
POST /v3.0/parameter-groups
```

#### 요청

| 이름                 | 종류   | 형식     | 필수 | 설명                   |
|--------------------|------|--------|----|----------------------|
| parameterGroupName | Body | String | O  | 파라미터 그룹을 식별할 수 있는 이름 |
| description        | Body | String | X  | 파라미터 그룹에 대한 추가 정보    |
| dbVersion          | Body | Enum   | O  | DB 엔진 유형             |

<details><summary>예시</summary>
<p>

```json
{
    "parameterGroupName": "parameter-group",
    "dbVersion": "MYSQL_V8023"
}
```

</p>
</details>

#### 응답

| 이름               | 종류   | 형식   | 설명           |
|------------------|------|------|--------------|
| parameterGroupId | Body | UUID | 파라미터 그룹의 식별자 |

---

### 파라미터 그룹 복사하기

```
POST /v3.0/parameter-groups/{parameterGroupId}/copy
```

#### 요청

| 이름                 | 종류   | 형식     | 필수 | 설명                   |
|--------------------|------|--------|----|----------------------|
| parameterGroupId   | URL  | UUID   | O  | 파라미터 그룹의 식별자         |
| parameterGroupName | Body | String | O  | 파라미터 그룹을 식별할 수 있는 이름 |
| description        | Body | String | X  | 파라미터 그룹에 대한 추가 정보    |

<details><summary>예시</summary>
<p>

```json
{
    "parameterGroupName": "parameter-group-copy",
    "description": "copy"
}
```

</p>
</details>

#### 응답

| 이름               | 종류   | 형식   | 설명           |
|------------------|------|------|--------------|
| parameterGroupId | Body | UUID | 파라미터 그룹의 식별자 |

---

### 파라미터 그룹 수정하기

```
PUT /v3.0/parameter-groups/{parameterGroupId}
```

#### 요청

| 이름                 | 종류   | 형식     | 필수 | 설명                   |
|--------------------|------|--------|----|----------------------|
| parameterGroupId   | URL  | UUID   | O  | 파라미터 그룹의 식별자         |
| parameterGroupName | Body | String | X  | 파라미터 그룹을 식별할 수 있는 이름 |
| description        | Body | String | X  | 파라미터 그룹에 대한 추가 정보    |

<details><summary>예시</summary>
<p>

```json
{
    "parameterGroupName": "parameter-group"
}
```

</p>
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 파라미터 수정하기

```
PUT /v3.0/parameter-groups/{parameterGroupId}/parameters
```

#### 요청

| 이름                             | 종류   | 형식     | 필수 | 설명           |
|--------------------------------|------|--------|----|--------------|
| parameterGroupId               | URL  | UUID   | O  | 파라미터 그룹의 식별자 |
| modifiedParameters             | Body | Array  | O  | 변경할 파라미터 목록  |
| modifiedParameters.parameterId | Body | UUID   | O  | 파라미터의 식별자    |
| modifiedParameters.value       | Body | String | O  | 변경할 파라미터 값   |

<details><summary>예시</summary>
<p>

```json
{
    "modifiedParameters": [
        {
            "parameterId": "3abac558-7274-44e1-9f4a-f100f53f67ba",
            "value": "0"
        }
    ]
}
```

</p>
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 파라미터 그룹 재설정하기

```
PUT /v3.0/parameter-groups/{parameterGroupId}/reset
```

#### 요청

| 이름               | 종류  | 형식   | 필수 | 설명           |
|------------------|-----|------|----|--------------|
| parameterGroupId | URL | UUID | O  | 파라미터 그룹의 식별자 |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 파라미터 그룹 삭제하기

```
DELETE /v3.0/parameter-groups/{parameterGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름               | 종류  | 형식   | 필수 | 설명           |
|------------------|-----|------|----|--------------|
| parameterGroupId | URL | UUID | O  | 파라미터 그룹의 식별자 |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

## 사용자 그룹

### 사용자 그룹 목록 보기

```
GET /v3.0/user-groups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                       | 종류   | 형식       | 설명                                |
|--------------------------|------|----------|-----------------------------------|
| userGroups               | Body | Array    | 사용자 그룹 목록                         |
| userGroups.userGroupId   | Body | UUID     | 사용자 그룹의 식별자                       |
| userGroups.userGroupName | Body | String   | 사용자 그룹을 식별할 수 있는 이름               |
| userGroups.createdYmdt   | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| userGroups.updatedYmdt   | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "userGroups": [
        {
            "userGroupId": "1aac0437-f32d-4923-ad3c-ac61c1cfdfe0",
            "userGroupName": "dev-team",
            "createdYmdt": "2023-02-23T10:07:54+09:00",
            "updatedYmdt": "2023-02-26T01:15:50+09:00"
        }
    ]
}
```

</p>
</details>

---

### 사용자 그룹 상세 보기

```
GET /v3.0/user-groups/{userGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름          | 종류  | 형식   | 필수 | 설명          |
|-------------|-----|------|----|-------------|
| userGroupId | URL | UUID | O  | 사용자 그룹의 식별자 |

#### 응답

| 이름                | 종류   | 형식       | 설명                                                                                                        |
|-------------------|------|----------|-----------------------------------------------------------------------------------------------------------|
| userGroupId       | Body | UUID     | 사용자 그룹의 식별자                                                                                               |
| userGroupName     | Body | String   | 사용자 그룹을 식별할 수 있는 이름                                                                                       |
| userGroupTypeCode | Body | Enum     | 사용자 그룹 종류    <br /> `ENTIRE`: 프로젝트 멤버 전체를 포함하는 사용자 그룹 <br /> `INDIVIDUAL_MEMBER`: 특정 프로젝트 멤버를 포함하는 사용자 그룹 |
| members           | Body | Array    | 프로젝트 멤버 목록                                                                                                |
| members.memberId  | Body | UUID     | 프로젝트 멤버의 식별자                                                                                              |
| createdYmdt       | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                         |
| updatedYmdt       | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                         |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "userGroupId": "1aac0437-f32d-4923-ad3c-ac61c1cfdfe0",
    "userGroupName": "dev-team",
	"userGroupTypeCode": "INDIVIDUAL_MEMBER",
    "members": [
        {
            "memberId": "1321e759-2ef3-4b85-9921-b13e918b24b5"
        }
    ],
    "createdYmdt": "2023-02-23T10:07:54+09:00",
    "updatedYmdt": "2023-02-26T01:15:50+09:00"
}
```

</p>
</details>

---

### 사용자 그룹 생성하기

```
POST /v3.0/user-groups
```

#### 요청

| 이름            | 종류   | 형식      | 필수 | 설명                                                          |
|---------------|------|---------|----|-------------------------------------------------------------|
| userGroupName | Body | String  | O  | 사용자 그룹을 식별할 수 있는 이름                                         |
| memberIds     | Body | Array   | O  | 프로젝트 멤버의 식별자 목록 <br /> `selectAllYN`이 true인 경우 해당 필드 값은 무시됨 |
| selectAllYN   | Body | Boolean | X  | 프로젝트 멤버 전체 유무 <br /> true인 경우 해당 그룹은 전체 멤버에 대해 설정됨          |

<details><summary>예시</summary>
<p>

```json
{
    "userGroupName": "dev-team",
    "memberIds": [
        "1321e759-2ef3-4b85-9921-b13e918b24b5"
    ]
}
```

```json
{
    "userGroupName": "dev-team",
    "selectAllYN":true
}
```

</p>
</details>

#### 응답

| 이름          | 종류   | 형식   | 설명          |
|-------------|------|------|-------------|
| userGroupId | Body | UUID | 사용자 그룹의 식별자 |

---

### 사용자 그룹 수정하기

```
PUT /v3.0/user-groups/{userGroupId}
```

#### 요청

| 이름            | 종류   | 형식      | 필수 | 설명                                                 |
|---------------|------|---------|----|----------------------------------------------------|
| userGroupId   | URL  | UUID    | O  | 사용자 그룹의 식별자                                        |
| userGroupName | Body | String  | X  | 사용자 그룹을 식별할 수 있는 이름                                |
| memberIds     | Body | Array   | X  | 프로젝트 멤버의 식별자 목록                                    |
| selectAllYN   | Body | Boolean | X  | 프로젝트 멤버 전체 유무 <br /> true인 경우 해당 그룹은 전체 멤버에 대해 설정됨 |

<details><summary>예시</summary>
<p>

```json
{
    "userGroupName": "dev-team",
    "memberIds": [
        "1321e759-2ef3-4b85-9921-b13e918b24b5",
        "f9064b09-2b15-442e-a4b0-3a5a2754555e"
    ]
}
```

</p>
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 사용자 그룹 삭제하기

```
DELETE /v3.0/user-groups/{userGroupId}
```

#### 요청

| 이름          | 종류  | 형식   | 필수 | 설명          |
|-------------|-----|------|----|-------------|
| userGroupId | URL | UUID | O  | 사용자 그룹의 식별자 |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

## 알림 그룹

### 알림 그룹 목록 보기

```
GET /v3.0/notification-groups
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                                       | 종류   | 형식       | 설명                                |
|------------------------------------------|------|----------|-----------------------------------|
| notificationGroups                       | Body | Array    | 알림 그룹 목록                          |
| notificationGroups.notificationGroupId   | Body | UUID     | 알림 그룹의 식별자                        |
| notificationGroups.notificationGroupName | Body | String   | 알림 그룹을 식별할 수 있는 이름                |
| notificationGroups.notifyEmail           | Body | Boolean  | 이메일 알림 여부                         |
| notificationGroups.notifySms             | Body | Boolean  | SMS 알림 여부                         |
| notificationGroups.isEnabled             | Body | Boolean  | 활성화 여부                            |
| notificationGroups.createdYmdt           | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| notificationGroups.updatedYmdt           | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "notificationGroups": [
        {
            "notificationGroupId": "b3901f17-9971-4d1e-8a81-8448cf533dc7",
            "notificationGroupName": "dev-team-noti",
            "notifyEmail": true,
            "notifySms": false,
            "isEnabled": true,
            "createdYmdt": "2023-02-20T13:34:13+09:00",
            "updatedYmdt": "2023-02-20T13:34:13+09:00"
        }
    ]
}
```

</p>
</details>

---

### 알림 그룹 상세 보기

```
GET /v3.0/notification-groups/{notificationGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                  | 종류  | 형식   | 필수 | 설명         |
|---------------------|-----|------|----|------------|
| notificationGroupId | URL | UUID | O  | 알림 그룹의 식별자 |

#### 응답

| 이름                         | 종류   | 형식       | 설명                                |
|----------------------------|------|----------|-----------------------------------|
| notificationGroupId        | Body | UUID     | 알림 그룹의 식별자                        |
| notificationGroupName      | Body | String   | 알림 그룹을 식별할 수 있는 이름                |
| notifyEmail                | Body | Boolean  | 이메일 알림 여부                         |
| notifySms                  | Body | Boolean  | SMS 알림 여부                         |
| isEnabled                  | Body | Boolean  | 활성화 여부                            |
| dbInstances                | Body | Array    | 감시 대상 DB 인스턴스 목록                  |
| dbInstances.dbInstanceId   | Body | UUID     | DB 인스턴스의 식별자                      |
| dbInstances.dbInstanceName | Body | String   | DB 인스턴스를 식별할 수 있는 이름              |
| userGroups                 | Body | Array    | 사용자 그룹 목록                         |
| userGroups.userGroupId     | Body | UUID     | 사용자 그룹의 식별자                       |
| userGroups.userGroupName   | Body | String   | 사용자 그룹을 식별할 수 있는 이름               |
| createdYmdt                | Body | DateTime | 생성 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| updatedYmdt                | Body | DateTime | 수정 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "notificationGroupId": "b3901f17-9971-4d1e-8a81-8448cf533dc7",
    "notificationGroupName": "dev-team-noti",
    "notifyEmail": true,
    "notifySms": false,
    "isEnabled": true,
    "dbInstances": [
        {
            "dbInstanceId": "ed5cb985-526f-4c54-9ae0-40288593de65",
            "dbInstanceName": "database"
        }
    ],
    "userGroups": [
        {
            "userGroupId": "1aac0437-f32d-4923-ad3c-ac61c1cfdfe0",
            "userGroupName": "dev-team"
        }
    ],
    "createdYmdt": "2023-02-20T13:34:13+09:00",
    "updatedYmdt": "2023-02-20T13:34:13+09:00"
}
```

</p>
</details>

---

### 알림 그룹 생성하기

```
POST /v3.0/notification-groups
```

#### 요청

| 이름                    | 종류   | 형식      | 필수 | 설명                          |
|-----------------------|------|---------|----|-----------------------------|
| notificationGroupName | Body | String  | O  | 알림 그룹을 식별할 수 있는 이름          |
| notifyEmail           | Body | Boolean | X  | 이메일 알림 여부<br/>- 기본값: `true` |
| notifySms             | Body | Boolean | X  | SMS 알림 여부<br/>- 기본값: `true` |
| isEnabled             | Body | Boolean | X  | 활성화 여부<br/>- 기본값: `true`    |
| dbInstanceIds         | Body | Array   | O  | 감시 대상 DB 인스턴스의 식별자 목록       |
| userGroupIds          | Body | Array   | O  | 사용자 그룹의 식별자 목록              |

<details><summary>예시</summary>
<p>

```json
{
    "notificationGroupName": "dev-team-noti",
    "notifyEmail": false,
    "isEnable": true,
    "dbInstanceIds": [
        "ed5cb985-526f-4c54-9ae0-40288593de65"
    ],
    "userGroupIds": [
        "1aac0437-f32d-4923-ad3c-ac61c1cfdfe0"
    ]
}
```

</p>
</details>

#### 응답

| 이름                  | 종류   | 형식   | 설명         |
|---------------------|------|------|------------|
| notificationGroupId | Body | UUID | 알림 그룹의 식별자 |

---

### 알림 그룹 수정하기

```
PUT /v3.0/notification-groups/{notificationGroupId}
```

#### 요청

| 이름                    | 종류   | 형식      | 필수 | 설명                    |
|-----------------------|------|---------|----|-----------------------|
| notificationGroupId   | URL  | UUID    | O  | 알림 그룹의 식별자            |
| notificationGroupName | Body | String  | X  | 알림 그룹을 식별할 수 있는 이름    |
| notifyEmail           | Body | Boolean | X  | 이메일 알림 여부             |
| notifySms             | Body | Boolean | X  | SMS 알림 여부             |
| isEnabled             | Body | Boolean | X  | 활성화 여부                |
| dbInstanceIds         | Body | Array   | X  | 감시 대상 DB 인스턴스의 식별자 목록 |
| userGroupIds          | Body | Array   | X  | 사용자 그룹의 식별자 목록        |

<details><summary>예시</summary>
<p>

```json
{
    "notifyEmail": true,
    "dbInstanceIds": [
        "ed5cb985-526f-4c54-9ae0-40288593de65",
        "d51b7da0-682f-47ff-b588-b739f6adc740"
    ]
}
```

</p>
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

### 알림 그룹 삭제하기

```
DELETE /v3.0/notification-groups/{notificationGroupId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                  | 종류  | 형식   | 필수 | 설명         |
|---------------------|-----|------|----|------------|
| notificationGroupId | URL | UUID | O  | 알림 그룹의 식별자 |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

</p>
</details>

---

## 모니터링

### Metric 목록 보기

```
GET /v3.0/metrics
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                  | 종류   | 형식     | 설명        |
|---------------------|------|--------|-----------|
| metrics             | Body | Array  | Metric 목록 |
| metrics.measureName | Body | Enum   | 조회 지표 유형  |
| metrics.unit        | Body | String | 측정값 단위    |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "metrics": [
        {
            "measureName": "CPU_USAGE",
            "unit": "%"
        }
    ]
}
```

</p>
</details>

---

### 통계 정보 조회

```
GET /v3.0/metric-statistics
```

#### 요청

| 이름           | 종류    | 형식       | 필수 | 설명                                |
|--------------|-------|----------|----|-----------------------------------|
| dbInstanceId | Query | UUID     | O  | DB 인스턴스의 식별자                      |
| measureNames | Query | Array    | O  | 조회 지표 목록<br/>- 최소 크기: `1`         |
| from         | Query | Datetime | O  | 시작 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| to           | Query | Datetime | O  | 종료 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |
| interval     | Query | Number   | X  | 조회 간격                             |

#### 응답

| 이름                                | 종류   | 형식        | 설명       |
|-----------------------------------|------|-----------|----------|
| metricStatistics                  | Body | Array     | 통계 정보 목록 |
| metricStatistics.measureName      | Body | Enum      | 측정 항목 유형 |
| metricStatistics.unit             | Body | String    | 측정값 단위   |
| metricStatistics.values           | Body | Array     | 측정값 목록   |
| metricStatistics.values.timestamp | Body | Timestamp | 측정 시간    |
| metricStatistics.values.value     | Body | Object    | 측정값      |

<details><summary>예시</summary>
<p>

```json
{
    "metricStatistics": [
        {
            "measureName": "MYSQL_STATUS",
            "unit": "",
            "values": [
                [
                    1679298540,
                    "1"
                ],
                [
                    1679298600,
                    "1"
                ],
                [
                    1679298660,
                    "1"
                ]
            ]
        }
    ]
}
```

</p>
</details>

---

## 이벤트

### 이벤트 카테고리

이벤트는 카테고리로 분류할 수 있으며 아래와 같습니다.

| 이벤트 카테고리    | 설명      |
|-------------|---------|
| ALL         | 전체      |
| BACKUP      | 백업      |
| DB_INSTANCE | DB 인스턴스 |
| JOB         | 작업      |
| TENANT      | 테넌트     |
| MONITORING  | 모니터링    |

### 이벤트 목록 조회

```
GET /v3.0/events
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름                | 종류    | 형식       | 필수 | 설명                                                                                                                                   |
|-------------------|-------|----------|----|--------------------------------------------------------------------------------------------------------------------------------------|
| page              | Query | Number   | O  | 조회할 목록의 페이지<br/>- 최솟값: `1`                                                                                                           |
| size              | Query | Number   | O  | 조회할 목록의 페이지 크기<br/>- 최솟값: `1`<br/>- 최댓값: `100`                                                                                       |
| from              | Query | Datetime | O  | 시작 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                    |
| to                | Query | Datetime | O  | 종료 일시(YYYY-MM-DDThh:mm:ss.SSSTZD)                                                                                                    |
| eventCategoryType | Query | Enum     | O  | 조회할 이벤트 카테고리 유형<br/>- `ALL`: 전체<br/>- `INSTANCE`: DB 인스턴스<br/>- `BACKUP`: 백업<br/>- `DB_SECURITY_GROUP`: DB 보안 그룹<br/>- `TENANT`: 테넌트 |
| sourceId          | Query | String   | X  | 이벤트가 발생한 대상 리소스의 식별자                                                                                                                 |
| keyword           | Query | String   | X  | 이벤트 메시지에 포함된 문자열 검색어                                                                                                                 |
| ascendingOrder    | Query | Enum     | X  | 이벤트 메시지 정렬 순서<br/>- `ASC`: 오름차순<br/>- `DESC`: 내림차순<br/>- 기본값: `DESC`                                                                 |

#### 응답

| 이름                       | 종류   | 형식       | 설명                                    |
|--------------------------|------|----------|---------------------------------------|
| totalCounts              | Body | Number   | 전체 이벤트 목록 수                           |
| events                   | Body | Array    | 이벤트 목록                                |
| events.eventCategoryType | Body | Enum     | 이벤트 카테고리 유형                           |
| events.eventCode         | Body | Enum     | 발생한 이벤트의 유형                           |
| events.sourceId          | Body | String   | 이벤트 소스의 식별자                           |
| events.sourceName        | Body | String   | 이벤트 소스를 식별할 수 있는 이름                   |
| events.messages          | Body | Array    | 이벤트 메시지 목록                            |
| events.messages.langCode | Body | String   | 언어 코드                                 |
| events.messages.message  | Body | String   | 이벤트 메시지                               |
| events.eventYmdt         | Body | DateTime | 이벤트 발생 일시(YYYY-MM-DDThh:mm:ss.SSSTZD) |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 28,
    "events": [
        {
            "eventCategoryType": "INSTANCE",
            "eventCode": "INSTC_02_01",
            "sourceId": "76f00947-356e-4a20-8922-428368cc45ed",
            "sourceName": "db-instance",
            "messages": [
                {
                    "langCode": "EN",
                    "message": "DB instance started"
                },
                {
                    "langCode": "JA",
                    "message": "DBインスタンスの起動"
                },
                {
                    "langCode": "KO",
                    "message": "DB 인스턴스 시작"
                },
                {
                    "langCode": "ZH",
                    "message": "DB instance started"
                }
            ],
            "eventYmdt": "2023-03-20T16:31:59+09:00"
        }
    ]
}
```

</p>
</details>

---

### 구독 가능한 이벤트 코드 목록 보기

```
GET /v3.0/event-codes
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

| 이름                           | 종류   | 형식    | 설명          |
|------------------------------|------|-------|-------------|
| eventCodes                   | Body | Array | 이벤트 코드 목록   |
| eventCodes.eventCode         | Body | Enum  | 이벤트 코드      |
| eventCodes.eventCategoryType | Body | Enum  | 이벤트 카테고리 유형 |

<details><summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "eventCodes": [
        {
            "eventCode": "INSTC_05_01",
            "eventCategoryType": "INSTANCE"
        }
    ]
}
```

</p>
</details>

---
