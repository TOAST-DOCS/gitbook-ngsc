## Storage > NAS(Offline) > 콘솔 사용 가이드
> [참고] 
> NAS Volume은 NHN Cloud 클라우드 플랫폼 인프라 내에 있는 서버에서만 마운트할 수 있습니다.
> 사설 IP로 통신하도록 구성되어 있어 외부에서 접근이 불가합니다.

## NAS 신청

1. **Storage > NAS(offline)** 으로 이동한 뒤, **NAS 서비스 이용 신청** 버튼을 클릭합니다.

2. 관련 정보를 입력합니다.
    * Volume 이름
        * NAS Volume 이름으로 영문/숫자 형태로 8자리 이하의 이름을 입력합니다. 입력된 Volume 이름으로 실제 Nas Volume 이 생성이 됩니다.
    * VPC & 서브넷 
        * NAS를 연결 할 네트워크를 선택합니다.
    * 신청 크기(GB)  
        * NAS 는 신규 생성시 300GB 이상부터 신청이 가능합니다. 필요한 용량을 슬라이더를 이용하여 선택합니다.
    * Snapshot 사용
        * Snapshot data 저장은 NAS Volume 을 사용합니다. Snapshot 을 이용한 복구는 별도 문의 바랍니다.

> [참고] NAS Volume을 사용하기 위해서는 VPC 라우팅 테이블이 인터넷 게이트웨이와 연결되어야 합니다. 인터넷이 되지 않는 VPC 에서는 NAS Volume을 할당받을 수 없습니다.

3. NAS Volume 생성이 완료되면 메일로 안내가 됩니다.

## Volume 증설 및 삭제

NAS 이용내역에서 Volume 증설 및 삭제를 할 수 있습니다. Volume증설은 NAS 의 상태가 **신청접수**, **이용중** 일 경우에만 버튼이 활성화됩니다.

Volume 증설을 하기 위해서:

1. **Volume 증설** 버튼을 클릭합니다.  

2. 대화창에서 증설할 추가 용량 정보를 입력후 **확인** 버튼을 클릭합니다.

Volume 삭제를 하기 위해서:

1. **Volume 삭제** 버튼을 클릭합니다.  

2. 대화창에서 삭제할 정보를 확인 후 **확인** 버튼을 클릭합니다.


## Volume 연결

### nfs 패키지 설치

* Debian, Ubuntu: nfs-common, rpcbind  
  ```
  sudo apt-get install nfs-common rpcbind
  ```
* CentOS: nfs-utils, rpcbind  
  ```
  sudo yum install nfs-utils rpcbind
  ```

### rpcbind 서비스 실행

```
sudo service rpcbind start
```

### NAS volume 마운트

```
sudo mount -t nfs {nas source} {mount point}
```

* nas source: NAS volume 정보  
  예) 192.168.0.241:/data
* mount point: NAS volume 을 마운트 할 디렉터리  
  예) /mnt
