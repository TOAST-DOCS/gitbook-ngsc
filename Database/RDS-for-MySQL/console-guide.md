## Database > RDS for MySQL > 콘솔 사용 가이드

## 간단히 시작하기

* 가장 먼저 해야 할 일은 DB 인스턴스를 생성하는 일입니다.
* **Console > Database > RDS for MySQL**의 **DB Instance**탭에서 좌측 상단 **+ 생성** 버튼을 누르시면 그림과 같이 페이지 하단에 생성 레이어가 표시됩니다.

![rds_01_20210112](https://static.toastoven.net/prod_rds/21.01.12/rds_01_20210112.png)

* 설정 레이어에 표시된 필수 항목을 모두 입력하신 후, 레이어 우측 상단의 **다음** 버튼을 눌러주세요.
    * DB Instance: DB 인스턴스 이름을 입력합니다.
    * 설명: DB 인스턴스의 설명을 입력합니다.
    * DB Engine: 생성 하시고자 하시는 Database Engine 버전을 선택합니다.
    * DB Port: Database 의 Port 번호를 입력합니다.
        * 10000~12000 사이로 설정할 수 있습니다.
    * DB ID: Database 생성 시 만들어질 관리자 계정 아이디를 입력합니다.
    * DB Password: Database 생성 시 만들어질 관리자 계정 비밀번호를 입력합니다.
    * VPC Subnet: 생성할 DB 인스턴스와 private network 통신을 원하는 Compute & Network 상품의 subnet 을 선택합니다.
    * Floating IP: 토스트클라우드 외부 네트워크와 연결을 원하실 경우 Floating IP 를 사용으로 설정합니다.
    * Flavor: DB 인스턴스의 사양을 선택합니다.
    * Storage 타입: DB 인스턴스 볼륨의 타입을 지정합니다.
        * HDD 및 SSD를 선택할 수 있습니다.
    * Storage: DB 인스턴스 볼륨의 크기를 입력합니다.
        * 20GB~2TB 크기로 생성할 수 있습니다.
    * Availabillity Zone: DB 인스턴스가 생성 될 Zone 을 선택합니다.
    * 고가용성: DB 인스턴스를 생성할 때 Master와 서로 다른 Availability Zone에 Candidate Master를 생성합니다.
    * Ping Interval: 고가용성 사용 시, Master 인스턴스 상태를 확인하는 시간 간격을 설정합니다. 4회 실패 시 장애로 식별합니다.
        * 1초~600초 사이로 설정할 수 있습니다.
    * 기본 알림: DB 인스턴스의 이벤트들 중 미리 정의된 이벤트에 대한 알림을 등록할 수 있습니다.
        * 기본 알림 사용 시 수신 그룹을 선택해야 합니다.

> [참고] 선택한 Compute & Network 상품의 VPC Subnet이 Internet Gateway와 연결이 되어있지 않다면, Floating IP를 사용할 수 없습니다.
> [참고] 한번 선택한 VPC Subnet은 변경할 수 없습니다.
> [참고] Candidate Master 인스턴스는 반드시 Master와 서로 다른 Availability Zone에 생성되며, 목록에 표시되지 않습니다.
> [참고] 인스턴스 목록은 각 인스턴스의 생성 순서대로 정렬되며, Candidate Master는 Master의 고가용성 옵션 사용 시점에 생성되기 때문에 장애 조치 이후 인스턴스의 순서가 바뀔 수 있습니다.
> [참고] 기본 알림 사용 시 해당 인스턴스에 대한 알림이 자동 등록되며, 이름은 "{인스턴스 이름}-default"로 설정됩니다. 등록되는 알림은 변경 및 삭제할 수 있으며, 적용되는 인스턴스도 변경할 수 있습니다.

![backup_and_access_ko](https://static.toastoven.net/prod_rds/22.08.09/backup_and_access_ko.png)

* 자동 백업 및 접근 제어 설정을 한 후, **다음** 버튼을 클릭합니다.
* 쿼리 지연 대기 시간: 백업 수행 시에 FLUSH TABLES WITH READ LOCK 지연 대기 시간을 설정할 수 있습니다.
    * 0~21600 사이 값으로 설정할 수 있습니다.
* 백업 보관 기간: 자동 백업을 하려면 1일 이상 선택합니다.
    * 0~730 사이 값으로 설정할 수 있습니다.
    * 0 입력 시 자동으로 백업을 하지 않습니다.
* 테이블 잠금 사용: 백업 수행 시 FLUSH TABLES WITH READ LOCK 구문으로 테이블 잠금 여부를 설정합니다.
* 백업 복제 리전: 자동 백업 시, 다른 리전에 백업 파일이 복제되도록 설정합니다.
* 백업 재시도 횟수: 백업 실패 시 재시도할 횟수를 지정합니다.
    * 1회 이상 입력해야 재시도합니다.
    * 자동 백업 수행 시에만 동작합니다.
* 백업 실행 주기: 자동 백업을 실행하는 주기를 설정합니다.
    * 추가 버튼을 눌러 백업 시작 시간과 Duration, 백업 재시도 만료 시간을 여러 개 추가할 수 있습니다.
    * 각 주기는 백업 시작 시간부터 Duration으로 설정된 시간까지 중복되지 않게 추가할 수 있습니다.
    * 이전에 실행한 백업이 종료되지 않으면 해당 시간대의 백업은 실행되지 않습니다.
* 백업 시작 시간: 자동 백업은 백업 시작 시간부터 Duration 사이 중 임의의 시점에 시작됩니다.
    * Duration은 백업이 시작되는 시간을 의미합니다.
    * Duration 안에 백업이 종료되는 것을 의미하지 않습니다.
* 백업 재시도 만료 시간: 백업 실패 시 재시도 횟수만큼 재시도하지만, 다음 백업 시간에 영향을 주지 않도록 설정한 시간까지만 재시도합니다.
* 사용자 접근 제어: DB 인스턴스에 접근 가능한 사용자를 CIDR 형식으로 입력합니다.
    * 사용자 접근 제어에 등록되지 않은 IP 는 접속이 불가능합니다.
    * 접근 제어 시, 방향 설정에서 `수신/송신`에 대해 각각 허용 여부를 선택합니다.
        * 수신: 인스턴스로 유입되는 방향을 의미합니다.
        * 송신: 인스턴스에서 나가는 방향을 의미합니다.

> [참고] 금융망에서는 백업 복제 리전 기능이 제공되지 않습니다.

![db_configuration_0_ko](https://static.toastoven.net/prod_rds/22.03.15/db_configuration_0_ko.png)

* 설정값을 변경한 후 **생성** 버튼을 누릅니다.
    * **수식 사용** 값이 **Y**로 표기된 항목은 **파라미터 값**에 수식을 입력할 수 있습니다.
    * 수식에 사용 가능한 변수는 다음과 같습니다(대소문자 구분).
        * ramSizeByte: DB 인스턴스에 할당된 메모리(바이트 단위)
        * storageSizeByte: DB 인스턴스 볼륨의 크기 (바이트 단위)
        * vCPU: CPU 개수
        * dbPort
    * 수식에 사용 가능한 연산자는 다음과 같습니다.
        * \* \/ + - , ( ) [ ]
    * 수식에 사용 가능한 함수는 다음과 같습니다(대소문자 구분).
        * min([a, b, ..., z])
        * max([a, b, ..., z])
        * sum([a, b, ..., z])
    * 수식을 계산한 값의 소수점 이하 9번째 자리에서 반올림한 결과가 DB 인스턴스에 적용됩니다.
    * 소수가 지원되지 않는 파라미터의 경우, 수식을 계산한 값의 소수점 이하를 버림한 결과가 DB 인스턴스에 적용됩니다.
* 최종적으로 **확인** 버튼을 누르면, DB 인스턴스가 생성됩니다.
* 생성되기 까지 수분에서 수십분이 소요 됩니다.

### DB 인스턴스 접속

* 생성된 DB 인스턴스를 선택하면 상세 설정을 확인할 수 있습니다.
* 인스턴스 [상세 설정]의 [접속 정보]에서 접속 가능한 도메인 정보를 확인할 수 있습니다.
* Floating IP 를 ‘사용’으로 설정하지 않은 DB 인스턴스는 외부에서 접근할 수 없습니다.

![instance_detail_0_ko](https://static.toastoven.net/prod_rds/22.06.14/instance_detail_0_ko.png)

* 외부에서 접속을 테스트하기 위해 우측 상단의 **변경** 버튼을 누릅니다.
* Floating IP 항목을 **사용함**으로 수정합니다.
* **확인** 버튼을 눌러 수정 사항을 반영합니다.

![instance_detail_1_ko](https://static.toastoven.net/prod_rds/22.06.14/instance_detail_1_ko.png)

* 설정 후 Floating IP가 생성되어 외부에서 접근이 가능해진것을 확인할 수 있습니다.

![rds_06_20210112](https://static.toastoven.net/prod_rds/21.01.12/rds_06_20210112.png)

* MySQL Workbench 접속 예시입니다.

#### 제약 사항

* 사용자 Compute 상품의 인스턴스가 dns 서버에 접속할 수 없는 네트워크 환경일 경우 해당 인스턴스는 도메인을 통해 RDS 인스턴스에 접속할 수 없습니다.
* 사용자가 사용 중인 ISP 사에서 보안을 위해 잘 알려진 포트를 차단할 수 있습니다. 이럴 경우 NHN Cloud의 RDS에 접속할 수 없을 수도 있으며 포트 번호를 변경해야 합니다.

## DB 인스턴스

### 고가용성

* 서로 다른 Availability Zone에 Candidate Master를 생성하여 장애가 발생했을 때 장애 조치를 할 수 있습니다.
* 고가용성을 사용한 인스턴스의 경우, 일부 옵션을 변경할 때 접속 정보는 변하지 않지만 Master와 Candidate Master 인스턴스는 서로 바뀔 수 있습니다.
* 장애가 발생해 고가용성 인스턴스에 장애 조치가 수행됐을 때, 변경된 새 Master 인스턴스는 기존 Master 인스턴스의 백업을 승계하지 않습니다.

> [참고] 고가용성 인스턴스 사용 시, MySQL 쿼리문을 사용해 다른 인스턴스 또는 외부 MySQL의 Master로부터 강제로 복제하도록 설정하면 고가용성 및 일부 기능들이 정상적으로 동작하지 않습니다.
> [참고] DB 인스턴스의 스토리지 사용량이 가득 차면 고가용성 감시 프로세스가 장애로 감지해 장애 조치를 수행하므로 주의하시기 바랍니다.

#### 고가용성 일시 중지 및 다시 시작

* Master 인스턴스에 일시적인 작업으로 인한 연결 중단 혹은 대량의 부하가 예상되는 상황에서 일시적으로 고가용성 기능을 중지할 수 있습니다.
* 고가용성 기능이 일시 중지되면 장애를 감지하지 않으며 따라서 장애 조치가 되지 않습니다.
* 고가용성 기능이 일시 중지된 상태에서 재시작이 필요한 작업을 수행하여도 일시 중지된 고가용성 기능이 재개되지 않습니다.
* 고가용성 기능이 일지 중지되어도 데이터 복제는 정상적으로 이루어지나, 장애가 감지되지 않기 때문에 장시간 일시 중지 상태로 유지하는 것을 권장하지 않습니다.

#### 제약 사항

* 고가용성 인스턴스는 최초 1회의 장애 조치를 보장합니다. 장애가 발생하여 장애 조치한 경우, Candidate Master 인스턴스가 고가용성 기능을 사용하지 않는 일반 Master로 변경됩니다.
* 변경된 새 Master 인스턴스는 기존 Master 인스턴스의 접속에 사용되는 도메인을 승계합니다.
* 또한 고가용성 옵션을 새로 지정해 사용할 수 있습니다.
* 장애 조치를 수행한 기존의 Master 인스턴스는 접속 정보가 변경되고, ‘중지됨’ 상태로 전환됩니다.
* 장애 조치를 수행한 기존의 Master 인스턴스는 인스턴스 재시작 기능을 이용해 재구동을 시도할 수는 있으나, 장애로 인한 데이터 손실 등의 이유로 다시 구동할 수 없거나 정상적으로 작동하지 않을 수 있습니다.
* Read Only Slave 인스턴스에는 고가용성 기능을 제공하지 않습니다.
* 고가용성 기능을 사용하는 인스턴스를 재시작하거나 옵션을 변경하는 동안에는 해당 인스턴스의 Read Only Slave 조작을 할 수 없게 됩니다.
* 고가용성 기능은 도메인을 기반으로 하고 있기 때문에 사용자 Compute 서비스의 인스턴스가 DNS 서버에 접속할 수 없는 네트워크 환경일 경우 해당 인스턴스는 도메인을 통해 RDS 인스턴스에 접속할 수 없고, 장애 조치 발생 시 정상적인 접속이 불가능합니다.

### Flavor

* NHN Cloud Compute & Network 상품에서 제공하는 일부 사양으로 DB 인스턴스를 생성할 수 있습니다.

### 재시작

* DB 인스턴스 재시작을 수행하면 DB 인스턴스에 설치된 MySQL이 재시작됩니다.
* MySQL의 재시작은 MySQL 엔진의 상태에 따라서 오랜 시간이 걸릴 수 있으며, RDS for MySQL은 별도의 제한 시간 없이 MySQL이 정상적으로 종료, 시작될 때까지 대기합니다.
* MySQL이 재시작되면 `인스턴스 종료`, `인스턴스 시작` 이벤트가 생성됩니다.
* 읽기 복제본의 경우 재시작하는 동안 복제가 중단되며, 재시작이 완료되면 복제가 자동으로 재개됩니다.

#### 고가용성 DB 인스턴스의 재시작

* 고가용성 DB 인스턴스의 경우 일반 인스턴스에서는 제공하지 않는 2가지 옵션을 제공합니다.

![restart_ha_instance_ko](https://static.toastoven.net/prod_rds/21.11.09/restart_ha_instance_ko.png)

* ❶ [장애 조치를 이용한 재시작] 옵션을 선택하면 순단 시간을 최소화하며 재시작을 진행할 수 있습니다.
    * Master 인스턴스가 교체되며, 바이너리 로그(binary log)가 모두 삭제되기 때문에 재시작을 수행하기 전 시각으로 시점 복원이 불가능합니다.
    * Master 인스턴스에 lock이 있거나 복제 지연이 길 경우 장애 조치가 되지 않거나 시간이 많이 소요되므로, 트랜잭션이 모두 종료되었고 복제 지연이 없는지 확인해야 합니다.
* ❶ [장애 조치를 이용한 재시작] 옵션을 선택하지 않으면 고가용성 기능이 일시 정지되며, Master 인스턴스 -> Candidate Master 인스턴스 순으로 순차적으로 재시작됩니다.
    * 재시작이 완료되면 다시 고가용성 기능이 실행됩니다.
* ❷ [현재 시점 백업 진행] 옵션을 선택하면 재시작이 완료되는 즉시 백업을 진행합니다.
    * [장애 조치를 이용한 재시작] 옵션을 선택하면 재시작을 수행하기 전 시각으로 복원이 불가능하므로, 가급적 이 옵션을 같이 선택하는 것을 권장합니다.

### 강제 재시작

* DB 인스턴스 강제 재시작을 수행하면 DB 인스턴스 자체가 재시작됩니다.
* 고가용성 인스턴스는 강제 재시작 기능을 지원하지 않습니다.
* 우선 MySQL에 SIGTERM 명령을 내려 정상 종료되기를 10분간 기다립니다.
* 10분 안에 정상 종료가 진행되면 이후 인스턴스의 OS가 재시작됩니다.
* 10분 안에 정상 종료가 진행되지 않으면 OS가 강제로 재시작됩니다.
    * OS가 강제로 재시작되면 작업 중인 일부 트랜잭션이 유실될 수 있으며, 데이터 볼륨이 손상되어 복구가 불가능해질 수 있습니다.
* 강제 재시작 이후 인스턴스의 상태가 정상으로 돌아오지 않을 수 있습니다. 해당 상황 발생 시 고객 센터로 문의해 주세요.

> [주의] 데이터가 유실되거나 데이터 볼륨이 손상될 가능성이 있으므로 해당 기능은 긴급하고 불가피한 상황 이외에는 사용을 지양해야 합니다.

### 중지

* 당분간 사용하지 않지만 조만간 다시 사용해야 할 DB 인스턴스의 경우, DB 인스턴스의 기능을 중지할 수 있습니다.
* 중지를 요청하면 그 즉시 DB 인스턴스는 종료되며, 중지가 완료되면 DB 인스턴스의 상태가 `종료됨` 상태로 변경됩니다.
    * 이외에도 고가용성 인스턴스의 경우, 장애 조치가 발생할 때 이전 마스터가 중지됩니다.
* 중지된 상태의 인스턴스는 중지된 순간부터 90일간 할인된 요금이 과금되며, 90일이 지난 시점부터는 정상 요금이 과금됩니다.
* 불필요한 요금이 청구되지 않도록 사용하지 않는 인스턴스는 꼭 삭제해 주시기 바랍니다.

> [참고] 고가용성 인스턴스, 읽기 복제본이 있는 마스터 인스턴스, 읽기 복제본은 중지할 수 없습니다.

### 백업

* RDS에서는 모든 백업을 수행한 뒤, 생성한 백업을 자체적인 Object Storage로 업로드하여 보관합니다.
* 자동 백업에 한하여, 원본 인스턴스의 데이터 볼륨 크기만큼의 백업 용량을 무료로 제공합니다.
* 별도의 과금을 원하지 않으면, 백업 주기를 적절하게 조절해야 합니다.
* 백업이 실행되는 동안에는 백업으로 인한 성능 저하가 발생할 수 있습니다.
* 서비스에 영향을 주지 않기 위해서, 서비스의 부하가 적은 시간에 백업을 하는 것이 유리합니다.
* NHN Cloud RDS 는 시점 복원을 지원합니다.
    * 바이너리 로그 사이즈와 보관 주기가 너무 짧은 경우 원하는 시점으로 복원이 어려울 수 있습니다.
* 복원 중인 DB 인스턴스를 백업할 수 없습니다.

#### 자동 백업

* DB 인스턴스의 백업 주기를 1일 이상으로 설정 시, 자동 백업이 활성화 됩니다.
    * 백업 주기를 1일 이상에서 없음으로 변경하는 즉시 모든 자동 백업은 서버에서 그 즉시 삭제됩니다.
    * 삭제된 백업은 복원할 수 없습니다.
* 설정된 백업 주기 만큼 백업 파일을 유지합니다.
* 자동 백업은 백업 시작 시각 부터 Duration 사이 중 임의의 시점에 시작됩니다.
* Duration 은 백업이 시작되는 시각을 의미합니다.
    * Duration 안에 백업을 완료하는 의미가 아닙니다.
    * Duration 안에 백업을 완료하지 못하더라도 백업은 종료되지 않습니다.
* 자동 백업은 원본 인스턴스를 모두 삭제할 경우 같이 삭제됩니다.
* 백업 실패 시 백업 재시도 횟수가 1회 이상 설정돼 있으면 지정된 횟수만큼 재시도합니다.

> [참고] MySQL 5.7 이상에서는 백업 중에 인덱스를 생성하거나 다시 빌드하면 백업에 실패합니다.
> [참고] 백업 도중 DDL 쿼리를 수행하면 백업에 실패합니다.

#### 수동 백업

* 자동 백업 이외에 언제든지 수동으로 백업할 수 있습니다.
* 수동 백업은 명시적으로 삭제하지 않는 한 삭제되지 않습니다.

### 복원

* 보관된 백업을 이용하여 원하는 시점으로 DB 인스턴스를 복원할 수 있습니다.
* 복원 시, 원본 DB 인스턴스를 변경하지 않고 새로운 DB 인스턴스를 생성합니다.
* 복원할 DB 인스턴스의 스토리지 크기를 원본 DB 인스턴스의 스토리지 크기보다 작게 지정할 경우, 복원에 실패할 수 있습니다.

> [참고] 복원이 진행되는 동안 바이너리 로그 파일의 사이즈만큼 Object Storage 사용량이 발생할 수 있습니다.
> [참고] 바이너리 로그 파일이 존재하지 않을 경우에는 시점 복원을 할 수 없습니다.

#### 스냅샷 복원

* 수동 백업 혹은 자동 백업을 이용하여 백업 시점으로 복원할 수 있습니다.
* 복원할 백업을 수행한 DB 인스턴스를 선택한 후, **추가기능 > 복원**을 클릭합니다.

![restore_menu_ko](https://static.toastoven.net/prod_rds/21.11.09/restore_menu_ko.png)

* 복원할 DB 인스턴스의 정보를 입력합니다.

![restore_by_snapshot_ko](https://static.toastoven.net/prod_rds/21.11.09/restore_by_snapshot_ko.png)

* ❶ [스냅샷 복원]을 선택합니다.
* ❷ 복원할 수동 백업 혹은 자동 백업을 선택합니다.

#### 시점 복원

* 수동 백업 혹은 자동 백업과 바이너리 로그 파일을 이용하여 특정 시점으로 복원할 수 있습니다.
* 복원할 시각 혹은 바이너리 로그 파일의 position을 이용하여 복원할 시점을 특정할 수 있습니다.

![restore_by_time_ko](https://static.toastoven.net/prod_rds/21.11.09/restore_by_time_ko.png)

* ❶ [시점 복원 (by timestamp)]를 선택합니다.
* ❷ 복원할 시각을 선택합니다.

![restore_by_position_ko](https://static.toastoven.net/prod_rds/21.11.09/restore_by_position_ko.png)

* ❶ [시점 복원 (by binlog)]를 선택합니다.
* ❷ 복원에 사용할 백업 파일을 선택합니다.
* ❸ 복원을 진행할 마지막 바이너리 로그 파일을 선택합니다.
* ❹ 복원을 진행할 마지막 바이너리 로그 파일의 position 정보를 입력합니다.
    * 입력한 position 직전까지의 변경 사항이 복원됩니다. (입력한 position은 복원되지 않습니다.)
    * 바이너리 로그 파일의 position 정보는 mysqlbinlog 유틸리티를 이용하여 조회할 수 있습니다.
    * 자세한 사용법은 [mysqlbinlog](https://dev.mysql.com/doc/refman/8.0/en/mysqlbinlog.html)를 참고합니다.

### 복제

* 읽기 성능을 높이기 위해서 MySQL 이 지원하는 Read Only Slave 를 만들 수 있습니다.
* Read Only Slave 를 만들기 위해서 원본 DB 인스턴스를 선택한 후 **추가기능 > 복제본 생성**을 누릅니다.

![additional_function_0_ko](https://static.toastoven.net/prod_rds/22.06.14/additional_function_0_ko.png)

* 복제본 생성을 위한 설정을 입력한 후, **복제** 버튼을 누르면 복제본이 생성됩니다.
* 원본 DB 인스턴스와 동일한 사양 혹은 더 높은 사양으로 만드는 것을 권장하며, 낮은 사양으로 생성 시 복제 지연이 발생할 수 있습니다.
* 복재본 생성시, 원본 DB 인스턴스의 I/O 성능이 평소보다 낮아 질 수 있습니다.
* 원본 DB 인스턴스의 크기에 비례하여 복제본 생성 시간이 늘어 날 수 있습니다.

> [참고] 복제가 진행되는 동안 바이너리 로그 파일의 사이즈만큼 Object Storage 사용량이 발생할 수 있습니다.
> [참고] 복제가 완료되면, Master 인스턴스의 접근 규칙(access rule) 항목에 Read Only Slave의 규칙이 추가됩니다.

#### 제약 사항

* 하나의 원본 인스턴스는 최대 5개의 복재 본을 만들 수 있습니다.
* 복제본의 복제본을 만들 수 없습니다.

### 승격

* 복제 관계를 끊고 Read Only Slave 에서 Master 로 변경하는 것을 승격이라 부릅니다.
* 승격된 복제본은 더이상 원본 DB 인스턴스의 수정사항을 자동으로 반영하지 않습니다.
* 승격된 복제본은 독립된 DB 인스턴스로서 동작하게 됩니다.
* 승격 하려는 복제본과 원본 DB 인스턴스 사이에 복제 지연이 있는 경우, 해당 지연이 없어 질 때까지, 승격되지 않습니다.

### 용량 확보

* DB 인스턴스의 리소스를 제거하여 디스크 용량을 확보할 수 있습니다.

#### 바이너리 로그 삭제

![rds_08_20220614_ko](https://static.toastoven.net/prod_rds/22.06.14/rds_08_20220614_ko.png)

* 바이너리 로그 파일을 삭제하여 디스크 공간을 확보합니다.

> [주의] 선택한 대상 바이너리 로그 파일과 그 이전에 생성된 바이너리 로그 파일 모두 삭제됩니다.
> [주의] 바이너리 로그 파일을 삭제했을 때 삭제한 바이너리 로그 파일에 따라 특정 시점으로 복원하지 못할 수도 있습니다.
> [주의] 바이너리 로그 파일이 없을 때는 시점 복원을 할 수 없습니다.

### Storage 확장

![rds_09_20220614_ko](https://static.toastoven.net/prod_rds/22.06.14/rds_09_20220614_ko.png)

* DB 인스턴스의 Storage 크기를 확장합니다.
* Read Only Slave 가 존재하는 경우, Master 와 같은 Storage 크기로 함께 확장됩니다.
* 대상 DB 인스턴스가 재시작 됩니다.
* 고가용성 인스턴스일 경우 장애 조치를 이용해 재시작할 수 있습니다.
    * 장애 조치를 이용하여 스토리지를 확장할 경우 마스터가 교체됩니다. 중단 시간은 줄지만 전체 작업 시간은 더 걸릴 수 있습니다.
    * 마스터가 교체되면 bin log 파일이 모두 삭제되며, 교체되기 전 시간으로 시점 복원이 불가능합니다.
    * Master에 lock이 있거나 복제 지연이 길 경우 장애 조치가 되지 않거나 시간이 많이 소요되므로, 트랜잭션이 모두 종료되었고 복제 지연이 없는지 확인해야 합니다.

### DB 스키마 & DB User 관리

* DB 스키마와 DB User를 웹 콘솔에서 관리할 수 있습니다.

> [참고] 더이상 DB 스키마와 DB User를 쿼리를 통해 생성, 수정, 삭제할 수 없습니다.

![db_schema_and_user_list_20210209_ko](https://static.toastoven.net/prod_rds/21.03.09/rds_01_20210309.png)

* **변경** 버튼 클릭 시, DB 스키마와 사용자를 변경할 수 있게 활성화됩니다.

![db_schema_and_user_modify_20210209_ko](https://static.toastoven.net/prod_rds/21.03.09/rds_02_20210309.png)

* **추가** 버튼 클릭 시, DB 스키마와 DB User의 변경 사항이 한꺼번에 적용됩니다.
* DB 스키마의 이름 변경은 지원하지 않습니다.
* DB User의 권한은 4개의 권한으로 구성됩니다.
    * READ: 데이터를 조회할 수 있습니다.
    * CRUD: READ 권한에 더해 DML 질의를 실행할 수 있습니다.
    * DDL: CRUD 권한에 더해 DDL 질의를 실행할 수 있습니다.
    * CUSTOM: 기존에 사용 중이던 사용자의 권한입니다. CUSTOM 권한으로 변경할 수는 없으며, CUSTOM 권한인 사용자는 오직 삭제만 가능합니다.
* 아래의 DB User는 정책상 사용할 수 없습니다.
    * mysql.session
    * mysql.sys
    * sqlgw
    * admin
    * etladm
    * alertman
    * prom
    * rds_admin
    * rds_mha
    * rds_repl

* DB 스키마와 DB User 항목의 **동기화** 버튼 클릭 시, DB 인스턴스에 생성된 DB 스키마와 DB User 정보들을 각각 가져올 수 있습니다.

## 로그 파일

* DB 인스턴스에 접속하지 않고 편하게 로그파일을 보거나 다운로드할 수 있습니다.
* **DB Instance**를 선택한 후, **Events & Log** 탭을 누르면 설정에 따라 error.log, slow_query.log, general.log, server_audit.log 파일을 볼 수 있습니다.

![rds_13_20210112](https://static.toastoven.net/prod_rds/21.01.12/rds_13_20210112.png)

* 단 반드시 DB Configuration 에서 해당 로그를 남기도록 설정을 해야 합니다.
* **보기** 버튼을 누르면 팝업이 열리며, 로그 파일을 볼 수 있습니다.
* 로그 길이에 입력 된 라인수 만큼 볼 수 있으며, 끝에서부터 512KB 사이즈의 로그를 볼 수 있습니다.

![rds_14_20210112](https://static.toastoven.net/prod_rds/21.01.12/rds_14_20210112.png)

* 로그 파일 전체를 보고 싶다면, **다운로드** 버튼을 눌러 직접 파일을 다운로드 받아야 합니다.

![rds_15_20210112](https://static.toastoven.net/prod_rds/21.01.12/rds_15_20210112.png)

* **다운로드** 버튼을 누르면 팝업이 나타납니다.
* **가져오기** 버튼을 누른후 잠시 기다리면, **다운로드** 버튼이 활성화 되어 다운로드 가능한 상태가 됩니다.
* 로그 파일은 임시 Object Storage 에 업로드 되어 최대 5분 동안 다운로드할 수 있도록 유지됩니다.

> [참고] Object Storage에 업로드 되고 삭제되는 5분간 Object Storage 사용 요금이 청구될 수 있습니다.

### Audit Log

* DB Configuration의 설정을 통해 audit log를 남길 수 있습니다.
* 생성된 audit log 파일은 Event & Log 탭에서 확인하거나, 다운로드할 수 있습니다.
* 자세한 설정값은 다음 사이트에서 확인 가능합니다.
    * https://mariadb.com/kb/en/mariadb-audit-plugin-options-and-system-variables

> [주의] MySQL 5.7.15 8.0.18, 8.0.23 버전은 지원하지 않습니다.

## 이벤트

![event_list_0_ko](https://static.toastoven.net/prod_rds/22.08.09/event_list_0_ko.png)

DB 인스턴스와 관련된 여러 작업 중 발생하는 각종 이벤트 및 알림 그룹의 감시 설정 결과를 확인할 수 있습니다.

* ❶ 이벤트 유형을 선택하여 조회 할 수 있습니다.
* ❷ 이벤트 소스 혹은 메시지를 검색할 수 있습니다.
* ❸ 이벤트 발생 기간을 선택할 수 있습니다.
* ❹ 검색 조건에 맞는 이벤트를 CSV 파일로 저장할 수 있습니다.

### 이벤트 구독

![event_sub_list_0_ko](https://static.toastoven.net/prod_rds/210615/event_sub_list_0_ko.png)

이벤트 구독 현황을 조회할 수 있습니다.

* ❶ **구독 이름** 혹은 **이벤트 소스** 로 검색할 수 있습니다.
* ❷ 이벤트 구독을 신규로 생성 할 수 있습니다.
* ❸ 수정 하고자 하는 구독을 선택하여 구독 내용을 수정할 수 있습니다.
* ❹ 삭제 하고자 하는 구독을 선택하여 삭제할 수 있습니다.

### 이벤트 구독 등록 및 수정

![event_sub_popup_0_ko](https://static.toastoven.net/prod_rds/210615/event_sub_popup_0_ko.png)

* ❶ 이벤트 구독 이름을 입력합니다.
* ❷ 이벤트 구독을 원하는 유형을 선택합니다. 유형에 따라 선택할 수 있는 이벤트 코드 및 이벤트 소스가 제한됩니다.
* ❸ 이벤트 구독을 원하는 코드를 선택합니다.
* ❹ 이벤트 구독을 원하는 소스를 선택합니다.
* ❺ 알림이 발송될 사용자 그룹을 선택합니다. 아무 그룹도 선택하지 않으면 알림 발송이 되지 않습니다.
* ❻ 활성화 여부를 선택합니다.

## 서버 대시보드

![server_dashboard_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_1_ko.png)

각종 성능 지표를 차트 형태로 확인할 수 있습니다.

* ❶ 인스턴스 이름 혹은 IP 주소로 검색할 수 있습니다.
* ❷ 조건에 맞는 서버가 표시됩니다. 서버의 상태에 따라 우측의 아이콘 색상이 변경됩니다.
    * 초록색: 정상 상태
    * 빨간색: 에러 상태
    * 회색: 삭제된 서버
* ❸ 레이아웃을 선택할 수 있습니다.
* ❹ 레이아웃을 수정하거나 삭제할 수 있습니다.
* ❺ **레이아웃 만들기** 창이 나타납니다.
* ❻ 레이아웃에 차트를 추가할 수 있습니다.
* ❼ 레이아웃에 여러 대의 서버를 비교해 볼 수 있는 그룹을 추가할 수 있습니다.
    * 그룹 차트는 서버 간 데이터 비교를 위해 라인 차트로 통일됩니다.
    * 그룹 차트는 기존에 저장된 차트 정보를 공유하지 않기 때문에 차트를 별도로 추가해 주셔야 합니다.
* ❽ 조회 기간을 현재 시각으로 설정 후, 차트를 갱신합니다.
* ❾ 조회 기간을 변경할 수 있습니다.
* ➓ 차트가 표시됩니다.

### 레이아웃 추가

![server_dashboard_layout_create_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_layout_create_1_ko.png)

* ❶ **레이아웃 만들기** 버튼을 클릭합니다.
* ❷ 레이아웃 이름을 입력합니다.

### 레이아웃 수정 및 삭제

![server_dashboard_layout_modify_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_layout_modify_1_ko.png)

* ❶ **관리** 버튼을 클릭합니다.
* ❷ 해당 레이아웃을 수정할 수 있는 편집 화면으로 변경됩니다.
* ❸ 해당 레이아웃을 삭제할 수 있습니다.

![server_dashboard_layout_modify_2_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_layout_modify_2_ko.png)

* ❶ **확인** 버튼을 클릭하면 변경 사항이 저장됩니다.
* ❷ **취소** 버튼을 클릭하면 변경 사항이 취소됩니다.

### 차트 추가

![server_dashboard_chart_add_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_add_1_ko.png)

* ❶ 차트를 추가하기 위해 원하는 레이아웃을 먼저 선택합니다.
* ❷ **차트 추가** 버튼을 클릭하면 아래와 같이 **차트 추가** 창이 나타납니다.

![server_dashboard_chart_add_2_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_add_2_ko.png)

* ❶ 추가하고자 하는 차트를 체크박스로 여러 개를 선택해서 추가할 수 있습니다.
* ❷ 텍스트 영역을 클릭 시 추가하고자 하는 차트를 미리 볼 수 있습니다.

### 차트 수정

![server_dashboard_1_ko](https://static.toastoven.net/prod_rds/210615/server_dashboard_1_ko.png)

* ❶ 차트의 상단 영역을 마우스로 드래그하여 차트를 이동시킬 수 있습니다.
* ❷ 차트를 삭제할 수 있습니다.
* ❸ 차트 우측 하단을 마우스로 드래그하여 차트의 크기를 변경할 수 있습니다.

### 그룹 추가

![server_dashboard_chart_group_add_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_add_1_ko.png)

* ❶ 그룹을 추가하기 위해 원하는 레이아웃을 먼저 선택합니다.
* ❷ **그룹 추가** 버튼을 클릭하면 아래와 같이 **그룹 추가** 창이 나타납니다.

![server_dashboard_chart_group_add_2_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_add_2_ko.png)

* ❶ 그룹 이름을 입력합니다.
* ❷ 차트에 표기할 조회 대상 인스턴스를 선택합니다.
    * 조회 대상 인스턴스는 10개까지만 등록할 수 있습니다.

### 그룹 수정 및 삭제

![server_dashboard_chart_group_modify_delete_1_ko.png](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_modify_delete_1_ko.png)

* ❶ 수정 혹은 삭제할 그룹의 **더보기 메뉴** 버튼을 클릭하면 드롭다운 메뉴가 나타납니다.
* ❷ **편집** 버튼을 클릭하면 **그룹 편집** 창이 나타납니다.
* ❸ **삭제** 버튼을 클릭하면 그룹을 삭제할 수 있습니다.

![server_dashboard_chart_group_modify_delete_2_ko.png](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_modify_delete_2_ko.png)

* ❶ 수정하려는 항목을 수정한 후 확인 버튼을 클릭합니다.

### 그룹 차트 추가

![server_dashboard_chart_group_add_chart_1_ko](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_add_chart_1_ko.png)

* ❶ 추가하고자 하는 차트를 체크박스로 여러 개를 선택해서 추가할 수 있습니다.
* ❷ 텍스트 영역을 클릭 시 추가하고자 하는 차트를 미리 볼 수 있습니다.

### 그룹 차트 라인 색상 변경

![server_dashboard_chart_group_modify_series_color_1_ko.png](https://static.toastoven.net/prod_rds/22.07.12/server_dashboard_chart_group_modify_series_color_1_ko.png)

* ❶ 색상을 변경하려는 서버를 목록에서 클릭하면 색상 변경 창이 나타납니다.
* ❷ 상단 색상표에서 원하는 색상을 클릭해서 변경할 수 있습니다.
    * 기본 색상은 해당 색상표에 나타나는 순서대로 지정됩니다.
* ❸ 색상 코드를 직접 입력해서 변경할 수 있습니다.
    * 직접 입력 시에는 3자리 또는 6자리 색상 코드를 지원합니다.
    * 예) #000 또는 #000000
* ❹ 색상 선택기에서 원하는 색상을 선택하여 변경할 수 있습니다.

## 사용자 그룹

알림 그룹 및 이벤트 구독을 통해 알림을 받을 사용자를 그룹으로 관리할 수 있습니다.

### 사용자 그룹 생성

![user_group_create_0_ko](https://static.toastoven.net/prod_rds/210615/user_group_create_0_ko.png)

* ❶ **사용자 그룹 생성** 버튼을 클릭하면 **사용자 그룹 생성** 창이 나타납니다.

![user_group_create_1_ko](https://static.toastoven.net/prod_rds/23.06.13/user_group_create_1_ko.png)

* ❷ 그룹이름을 입력합니다.
* ❸ 통보 대상 사용자가 표시됩니다. **x** 버튼을 클릭하면 통보 대상에서 제외됩니다.
* ❹ 통보 대상에 사용자를 추가합니다.
* ❺ **전체 프로젝트 멤버**를 통보 대상에 추가합니다. 
  * 추가 시 개별 사용자 추가는 취소됩니다.
  * 해당 사용자 그룹을 이용하여 알람을 전송하게 되는 경우, 당시 전체 프로젝트 멤버 대상으로 알람을 전송합니다.

### 사용자 그룹 수정

![user_group_modify_0_ko](https://static.toastoven.net/prod_rds/210615/user_group_modify_0_ko.png)

* ❶ 수정하려고 하는 사용자 그룹의 **편집** 버튼을 클릭하면 **사용자 그룹 수정** 창이 나타납니다.

![user_group_modify_1_ko](https://static.toastoven.net/prod_rds/210615/user_group_modify_1_ko.png)

* ❷ 수정하려는 항목을 수정 후, **확인** 버튼을 클릭하면 사용자 그룹이 수정됩니다.

### 사용자 그룹 삭제

![user_group_delete_0_ko](https://static.toastoven.net/prod_rds/210615/user_group_delete_0_ko.png)

* ❶ 삭제하려는 사용자 그룹의 **삭제** 버튼을 클릭합니다.

## 알림 그룹

인스턴스의 성능 지표에 감시 설정을 추가하여 알림을 받을 수 있습니다.

### 알림 그룹 생성

![notification_group_create_0_ko](https://static.toastoven.net/prod_rds/210615/notification_group_create_0_ko.png)

* ❶ **그룹 만들기** 버튼을 클릭합니다.

![notification_group_create_1_ko](https://static.toastoven.net/prod_rds/210615/notification_group_create_1_ko.png)

* ❷ 알림 그룹 이름을 입력합니다.
* ❸ 알림 유형을 선택합니다. 다중 선택이 가능합니다.
* ❹ 활성화 여부를 설정합니다.
* ❺ 감시 대상 인스턴스를 선택합니다.
* ❻ 사용자 그룹을 선택합니다.

### 알림 그룹 수정

![notification_group_modify_0_ko](https://static.toastoven.net/prod_rds/210615/notification_group_modify_0_ko.png)

* ❶ 수정 하고자 하는 알림 그룹의 **편집** 버튼을 클릭합니다.

![notification_group_modify_1_ko](https://static.toastoven.net/prod_rds/210615/notification_group_modify_1_ko.png)

* ❷ 수정 하려는 항목을 수정 한후 **확인** 버튼을 클릭합니다.

### 알림 그룹 삭제

![notification_group_modify_2_ko](https://static.toastoven.net/prod_rds/210615/notification_group_modify_2_ko.png)

* ❶ **삭제** 버튼을 클릭하면 등록된 알림 그룹을 삭제할 수 있습니다.

### 감시 설정 추가

![notification_group_watchdog_0_ko](https://static.toastoven.net/prod_rds/210615/notification_group_watchdog_0_ko.png)

* ❶ 감시 설정을 수정하려고 하는 알림 그룹의 **감시 설정** 버튼을 클릭합니다.

![notification_group_watchdog_1_ko](https://static.toastoven.net/prod_rds/210615/notification_group_watchdog_1_ko.png)

* ❷ **감시 설정** 버튼을 클릭합니다.

![notification_group_watchdog_2_ko](https://static.toastoven.net/prod_rds/210615/notification_group_watchdog_2_ko.png)

* ❸ 감시할 항목을 선택합니다.
* ❹ 비교 방법을 선택합니다.
* ❺ 임겟값을 입력합니다. 항목에 따라 입력할 수 있는 최댓값이 다릅니다.
* ❻ 지속 시간을 입력합니다.
* ❼ 추가 버튼을 클릭하면 감시 설정이 등록됩니다. 취소 버튼을 클릭하면 감시 설정이 등록되지 않습니다.

### 감시 설정 수정 및 삭제

![notification_group_watchdog_3_ko](https://static.toastoven.net/prod_rds/210615/notification_group_watchdog_3_ko.png)

* ❶ **편집** 버튼을 클릭하면 해당 감시 설정을 수정할 수 있습니다.
* ❷ **삭제** 버튼을 클릭하면 해당 감시 설정을 삭제할 수 있습니다.

## 사용자 권한 분리

* 프로젝트 멤버의 권한을 RDS for MySQL ADMIN / RDS for MySQL MEMBER로 구분하여 부여할 수 있습니다.
* RDS for MySQL ADMIN 권한은 기존처럼 모든 기능을 사용 가능합니다.
* RDS for MySQL MEMBER 권한은 정보를 조회하는 기능만 사용 가능합니다.
    * 인스턴스를 생성, 수정, 삭제하거나, 인스턴스를 대상으로 하는 어떠한 기능도 사용할 수 없습니다.
    * 단, Notification 탭에서 알람과 관련된 기능은 사용 가능합니다.

## Processlist

### 기본 데이터 조회

![processlist_1_ko](https://static.toastoven.net/prod_rds/22.07.12/processlist_1_ko.png)

DB에서 수집된 프로세스 리스트와 InnoDB 상태를 차트와 표 형태로 확인할 수 있습니다.
차트에서는 시간대별로 수집된 개수를 나타내며 표에서 상세정보를 확인할 수 있습니다.

* ❶ 인스턴스 이름 혹은 IP 주소로 검색할 수 있습니다.
* ❷ 조건에 맞는 서버가 표시됩니다. 서버의 상태에 따라 우측의 아이콘 색상이 변경됩니다.
    * 초록색: 정상 상태
    * 빨간색: 에러 상태
    * 회색: 삭제된 서버
* ❸ Process List와 InnoDB Status 정보를 탭으로 선택해서 조회합니다.
* ❹ 조회 기간을 현재 시각으로 설정한 후, 차트를 갱신합니다
* ❺ 조회 기간을 변경할 수 있습니다.
    * 조회 기간은 최대 1시간 이내로 설정해야 합니다.
* ❻ 검색 키워드를 입력해 조회 기간 내의 데이터를 검색합니다.
* ❼ 한 페이지 내에 조회할 데이터 개수를 지정합니다(기본값: 20개).
* ❽ **CSV 저장** 버튼을 클릭하면 전체 조회 데이터를 CSV 파일 형식으로 저장합니다.
* ❾ 수집된 프로세스 리스트 정보를 최신순으로 정렬하여 출력합니다.
* ➓ **상세보기** 버튼을 클릭하면 해당하는 줄의 상세 정보를 확인할 수 있습니다.
* ⓫ **보기** 버튼을 클릭하면 해당하는 시간의 InnoDB Status 정보를 확인할 수 있습니다.

### 키워드 검색

![processlist_search_1_ko](https://static.toastoven.net/prod_rds/22.07.12/processlist_search_1_ko.png)

* ❶ 검색 키워드를 입력해 데이터 필드 전체를 탐색합니다.
    * 검색 키워드는 대소문자를 구분하지 않으며 띄어쓰기로 여러 키워드를 구분해서 입력할 수 있습니다.
    * 문장 검색이 필요한 경우 따옴표("",'')로 감싸서 입력합니다.
        * 예시) "for semi-sync" slave
* ❷ 검색 기간은 최신순으로 자동 설정되며 10분 단위로 조회하거나 수집된 데이터가 많은 경우 1,000건 이상 되는 시점으로 제한해서 제한된 단위로 조회합니다.
* ❸ **추가검색** 버튼을 클릭하면 위와 동일한 단위로 추가 조회합니다.
* ❹ **초기화** 버튼을 클릭하면 입력된 검색 키워드, 검색 기간 등이 모두 초기화됩니다.
* ❺ **CSV 저장** 버튼을 클릭하면 자동 설정된 검색 기간과 상관없이 전체 기간을 조회해서 저장합니다.

### 차트 조회 범위 설정

![processlist_zoom_1_ko](https://static.toastoven.net/prod_rds/22.07.12/processlist_zoom_1_ko.png)

* ❶ 차트를 드래그해서 조회 범위를 설정할 수 있습니다.

![processlist_zoom_2_ko](https://static.toastoven.net/prod_rds/22.07.12/processlist_zoom_2_ko.png)

* ❶ 확대 상태를 초기화할 수 있습니다.
* ❷ 확대된 상태에서 검색 시 확대 범위 내에서만 검색합니다.
* ❸ **CSV 저장** 버튼을 클릭하면 확대 범위 내 전체 데이터를 저장합니다.

### 차트 시점 조회

![processlist_select_1_ko](https://static.toastoven.net/prod_rds/22.07.12/processlist_select_1_ko.png)

* ❶ 차트 데이터를 선택하면 해당 시점의 데이터만 조회합니다.
* ❷ 차트 데이터가 선택된 상태에서 검색 시 선택된 시점 내에서만 검색합니다.
* ❸ **CSV 저장** 버튼을 클릭하면 선택된 시점 내 전체 데이터를 저장합니다.

## 부록

### 부록1. 하이퍼바이저 점검을 위한 DB 인스턴스 마이그레이션 가이드

NHN Cloud는 주기적으로 DB 인스턴스의 하이퍼바이저 소프트웨어를 업데이트하여 보안과 안정성을 향상시키고 있습니다.
점검 대상 하이퍼바이저에서 구동 중인 DB 인스턴스는 마이그레이션을 통해 점검이 완료된 하이퍼바이저로 이동해야 합니다.

DB 인스턴스 마이그레이션은 NHN Cloud 콘솔에서 시작할 수 있습니다.
DB 구성에 따라 특정 DB 인스턴스를 선택하여 마이그레이션 시, 연관된 DB 인스턴스(예를 들면 Slave 인스턴스)도 점검 대상이면 같이 마이그레이션을 진행합니다.
아래 가이드에 따라 콘솔에 있는 마이그레이션 기능을 이용하시기 바랍니다.
점검 대상으로 지정된 DB 인스턴스가 있는 프로젝트로 이동합니다.

#### 1. 점검 대상 DB 인스턴스를 확인 합니다.

이름 옆에 마이그레이션 버튼이 있는 DB 인스턴스가 점검 대상 인스턴스입니다.

![rds_planed_migration_0](https://static.toastoven.net/prod_rds/planned_migration_alarm/image0_kr.png)

마이그레이션 버튼 위에 마우스 커서를 올리면 자세한 점검 일정을 확인할 수 있습니다.

![rds_planed_migration_1](https://static.toastoven.net/prod_rds/planned_migration_alarm/image1_kr.png)

#### 2. 점검 대상 DB 인스턴스에 접속 중인 응용 프로그램을 종료해야 합니다.

DB에 연결된 서비스에 영향을 주지 않도록 적절한 조치를 취하시길 바랍니다.
서비스에 영향을 줄 수밖에 없을 때는 NHN Cloud 고객 센터로 연락해 주시면 적합한 조치를 안내해 드리겠습니다.

#### 3. 점검 대상 DB 인스턴스를 선택하고 마이그레이션 버튼을 클릭한 후 DB 인스턴스 마이그레이션 확인을 묻는 창이 나타나면 확인 버튼을 클릭합니다.

![rds_planed_migration_2](https://static.toastoven.net/prod_rds/planned_migration_alarm/image2_kr.png)

#### 4. DB 인스턴스 마이그레이션이 끝날 때까지 대기합니다.

DB 인스턴스 상태가 변경되지 않는다면 '새로 고침'을 해보시기 바랍니다.

![rds_planed_migration_3](https://static.toastoven.net/prod_rds/planned_migration_alarm/image3_kr.png)

DB 인스턴스가 마이그레이션되는 동안에는 아무런 조작을 할 수 없습니다.
DB 인스턴스 마이그레이션이 정상적으로 완료되지 않으면 자동으로 관리자에게 보고되며, NHN Cloud에서 별도로 연락을 드립니다.

### 부록2. RDS를 이용하여 Federated Storage Engine 사용 시 구성 가이드

Federated Storage Engine을 사용하는 경우 다음을 고려해야 합니다.

#### 로컬 노드로써 RDS를 이용하는 구성의 경우

* 리모트 노드로의 송신을 허용하는 설정이 필요합니다.
    * 인스턴스 상세 설정의 **백업 & Access 제어** 탭에서 설정이 가능합니다.
    * '#간단히 시작하기'의 '사용자 접근 제어'를 참고 바랍니다.
* 만약 로컬 노드 역할의 RDS에 Read Only Slave를 추가한 구성으로 사용할 경우, DB Configuration의 replicate-ignore-table에 federated 설정된 테이블을 명시해야 합니다.
    * Read Only Slave를 구성하게 될 경우, federated 테이블 또한 복제되어 Master와 Read Only Slave가 리모트 노드를 함께 바라보게 됩니다.
    * 이 경우 Master에 수행한 데이터 입력이 federated 설정에 따라 리모트 노드에도 수행되고, Read Only Slave에서도 마찬가지로 동일한 입력이 수행되어 중복 키 에러 등으로 인한 복제 중단이 발생할 수 있습니다.
    * Read Only Slave가 federated 테이블은 복제하지 않도록 replicate-ignore-table에 설정이 필요합니다.
    * 인스턴스 상세 설정의 DB Configuration 탭에서 설정이 가능합니다.

#### 리모트 노드로써 RDS를 이용하는 구성의 경우

* 로컬 노드에서의 수신을 허용하는 설정이 필요합니다.
    * 인스턴스 상세 설정의 **백업 & Access 제어** 탭에서 설정이 가능합니다.
    * '#간단히 시작하기'의 '사용자 접근 제어'를 참고 바랍니다.