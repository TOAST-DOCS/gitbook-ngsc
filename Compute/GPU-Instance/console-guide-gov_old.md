## Compute > GPU Instance > 콘솔 사용 가이드

## GPU Instance 활성화

GPU Instance를 사용하기 위해서는 먼저 Compute > GPU Instance 생성요청을 해야 합니다.

**서비스 선택 > GPU Instance 를 클릭합니다.**

![GPU_Instance_activation_1_modify.png](http://static.toastoven.net/prod_gpu/GPU_Instance_activation_1_modify.png)  

**서비스 활성화 확인버튼을 클릭합니다.**

![GPU_Instance_activation_2_modify.png](http://static.toastoven.net/prod_gpu/GPU_Instance_activation_2_modify.png)



## GPU Instance 생성 요청

**Compute > GPU Instance 으로 이동한 뒤, 신청(1:1문의) 버튼을 클릭합니다.**

![GPU_Instance_apply_1_modify.png](http://static.toastoven.net/prod_gpu/GPU_Instance_apply_1_modify.png)


**문의 제목과 문의 내용을 작성한후 확인 버튼을 클릭합니다.**

![GPU_Instance_apply_2_modify.png](http://static.toastoven.net/prod_gpu/GPU_Instance_apply_2_modify.png)


**GPU Instance 생성요청시 작성할 내용**

```
GPU 사용을 위한 관련정보를 입력합니다.
=============================================================================
* 고객 TOAST 아이디 email (ex. email@toast.com) : 

* 고객 프로젝트 ID (ex. ae1bfd024d8841a295988638d90979a4) : 
   Network > VPC > 서브넷 > 프로젝트 ID 에서 확인가능합니다.

* 가용성 영역 (ex. pub-a) :
  pub-a, pub-b 중 선택가능합니다.

* 설치할 인스턴스의 이름 (ex. gpu-instance-001) :
   20자 이내로 작성해주세요. 영문자와 숫자, '-', '.'만 입력 가능합니다.

* 인스턴스 타입과 수량 (ex. g2.c8m96 1대) :
   g2.c8m96 : 8 Core, Memory 96 GB + GPU 1개
   g2.c16m192 : 16 Core, Memory 192 GB + GPU 2개

* 설치할 인스턴스에서 사용할 키페어 이름 (ex. gpu_connect_key) :
   원하는 키 페어가 없는 경우 생성해 주세요.

* 인스턴스의 블럭스토리지 타입과 크기(GB) (ex. HDD 100 GB) :
   블럭스토리지 타입 : SSD와 HDD중 선택가능합니다.
   블럭스토리지 크기는 20 GB ~ 1000GB 로 생성가능합니다.

* 연결되어야할 Subnet 정보 (ex. Default Network / 7245176d-1402-47fa-87a6-2c2eafe8807b) 
  네트워크 이름 / ID / 복수개 가능

=============================================================================


GPU  인스턴스 사용을 위한 정보입니다.
=============================================================================
* 서비스는 바로 이용할 수 없고 사전 상담 후에 사용할 수 있습니다. 사전 상담은 2~3일 정도 걸릴 수 있습니다.

* GPU 인스턴스는 Ubuntu 18.04로 설치됩니다.

* GPU 인스턴스 IP는 자동 할당됩니다.

* GPU 인스턴스가 생성이 완료되면 메일로 안내가 됩니다.

=============================================================================
```

* 고객 TOAST 아이디 email : GPU 인스턴스 생성요청 할 아이디정보를 입력합니다.
* 고객 프로젝트 ID : 아래 그림처럼 **Network > VPC > 서브넷 > 프로젝트 ID** 에서 확인가능합니다.
* 가용성 영역 : pub-a, pub-b 중 선택가능합니다.
* 설치할 인스턴스의 이름 : 20자 이내로 작성해주세요. 영문자와 숫자, '-', '.'만 입력 가능합니다.
* GPU 인스턴스 타입과 인스턴스 수

| 인스턴스 타입 | GPU 수 | Core 수 | 메모리 용량 (GB) |
| --- | --- | --- | --- |
| g2.c8m96 | 1 | 8 | 96 |
| g2.c16m192 | 2 | 16 | 192 |

* 설치할 인스턴스에서 사용할 키페어 이름 : 원하는 키 페어가 없는 경우 생성해 주세요.
* 인스턴스의 블럭스토리지 타입과 크기(GB) : 블럭스토리지 타입은 HDD와 SSD 중 선택가능하며 블럭스토리지 크기는 20 GB ~ 1000GB 로 생성가능합니다.
* 연결되어야할 Subnet 정보 : 아래그림처럼 **Network > VPC > 서브넷 > 서브넷 이름, CIDR** 에서 확인가능합니다.
* 인스턴스 생성에 대한 자세한 내용은 [Instance 개요](http://docs.toast.com/ko/Compute/Instance/ko/overview/)를 참고하시기 바랍니다.


![GPU_Instance_subnetID_1](http://static.toastoven.net/prod_gpu/GPU_Instance_subnetID_1.png)


**GPU 인스턴스 사용을 위한 정보**

* 서비스는 바로 이용할 수 없고 사전 상담 후에 사용할 수 있습니다. 사전 상담은 2~3일 정도 걸릴 수 있습니다.

* GPU 인스턴스는 Ubuntu 18.04로 설치됩니다.

* GPU 인스턴스 IP는 자동 할당됩니다.

* GPU 인스턴스가 생성이 완료되면 메일로 안내가 됩니다.


## GPU Instance 접속

인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
인스턴스에 Floating IP가 연결되어있어야 하며 보안그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.

SSH 클라이언트와 설정한 키페어를 이용해 인스턴스에 접속 합니다. 
SSH 연결에 대한 자세한 가이드는 [SSH 연결 가이드](https://docs.toast.com/ko/Compute/Instance/ko/overview/#linux)를 참고하시기 바랍니다.



## GPU 정보 확인

```
# GPU 정보 확인
shell > nvidia-smi
```

![GPU_assign_1.png](http://static.toastoven.net/prod_gpu/GPU_assign_1.png)


* GPU : 0 (GPU 1개 할당)
* Name : Tesla V100-SXM2 (GPU 모델)
* Persistence_mode : On (Persistence mode enable 상태)
* Temp : 30C (현재 GPU 온도)
* Pwr Usage/Capacity : 36W / 300W (현재 전력사용량 / 최대 전력사용량)
* Memory-Usage : 32480MiB (GPU 최대 할당 메모리)
* GPU-Util : 0% (GPU Util 사용률)

## 일반 인스턴스와 GPU 인스턴스와 차이점

* GPU 인스턴스는 사양 변경이 안됩니다.
