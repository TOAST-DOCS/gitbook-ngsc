## Compute > GPU Instance > 콘솔 사용 가이드

## GPU Instance 활성화

GPU Instance를 사용하기 위해서는 먼저 Compute > GPU Instance 생성요청을 해야 합니다.

**서비스 선택 > GPU Instance 를 클릭합니다.**

![GPU_Instance_activation_1_modify.png](http://static.toastoven.net/prod_gpu/ko_TG_C1.jpg)  
[그림1]

**서비스 활성화 확인버튼을 클릭합니다.**

![GPU_Instance_activation_2_modify.png](http://static.toastoven.net/prod_gpu/ko_TG_C2.jpg)
[그림2]



## GPU Instance 생성 요청

**Compute > GPU Instance 으로 이동한 뒤, GPU Instance 생성 버튼을 클릭합니다.**

![GPU_Instance_apply_1_modify.png](http://static.toastoven.net/prod_gpu/ko_TG_C3.jpg)
[그림3]

**GPU Instance 생성할 OS 이미지 정보를 확인합니다.**

![GPU_Instance_apply_2_modify.png](http://static.toastoven.net/prod_gpu/ko_TG_C4.jpg)
[그림4]

**생성할 인스턴스 타입을 선택합니다.**
![GPU_Instance_apply_3_modify.png](http://static.toastoven.net/prod_gpu/ko_TG_C5.jpg)
[그림5]

**이후 진행은 일반 Instance 생성과정과 동일합니다.**

인스턴스 생성에 대한 자세한 내용은 [Instance 개요](http://docs.toast.com/ko/Compute/Instance/ko/overview/)를 참고하시기 바랍니다


## GPU 인스턴스 정보

**GPU 인스턴스 타입별 사양**

| 인스턴스 타입   | GPU 모델 | GPU   | GPU 메모리 | vCPU  | 메모리 |
| --------------- | :--------: | -----: | ----------: | -----: | ------: |
| g2.v100.c8m90   | V100     | 1 개  | 32 GB      | 8 개  | 90 GB  |
| g2.v100.c16m180 | V100     | 2 개  | 64 GB      | 16 개 | 180 GB |
| g2.v100.c32m360 | V100     | 4 개  | 128 GB     | 32 개 | 360 GB |
| g2.v100.c64m720 | V100     | 8 개  | 256 GB     | 64 개 | 720 GB |
| g2.t4.c4m32     | T4       | 1 개  | 16 GB      | 4 개  | 32 GB  |
| g2.t4.c8m64     | T4       | 2 개  | 32 GB      | 8 개  | 64 GB  |
| g2.t4.c16m128   | T4       | 4 개  | 64 GB      | 16 개 | 128 GB |
| g2.t4.c32m256   | T4       | 8 개  | 128 GB     | 32 개 | 256 GB |
| g2.t4.c64m384   | T4       | 12 개 | 192 GB     | 64 개 | 384 GB |



## GPU Instance 접속 방법

**인스턴스 생성이 완료되었다면 접속정보대로 접속합니다.**

**Linux GPU Instance 접속 방법**

인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
인스턴스에 Floating IP가 연결되어있어야 하며 보안그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.
SSH 클라이언트와 설정한 키페어를 이용해 인스턴스에 접속 합니다.

SSH 연결에 대한 자세한 가이드는 [Linux 인스턴스 접속 방법](https://docs.toast.com/ko/Compute/Instance/ko/overview/#linux)를 참고하시기 바랍니다.

**Windows GPU Instance 접속 방법**

Windows 서버에 접속하려면, NHN Cloud 콘솔에서 접속하려는 Windows 인스턴스를 선택합니다. 인스턴스 상세 화면의 인스턴스 접속(Instance 접속) 탭에서 비밀번호 확인 버튼을 클릭하여 Windows 서버에 설정된 비밀번호를 확인합니다.

비밀번호 확인 옆의 연결 버튼을 클릭해 원격 데스크톱 접속 설정이 저장된 .rdp 파일을 받아서 실행하면 Windows 서버에 접속합니다. Windows 서버의 ID는 Administrator이며, 비밀번호는 NHN Cloud 콘솔에서 확인한 비밀번호를 이용합니다.

RDP 연결에 대한 자세한 가이드는 [Windows 인스턴스 접속 방법](https://docs.toast.com/ko/Compute/Instance/ko/overview/#windows)을 참고하시기 바랍니다.

## GPU 정보 확인

**인스턴스에 접속해서 nvidia-smi 명령어 실행후 GPU정보를 확인합니다.**

```
# GPU 정보 확인
(Linux) shell > nvidia-smi
(Windows) cmd > C:\Program Files\NVIDIA Corporation\NVSMI\nvidia-smi.exe
```

![GPU_active_3.png](http://static.toastoven.net/prod_gpu/nvidia-smi_stress2_1_70.png)
[그림6]

1. 버전 정보
    * NVIDIA Driver Version : 440.33.01
    * CUDA Version : 10.2
2. GPU 정보
    * 사용가능한 GPU 수 : GPU 0~7 (8개 사용)
    * GPU 모델명 : Tesla V100-SXM2
    * Persistence mode 상태 : Persistence_mode On
    * GPU 현재 온도 : Temp 52C
    * 현재 전력사용량 / 최대 전력사용량 : Pwr Usage/Capacity 298W / 300W
    * 현재 메모리 사용량 / 최대 메모리 사용량 : Memory-Usage 29283MiB / 32480MiB
    * GPU Utilization : GPU-util 100%
3. Processes 정보
    * GPU별 PID, Process name, GPU Memory Usage 정보
    * Type 정보
        * C = compute = CUDA or OpenCL
        * G = graphics = DirectX or OpenGL

**nvcc명령어로 CUDA 버전을 확인합니다.**

```
# CUDA 버전 정보 확인
/usr/local/cuda/bin/nvcc  --version

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2020 NVIDIA Corporation
Built on Thu_Jun_11_22:26:38_PDT_2020
Cuda compilation tools, release 11.0, V11.0.194
Build cuda_11.0_bu.TC445_37.28540450_0
```




## 일반 인스턴스와 GPU 인스턴스와 차이점

* GPU 인스턴스는 사양 변경을 할수 없습니다.
* NVIDIA와 CUDA의 버전은 별도 공지없이 업데이트됩니다.
