## Database > RDS for MySQL > 알림

## 이벤트

이벤트는 RDS for MySQL이나 사용자에 의해 발생한 중요한 사건을 의미합니다. 이벤트는 이벤트 유형, 발생 일시, 원본 소스와 메시지로 구성됩니다. 이벤트는 웹 콘솔에서 조회 가능하며, 구독을 통해 이메일, SMS으로 이벤트 발생 알림을 받을 수 있습니다. 이벤트의 유형과 발생 가능한 이벤트는 아래와 같습니다.

| 이벤트 코드      | 이벤트 유형            | 구독 가능 여부 | 설명                                   |
|-------------|-------------------|----------|--------------------------------------|
| BACUP_01_00 | BACKUP            | 아니오      | DB 인스턴스 백업 시작                        |
| BACUP_01_01 | BACKUP            | 아니오      | DB 인스턴스 백업 완료                        |
| BACUP_01_04 | BACKUP            | 아니오      | DB 인스턴스 백업 실패                        |
| BACUP_02_01 | BACKUP            | 예        | 백업 삭제 완료                             |
| BACUP_04_00 | BACKUP            | 예        | 오브젝트 스토리지 업로드 시작                     |
| BACUP_04_01 | BACKUP            | 예        | 오브젝트 스토리지 업로드 완료                     |
| BACUP_04_04 | BACKUP            | 예        | 오브젝트 스토리지 업로드 실패                     |
| BACUP_05_00 | BACKUP            | 예        | 백업 내보내기 시작                           |
| BACUP_05_01 | BACKUP            | 예        | 백업 내보내기 완료                           |
| BACUP_05_04 | BACKUP            | 예        | 백업 내보내기 실패                           |
| BACUP_06_01 | BACKUP            | 아니오      | DB 인스턴스 백업 실패(알려진 원인)                |
| SECGP_01_01 | DB_SECURITY_GROUP | 아니오      | DB 보안 그룹 생성                          |
| SECGP_02_00 | DB_SECURITY_GROUP | 아니오      | DB 보안 그룹 변경 시작                       |
| SECGP_02_01 | DB_SECURITY_GROUP | 아니오      | DB 보안 그룹 변경 완료                       |
| SECGP_02_04 | DB_SECURITY_GROUP | 아니오      | DB 보안 그룹 변경 실패                       |
| SECGP_03_01 | DB_SECURITY_GROUP | 아니오      | DB 보안 그룹 삭제                          |
| INSTC_01_00 | INSTANCE          | 아니오      | DB 인스턴스 생성 시작                        |
| INSTC_01_01 | INSTANCE          | 아니오      | DB 인스턴스 생성 완료                        |
| INSTC_01_04 | INSTANCE          | 아니오      | DB 인스턴스 생성 실패                        |
| INSTC_02_01 | INSTANCE          | 예        | DB 인스턴스 시작                           |
| INSTC_03_01 | INSTANCE          | 예        | DB 인스턴스 종료                           |
| INSTC_04_00 | INSTANCE          | 예        | DB 인스턴스 삭제 시작                        |
| INSTC_04_01 | INSTANCE          | 예        | DB 인스턴스 삭제 완료                        |
| INSTC_04_04 | INSTANCE          | 예        | DB 인스턴스 삭제 실패                        |
| INSTC_05_00 | INSTANCE          | 예        | DB 인스턴스 백업 시작                        |
| INSTC_05_01 | INSTANCE          | 예        | DB 인스턴스 백업 완료                        |
| INSTC_05_04 | INSTANCE          | 예        | DB 인스턴스 백업 실패                        |
| INSTC_06_00 | INSTANCE          | 예        | DB 인스턴스 복원 시작                        |
| INSTC_06_01 | INSTANCE          | 예        | DB 인스턴스 복원 완료                        |
| INSTC_06_04 | INSTANCE          | 예        | DB 인스턴스 복원 실패                        |
| INSTC_07_01 | INSTANCE          | 예        | 자동 백업 설정 활성화                         |
| INSTC_08_01 | INSTANCE          | 예        | 자동 백업 설정 비활성화                        |
| INSTC_09_00 | INSTANCE          | 예        | 상세 설정 변경 시작                          |
| INSTC_09_01 | INSTANCE          | 예        | 상세 설정 변경 완료                          |
| INSTC_09_04 | INSTANCE          | 예        | 상세 설정 변경 실패                          |
| INSTC_10_00 | INSTANCE          | 예        | 백업 및 사용자 접근 제어 설정 변경 시작              |
| INSTC_10_01 | INSTANCE          | 예        | 백업 및 사용자 접근 제어 설정 변경 완료              |
| INSTC_10_04 | INSTANCE          | 예        | 백업 및 사용자 접근 제어 설정 변경 실패              |
| INSTC_11_01 | INSTANCE          | 예        | 사용자 접근 제어 변경 완료                      |
| INSTC_13_01 | INSTANCE          | 예        | 플로팅 IP 연결                            |
| INSTC_14_01 | INSTANCE          | 예        | 플로팅 IP 연결 해제                         |
| INSTC_15_00 | INSTANCE          | 예        | DB 인스턴스 복제 시작                        |
| INSTC_15_01 | INSTANCE          | 예        | DB 인스턴스 복제 완료                        |
| INSTC_15_04 | INSTANCE          | 예        | DB 인스턴스 복제 실패                        |
| INSTC_16_00 | INSTANCE          | 예        | DB 인스턴스 승격 시작                        |
| INSTC_16_01 | INSTANCE          | 예        | DB 인스턴스 승격 완료                        |
| INSTC_16_04 | INSTANCE          | 예        | DB 인스턴스 승격 실패                        |
| INSTC_21_01 | INSTANCE          | 예        | DB 인스턴스 정상화                          |
| INSTC_22_01 | INSTANCE          | 예        | DB 인스턴스 용량 부족                        |
| INSTC_23_01 | INSTANCE          | 예        | DB 인스턴스 연결 실패                        |
| INSTC_24_00 | INSTANCE          | 예        | DB 인스턴스 타입 변경 시작                     |
| INSTC_24_01 | INSTANCE          | 예        | DB 인스턴스 타입 변경 완료                     |
| INSTC_24_04 | INSTANCE          | 예        | DB 인스턴스 타입 변경 실패                     |
| INSTC_25_00 | INSTANCE          | 예        | Storage 확장 시작                        |
| INSTC_25_01 | INSTANCE          | 예        | Storage 확장 완료                        |
| INSTC_25_04 | INSTANCE          | 예        | Storage 확장 실패                        |
| INSTC_26_00 | INSTANCE          | 예        | DB 인스턴스 장애 조치 발생                     |
| INSTC_26_01 | INSTANCE          | 예        | DB 인스턴스 장애 조치 완료                     |
| INSTC_26_04 | INSTANCE          | 예        | DB 인스턴스 장애 조치 실패                     |
| INSTC_27_01 | INSTANCE          | 예        | DB 인스턴스 용량 확보                        |
| INSTC_27_04 | INSTANCE          | 예        | DB 인스턴스 용량 확보 실패                     |
| INSTC_28_01 | INSTANCE          | 예        | 고가용성 DB 인스턴스 시작                      |
| INSTC_29_01 | INSTANCE          | 예        | 고가용성 DB 인스턴스 종료                      |
| INSTC_30_01 | INSTANCE          | 예        | 복제 중단                                |
| INSTC_31_00 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 복구 시작         |
| INSTC_31_01 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 복구 완료         |
| INSTC_31_04 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 복구 실패         |
| INSTC_32_00 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 재구축 시작        |
| INSTC_32_01 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 재구축 완료        |
| INSTC_32_04 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 재구축 실패        |
| INSTC_33_00 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 제거 시작         |
| INSTC_33_01 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 제거 완료         |
| INSTC_33_04 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스 고가용성 제거 실패         |
| INSTC_34_01 | INSTANCE          | 예        | 고가용성 일시 중지                           |
| INSTC_34_04 | INSTANCE          | 예        | 고가용성 일시 중지 실패                        |
| INSTC_35_01 | INSTANCE          | 예        | 고가용성 다시 시작                           |
| INSTC_35_04 | INSTANCE          | 예        | 고가용성 다시 시작 실패                        |
| INSTC_36_01 | INSTANCE          | 예        | 장애 조치를 이용한 인스턴스 재시작 완료               |
| INSTC_36_04 | INSTANCE          | 예        | 장애 조치를 이용한 인스턴스 재시작 실패               |
| INSTC_37_01 | INSTANCE          | 예        | DB User 생성                           |
| INSTC_37_04 | INSTANCE          | 예        | DB User 생성 실패                        |
| INSTC_38_01 | INSTANCE          | 예        | DB User 변경                           |
| INSTC_38_04 | INSTANCE          | 예        | DB User 변경 실패                        |
| INSTC_39_01 | INSTANCE          | 예        | DB User 삭제                           |
| INSTC_40_01 | INSTANCE          | 예        | DB 스키마 생성                            |
| INSTC_40_04 | INSTANCE          | 예        | DB 스키마 생성 실패                         |
| INSTC_41_01 | INSTANCE          | 예        | DB 스키마 삭제                            |
| INSTC_42_04 | INSTANCE          | 아니오      | CPU 코어 수 제한                          |
| INSTC_43_04 | INSTANCE          | 아니오      | RAM 용량 제한                            |
| INSTC_44_04 | INSTANCE          | 아니오      | 개별 볼륨 크기 제한                          |
| INSTC_45_04 | INSTANCE          | 아니오      | 프로젝트 전체 볼륨 크기 제한                     |
| INSTC_46_04 | INSTANCE          | 아니오      | Read Only Slave 개수 제한                |
| INSTC_47_00 | INSTANCE          | 예        | DB 인스턴스 백업 및 내보내기 시작                 |
| INSTC_47_01 | INSTANCE          | 예        | DB 인스턴스 백업 및 내보내기 완료                 |
| INSTC_47_04 | INSTANCE          | 예        | DB 인스턴스 백업 및 내보내기 실패                 |
| INSTC_48_00 | INSTANCE          | 예        | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 시작     |
| INSTC_48_01 | INSTANCE          | 예        | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 완료     |
| INSTC_48_04 | INSTANCE          | 예        | 오브젝트 스토리지에 있는 백업으로 DB 인스턴스 복원 실패     |
| INSTC_49_00 | INSTANCE          | 예        | DB 인스턴스 강제 재시작 실행                    |
| INSTC_50_00 | INSTANCE          | 예        | 백업 내보내기 시작                           |
| INSTC_50_01 | INSTANCE          | 예        | 백업 내보내기 완료                           |
| INSTC_50_04 | INSTANCE          | 예        | 백업 내보내기 실패                           |
| INSTC_51_01 | INSTANCE          | 예        | DB 인스턴스 백업 실패                        |
| INSTC_52_01 | INSTANCE          | 예        | DB 인스턴스 백업 및 내보내기 실패                 |
| INSTC_53_00 | INSTANCE          | 예        | DB 인스턴스 중지 시작                        |
| INSTC_53_01 | INSTANCE          | 예        | DB 인스턴스 중지 완료                        |
| INSTC_53_04 | INSTANCE          | 예        | DB 인스턴스 중지 실패                        |
| INSTC_54_00 | INSTANCE          | 예        | DB 인스턴스 복제 재구축 시작                    |
| INSTC_54_01 | INSTANCE          | 예        | DB 인스턴스 복제 재구축 완료                    |
| INSTC_54_04 | INSTANCE          | 예        | DB 인스턴스 복제 재구축 실패                    |
| INSTC_55_01 | INSTANCE          | 예        | 복제 딜레이로 인한 장애 조치 재시작 실패              |
| INSTC_56_00 | INSTANCE          | 예        | DB 인스턴스 보안 그룹 변경 시작                  |
| INSTC_56_01 | INSTANCE          | 예        | DB 인스턴스 보안 그룹 변경 완료                  |
| INSTC_56_04 | INSTANCE          | 예        | DB 인스턴스 보안 그룹 변경 실패                  |
| INSTC_57_00 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스를 일반 DB 인스턴스로 변경 시작 |
| INSTC_57_01 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스를 일반 DB 인스턴스로 변경 완료 |
| INSTC_57_04 | INSTANCE          | 예        | 장애 조치 완료된 DB 인스턴스를 일반 DB 인스턴스로 변경 실패 |
| INSTC_58_00 | INSTANCE          | 예        | 파라미터 그룹 변경 시작                        |
| INSTC_58_01 | INSTANCE          | 예        | 파라미터 그룹 변경 완료                        |
| INSTC_58_04 | INSTANCE          | 예        | 파라미터 그룹 변경 실패                        |
| INSTC_59_00 | INSTANCE          | 예        | 파라미터 그룹 변경 사항 적용 시작                  |
| INSTC_59_01 | INSTANCE          | 예        | 파라미터 그룹 변경 사항 적용 완료                  |
| INSTC_59_04 | INSTANCE          | 예        | 파라미터 그룹 변경 사항 적용 실패                  |
| INSTC_60_00 | INSTANCE          | 예        | DB 인스턴스 마이그레이션 시작                    |
| INSTC_60_01 | INSTANCE          | 예        | DB 인스턴스 마이그레이션 완료                    |
| INSTC_60_04 | INSTANCE          | 예        | DB 인스턴스 마이그레이션 실패                    |
| INSTC_61_00 | INSTANCE          | 예        | 예비 마스터 재구축 시작                        |
| INSTC_61_01 | INSTANCE          | 예        | 예비 마스터 재구축 완료                        |
| INSTC_61_04 | INSTANCE          | 예        | 예비 마스터 재구축 실패                        |
| INSTC_62_00 | INSTANCE          | 예        | DB 엔진 버전 업그레이드 시작                    |
| INSTC_62_01 | INSTANCE          | 예        | DB 엔진 버전 업그레이드 완료                    |
| INSTC_62_04 | INSTANCE          | 예        | DB 엔진 버전 업그레이드 실패                    |
| INSTC_63_01 | INSTANCE          | 예        | DB 인스턴스 바이너리 로그 해독                   |
| INSTC_64_04 | INSTANCE          | 예        | DB 엔진 버전 업그레이드 사전 점검 실패              |
| INSTC_65_00 | INSTANCE          | 예        | 인증 플러그인 활성화 시작                       |
| INSTC_65_01 | INSTANCE          | 예        | 인증 플러그인 활성화 완료                       |
| INSTC_65_04 | INSTANCE          | 예        | 인증 플러그인 활성화 실패                       |
| INSTC_66_00 | INSTANCE          | 예        | DB 인스턴스 강제 승격 시작                     |
| INSTC_66_01 | INSTANCE          | 예        | DB 인스턴스 강제 승격 완료                     |
| INSTC_66_04 | INSTANCE          | 예        | DB 인스턴스 강제 승격 실패                     |
| INSTC_67_00 | INSTANCE          | 예        | OS 버전 업그레이드 시작                       |
| INSTC_67_01 | INSTANCE          | 예        | OS 버전 업그레이드 종료                       |
| INSTC_67_04 | INSTANCE          | 예        | OS 버전 업그레이드 실패                       |
| INSTC_68_01 | INSTANCE          | 예        | 네트워크 통신 실패                           |
| JOB_01_04   | JOB               | 예        | Job 실행 실패                            |
| TENAT_01_04 | TENANT            | 예        | CPU 코어 수 제한                          |
| TENAT_02_04 | TENANT            | 예        | RAM 용량 제한	                           |
| TENAT_03_04 | TENANT            | 예        | 개별 볼륨 크기 제한                          |
| TENAT_04_04 | TENANT            | 예        | 프로젝트 전체 볼륨 크기 제한                     |
| TENAT_05_04 | TENANT            | 예        | Read Only Slave 개수 제한                |

## 이벤트 구독

이벤트 유형, 코드 및 소스로 구분하여 이벤트를 구독할 수 있습니다. 이벤트 유형으로 구독하면 이벤트 유형에 포함된 모든 이벤트 코드의 알림을 받습니다. 알림이 너무 광범위할 경우 이벤트 코드와 소스로 세분화해 구독할 수 있습니다. 프로젝트 멤버만 알림을 받을 사용자로 선택할 수 있습니다. 기본적으로 이메일로 이벤트 알림이 발송되며, 실명을 인증한 휴대전화 번호가 등록된 경우에만 SMS로 추가 이벤트 알림이 발송됩니다.

![event_subscription_01_ko](https://static.toastoven.net/prod_rds/23.04.11/event_subscription_01_ko.png)

* ❶ **이벤트 구독 등록**을 누르면 이벤트 구독을 등록할 수 있는 팝업 창이 나타납니다.
* ❷ 구독할 이벤트 유형을 선택합니다. 이벤트 유형에 따라 선택할 수 있는 이벤트 코드가 변경됩니다.
* ❸ 구독할 이벤트 코드를 선택합니다.
* ❹ 구독할 이벤트 소스를 선택합니다.
* ❺ 이벤트 알림을 받을 사용자 그룹을 선택합니다.
* ❻ 활성화 여부를 선택합니다. 활성화 여부를 `아니요`로 선택할 경우 이벤트 발생 알림을 발송하지 않습니다

## 사용자 그룹

알림을 받을 사용자를 그룹으로 관리할 수 있습니다. 알림 대상은 반드시 프로젝트 멤버로 등록되어 있어야 합니다. 사용자 그룹에 속한 사용자가 프로젝트 멤버에서 제외되면 사용자 그룹에 속해 있더라도 알림을 받을 수 없습니다.

> [주의]
> 실명 인증을 진행하지 않아 휴대폰 정보가 없을 경우 SMS 알림을 받을 수 없습니다.

### 사용자 그룹 생성

![user_group_01_ko](https://static.toastoven.net/prod_rds/23.06.13/user_group_01_ko.png)

* ❶ **사용자 그룹 생성**을 누르면 사용자 그룹을 생성할 수 있는 팝업 창이 나타납니다.
* ❷ 사용자 그룹에 추가된 사용자가 나타납니다.
* ❸ **x**를 누르면 추가된 사용자를 제외할 수 있습니다.
* ❹ 사용자 목록에 사용자가 많을 경우 검색 조건을 입력하여 결과를 제한할 수 있습니다.
* ❺ **전체 프로젝트 멤버**를 통보 대상에 추가합니다. 
  * 추가 시 개별 사용자 추가는 취소됩니다.
  * 해당 사용자 그룹을 이용하여 알람을 전송하게 되는 경우, 당시 전체 프로젝트 멤버 대상으로 알람을 전송합니다.
* ❻ **확인**를 눌러 사용자 그룹에 사용자를 추가합니다.

## 알림 그룹

알림 그룹을 통해 성능 지표에 대한 알림을 받을 수 있습니다. 지정합니다. -> 알림 그룹에 감시 대상 인스턴스와 알림을 받을 사용자 그룹을 지정합니다. 감시 설정을 통해 알림을 받을 성능 지표의 임겟값과 조건을 설정합니다. 설정된 지표가 감시 설정의 조건을 충족하면 연결된 사용자 그룹에 알림을 발송하게 됩니다. 알림 그룹에 설정된 알림 유형에 따라 SMS 혹은 메일로 알림을 발송합니다.

### 알림 그룹 생성

![notification_group_01_ko.png](https://static.toastoven.net/prod_rds/23.04.11/notification_group_01_ko.png)

* ❶ **그룹 만들기**를 누르면 알림 그룹을 생성할 수 있는 팝업 창이 나타납니다.
* ❷ 알림을 받을 방법을 선택합니다.
* ❸ 활성화되지 않은 알림 그룹은 알림 발송을 하지 않습니다.
* ❹ 감시 대상 DB 인스턴스를 선택합니다.
* ❺ 알림을 받을 사용자 그룹을 선택합니다.

## 감시 설정

감시 설정은 감시 항목, 비교 방법, 임겟값 및 지속 시간으로 구성됩니다. 감시 항목의 성능 지푯값과 임겟값을 비교하여 조건이 충족하는지 판단합니다. 조건이 지속 시간 이상 연속해서 충족한다면 알림을 발송합니다. 예를 들어, CPU 사용률의 임겟값이 90% 이상이고 지속 시간이 5분이라면, 해당 알림 그룹과 연동된 DB 인스턴스의 CPU 사용률이 90% 이상인 상태가 5분 이상 지속되었을 때 사용자 그룹에 정의된 사용자들에게 알림을 보냅니다. 만약 CPU 사용률이 90% 이상이 되어도, 5분 이내에 90% 미만으로 떨어지면 알림이 발생하지 않습니다.

### 감시 설정 항목

감시 가능한 성능 지표 항목은 다음과 같습니다.

| 힝목                         | 단위                 |
|----------------------------|--------------------|
| CPU 사용률                    | %                  |
| CPU 사용량(IO Wait)           | %                  |
| CPU 사용량(Nice)              | %                  |
| CPU 사용량(System)            | %                  |
| CPU 사용량(User)              | %                  |
| Load Average 1M            |                    |
| Load Average 5M            |                    |
| Load Average 15M           |                    |
| 메모리 사용량                    | %                  |
| 메모리 사용량(바이트)               | MB                 |
| 메모리 여유량(바이트)               | MB                 |
| 메모리 버퍼(바이트)                | MB                 |
| 캐시된 메모리(바이트)               | MB                 |
| 스왑 사용량                     | MB                 |
| 스왑 전체 크기                   | MB                 |
| Storage 사용량                | %                  |
| Storage 남은 사용량             | MB                 |
| Storage IO Read            | KB/sec             |
| Storage IO Write           | KB/sec             |
| 스토리지 결함                    | 비정상: 0, 정상: 1      |
| Network in BPS             | KB/sec             |
| Network out BPS            | KB/sec             |
| Database Connection Status | 접속 불가: 0, 접속 가능: 1 |
| Queries Per Second         | counts/sec         |
| Connection Total           | counts             |
| Connection Running         | counts             |
| Connection Cached          | counts             |
| Connection Ratio           | counts             |
| Database Activity Select   | counts/sec         |
| Database Activity Insert   | counts/sec         |
| Database Activity Update   | counts/sec         |
| Database Activity Delete   | counts/sec         |
| Database Activity Replace  | counts/sec         |
| Database Activity Call     | counts/sec         |
| Buffer Pool Total          | MB                 |
| Buffer Pool Used           | MB                 |
| Slow Query                 | counts/min         |
| 복제 딜레이                     | sec                |
| Row Access Index           | counts/min         |
| Row Access Full scan       | counts/min         |

### 감시 설정 추가

![notification_group_02_ko.png](https://static.toastoven.net/prod_rds/23.04.11/notification_group_02_ko.png)

* ❶ **감시 설정**을 누르면 감시 설정을 변경할 수 있는 팝업 창이 나타납니다.
* ❷ **감시 설정 추가** 를 눌러 신규 감시 설정을 추가합니다.
* ❸ 감시할 항목과 비교 방법, 임곗값, 지속 시간을 입력한 뒤 **추가**를 클릭합니다.

### 감시 설정 변경 및 삭제

![notification_group_03_ko.png](https://static.toastoven.net/prod_rds/23.04.11/notification_group_03_ko.png)

* ❶ 버튼을 클릭하면 추가된 감시 설정을 변경할 수 있습니다.
* ❷ 버튼을 클릭하면 추가된 감시 설정을 삭제할 수 있습니다.

## Processlist

RDS for MySQL은 데이터베이스에서 발생하는 다양한 문제를 분석하기 위해 데이터베이스에서 수행된 쿼리와 InnoDB 엔진의 상태를 수집합니다. 1초에 한 번씩 `information_schema.processlist`의 결과와, `SHOW ENGINE INNODB STATUS`의 결과를 수집하며 최근 4일간의 데이터를 웹 콘솔의 Processlist 탭에서 조회할 수 있습니다.

> [참고] 수집된 데이터는 내부 오브젝트 스토리지에 저장되며 별도로 과금하지 않습니다.

수집하는 항목은 다음과 같습니다.

| 항목            | 설명                                                |
|---------------|---------------------------------------------------|
| LogTime       | 수집 시각                                             |
| PID           | 프로세스 ID                                           |
| DB            | 사용 중인 DB                                          |
| User          | 사용자                                               |
| Host          | 사용자가 접속하고 있는 호스트명, IP 주소                          |
| Command       | Thread 실행 중인 명령 타입(Sleep, Query, Quit, Kill 등)    |
| State         | Thread의 상태(Starting, Rolling back, System lock 등) |
| ExecTime      | Thread가 현재 상태를 유지한 시간(초)                          |
| Query         | 실행 중인 쿼리                                          |
| InnoDB Status | InnoDB 상태 정보                                      |