## Database > RDS for MySQL > 모니터링

DB 인스턴스의 각종 성능 지표와 DB 인스턴스와 관련된 여러 작업 중 발생하는 각종 이벤트를 모니터링할 수 있습니다.
서버 대시보드를 통해 성능 지표를 차트 형태로 확인할 수 있습니다.
알림 그룹에 감시 설정을 하면 특정 지표의 변화를 감지하여 해당 변경 사항에 대하여 이메일 혹은 SMS로 알림을 받을 수 있습니다.

## 이벤트

이벤트는 RDS for MySQL이나 사용자에 의해 발생한 중요한 사건을 의미합니다. 이벤트는 이벤트 유형, 발생 일시, 이벤트 소스와 메시지로 구성됩니다. 이벤트는 웹 콘솔에서 조회 가능하며, 구독을 통해 이메일, SMS로 이벤트 발생 알림을 받을 수 있습니다. 이벤트의 유형과 발생 가능한 이벤트는 아래와 같습니다.

| 이벤트 유형   | 이벤트 코드      | 이벤트 메시지                          |
|----------|-------------|----------------------------------|
| INSTANCE | INSTC_02_01 | DB Instance 시작                   |
| INSTANCE | INSTC_03_01 | DB Instance 종료                   |
| INSTANCE | INSTC_04_00 | DB Instance 삭제 시작                |
| INSTANCE | INSTC_04_01 | DB Instance 삭제 완료                |
| INSTANCE | INSTC_04_04 | DB Instance 삭제 실패                |
| INSTANCE | INSTC_05_00 | DB Instance 백업 시작                |
| INSTANCE | INSTC_05_01 | DB Instance 백업 완료                |
| INSTANCE | INSTC_05_04 | DB Instance 백업 실패                |
| INSTANCE | INSTC_06_00 | DB Instance 복원 시작                |
| INSTANCE | INSTC_06_01 | DB Instance 복원 완료                |
| INSTANCE | INSTC_06_04 | DB Instance 복원 실패                |
| INSTANCE | INSTC_07_01 | 자동 백업 설정 활성화                     |
| INSTANCE | INSTC_08_01 | 자동 백업 설정 비활성화                    |
| INSTANCE | INSTC_09_00 | 상세 설정 변경 시작                      |
| INSTANCE | INSTC_09_01 | 상세 설정 변경 완료                      |
| INSTANCE | INSTC_09_04 | 상세 설정 변경 실패                      |
| INSTANCE | INSTC_10_00 | 백업 설정 변경 시작                      |
| INSTANCE | INSTC_10_01 | 백업 설정 변경 완료                      |
| INSTANCE | INSTC_10_04 | 백업 설정 변경 실패                      |
| INSTANCE | INSTC_11_01 | 사용자 접근 제어 변경 완료                  |
| INSTANCE | INSTC_12_00 | DB 구성 변경 시작                      |
| INSTANCE | INSTC_12_01 | DB 구성 변경 완료                      |
| INSTANCE | INSTC_12_04 | DB 구성 변경 실패                      |
| INSTANCE | INSTC_13_01 | 플로팅 IP 연결                        |
| INSTANCE | INSTC_14_01 | 플로팅 IP 연결 해제                     |
| INSTANCE | INSTC_15_00 | DB Instance 복제 시작                |
| INSTANCE | INSTC_15_01 | DB Instance 복제 완료                |
| INSTANCE | INSTC_15_04 | DB Instance 복제 실패                |
| INSTANCE | INSTC_16_00 | DB Instance 승격 시작                |
| INSTANCE | INSTC_16_01 | DB Instance 승격 완료                |
| INSTANCE | INSTC_16_04 | DB Instance 승격 실패                |
| INSTANCE | INSTC_21_01 | DB Instance 정상화                  |
| INSTANCE | INSTC_22_01 | DB Instance 용량 부족                |
| INSTANCE | INSTC_23_01 | DB Instance 연결 실패                |
| INSTANCE | INSTC_24_00 | 인스턴스 타입 변경 시작                    |
| INSTANCE | INSTC_24_01 | 인스턴스 타입 변경 완료                    |
| INSTANCE | INSTC_24_04 | 인스턴스 타입 변경 실패                    |
| INSTANCE | INSTC_25_00 | Storage 확장 시작                    |
| INSTANCE | INSTC_25_01 | Storage 확장 완료                    |
| INSTANCE | INSTC_25_04 | Storage 확장 실패                    |
| INSTANCE | INSTC_26_00 | failover 발생                      |
| INSTANCE | INSTC_26_01 | failover 완료                      |
| INSTANCE | INSTC_26_04 | failover 실패                      |
| INSTANCE | INSTC_27_01 | DB Instance 용량 확보                |
| INSTANCE | INSTC_28_01 | HA DB Instance 시작                |
| INSTANCE | INSTC_29_01 | HA DB Instance 종료                |
| INSTANCE | INSTC_30_01 | 복제 중단                            |
| INSTANCE | INSTC_34_01 | 고가용성 일시 중지                       |
| INSTANCE | INSTC_34_04 | 고가용성 일시 중지 실패                    |
| INSTANCE | INSTC_35_01 | 고가용성 다시 시작                       |
| INSTANCE | INSTC_35_04 | 고가용성 다시 시작 실패                    |
| INSTANCE | INSTC_36_01 | 장애 조치를 이용한 인스턴스 재시작 완료           |
| INSTANCE | INSTC_36_04 | 장애 조치를 이용한 인스턴스 재시작 실패           |
| INSTANCE | INSTC_37_01 | DB User 생성                       |
| INSTANCE | INSTC_37_04 | DB User 생성 실패                    |
| INSTANCE | INSTC_38_01 | DB User 변경                       |
| INSTANCE | INSTC_38_04 | DB User 변경 실패                    |
| INSTANCE | INSTC_39_01 | DB User 삭제                       |
| INSTANCE | INSTC_40_01 | DB 스키마 생성                        |
| INSTANCE | INSTC_40_04 | DB 스키마 생성 실패                     |
| INSTANCE | INSTC_41_01 | DB 스키마 삭제                        |
| INSTANCE | INSTC_47_00 | DB 인스턴스 백업 및 내보내기 시작             |
| INSTANCE | INSTC_47_01 | DB 인스턴스 백업 및 내보내기 완료             |
| INSTANCE | INSTC_47_04 | DB 인스턴스 백업 및 내보내기 실패             |
| INSTANCE | INSTC_48_00 | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 시작 |
| INSTANCE | INSTC_48_01 | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 완료 |
| INSTANCE | INSTC_48_04 | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 실패 |
| INSTANCE | INSTC_49_00 | DB 인스턴스 강제 재시작 실행                |
| INSTANCE | INSTC_50_00 | 백업 내보내기 시작                       |
| INSTANCE | INSTC_50_01 | 백업 내보내기 완료                       |
| INSTANCE | INSTC_50_04 | 백업 내보내기 실패                       |
| INSTANCE | INSTC_51_01 | 사용자 쿼리 지연으로 인한 백업 실패             |
| INSTANCE | INSTC_52_01 | 사용자 쿼리 지연으로 인한 백업 및 내보내기 실패      |
| INSTANCE | INSTC_53_00 | DB 인스턴스 중지 시작                    |
| INSTANCE | INSTC_53_01 | DB 인스턴스 중지 완료                    |
| INSTANCE | INSTC_53_04 | DB 인스턴스 중지 실패                    |
| INSTANCE | INSTC_55_01 | 복제 지연으로 인한 장애 조치 재시작 실패          |
| BACKUP   | BACUP_02_01 | 백업 삭제 완료                         |
| BACKUP   | BACUP_05_00 | 백업 내보내기 시작                       |
| BACKUP   | BACUP_05_01 | 백업 내보내기 완료                       |
| BACKUP   | BACUP_05_04 | 백업 내보내기 실패                       |
| BACKUP   | BACUP_06_01 | 사용자 쿼리 지연으로 인한 백업 실패             |
| TENANT   | TENAT_01_04 | CPU 코어 수 제한                      |
| TENANT   | TENAT_02_04 | RAM 용량 제한                        |
| TENANT   | TENAT_03_04 | 개별 볼륨 크기 제한                      |
| TENANT   | TENAT_04_04 | 프로젝트 전체 볼륨 크기 제한                 |
| TENANT   | TENAT_05_04 | Read Only Slave 개수 제한            |

### 이벤트 구독

이벤트를 구독하면 이벤트 발생 시 이메일 및 SMS로 알림을 받을 수 있습니다. 이벤트 구독에 연결된 사용자 그룹의 사용자들에게 알림을 발송합니다. 이벤트 유형, 이벤트 코드, 이벤트 소스로 세분화하여 구독할 수 있습니다. 잠시 알림을 중단하려면 이벤트 구독의 활성화 여부를 수정합니다. 활성화하지 않으면 알림을 발송하지 않습니다.

## 서버 대시보드

서버 대시보드에서 성능 지표를 차트 형태로 시각화해 볼 수 있습니다. 지표는 1분에 한 번씩 수집되며 최대 1년간 보관됩니다. 지표 데이터는 1분, 5분, 30분, 2시간, 1일 단위의 평균값으로 집계됩니다.

### 레이아웃

차트를 보기 위해서 레이아웃을 먼저 구성해야 합니다. 레아아웃은 여러 차트로 구성되며 각 차트의 위치와 크기를 저장합니다.
RDS for MySQL은 **기본 시스템 지표**, **기본 MySQL 지표** 2개의 기본 레이아웃을 제공합니다. 기본 레이아웃은 사용자가 수정, 삭제할 수 없습니다.

### 차트

DB 인스턴스의 각종 성능 지표를 차트 형태로 볼 수 있습니다. 성능 지표마다 각기 다른 형태의 차트로 구성되어 있습니다.
RDS for MySQL에서 지원하는 차트는 아래와 같습니다.

| 차트                 | 지표(단위)                                                                                           |
|--------------------|--------------------------------------------------------------------------------------------------|
| CPU 사용률            | cpu used (%)                                                                                     |
| CPU 상세             | cpu user (%)<br> cpu system (%)<br>cpu nice (%)<br>cpu IO wait(%)                                |
| 메모리 사용량            | memory used (%)                                                                                  |
| 메모리 상세             | memory used (bytes)<br> memory buffers (bytes)<br> memory cached (bytes)<br> memory free (bytes) |
| 스왑 사용량             | swap used (bytes)<br> swap total (bytes)                                                         |
| Storage 사용량        | storage used (%)                                                                                 |
| Storage IO         | disk read (bytes)<br> disk write (bytes)                                                         |
| 네트워크 데이터 송수신       | nic incoming (bytes)<br> nic outgoing (bytes)                                                    |
| CPU 평균 부하          | 1m<br> 5m<br> 15m                                                                                |
| Queries Per Second | qps                                                                                              |
| Connection         | total<br> running<br> cached                                                                     |
| Connection Ratio   | ratio (%)                                                                                        |
| Database Activity  | select<br> insert<br> update<br> delete<br> replace<br> call<br>                                 |
| Buffer Pool        | buffer pool total (bytes)<br> buffer pool used (%)                                               |
| Slow Query         | counts                                                                                           |
| 복제 지연              | sec (초)                                                                                          |
| Row Access         | index<br> full scan                                                                              |

## 알림 그룹

알림 그룹을 통해 성능 지표에 대한 알림을 받을 수 있습니다.
지정합니다. -> 알림 그룹에 감시 대상 인스턴스와 알림을 받을 사용자 그룹을 지정합니다.
감시 설정을 통해 알림을 받을 성능 지표의 임겟값과 조건을 설정합니다.
설정된 지표가 감시 설정의 조건을 충족하면 연결된 사용자 그룹에 알림을 발송하게 됩니다.
알림 그룹에 설정된 알림 유형에 따라 SMS 혹은 메일로 알림을 발송합니다.

### 감시 설정

감시 설정은 항목과 비교 방법, 임겟값, 지속 시간으로 구성됩니다. 감시 설정의 지속 시간은 중요한 요소입니다. 지속 시간은 감시 대상이 지정한 임계치에 도달한 후 그 상태가 지속되는 시간을 조건으로 지정할 때 사용합니다. 예를 들어, CPU 사용률의 임계치가 90% 이상이고 지속 시간이 5분이라면, 해당 알림 그룹과 연동된 서버의 CPU 사용률이 90% 이상인 상태가 5분 이상 지속되었을 때 사용자 그룹에 정의된 사용자들에게 알림을 보냅니다. 만약 CPU 사용률이 90% 이상이 되어도, 5분 이내에 90% 미만으로 떨어지면 알림이 발생하지 않습니다.

## 사용자 그룹

알림을 받을 사용자를 그룹으로 관리할 수 있습니다. 알림 대상은 반드시 프로젝트 멤버로 등록이 되어 있어야 합니다.
사용자 그룹에 속한 사용자가 프로젝트 멤버에서 제외되면, 사용자 그룹에 속해 있다 하더라도 알림을 받을 수 없습니다.

> [주의] 실명 인증을 진행하지 않아 휴대폰 정보가 없을 경우 SMS 알림을 받을 수 없습니다.

