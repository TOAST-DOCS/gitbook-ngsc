## Database > RDS for MySQL > 개발자 가이드

## Migration

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

* `ERROR 1227` 오류는 mysqldump 파일의 저장된 객체(트리거, 뷰, 함수 또는 이벤트)에 DEFINER 정의가 되어 있을 때 발생합니다.
* 이를 해결하기 위해서는 mysqldump 파일에서 `DEFINER`부분을 삭제 후 진행합니다.

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
* Master RDS 인스턴스로부터 내보낼 경우.

```
mysqldump -h{rds_master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --master-data=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* Read Only Slave RDS 인스턴스로부터 내보낼 경우.

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
* 외부 네트워크를 통해 가져오면(import) 오래 걸릴 수 있기 때문에,
* 내부 NHN Cloud Image를 생성하고 백업 파일을 복사한 후, NHN Cloud로 가져오기를 권장합니다.
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

## 오브젝트 스토리지를 이용한 백업 및 복원

* RDS for MySQL의 백업 파일을 오브젝트 스토리지로 내보내거나, 오브젝트 스토리지의 백업 파일을 이용하여 DB 인스턴스를 복원할 수 있습니다.
* RDS for MySQL은 Percona XtraBackup을 이용하여 백업 및 복원을 수행하므로, 오브젝트 스토리지의 백업 파일을 사용하기 위해서는 MySQL 각 버전별 권장하는 XtraBackup을 사용해야 합니다.

| MySQL 버전 | XtraBackup 버전 |
|----------|---------------|
| 5.7.15   | 2.4.20        |
| 5.7.19   | 2.4.20        |
| 5.7.26   | 2.4.20        |
| 5.7.33   | 2.4.20        |
| 5.7.37   | 2.4.20        |
| 8.0.18   | 8.0.26        |
| 8.0.23   | 8.0.26        |
| 8.0.28   | 8.0.28        |

* XtraBackup의 설치에 대한 자세한 설명은 Percona 홈페이지를 참고합니다.
    * https://www.percona.com/doc/percona-xtrabackup/2.4/index.html
    * https://www.percona.com/doc/percona-xtrabackup/8.0/index.html

> [주의] 현재 5.7.33 버전에서는 오브젝트 스토리지의 백업 파일을 이용한 DB 인스턴스 복원은 제한됩니다.
> [주의] 권장하는 XtraBackup 이외의 버전을 사용하면 정상으로 동작하지 않을 수 있습니다.
> [주의] 오브젝트 스토리지의 백업 파일과 복원하려는 MySQL의 버전은 동일해야 합니다.

### 오브젝트 스토리지에 백업 내보내기

* NHN Cloud의 오브젝트 스토리지에 RDS for MySQL의 백업을 내보낼 수 있습니다.
* 웹 콘솔의 **Instance** 탭에서 DB 인스턴스를 선택한 후, **추가 기능** 메뉴에서 **오브젝트 스토리지로 백업 내보내기** 버튼을 클릭하면 수동 백업을 진행합니다. 백업된 파일을 곧바로 사용자가 지정한 오브젝트 스토리지로 업로드할 수 있습니다.
* 또한, DB 인스턴스 상세 화면의 **백업 & Acess 제어** 탭에서 기존 백업 파일을 선택하고 **오브젝트 스토리지로 백업 내보내기** 버튼을 클릭하면 사용자가 지정한 오브젝트 스토리지로 업로드할 수 있습니다.
* 백업 파일은 사용자가 지정한 오브젝트 스토리지의 컨테이너에 멀티 파트로 구성된 오브젝트로 업로드됩니다.

### 오브젝트 스토리지의 백업 파일을 이용하여 수동으로 복원

* 오브젝트 스토리지의 백업 파일을 이용하여 직접 MySQL을 복원할 수 있습니다.
* 복원할 MySQL 및 XtraBackup이 설치되어 있다고 가정합니다.
* 오브젝트 스토리지의 백업을 복원하고자 하는 서버에 다운로드합니다.
* MySQL 서비스를 정지합니다.
* MySQL 데이터 저장 경로의 모든 파일을 삭제합니다.

```
rm -rf {MySQL 데이터 저장 경로}/*
```

* 다운로드한 백업 파일의 압축을 해제하고 복원합니다.
* XtraBackup 2.4.20 예제

```
cat {백업 파일 저장 경로} | xbstream -x -C {MySQL 데이터 저장 경로}
innobackupex --decompress {MySQL 데이터 저장 경로}
innobackupex --defaults-file={my.cnf 경로} --apply-log {MySQL 데이터 저장 경로}
```

* XtraBackup 8.0.12 예제

```
cat {백업 파일 저장 경로} | xbstream -x -C {MySQL 데이터 저장 경로}
xtrabackup --decompress --target-dir={MySQL 데이터 저장 경로}
xtrabackup --prepare --target-dir={MySQL 데이터 저장 경로}
xtrabackup --defaults-file={my.cnf 경로} --copy-back --target-dir={MySQL 데이터 저장 경로}
```

* 압축 해제 후 불필요한 파일을 제거합니다.

```
find {MySQL 데이터 저장 경로} -name "*.qp" -print0 | xargs -0 rm
```

* MySQL 서비스를 시작합니다.

### 오브젝트 스토리지의 RDS for MySQL 백업 파일을 이용하여 DB 인스턴스 생성

* 오브젝트 스토리지의 RDS for MySQL 백업 파일을 이용해 동일 리전, 다른 프로젝트의 RDS for MySQL로 복원할 수 있습니다.
* [오브젝트 스토리지에 백업 내보내기](./developer-guide/#_5 )를 참고하여 백업 파일을 오브젝트 스토리지로 내보냅니다.
* 복원할 프로젝트의 웹 콘솔에 접속한 뒤 Instance 탭에서 오브젝트 스토리지에 있는 백업으로 복원 버튼을 클릭합니다.
* 백업 파일이 저장된 오브젝트 스토리지의 정보 및 DB 인스턴스의 정보를 입력한 후 **생성** 버튼을 클릭합니다.

### 오브젝트 스토리지의 외부 MySQL 백업 파일을 이용하여 DB 인스턴스 생성

* 일반 MySQL 백업 파일을 이용하여 RDS for MySQL의 DB 인스턴스로 복원할 수 있습니다.

> [주의] innodb_data_file_path의 설정값이 ibdata1:12M:autoextend가 아니면 RDS for MySQL의 DB 인스턴스로 복원할 수 없습니다.

* MySQL이 설치된 서버에서 아래의 명령어를 이용하여 백업을 수행합니다.
* XtraBackup 2.4.20 예제

```
innobackupex --defaults-file={my.cnf 경로} --user {사용자} --password '{비밀번호}' --socket {MySQL 소켓 파일 경로} --compress --compress-threads=1 --stream=xbstream {백업 파일이 생성될 디렉터리} 2>>{백업 로그 파일 경로} > {백업 파일 경로}
```

* XtraBackup 8.0.12 예제

```
xtrabackup --defaults-file={my.cnf 경로} --user={사용자} --password='{비밀번호}' --socket={MySQL 소켓 파일 경로} --compress --compress-threads=1 --stream=xbstream --backup {백업 파일이 생성될 디렉터리} 2>>{백업 로그 파일 경로} > {백업 파일 경로}
```

* 백업 로그 파일의 마지막 줄에 `completed OK!`가 있는지 확인합니다.
    * completed OK!가 없다면 백업이 정상적으로 종료되지 않은 것이므로 로그 파일에 있는 에러 메시지를 참고하여 백업을 다시 진행합니다.
* 완료된 백업 파일을 오브젝트 스토리지에 업로드합니다.
    * 한 번에 업로드할 수 있는 최대 파일 크기는 5GB입니다.
    * 백업 파일의 크기가 5GB보다 클 경우 split과 같은 유틸리티를 이용해 백업 파일을 5GB 이하로 자른 뒤 멀티 파트로 업로드해야 합니다.
    * 자세한 사항은 https://docs.ngsc-nhncloud.com/ko/Storage/Object%20Storage/ko/api-guide/#_43를 참고합니다.
* 복원할 프로젝트의 웹 콘솔에 접속한 뒤 Instance 탭에서 오브젝트 스토리지에 있는 백업으로 복원 버튼을 클릭합니다.
* 백업 파일이 저장된 오브젝트 스토리지의 정보 및 DB 인스턴스의 정보를 입력한 후 **생성** 버튼을 클릭합니다.

## Procedure

* RDS for MySQL은 사용자의 편의를 제공하기 위하여 사용자 계정에서 제한되는 몇몇 기능들을 수행하는 프로시저들을 자체적으로 제공하고 있습니다.

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
* MySQL error code 1062: 'Duplicate entry ? for key ?'

```
mysql> CALL mysql.tcrds_repl_skip_repl_error();
```

### tcrds_repl_next_changemaster

* Master의 다음 바이너리(binary log) 로그를 읽을 수 있도록 복제 정보를 변경합니다.
* 다음과 같은 복제 에러 발생 시 tcrds_repl_next_changemaster 프로시저를 실행하면 복제 에러를 해결할 수 있습니다.
    * 예) MySQL error code 1236 (ER_MASTER_FATAL_ERROR_READING_BINLOG): Got fatal error from master when reading data from binary log

```
mysql> CALL mysql.tcrds_repl_next_changemaster();
```
