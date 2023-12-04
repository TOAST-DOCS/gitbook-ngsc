## Database > RDS for MySQL > DB 인스턴스

## DB 인스턴스

DB 인스턴스는 가상 장비와 설치된 MySQL을 아우르는 개념으로, RDS for MySQL에서 제공하는 MySQL의 단위입니다.
DB 인스턴스의 운영체제에 직접 접근할 수 없으며, 오직 DB 인스턴스 생성 시 입력하신 포트를 통해서 데이터베이스로만 접근할 수 있습니다. 사용할 수 있는 포트 범위는 아래와 같은 제약 사항이 있습니다.

* 사용할 수 있는 포트 범위는 3306~43306 사이입니다.

DB 인스턴스는 고객이 부여하는 이름과 자동으로 부여되는 32바이트 아이디로 식별됩니다.
DB 인스턴스 이름은 아래와 같은 제약 사항이 있습니다.

* DB 인스턴스 이름은 리전별로 고유해야 합니다.
* DB 인스턴스 이름은 1~100 사이의 영문자, 숫자, 일부 기호(-, _, .)만 사용할 수 있으며, 첫 번째 글자는 영문자만 사용할 수 있습니다.

DB 인스턴스는 생성 시 사용자 계정과 비밀번호를 설정해야 하며, 아래와 같은 제약 사항이 있습니다.

* 사용자 계정은 1~32자 사이의 글자이어야 합니다.
* 비밀번호는 4~16자 사이의 글자이어야 합니다.

### 가용성 영역

NHN Cloud는 물리 하드웨어 문제로 생기는 장애에 대비하기 위해 전체 시스템을 여러 개의 가용성 영역으로 나누어 두었습니다. 이 가용성 영역별로 저장 시스템, 네트워크 스위치, 상면, 전원 장치가 모두 별도로 구성돼 있습니다. 한 가용성 영역 내에서 생기는 장애는 다른 가용성 영역에 영향을 주지 않으므로 서비스 전체의 가용성이 높아집니다. DB 인스턴스를 여러 가용성 영역에 나눠 구축한다면 서비스의 가용성을 더욱 높일 수 있습니다. 여러 가용성 영역에 흩어져서 생성된 DB 인스턴스끼리 네트워크 통신이 가능하며 이때 발생하는 네트워크 사용 비용은 부과되지 않습니다.

> [주의]
> 이미 생성한 DB 인스턴스의 가용성 영역은 변경할 수 없습니다.

### DB 엔진

아래에 명시된 버전을 사용할 수 있습니다.

| 버전           | 비고                                                        |
|--------------|-----------------------------------------------------------|
| <strong>8.0</strong> ||
| MySQL 8.0.34 |                                                           | 
| MySQL 8.0.33 |                                                           |
| MySQL 8.0.32 |                                                           | 
| MySQL 8.0.28 |                                                           | 
| MySQL 8.0.23 |                                                           |
| MySQL 8.0.18 |                                                           |
| <strong>5.7</strong> ||
| MySQL 5.7.37 |                                                           |
| MySQL 5.7.33 | 외부의 백업본으로 DB 인스턴스를 복원할 수 없습니다.                   |
| MySQL 5.7.26 |                                                           |
| MySQL 5.7.19 |                                                           |
| MySQL 5.7.15 |                                                           |
| <strong>5.6</strong> ||
| MySQL 5.6.33 | 신규 DB 인스턴스를 생성할 수 없습니다. 기존 DB 인스턴스의 읽기 복제본 생성, 복원만 지원합니다. |

DB 엔진의 경우 생성 이후 웹 콘솔의 수정 기능을 통해 버전 업그레이드가 가능합니다.
DB 엔진에 대한 자세한 사항은 [DB 엔진](db-engine/)에서 확인할 수 있습니다.

### DB 인스턴스 타입

DB 인스턴스는 타입에 따라 서로 다른 CPU 코어 수와 메모리 용량을 가지고 있습니다.
DB 인스턴스 생성 시 데이터베이스 워크로드에 따라 알맞은 DB 인스턴스 타입을 선택해야 합니다.

| 타입 | 설명                                                        |
|----|-----------------------------------------------------------|
| m2 | CPU와 메모리를 균형 있게 설정한 타입입니다.                                |
| c2 | CPU의 성능을 높게 설정한 인스턴스 타입입니다.                               |
| r2 | 다른 자원에 비해 메모리의 사용량이 많은 경우 사용할 수 있습니다.                     |
| x1 | 고사양의 CPU와 메모리를 지원하는 타입입니다. 높은 성능이 필요한 서비스나 애플리케이션에 사용합니다. |

이미 생성한 DB 인스턴스의 타입은 웹 콘솔을 통해 손쉽게 변경 가능합니다.

> [주의]
> 이미 생성한 DB 인스턴스의 타입 변경 시 DB 인스턴스가 종료되므로 수분의 다운 타임이 발생합니다.

### DB 인스턴스 상태

DB 인스턴스의 상태는 아래와 같은 값들로 구성되며, 사용자의 행위와 현재 상태에 따라 변경됩니다.

| 상태                | 설명                                            |
|-------------------|-----------------------------------------------|
| BEFORE_CREATE     | 생성 이전                                         |
| AVAILABLE         | 사용 가능                                         |
| STORAGE_FULL      | 용량 부족                                         |
| FAIL_TO_CREATE    | 생성 실패                                         |
| FAIL_TO_CONNECT   | 연결 실패                                         |
| REPLICATION_STOP  | 복제 중단                                         |
| FAILOVER          | 장애 조치 완료                                      |
| FAILOVER_SHUTDOWN | 장애 조치 완료(정지), 2023년 4월 11일 이전에 장애 조치된 DB 인스턴스 |
| SHUTDOWN          | 중지됨                                           |

### DB 인스턴스 작업

DB 인스턴스에서 수행되는 작업은 아래와 같은 값들로 구성되며, 웹 콘솔 조작 혹은 사전에 지정된 자동화 배치에 따라 작업이 시작됩니다.

| 작업                       | 설명           |
|--------------------------|--------------|
| APPLYING_PARAMETER_GROUP | 파라미터 그룹 적용 중 |
| BACKING_UP               | 백업 중         |
| CANCELING                | 취소 중         |
| CREATING                 | 생성 중         |
| CREATING_SCHEMA          | DB 스키마 생성 중	 |
| CREATING_USER            | 사용자 생성 중	    |
| DELETING                 | 삭제 중         |
| DELETING_SCHEMA          | DB 스키마 삭제 중  |
| DELETING_USER            | 사용자 삭제 중     |
| EXPORTING_BACKUP         | 백업을 내보내는 중   |
| FAILING_OVER             | 장애 조치 중      |
| MIGRATING                | 마이그레이션 중     |
| MODIFYING                | 수정 중         |
| PREPARING                | 준비 중         |
| PROMOTING                | 승격 중         |
| REBUILDING               | 재구축 중        |
| REPAIRING                | 복구 중         |
| REPLICATING              | 복제 중         |
| RESTARTING               | 재시작 중        |
| RESTARTING_FORCIBLY      | 강제 재시작 중     |
| RESTORING                | 복원 중         |
| STARTING                 | 시작 중         |
| STOPPING                 | 정지 중         |
| SYNCING_SCHEMA           | DB 스키마 동기화 중 |
| SYNCING_USER             | 사용자 동기화 중	   |
| UPDATING_USER            | 사용자 수정 중	    |

> [주의]
> DB 인스턴스는 한번에 하나의 작업만 처리할 수 있습니다.
> 동시에 작업을 요청한 경우 먼저 요청한 작업만 성공하며, 이후 요청한 작업은 모두 실패하게 됩니다.
> 요청에 실패한 작업은 이벤트 화면에서 확인할 수 있습니다.

### 스토리지

DB 인스턴스는 HDD, SSD 2가지 스토리지 종류를 지원합니다. 스토리지 종류에 따라 성능과 가격이 다르므로, 데이터베이스 워크로드에 따라 알맞은 스토리지 종류를 선택해야 합니다. 스토리지는 최소 20GB~2TB까지 생성할 수 있습니다.

### 스토리지 크기 확장

DB 인스턴스의 스토리지 종류는 변경할 수 없지만 스토리지 크기는 웹 콘솔에서 손쉽게 확장할 수 있습니다. 스토리지 크기 확장 과정에서 DB 인스턴스가 종료되며, 서비스 부하에 따라 수분의 다운 타임이 발생합니다. 읽기 복제본이 존재하는 경우 마스터의 스토리지 크기 확장 시 읽기 복제본의 스토리지 크기도 같이 확장됩니다. 읽기 복제본이 여러 대 있는 경우 스토리지 크기 확장은 순차적으로 진행됩니다. 스토리지 크기 확장 중 오류가 발생할 경우 일부 읽기 복제본의 스토리지 크기가 확장되지 않을 수 있으며, 확장에 실패한 읽기 복제본의 경우 이후 개별적으로 확장 가능합니다. 스토리지 크기는 현재 크기보다 작게 변경할 수 없습니다.

### 네트워크

DB 인스턴스 생성 시 연결할 VPC 서브넷을 선택해야 합니다. 동일한 서브넷에 연결된 Compute 서비스의 인스턴스 간에는 별도의 플로팅 IP 없이 통신할 수 있습니다.

> [주의]
> 이미 생성한 DB 인스턴스의 서브넷은 변경할 수 없습니다.

### 플로팅 IP

외부에서 DB 인스턴스에 접근하려면 플로팅 IP를 DB 인스턴스에 연결해야 합니다. Internet Gateway가 연결된 서브넷을 연결할 경우에만 플로팅 IP를 생성할 수 있습니다. 플로팅 IP는 사용과 동시에 과금이 되며, 이와 별개로 플로팅 IP를 통한 인터넷 방향의 트래픽이 발생할 경우 별도 과금합니다.

### DB 보안 그룹

DB 보안 그룹은 외부 침입에 대비해 접속을 제한하기 위해서 사용합니다. 송수신 트래픽에 대해서 특정 포트 범위 혹은 데이터베이스 포트에 대해서 접근을 허용할 수 있습니다. DB 인스턴스에 여러 개의 DB 보안 그룹을 적용할 수 있습니다. DB 보안 그룹에 대한 자세한 설명은 [DB 보안 그룹](db-security-group/) 항목을 참고합니다.

### 백업

DB 인스턴스의 데이터베이스를 주기적으로 백업하도록 설정하거나, 웹 콘솔을 통해 원하는 시기에 백업을 생성할 수 있습니다. 백업이 수행되는 동안 성능 저하가 발생할 수 있습니다. 서비스에 영향을 주지 않기 위해 서비스의 부하가 적은 시간에 백업하는 것을 권장합니다. 백업으로 인한 성능 저하를 원치 않으면 고가용성 구성을 사용하거나 읽기 복제본에서 백업을 수행할 수 있습니다. 백업 파일은 내부 오브젝트 스토리지에 저장되며, 백업 용량에 따라 과금됩니다. 필요한 경우 NHN Cloud의 사용자 오브젝트 스토리지로 내보낼 수 있습니다. 예상치 못한 장애에 대비하기 위해서 주기적으로 백업을 수행하도록 설정하는 것을 권장합니다. 백업에 대한 자세한 설명은 [백업 및 복원](backup-and-restore/) 항목을 참고합니다.

### 복원

백업을 이용하여 새로운 DB 인스턴스를 생성할 수 있습니다. 백업을 수행한 DB 인스턴스와 바이너리 로그(binary log)가 존재한다면 특정 시점 혹은 원하는 바이너리 로그(binary log) position으로 복원이 가능합니다. RDS for MySQL이 아닌 외부 MySQL의 백업으로도 복원이 가능합니다. 복원 시 항상 새로운 DB 인스턴스가 생성되며, 기존 DB 인스턴스의 데이터베이스를 지우고 복원할 수 없습니다. 복원에 대한 자세한 설명은 [백업 및 복원](backup-and-restore/) 항목을 참고합니다.

### 기본 알림

DB 인스턴스 생성 시 기본 알림을 설정할 수 있습니다. 기본 알림을 설정하면 `{DB 인스턴스 이름}-default` 이름으로 새로운 알림 그룹이 생성되며 아래 알림 항목들이 자동으로 설정됩니다. 기본 알림으로 생성된 알림 그룹은 자유롭게 수정, 삭제할 수 있습니다. 알림 그룹에 대한 자세한 설명은 [알림 그룹](notification/) 항목을 참고합니다.

| 항목                         | 비교 방법 | 임겟값           | 지속 시간 |
|----------------------------|-------|---------------|-------|
| CPU 사용률                    | &gt;= | 80%           | 5분    |
| Storage 남은 사용량             | &lt;= | 5,120MB       | 5분    |
| Database Connection Status | &lt;= | 0             | 0분    |
| Storage 사용량                | &gt;= | 95%           | 5분    |
| 스토리지 결함                    | &lt;= | 0             | 0분    |
| Connection Ratio           | &gt;= | 85%           | 5분    |
| 메모리 사용량                    | &gt;= | 90%           | 5분    |
| Slow Query                 | &gt;= | 60 counts/min | 5분    |

### DB 인스턴스 중지

DB 인스턴스를 일정 시간 동안 사용하지는 않지만, 삭제를 원하지 않는 경우 중지할 수 있습니다. 중지된 DB 인스턴스의 가상 장비는 종료되며, 다시 구동하기 전에는 사용할 수 없습니다. 중지된 상태의 DB 인스턴스는 중지된 순간부터 90일간 할인된 요금이 과금되며, 90일이 지난 시점부터는 정상 요금이 과금됩니다. 불필요한 요금이 청구되지 않도록 사용하지 않는 DB 인스턴스는 꼭 삭제해 주시기를 바랍니다.

> [참고]
> 고가용성 DB 인스턴스, 읽기 복제본을 가지고 있는 마스터, 읽기 복제본은 중지할 수 없습니다.
> DB 인스턴스가 플로팅 IP를 사용 중일 경우 중지와 상관없이 플로팅 IP 요금이 과금됩니다.

### 읽기 복제본 생성

읽기 성능을 높이기 위해서 읽기 전용으로 사용할 수 있는 읽기 복제본을 생성할 수 있습니다. 읽기 복제본은 하나의 마스터에 대해서 최대 5대까지 생성할 수 있습니다. 읽기 복제본의 읽기 복제본은 생성할 수 없습니다. 읽기 복제본은 마스터와 동일한 사양 혹은 더 높은 사양으로 만드는 것을 권장합니다. 낮은 사양으로 생성 시 복제 지연이 발생할 수 있습니다.

> [주의]
> 읽기 복제본 생성 시 마스터의 I/O 성능이 평소보다 낮아질 수 있습니다.
> 마스터의 데이터베이스 크기에 비례하여 읽기 복제본 생성 시간이 늘어날 수 있습니다.

> [참고]
> 읽기 복제본 생성 과정에 필요한 바이너리 로그(binary log) 크기만큼 오브젝트 스토리지 과금이 발생할 수 있습니다.

#### 다른 리전의 서브넷에 읽기 복제본 생성
  * 서로 다른 리전에 존재하는 VPC 간 리전 피어링을 연결하면 다른 리전 VPC에 속한 서브넷에 읽기 복제본을 생성할 수 있습니다.
  * 리전 피어링이 연결되어 있더라도 라우트 설정이 올바르지 않을 경우 읽기 복제본 생성에 실패하거나 복제가 중단될 수 있습니다.
  * 리전 피어링 연결에 관한 자세한 사항은 [리전 피어링](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/#_2) 항목을 참고합니다.

### 읽기 복제본 승격

마스터와의 복제 관계를 끊고 읽기 복제본을 마스터로 변경하는 것을 승격이라 부릅니다. 승격된 마스터는 독립된 DB 인스턴스로서 동작하게 됩니다. 승격하려는 읽기 복제본과 마스터 사이에 복제 지연이 있는 경우, 해당 지연이 없어질 때까지 승격되지 않습니다.

### 읽기 복제본 강제 승격

마스터나 원본 리전의 상태와 상관없이 읽기 복제본의 현재 시점 데이터로 강제 승격합니다. 

### 읽기 복제본의 복제 중단

읽기 복제본은 여러 이유로 복제가 중단될 수 있습니다. 읽기 복제본의 상태가 `복제 중단`인 경우 빠르게 원인을 확인하여 정상화해야 합니다. `복제 중단` 상태가 장시간 지속될 경우 복제 딜레이가 늘어나게 됩니다. 정상화에 필요한 바이너리 로그(binary log)가 없는 경우 읽기 복제본을 재구축해야 합니다. 복제가 중단된 원인은 읽기 복제본에서 `SHOW SLAVE STATUS` 명령어를 통해 확인할 수 있습니다. `Last_Errno` 값이 1062인 경우 아래 Procedure를 에러가 사라질 때까지 호출할 수 있습니다.

```
mysql> CALL mysql.tcrds_repl_skip_repl_error();
```

### 읽기 복제본 재구축

읽기 복제본의 복제 중단을 해결할 수 없는 상황이면 재구축을 통해 정상화할 수 있습니다. 읽기 복제본 재구축 시 읽기 복제본의 데이터베이스를 모두 제거하고, 마스터의 데이터베이스를 토대로 재구축합니다. 이 과정에서 재구축에 필요한 백업 파일이 마스터에 존재하지 않는다면 마스터에서 백업이 수행되게 되며, 백업으로 인한 성능 저하가 발생할 수 있습니다.

### 강제 재시작

DB 인스턴스의 MySQL이 정상 동작하지 않는 경우 강제로 재시작할 수 있습니다. 강제 재시작의 경우 MySQL에 SIGTERM 명령을 내려 정상 종료되기를 10분간 기다립니다. 10분 안에 MySQL이 정상 종료되면 이후 가상 머신을 재부팅합니다. 10분 안에 정상 종료되지 않으면 가상 머신을 강제로 재부팅합니다. 가상 머신이 강제로 재부팅되면 작업 중인 일부 트랜잭션이 유실될 수 있으며, 데이터 볼륨이 손상되어 복구가 불가능해질 수 있습니다. 강제 재시작 이후 DB 인스턴스의 상태가 사용 가능 상태로 돌아오지 않을 수 있습니다. 해당 상황 발생 시 고객 센터로 문의해 주세요.

> [주의]
> 데이터가 유실되거나 데이터 볼륨이 손상될 가능성이 있으므로 해당 기능은 긴급하고 불가피한 상황 이외에는 사용을 지양해야 합니다.

> [참고]
> 고가용성 DB 인스턴스의 경우 강제 재시작할 수 없습니다.

### 용량 확보

급격한 부하로 바이너리 로그(binary log)가 과도하게 생성되어 스토리지의 용량이 부족할 경우 웹 콘솔의 용량 확보 기능을 이용해 바이너리 로그를 삭제할 수 있습니다. 웹 콘솔에서 용량 확보를 선택하면 DB 인스턴스의 바이너리 로그를 선택할 수 있는 팝업 화면이 표시됩니다. 바이너리 로그를 선택한 뒤 확인을 눌러 선택한 항목 이전에 생성된 모든 바이너리 로그를 삭제합니다. 용량 확보 기능은 일시적으로 용량을 확보하는 기능입니다. 계속해서 용량이 부족하다면 서비스 부하에 맞게 바이너리 로그의 저장 기간을 설정하거나 스토리지의 크기를 확장해야 합니다.

> [참고]
> MySQL 5.7 버전 이하에서는 `expire_logs_days`, MySQL 5.8 버전 이상에서는 `binlog_expire_logs_seconds` 파라미터로 바이너리 로그(binary log)의 저장 기간을 설정할 수 있습니다.

> [주의]
> 삭제된 바이너리 로그(binary log)에 따라 특정 시점으로의 복원이 되지 않을 수 있습니다.

### 파라미터 그룹 변경 사항 적용

DB 인스턴스에 연결된 파라미터 그룹의 파라미터가 수정된 경우, 해당 수정 사항을 반영해야 합니다. 변경된 파라미터 적용을 위해 재시작이 필요한 경우 DB 인스턴스가 재시작됩니다. 파라미터 그룹에 대한 자세한 설명은 [파라미터 그룹](parameter-group/) 항목을 참고합니다.

### 사용자 관리

RDS for MySQL에서는 웹 콘솔을 통해 데이터베이스에 접속할 사용자를 손쉽게 관리할 수 있는 기능을 제공합니다. DB 인스턴스를 생성할 때 사용자가 생성되며, 이미 생성된 DB 인스턴스에서 자유롭게 사용자를 생성, 수정, 삭제할 수 있습니다. 이를 위해 데이터베이스에서 쿼리를 통해 직접 사용자를 생성, 수정, 삭제하는 것을 허용하지 않습니다. 그 대신 미리 정의된 권한 템플릿을 이용하여 사용자에게 권한을 부여할 수 있습니다. 사용자에게 부여할 수 있는 권한 템플릿은 다음과 같습니다.

* **READ**
  * 조회 권한을 가지고 있습니다.

```sql
GRANT SELECT, SHOW VIEW, PROCESS, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO '{user_id}'@'{host}';
GRANT SELECT ON `mysql`.* TO '{user_id}'@'{host}';
GRANT SELECT, EXECUTE ON `sys`.* TO '{user_id}'@'{host}';
GRANT SELECT ON `performance_schema`.* TO '{user_id}'@'{host}';
```

* **CRUD**
  * READ 권한을 포함하며, 데이터를 변경할 수 있는 권한을 가지고 있습니다.

```sql
GRANT INSERT, UPDATE, DELETE, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE ON *.* TO '{user_id}'@'{host}';
```

* **DDL**
  * CRUD 권한을 포함하며, DDL 쿼리를 실행할 수 있는 권한을 가지고 있습니다.

```sql
GRANT CREATE, DROP, INDEX, ALTER, CREATE VIEW, REFERENCES, EVENT, ALTER ROUTINE, CREATE ROUTINE, TRIGGER, RELOAD ON *.* TO '{user_id}'@'{host}';
GRANT EXECUTE ON `mysql`.* TO '{user_id}'@'{host}';
```

* **CUSTOM**
  * 외부 데이터베이스 백업으로부터 DB 인스턴스를 복원한 경우, 데이터베이스에 존재하는 모든 사용자는 CUSTOM 권한으로 표현됩니다.
  * CUSTOM 권한 템플릿에는 어떤 권한이 있는지 알 수 없습니다.
  * CUSTOM 권한 템플릿에서 다른 권한 템플릿으로 변경한 경우 다시 CUSTOM 권한 템플릿으로 변경할 수 없습니다.

MySQL 5.7.33 버전 이상에서는 사용자 생성, 변경 시 인증 플러그인과 TLS Option을 지정할 수 있습니다. 인증 플러그인을 변경하려면 반드시 비밀번호를 같이 변경해야 하며, 비밀번호를 변경하지 않으면 기존 비밀번호를 사용합니다. 버전별 적용 가능한 인증 플러그인은 다음과 같습니다.

* 5.7 버전

| 인증 플러그인 | 설명                                   |
|---------|--------------------------------------|
| NATIVE  | `mysql_native_password`를 사용하여 인증합니다. |
| SHA256  | `sha256_password`를 사용하여 인증합니다.       |

* 8.0 버전

| 인증 플러그인      | 설명                                   |
|--------------|--------------------------------------|
| NATIVE       | `mysql_native_password`를 사용하여 인증합니다. |
| CACHING_SHA2 | `caching_sha2_password`를 사용하여 인증합니다. |

TLS Option을 지정하여 클라이언트와 데이터베이스 간의 연결을 암호화할 수 있습니다.

| TLS Option | 설명                                                                 |
|------------|--------------------------------------------------------------------|
| NONE       | 암호화된 연결을 적용하지 않습니다.                                                |
| SSL        | 암호화된 연결을 적용합니다.                                                    |
| X509       | 암호화된 연결을 적용하며 접속 시 인증서가 필요합니다. 접속에 필요한 인증서는 웹 콘솔에서 다운로드할 수 있습니다. |

## 고가용성 DB 인스턴스

고가용성 DB 인스턴스는 가용성과 데이터 내구성을 증가시키고, 장애 허용이 가능한 데이터베이스를 제공합니다. 고가용성 DB 인스턴스는 마스터, 예비 마스터로 구성되며 서로 다른 가용성 영역에 생성됩니다. 예비 마스터는 장애에 대비한 DB 인스턴스로 평소에는 사용할 수 없습니다. 고가용성 DB 인스턴스의 경우 예비 마스터에서 백업이 수행됩니다.

> [참고]
> 고가용성 DB 인스턴스의 경우, MySQL 쿼리문을 사용해 다른 DB 인스턴스 또는 외부 MySQL의 Master로부터 강제로 복제하도록 설정하면 고가용성 및 일부 기능들이 정상적으로 동작하지 않습니다.

### 장애 감지

예비 마스터에는 장애를 감지하기 위한 프로세스가 존재하여 주기적으로 마스터의 상태를 감지합니다. 이러한 감지 주기를 Ping 간격이라고 하며 4회 연속 상태 체크에 실패할 경우 장애 조치를 수행합니다. Ping 간격이 짧을수록 장애에 민감하게 반응하며, Ping 간격이 길수록 장애에 둔감하게 반응합니다. 서비스 부하에 맞게 적절한 Ping 간격을 설정하는 것이 중요합니다.

> [참고]
> 마스터의 스토리지 사용량이 가득 차면 고가용성 감시 프로세스가 장애로 감지해 장애 조치를 수행하므로 주의하십시오.

### 자동 장애 조치

예비 마스터에서 마스터의 상태 체크에 4회 연속 실패할 경우 마스터가 서비스를 제공하지 못한다고 판단하여 자동으로 장애 조치를 수행합니다. 스플릿 브레인 방지를 위해 장애가 발생한 마스터에 할당된 모든 사용자 보안 그룹의 연결을 해제하여 외부의 접속을 차단하며, 예비 마스터가 마스터의 역할을 대신합니다. 접속을 위한 내부 도메인의 A record는 장애가 발생한 마스터에서 예비 마스터로 변경되므로, 응용 프로그램의 변경은 필요하지 않습니다. 장애 조치가 완료되면 장애가 발생한 마스터의 종류는 장애 조치된 마스터로, 예비 마스터의 종류는 마스터로 변경됩니다. 장애 조치된 마스터를 복구하거나 재구축하기 전까지 장애 조치가 수행되지 않습니다. 승격된 마스터는 장애 조치된 마스터의 모든 자동 백업을 승계합니다. 장애 조치 과정에서 마스터가 변경되면 바이너리 로그가 모두 삭제되므로 기존 백업을 이용한 시점 복원은 지원하지 않습니다. 승격된 마스터에서 신규로 백업이 수행된 시각부터 시점 복원을
할 수 있습니다.

> [참고]
> 고가용성 기능은 도메인을 기반으로 하고 있기 때문에 접속을 시도하는 클라이언트가 DNS 서버에 접속할 수 없는 네트워크 환경일 경우 도메인을 통해 DB 인스턴스에 접속할 수 없고, 장애 조치 발생 시 정상적인 접속이 불가능합니다.
> 내부 도메인의 A record 변경이 반영되는 데 약 3초 정도 소요됩니다. 소요 시간은 접속을 시도하는 클라이언트 환경의 DNS Cache 정책에 따라 달라질 수 있습니다.

> [주의]
> 마스터와 예비 마스터 간의 바이너리 로그(binary log)의 position number 값이 100,000,000 이상 차이가 날 경우 장애 조치가 되지 않습니다.

### 장애 조치된 마스터

장애가 발생하여 장애 조치가 된 마스터를 장애 조치된 마스터라고 합니다. 장애 조치된 마스터의 자동 백업은 수행되지 않으며, 장애 조치된 마스터 복구, 재구축, 분리, 삭제를 제외한 다른 모든 기능은 수행할 수 없습니다.

### 장애 조치된 마스터 복구

장애 조치 과정에서 데이터의 정합성이 깨지지 않았고, 장애가 발생한 시점부터 복구를 시도하는 시점까지 바이너리 로그(binary log)가 유실되지 않았다면 장애 조치된 마스터와 승격된 마스터를 다시 고가용성 구성으로 복구할 수 있습니다. 장애 조치된 마스터의 데이터베이스 그대로 승격된 마스터와 복제 관계를 다시 설정하므로 데이터의 정합성이 깨졌거나 복구에 필요한 바이너리 로그(binary log)가 유실되었다면 복구는 실패합니다. 장애 조치된 마스터 복구에 실패할 경우 재구축을 통해 다시 고가용성 기능을 활성화할 수 있습니다.

> [참고]
> 2023년 4월 11일 이전에 장애 조치가 발생한 DB 인스턴스의 경우 복구를 지원하지 않습니다.

### 장애 조치된 마스터 재구축

장애 조치된 마스터 복구에 실패할 경우 재구축을 이용해 다시 고가용성 기능을 활성화할 수 있습니다. 재구축은 복구와 달리 장애 조치된 마스터의 데이터베이스를 모두 제거하고, 승격된 마스터의 데이터베이스를 토대로 재구축합니다. 이 과정에서 재구축에 필요한 백업 파일이 승격된 마스터에 존재하지 않을 경우 백업이 수행되며, 백업으로 인한 성능 저하가 발생할 수 있습니다.

### 장애 조치된 마스터 분리

장애 조치된 마스터 복구에 실패하여 데이터 보정이 필요할 경우 장애 조치된 마스터를 분리하여 고가용성 기능을 비활성화할 수 있습니다. 분리된 마스터와 승격된 마스터 간의 복제 관계가 끊어지며 각각 일반 DB 인스턴스로 동작합니다. 분리된 이후에는 다시 원래 구성으로 복구가 불가능합니다.

### 수동 장애 조치

고가용성 DB 인스턴스의 경우 재시작이 동반되는 작업을 수행하면 장애 조치를 이용한 재시작 여부를 선택할 수 있으며, 해당 작업은 아래와 같습니다.

* DB 인스턴스 재시작
* DB 인스턴스 타입 변경
* 파라미터 그룹 변경
* 재시작이 필요한 파라미터가 변경된 경우
* 파라미터 그룹 변경 사항 반영
* 스토리지 크기 변경

장애 조치를 이용한 재시작을 하게 되면 예비 마스터를 먼저 재시작합니다. 이후 장애 조치를 통해 예비 마스터를 마스터로 승격시키고 기존 마스터는 예비 마스터 역할을 하게 됩니다. 승격 시, 접속을 위한 내부 도메인의 A record는 마스터에서 예비 마스터로 변경되므로, 응용 프로그램의 변경은 필요하지 않습니다. 승격된 마스터는 이전 마스터의 모든 자동 백업을 승계합니다. 장애 조치 과정에서 마스터가 변경되며 바이너리 로그(binary log)가 모두 삭제되기 때문에 기존 백업을 이용한 시점 복원은 지원하지 않습니다. 승격된 마스터에서 신규로 백업이 수행된 시각부터 시점 복원을 할 수 있습니다.

> [참고]
> 장애 조치를 이용해 재시작하지 않으면 마스터와 예비 마스터가 순차적으로 재시작됩니다.

> [주의]
> 예비 마스터의 복제 딜레이 `Seconds_Behind_Master` 값이 1 이상인 경우 수동 장애 조치가 실패합니다. 복제 딜레이로 인해 재시작이 실패한 경우 이벤트 화면에서 확인할 수 있습니다.

### 고가용성 일시 중지

일시적인 작업으로 인한 연결 중단 또는 대량의 부하가 예상되는 상황에서 일시적으로 고가용성 기능을 중지할 수 있습니다. 고가용성 기능이 일시 중지되면 장애를 감지하지 않으므로 장애 조치를 수행하지 않습니다. 고가용성 기능이 일시 중지된 상태에서 재시작이 필요한 작업을 수행해도 일시 중지된 고가용성 기능이 재개되지 않습니다. 고가용성 기능이 일시 중지되어도 데이터 복제는 정상적으로 이루어지거나, 장애가 감지되지 않기 때문에 장시간 일시 중지 상태로 유지하는 것을 권장하지 않습니다.

### 예비 마스터 재구축

네트워크의 단절, 잘못된 FEDERATED 엔진 사용, 다른 마스터로부터의 복제 설정과 같은 다양한 원인으로 예비 마스터 복제가 중단될 수 있습니다. 복제 중단 상태의 예비 마스터는 자동 장애 조치가 실행되지 않습니다. 예비 마스터의 복제 중단을 해결하려면 예비 마스터를 재구축해야 합니다. 예비 마스터 재구축 시에는 예비 마스터의 데이터베이스를 모두 제거하며, 마스터의 데이터베이스를 토대로 재구축합니다. 이 과정에서 재구축에 필요한 백업 파일이 마스터 데이터베이스에 존재하지 않을 경우 마스터에서 백업이 수행되며, 백업으로 인한 성능 저하가 발생할 수 있습니다.

## MySQL Procedure

RDS for MySQL은 사용자의 편의를 제공하기 위하여 사용자 계정에서 제한되는 몇몇 기능들을 수행하는 프로시저들을 자체적으로 제공하고 있습니다.

### tcrds_active_process

* Processlist에서 Sleep 상태가 아닌 ACTIVE 상태의 쿼리를 조회합니다.
* 수행 시간이 오래된 순서로 출력되며 쿼리 내용(SQL)은 100자리까지만 출력됩니다.

```
mysql> CALL mysql.tcrds_active_process();
```

### tcrds_process_kill

* 특정 프로세스를 강제 종료합니다.
* 종료할 프로세스 아이디는 information_schema.processlist에서 확인할 수 있으며 tcrds_active_process와 tcrds_current_lock 프로시저를 이용해서 프로세스의 정보를 확인할 수 있습니다.

```
mysql> CALL mysql.tcrds_process_kill(processlist_id );
```

### tcrds_current_lock

* 현재 락을 기다리고 있는 프로세스와 락을 점유하고 있는 프로세스 정보를 확인합니다.
* (w) 칼럼 정보가 락을 획득하기 위해 대기하는 프로세스 정보
* (B) 칼럼 정보가 락을 점유하고 있는 프로세스 정보
* 락을 점유하는 프로세스를 강제로 종료하려면 (B)PROCESS 칼럼을 확인한 후, call tcrds_process_kill(process_id)를 수행합니다.

```
mysql> CALL mysql.tcrds_current_lock();
```

### tcrds_repl_changemaster

* 복제를 이용해 외부 MySQL DB를 NHN Cloud RDS로 가져올 때 사용합니다.
* NHN Cloud RDS의 복제 구성은 콘솔의 **복제본 생성**으로 할 수 있습니다.

```
mysql> CALL mysql. tcrds_repl_changemaster (master_instance_ip, master_instance_port, user_id_for_replication, password_for_replication_user, MASTER_LOG_FILE, MASTER_LOG_POS);
```

* 파라미터 설명
    * master_instance_ip: 복제 대상(Master) 서버의 IP
    * master_instance_port: 복제 대상(Master) 서버의 MySQL 포트
    * user_id_for_replication: 복제 대상(Master) 서버의 MySQL에 접속할 복제용 계정
    * password_for_replication_user: 복제용 계정 패스워드
    * MASTER_LOG_FILE: 복제 대상(Master)의 binary log 파일명
    * MASTER_LOG_POS: 복제 대상(Master)의 binary log 포지션

```
ex) call mysql.tcrds_repl_changemaster('10.162.1.1',10000,'db_repl','password','mysql-bin.000001',4);
```

> [주의] 복제용 계정이 복제 대상(Master) MySQL에 생성되어 있어야 합니다.

### tcrds_repl_init

* MySQL 복제 정보를 초기화합니다.

```
mysql> CALL mysql.tcrds_repl_init();
```

### tcrds_repl_slave_stop

* MySQL 복제를 멈춥니다.

```
mysql> CALL mysql.tcrds_repl_slave_stop();
```

### tcrds_repl_slave_start

* MySQL 복제를 시작합니다.

```
mysql> CALL mysql.tcrds_repl_slave_start();

```

### tcrds_repl_skip_repl_error

* SQL_SLAVE_SKIP_COUNTER=1를 수행합니다. 다음과 같은 Duplicate key 에러 발생 시 tcrds_repl_skip_repl_error 프로시저를 실행하면 복제 에러를 해결할 수 있습니다.
* `MySQL error code 1062: 'Duplicate entry ? for key ?'`

```
mysql> CALL mysql.tcrds_repl_skip_repl_error();
```

### tcrds_repl_next_changemaster

* Master의 다음 바이너리(binary log) 로그를 읽을 수 있도록 복제 정보를 변경합니다.
* 다음과 같은 복제 에러 발생 시 tcrds_repl_next_changemaster 프로시저를 실행하면 복제 에러를 해결할 수 있습니다.

예) MySQL error code 1236 (ER_MASTER_FATAL_ERROR_READING_BINLOG): Got fatal error from master when reading data from binary log

```
mysql> CALL mysql.tcrds_repl_next_changemaster();
```

### tcrds_innodb_monitor_reset

* information_schema.INNODB_METRICS 테이블의 counter를 0으로 재설정하는 innodb_monitor_reset variables를 실행하는 프로시저입니다.
* `SET GLOBAL innodb_monitor_reset = '{counter-name|module_name|pattern|all}';` 쿼리를 실행합니다.
* innodb_monitor_enable, innodb_monitor_disable는 RDS 파라미터로 제공합니다.

```
mysql> CALL mysql.tcrds_innodb_monitor_reset('{counter-name|module_name|pattern|all}');
```

```
ex) CALL mysql.tcrds_innodb_monitor_reset('dml_reads');
CALL mysql.tcrds_innodb_monitor_reset('module_dml');
```

### tcrds_innodb_monitor_reset_all

* counter 값을 재설정하는 innodb_monitor_reset_all variables를 실행하는 프로시저입니다.
* innodb_monitor_reset_all을 사용하려면 counter가 disable 상태여야 합니다.
* `SET GLOBAL tcrds_innodb_monitor_reset_all = '{counter-name|module_name|pattern|all}';` 쿼리를 실행합니다.

```
mysql> CALL mysql.tcrds_innodb_monitor_reset_all('{counter-name|module_name|pattern|all}');
```

## 데이터 마이그레이션

* RDS는 mysqldump를 이용하여 NHN Cloud RDS의 외부로 데이터로 내보내거나 외부로부터 가져올 수 있습니다.
* mysqldump 유틸리티는 MySQL을 설치했을 때 기본으로 제공됩니다.

### mysqldump를 이용하여 내보내기

* NHN Cloud RDS의 인스턴스를 준비하여 사용합니다.
* 내보낼 데이터를 저장하게 될 외부 인스턴스, 혹은 로컬 클라이언트가 설치된 컴퓨터의 용량이 충분히 확보되어 있는지 확인합니다.
* NHN Cloud의 외부로 데이터를 내보내야 할 경우 Floating IP를 생성하여 데이터를 내보낼 RDS 인스턴스에 연결합니다.
* 아래의 mysqldump 명령어를 통하여 외부로 데이터를 내보냅니다.

#### 파일로 내보낼 경우

```
mysqldump -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

#### NHN Cloud RDS 외부의 MySQL DB로 내보낼 경우

```
mysqldump -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --routines --events --triggers --databases {database_name1, database_name2, ...} | mysql -h{external_db_host} -u{external_db_id} -p{external_db_password} --port={external_db_port}
```

### mysqldump를 이용하여 가져오기

* 데이터를 가져올 NHN Cloud RDS 외부의 DB를 준비합니다.
* 가져올 NHN Cloud RDS 인스턴스의 용량이 충분한지 확인합니다.
* Floating IP를 생성하여 NHN Cloud RDS 인스턴스에 연결합니다.
* 아래의 mysqldump 명령어를 통하여 외부로부터 데이터를 가져옵니다.

```
mysqldump -h{external_db_host} -u{external_db_id} -p{external_db_password} --port={external_db_port} --single-transaction --set-gtid-purged=off --routines --events --triggers --databases {database_name1, database_name2, ...} | mysql -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port}
```

#### 데이터 가져오는 도중 `ERROR 1227` 오류가 발생할 경우

* `ERROR 1227` 오류는 mysqldump 파일의 저장된 객체(트리거, 뷰, 함수 또는 이벤트)에 DEFINER 정의가 되어 있을 때 발생합니다. 이를 해결하기 위해서는 mysqldump 파일에서 `DEFINER`부분을 삭제 후 진행합니다.

#### 데이터 가져오는 도중 `ERROR 1418` 오류가 발생할 경우

* `ERROR 1418` 오류는 mysqldump 파일의 함수 선언에 NO SQL, READS SQL DATA, DETERMINISTIC이 없으며 바이너리 로그가 활성화된 상태일 때 발생합니다.
    * 자세한 설명은 [The Binary Log](https://dev.mysql.com/doc/refman/8.0/en/binary-log.html) MySQL 문서를 참고합니다.
* 이를 해결하기 위해서는 mysqldump 파일을 적용할 DB 인스턴스의 `log_bin_trust_function_creators` 파라미터의 값을 `1`로 변경해야 합니다.

### 복제를 이용하여 내보내기

* 복제를 이용하여 NHN Cloud RDS의 데이터를 외부의 DB로 내보낼 수 있습니다.
* 외부의 DB 버전은 NHN Cloud RDS의 버전과 같거나 그보다 최신 버전이어야합니다.
* 데이터를 내보낼 NHN Cloud RDS Master 혹은 Read Only Slave 인스턴스를 준비합니다.
* Floating IP를 생성하여 데이터를 내보낼 NHN Cloud RDS 인스턴스들에 연결합니다.
* 아래의 명령어를 통해 NHN Cloud RDS 인스턴스로부터 데이터를 파일로 내보냅니다.
* Master RDS 인스턴스로부터 내보낼 경우

```
mysqldump -h{rds_master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --master-data=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* Read Only Slave RDS 인스턴스로부터 내보낼 경우

```
mysqldump -h{rds_read_only_slave_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --dump-slave=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* 백업된 파일을 열어 주석에 쓰여진 MASTER_LOG_FILE 및 MASTER_LOG_POS를 따로 기록합니다.
* NHN Cloud RDS 인스턴스로부터 데이터를 백업할 외부 로컬 클라이언트 혹은 DB가 설치된 컴퓨터의 용량이 충분한지 확인합니다.
* 외부 DB의 my.cnf(Windows의 경우 my.ini) 파일에 아래와 같은 옵션을 추가합니다.
* server-id의 경우 NHN Cloud RDS 인스턴스의 DB Configuration 항목의 server-id와 다른 값으로 입력합니다.

```
...
[mysqld]
...
server-id={server_id}
replicate-ignore-db=rds_maintenance
...
```

* 외부 DB를 재시작합니다.
* 백업된 파일을 아래의 명령어를 통해 외부의 DB에 입력합니다.

```
mysql -h{external_db_host} -u{exteranl_db_id} -p{external_db_password} --port={exteranl_db_port} < {local_path_and_file_name}
```

* NHN Cloud RDS 인스턴스에서 복제에 사용할 계정을 생성합니다.
* 새롭게 복제를 설정하기에 앞서 혹시 존재할 수도 있는 기존 복제 정보를 초기화하기 위하여 아래의 쿼리를 실행합니다. 이 때, RESET SLAVE를 실행할 경우 기존 복제 정보가 초기화됩니다.

```
STOP SLAVE;

RESET SLAVE;
```

* 복제에 사용할 계정 정보와 아까 따로 기록해 두었던 MASTER_LOG_FILE과 MSATER_LOG_POS를 이용하여 외부 DB에 아래와 같이 쿼리를 실행합니다.

```
CHANGE MASTER TO master_host = '{rds_master_instance_floating_ip}', master_user='{user_id_for_replication}', master_password='{password_forreplication_user}', master_port ={rds_master_instance_port}, master_log_file ='{MASTER_LOG_FILE}', master_log_pos = {MASTER_LOG_POS};

START SLAVE;
```

* 외부 DB와 NHN Cloud RDS 인스턴스의 원본 데이터가 같아지면 외부 DB에 STOP SLAVE 명령을 이용해 복제를 종료합니다.

### 복제를 이용하여 가져오기

* 복제를 이용해 외부 DB를 NHN Cloud RDS로 가져올 수 있습니다.
* NHN Cloud RDS 버전은 외부 DB 버전과 같거나 그보다 최신 버전이어야 합니다.
* 데이터를 내보낼 외부 MySQL 인스턴스에 연결합니다.
* 아래의 명령어를 통해 외부 MySQL 인스턴스로부터 데이터를 백업합니다.
* 외부 MySQL 인스턴스(마스터)로부터 가져올 경우

```
mysqldump -h{master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --master-data=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* 외부 MySQL 인스턴스(슬레이브)로부터 가져올 경우

```
mysqldump -h{slave_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --dump-slave=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* 백업된 파일을 열어 주석의 MASTER_LOG_FILE 및 MASTER_LOG_POS를 따로 기록합니다.
* NHN Cloud RDS 인스턴스로부터 데이터를 백업할 클라이언트나 컴퓨터의 용량이 충분한지 확인합니다.
* 외부 DB의 my.cnf(Winodws의 경우 my.ini) 파일에 아래 옵션을 추가합니다.
* server-id의 경우 NHN Cloud RDS 인스턴스의 DB Configuration 항목의 server-id와 다른 값으로 입력합니다.

```
...
[mysqld]
...
server-id={server_id}
replicate-ignore-db=rds_maintenance
...
```

* 외부 DB를 재시작합니다.
* 외부 네트워크를 통해 가져오면(import) 오래 걸릴 수 있기 때문에, 내부 NHN Cloud Image를 생성하고 백업 파일을 복사한 후, NHN Cloud로 가져오기를 권장합니다.
* 백업된 파일을 아래의 명령어로 NHN Cloud RDS에 입력합니다.
* 복제 구성은 DNS를 지원하지 않으므로 IP로 변환해 실행합니다.

```
mysql -h{rds_master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} < {local_path_and_file_name}
```

* 외부 MySQL 인스턴스에서 복제에 사용할 계정을 생성합니다.

```
mysql> CREATE USER 'user_id_for_replication'@'{external_db_host}' IDENTIFIED BY '<password_forreplication_user>';
mysql> GRANT REPLICATION CLIENT, REPLICATION SLAVE ON *.* TO 'user_id_for_replication'@'{external_db_host}';
```

* 복제에 사용할 계정 정보와 앞에서 따로 기록해 두었던 MASTER_LOG_FILE, MSATER_LOG_POS를 이용하여 NHN Cloud RDS에 다음과 같이 쿼리를 실행합니다.

```
mysql> call mysql.tcrds_repl_changemaster ('rds_master_instance_floating_ip',rds_master_instance_port,'user_id_for_replication','password_forreplication_user','MASTER_LOG_FILE',MASTER_LOG_POS );
```

* 복제를 시작하려면 아래 프로시저를 실행합니다.

```
mysql> call mysql.tcrds_repl_slave_start;
```

* 외부 DB와 NHN Cloud RDS 인스턴스의 원본 데이터가 같아지면 아래 명령을 이용해 복제를 종료합니다.

```
mysql> call mysql.tcrds_repl_init();
```