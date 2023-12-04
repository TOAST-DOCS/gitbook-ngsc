## Network > Service Gateway > 콘솔 사용 가이드

콘솔에서 **Service Gateway** 서비스를 사용하는 방법을 설명합니다.

## 서비스 게이트웨이

### 서비스 게이트웨이 생성

서비스 게이트웨이를 생성하는 방법은 다음과 같습니다.

1. **Network > Service Gateway**로 이동합니다.
2. **서비스 게이트웨이 생성** 버튼을 클릭하면 생성 화면이 나타납니다.
3. 서비스 게이트웨이에 사용할 **이름**을 입력합니다.
4. **VPC**를 선택합니다. 선택된 VPC에 종속된 서비스 게이트웨이가 생성됩니다.
5. **서브넷**을 선택합니다. 선택된 서브넷에서 서비스 게이트웨이의 IP가 할당됩니다.
6. **서비스**를 선택합니다. 서비스 게이트웨이에 할당된 IP로 접근 시 이 항목에서 선택된 서비스와 연결됩니다.

### 서비스 게이트웨이 조회

생성한 서비스 게이트웨이는 **Network > Service Gateway** 화면에서 확인할 수 있습니다. 서비스 게이트웨이를 선택하면 하단에 서비스 게이트웨이 정보가 나타납니다.

### 서비스 게이트웨이 변경

서비스 게이트웨이를 변경하는 방법은 다음과 같습니다. **이름**, **설명**만 변경할 수 있습니다.

1. **Network > Service Gateway**로 이동합니다.
2. **서비스 게이트웨이 변경** 버튼을 클릭한 후 변경 화면에서 원하는 항목을 변경합니다.

### 서비스 게이트웨이 삭제

서비스 게이트웨이를 삭제하려면 **Network > Service Gateway** 화면에서 삭제할 서비스 게이트웨이를 선택하고 **서비스 게이트웨이 삭제** 버튼을 클릭합니다.

## 서비스 게이트웨이 사용

### 서비스 게이트웨이 IP확인

1. **Network > Service Gateway**로 이동합니다.
2. 서비스 게이트웨이 목록에서 **IP 주소**를 확인합니다.<br>
   이 VM Instance에서 이 IP 주소로 접속 시 서비스 게이트웨이가 연결하고 있는 서비스로 연결됩니다.

### 서비스 게이트웨이 접속

생성된 서비스 게이트웨이의 IP 주소가 `192.168.1.42`라 할 경우 다음과 같은 방법으로 서비스에 접근이 가능합니다.

* VM Instance에서 서비스 게이트웨이 IP로 접속하면 서비스 게이트웨이 생성 시 선택된 서비스로 연결되어 서비스 사용이 가능합니다.
    * IP 주소를 사용하여 https 프로토콜을 이용할 경우 인증서 관련 오류가 발생할 수도 있습니다.
    * https 사용이 필요한 경우 VM Instance의 `/etc/hosts`에 IP 주소와 URL을 추가하시기 바랍니다.
    * 예시) IP 주소를 이용하여 오브젝트 스토리지에서 파일 다운로드

            ~# wget http://192.168.1.42/v1/AUTH_8222a22c22244badbf876dcd521f3f98/test-obs/test_file.txt

* 서비스 게이트웨이를 이용하여 서비스 접근 시 URL을 지원하지 않습니다. URL 접근이 필요한 경우 아래 예시와 같이 `/etc/hosts` 파일에 URL을 추가해야 합니다.
    * 예시) URL을 이용하여 **오브젝트 스토리지**에서 파일 다운로드<br>
      `/etc/hosts` 파일에 아래와 같이 서비스 게이트웨이의 IP 주소와 Object Storage의 URL을 추가합니다.

            192.168.1.42    api-storage.cloud.toast.com

        IP 주소 대신 `/etc/hosts`에 추가한 URL로 접속

            ~# wget https://api-storage.cloud.toast.com/v1/AUTH_8222a22c22244badbf876dcd521f3f98/test-obs/test_file.txt

## 서비스 게이트웨이에서 오브젝트 스토리지 사용 예제

**오브젝트 스토리지**에 관련된 내용은 예제 설명을 위한 수준에서만 기술합니다. 오브젝트 스토리지의 자세한 사용 방법은 **사용자 가이드 > Storage > Oject Storage**를 참고하시기 바랍니다.

### 서비스 게이트웨이 생성

**오브젝트 스토리지 API**를 사용하려면 **인증 토큰**을 발급받아야 합니다. 인터넷 사용이 불가능한 격리된 환경의 VPC에서 Object Storage를 사용하려면 인증 토큰도 서비스 게이트웨이를 이용하여 발급받아야 하며, 다음 절차에 따라서 서비스 게이트웨이를 생성해야 합니다.

1. **Object Storage** 서비스를 선택하여 서비스 게이트웨이를 생성합니다.<br>
   오브젝트 스토리지 API 접속을 위한 서비스 게이트웨이입니다.
2. **IaaS API Identity** 서비스를 선택하여 서비스 게이트웨이를 생성합니다.<br>
   인증 토큰(token) 발급을 위한 서비스 게이트웨이입니다.
3. 생성된 두 개의 서비스 게이트웨이에서 IP 주소를 확인합니다.

### /etc/hosts 파일 편집

**Object Storage**를 선택하여 생성한 서비스 게이트웨이의 IP 주소가 192.168.1.42이고 **IaaS API Identity**를 선택하여 생성한 서비스 게이트웨이의 IP 주소로 192.168.1.57을 할당받은 경우, VM Instance의 `/etc/hosts` 파일에 아래와 같이 IP 주소와 URL을 추가합니다.

> [참고] 오브젝트 스토리지의 API URL 주소는 콘솔 화면의 **Storage > Object Storage**에서 **API 엔드포인트 설정** 버튼을 클릭하여 확인할 수 있습니다.<br>
> [주의] 리전마다 사용하는 오브젝트 스토리지 API의 URL 주소는 다르기 때문에 **API 엔드포인트 설정**의 URL을 반드시 확인해 주시기 바랍니다.

```
192.168.1.42	api-identity.infrastructure.cloud.toast.com
192.168.1.57	api-storage.cloud.toast.com
```

### 인증 토큰 발급

오브젝트 스토리지의 **API 비밀번호 설정**을 하고 인증 토큰을 발급받습니다.

* API 비밀번호 설정
    1. **Storage > Object Storage**에서 **API 엔드포인트 설정** 버튼을 클릭합니다.
    2. **API 엔드포인트 설정** 화면의 **API 비밀번호 설정**에 사용할 비밀번호를 입력하고 **변경** 버튼을 클릭합니다.
    > [참고] 상세한 사용 방법은 [사용자 가이드 > Storage > Object Storage > API 가이드](https://docs.toast.com/ko/Storage/Object%20Storage/ko/api-guide/)를 참고하시기 바랍니다.

* 인증 토큰 발급 요청<br>
  **NHN Cloud 로그인 ID**와 앞서 설정한 **API 비밀번호 설정**의 비밀번호를 이용하여 아래와 같이 **IaaS API Identify** 서비스용으로 생성한 서비스 게이트웨이 URL에 토큰 발급을 요청합니다.
    * `auth.passwordCredentials.username`에는 NHN Cloud 로그인 ID 사용
    * `auth.passwordCredentials.password`에는 API 비밀번호 설정에 입력한 비밀번호 사용
  

            ~# curl -X POST -H 'Content-Type:application/json' https://api-identity.infrastructure.cloud.toast.com/v2.0/tokens -d '{"auth": {"tenantId": "2fda9d4b88244a0a92ff23841198e2e6", "passwordCredentials": {"username": "example@nhn.com", "password": "example123"}}}'

* 인증 토큰 발급 응답<br>
  아래 응답에서 `access.token.id` 항목의 값이 인증 토큰입니다. `access.token.expires`에 기록된 시간까지 인증 토큰이 유효합니다.

            {"access":{"token":{"id":"gAAAAABiVnmCOJVJhh1W2eXGo3aL0eaZxXmd-SMDMIE3zmip2lXy6eH0BlZAlTZBG20dWEm7TF4zi4YIOTKnc6yKh_wqZsyxgMWKkpVNShzE-k6GaSThBP54QeUePSjC2t-R10X6G4xL_Wecl-V-lV-bnOfVo6Ccpz6rv9eLYJnbJw7KrIMSSiY","expires":"2022-04-13T19:19:30Z","tenant":{"id":"2fda9d4b8821111192ff23841198e2e6","name":"tTMgSSSF","groupId":"XXj2zkH7777modGU","description":"","enabled":true,"project_domain":"NORMAL","swift":true},"issued_at":"2022-04-13T07:32:14.000441"},"serviceCatalog":[{"endpoints":[{"region":"KR1","publicURL":"https://api-identity.infrastructure.cloud.toast.com/v2.0"}],"type":"identity","name":"keystone"},{"endpoints":[{"region":"KR2","publicURL":"https://kr2-api-storage.cloud.toast.com/v1/AUTH_2fda9d4b88244a0a92ff23841198e2e6"},{"region":"KR1","publicURL":"https://api-storage.cloud.toast.com/v1/AUTH_2fda9d4b88244a0a92ff23841198e2e6"}],"type":"object-store","name":"swift"}],"user":{"id":"80884888887b45dbaf9b815117130671","username":"5111111c-b111-4b11-b11b-01111f81111f","name":"5211122c-bfc4-4115-b11b-05b52f84

### 오브젝트 스토리지 API 사용

인증 토큰 발급을 마쳤으면 오브젝트 스토리지 API를 사용할 수 있습니다. 오브젝트 스토리지에 example이라는 컨테이너를 생성하고 test_file.txt를 넣어 놨다고 가정할 경우, 아래와 같은 API 사용법으로 컨테이너에 있는 파일을 조회할 수 있습니다.

* 요청<br>
  `X-Auth-Token`에 인증 토큰을 추가하여 요청

        ~# curl -X GET -H 'X-Auth-Token:gAAAAABiVnmCOJVJhh1W2eXGo3aL0eaZxXmd-SMDMIE3zmip2lXy6eH0BlZAlTZBG20dWEm7TF4zi4YIOTKnc6yKh_wqZsyxgMWKkpVNShzE-k6GaSThBP54QeUePSjC2t-R10X6G4xL_Wecl-V-lV-bnOfVo6Ccpz6rv9eLYJnbJw7KrIMSSiY' https://api-storage.cloud.toast.com/v1/AUTH_2fda9d4b88244a0a92ff23841198e2e6/example

* 응답<br>
  오브젝트 스토리지 컨테이너에 있는 파일 목록 확인

        test_file.txt

