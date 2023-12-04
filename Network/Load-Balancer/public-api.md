## Network > Load Balancer > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

로드 밸런서, 리스너, 풀, 헬스 모니터, 멤버 API는 `network` 타입 엔드포인트를 이용합니다. 시크릿, 시크릿 컨테이너 API는 `key-manager` 타입 엔드포인트를 이용해 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |
| key-manager | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |


API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## 로드 밸런서

### 로드 밸런서 목록 보기

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 로드 밸런서 ID |
| name | Query | String | - | 조회할 로드 밸런서 이름 |
| provisioning_status | Query | Enum | - | 조회할 로드 밸런서의 프로비저닝 상태 |
| description | Query | String | - | 조회할 로드 밸런서의 설명 |
| vip_address | Query | String | - | 조회할 로드 밸런서의 IP |
| vip_port_id | Query | UUID | - | 조회할 로드 밸런서의 포트 ID |
| vip_subnet_id | Query | UUID | - | 조회할 로드 밸런서의 서브넷 ID |
| operating_status | Query | Enum | - | 조회할 로드 밸런서의 운영 상태 |
| loadbalancer_type | Query | String | - | 조회할 로드 밸런서의 타입<br>`shared` / `dedicated` 중 하나 |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancers | Body | Array | 로드 밸런서 정보 객체 목록 |
| loadbalancers.description | Body | String | 로드 밸런서 설명 |
| loadbalancers.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancers.tenant_id | Body | String | 테넌트 ID |
| loadbalancers.provider | Body | String | 로드 밸런서 프로바이더(공급자) |
| loadbalancers.name | Body | String | 로드 밸런서 이름 |
| loadbalancers.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancers.listeners.id | Body | UUID | 리스너 ID |
| loadbalancers.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancers.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancers.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| loadbalancers.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancers.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |
| loadbalancers.ipacl_groups | Body | Object | 로드 밸런서에 적용된 IP ACL 그룹 개체 |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| loadbalancers.ipacl_action | Body | UUID | 로드 밸런서에 적용된 IP ACL 그룹들의 action<br>`null`/`DENY`/`ALLOW` 중 하나 |
| loadbalancers.loadbalancer_type | Body | String | 로드 밸런서 타입<br>`shared` / `dedicated` 중 하나 |

<details><summary>예시</summary>

```json
{
  "loadbalancers": [
    {
      "ipacl_group_action": "DENY",
      "description": "",
      "provisioning_status": "ACTIVE",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "provider": "haproxy",
      "ipacl_groups": [
        {
          "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
        }
      ],
      "name": "LB-1",
      "loadbalancer_type": "shared",
      "listeners": [
        {
          "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
        }
      ],
      "vip_address": "192.168.0.187",
      "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
      "workflow_status": "SUCCESS",
      "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
      "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
      "operating_status": "ONLINE",
      "admin_state_up": true,
      "ipacl_groups": [
        {
         "ipacl_group_id": "79ebf206-3463-4df1-a54c-4fc939f8c26c"
         },
         {
         "ipacl_group_id": "947030cc-635f-42d3-b745-770cf7b562fd"
         }
       ],
       "ipacl_group_action": "DENY"
    }
  ]
}
```
</details>

---
### 로드 밸런서 보기

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |
| loadbalancer.ipacl_groups | Body | Object | 로드 밸런서에 적용된 IP ACL 그룹 개체 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| loadbalancer.ipacl_action | Body | UUID | 로드 밸런서에 적용된 IP ACL 그룹들의 action<br>`null`/`DENY`/`ALLOW` 중 하나 |
| loadbalancer.loadbalancer_type | Body | String | 로드 밸런서 타입<br>`shared` / `dedicated` 중 하나 |


<details><summary>예시</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true,
    "ipacl_groups": [
        {
         "ipacl_group_id": "79ebf206-3463-4df1-a54c-4fc939f8c26c"
         },
         {
         "ipacl_group_id": "947030cc-635f-42d3-b745-770cf7b562fd"
         }
     ],
     "ipacl_group_action": "DENY
  }
}
```
</details>

---
### 로드 밸런서 생성하기

```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancer | Body | Object | - | 로드 밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드 밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드 밸런서 설명 |
| loadbalancer.vip_subnet_id | Body | UUID | O | 로드 밸런서의 서브넷 ID |
| loadbalancer.vip_address | Body | String | - | 로드 밸런서의 IP |
| loadbalancer.admin_state_up | Body | Boolean | - | 로드 밸런서 관리자 제어 상태로 생략하면 `true`로 설정됨 |
| loadbalancer.loadbalancer_type | Body | String | - | 로드 밸런서 타입으로, `shared` / `dedicated` 사용 가능<br> 생략할 경우 `shared`로 설정됨 |




<details><summary>예시</summary>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
        "vip_address": "192.168.0.187",
        "admin_state_up": true
    }
}
```
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) 이름 |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |
| loadbalancer.ipacl_groups | Body | Object | 로드 밸런서에 적용된 IP ACL 그룹 개체 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| loadbalancer.ipacl_action | Body | UUID | 로드 밸런서에 적용된 IP ACL 그룹들의 action<br>`null`/`DENY`/`ALLOW` 중 하나 |
| loadbalancer.loadbalancer_type | Body | String | 로드 밸런서 타입<br>`shared` / `dedicated` 중 하나 |


<details><summary>예시</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true,
    "ipacl_groups": [],
    "ipacl_group_action": null
  }
}
```
</details>

---
### 로드 밸런서 수정하기

```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |
| loadbalancer | Body | Object | O | 로드 밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드 밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드 밸런서 설명 |
| loadbalancer.admin_state_up | Body | Boolean | - | 로드 밸런서의 관리자 제어 상태 |

<details><summary>예시</summary>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "admin_state_up": true
    }
}
```
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) 이름 |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |
| loadbalancer.ipacl_groups | Body | Object | 로드 밸런서에 적용된 IP ACL 그룹 개체 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| loadbalancer.ipacl_action | Body | UUID | 로드 밸런서에 적용된 IP ACL 그룹들의 action<br>`null`/`DENY`/`ALLOW` 중 하나 |
| loadbalancer.loadbalancer_type | Body | String | 로드 밸런서 타입<br>`shared` / `dedicated` 중 하나 |


<details><summary>예시</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true
    "ipacl_groups": [],
    "ipacl_group_action": null
  }
}
```
</details>

---
### 로드 밸런서 삭제하기

```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |


#### 응답
이 API는 응답 본문을 반환하지 않습니다.





















## 리스너
### 리스너 목록 보기

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| default_pool_id | Query | UUID | - | 리스너에 등록된 풀 ID |
| protocol | Query | Enum | - | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| description | Query | String | - | 리스너 설명 |
| name | Query | String | - | 리스너 이름 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| connection_limit | Query | Integer | - | 리스너의 connection limit |
| keepalive_timeout | Query | Integer | - | 리스너의 keepalive timeout |
| protocol_port | Query | Integer | - | 리스너 포트 번호 |
| id | Query | UUID | - | 리스너 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listeners | Body | Array | 리스너 정보 객체 목록 |
| listeners.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listeners.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listeners.description | Body | String | 리스너 설명 |
| listeners.name | Body | String | 리스너 이름 |
| listeners.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객 목록 |
| listeners.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listeners.tenant_id | Body | String | 테넌트 ID |
| listeners.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listeners.connection_limit | Body | Integer | 리스너의 connection limit |
| listeners.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listeners.default_tls_container_ref | Body | String| key-manager에 등록된 TLS 인증서 경로 |
| listeners.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listeners.protocol_port | Body | Integer | 리스너 포트 |
| listeners.id | Body | String| 리스너 ID |


<details><summary>예시</summary>
<p>

```json
{
  "listeners": [
    {
      "proxy_protocol": false,
      "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
      "protocol": "TERMINATED_HTTPS",
      "description": "",
      "name": "",
      "loadbalancers": [
        {
          "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
        }
      ],
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "admin_state_up": true,
      "connection_limit": 2000,
      "keepalive_timeout": 300,
      "tls_version": "TLSv1.0",
      "sni_container_ids": [],
      "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
      "sni_container_refs": [],
      "protocol_port": 443,
      "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
      "cert_expire_date": "2025-12-27T10:36:20+00:00"
    }
  ]
}
```

</p>
</details>


### 리스너 보기

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String| key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```

</p>
</details>



---
### 리스너 생성하기

```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listener | Body | Object | O | 리스너 정보 객체 |
| listener.protocol | Body | Enum | O | 리스너 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | - | 리스너 설명 |
| listener.name | Body | String | - | 리스너 이름 |
| listener.loadbalancer_id | Body | UUID | O | 로드 밸런서 ID |
| listener.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | O | 리스너 포트 |


<details><summary>예시</summary>
<p>

```json
{
  "listener": {
    "protocol": "TERMINATED_HTTPS",
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "loadbalancer_id":"7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.cloud.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```
</p>
</details>

---
### 리스너 수정하기

```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |
| listener | Body | Object | O | 리스너 정보 객체 |
| listener.description | Body | String | - | 리스너 설명 |
| listener.name | Body | String| - | 리스너 이름 |
| listener.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | key-manager에 등록된 SNI 인증서 경로 목록 |

<details><summary>예시</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.cloud.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": []
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.cloud.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```

</p>
</details>

---
### 리스너 삭제하기
지정한 리스너를 삭제합니다.
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.













## 풀
### 풀 목록 보기

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 풀 ID |
| name | Query | String | - | 풀 이름 |
| lb_algorithm | Query | Enum | - | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| protocol | Query | Enum | - | 멤버의 프로토콜 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| healthmonitor_id | Query | UUID | - | 풀의 헬스 모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pools | Body | Array | 풀 정보 객체 목록 |
| pools.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pools.protocol | Body | Enum | 멤버의 프로토콜 |
| pools.description | Body | String | 풀 설명 |
| pools.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pools.tenant_id | Body | String | 테넌트 ID |
| pools.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 설정값이 적용됩니다. |
| pools.healthmonitor_id | Body | String | 헬스 모니터 ID |
| pools.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pools.listeners.id | Body | String | 리스너 ID |
| pools.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pools.members.id | Body | String | 멤버 ID |
| pools.id | Body | UUID | 풀 ID |
| pools.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "pools": [
    {
      "lb_algorithm": "ROUND_ROBIN",
      "protocol": "HTTP",
      "description": "",
      "admin_state_up": true,
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "member_port": 80,
      "session_persistence": null,
      "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
      "listeners": [
        {
          "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
        }
      ],
      "members": [
        {
          "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
        },
        {
          "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
        }
      ],
      "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
      "name": ""
    }
  ]
}
```

</p>
</details>


### 풀 보기

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.member_port | Body | Integer | 멤버의 포트<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트값 |
| pool.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pool.session_persistence.type | Body | Enum | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pool.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 설정값이 적용됩니다. |
| pool.healthmonitor_id | Body | UUID | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>



---
### 풀 생성하기

```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| pool | Body | Object | O | 풀 정보 객체 |
| pool.listener_id | Body | UUID | O | 풀이 등록될 리스너 ID |
| pool.lb_algorithm | Body | Enum | O | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pool.protocol | Body | Enum | O | 멤버의 프로토콜 |
| pool.description | Body | String | - | 풀 설명 |
| pool.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| pool.member_port | Body | Integer | - | 멤버의 수신 포트<br>트래픽을 이 포트로 전달합니다.<br>기본 값은 -1입니다. |
| pool.session_persistence | Body | Object | - | 풀의 세션 지속성 객체 |
| pool.session_persistence.type | Body | Enum | - | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 설정값이 적용됩니다. |
| pool.name | Body | String | - | 풀 이름 |



<details><summary>예시</summary>
<p>

```json
{
  "pool": {
    "listener_id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "member_port": 80,
    "session_persistence": null,
    "name": ""
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.session_persistence | Body | Object | - | 풀의 세션 지속성 객체 |
| pool.session_persistence.type | Body | Enum | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pool.healthmonitor_id | Body | String | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>

---
### 풀 수정하기

```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 풀 ID |
| pool | Body | Object | O | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | - | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pool.description | Body | String | - |  풀 설명 |
| pool.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| pool.session_persistence | Body | Object | - | 풀의 세션 지속성 객체 |
| pool.session_persistence.type | Body | Enum | - | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 설정값이 적용됩니다. |
| pool.name | Body | String | - | 풀 이름 |



<details><summary>예시</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "description": "",
    "admin_state_up": true,
    "member_port": 80,
    "session_persistence": null,
    "name": ""
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP` 중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pools.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pool.session_persistence.type | Body | Enum | 세션 지속성<br> `SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나로 설정<br> `HTTP_COOKIE`, `APP_COOKIE`로 설정하는 경우 연결된 리스너의 프로토콜을 `HTTP` 또는 `TERMINATED_HTTPS`로 설정했는지 확인하는 것이 좋습니다.<br> 리스너의 프로토콜을 `TCP` 또는 `HTTPS`로 설정한 경우, 세션 지속성을 `HTTP_COOKIE`, `APP_COOKIE`로 설정해도 로드 밸런서는 세션 지속성 관련 동작을 하지 않습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 설정값이 적용됩니다. |
| pool.healthmonitor_id | Body | UUID | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>

---
### 풀 삭제하기
지정한 풀을 삭제합니다.
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 풀 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.




























## 헬스 모니터
### 헬스 모니터 목록 보기

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 헬스 모니터 ID |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| delay | Query | Integer | - | 상태 확인 간격(초) |
| expected_codes | Query | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다. |
| max_retries | Query | Integer | - | 최대 재시도 횟수 |
| http_method | Query | Enum | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| timeout | Query | Integer | - | 상태 확인 응답 대기 시간(초) |
| url_path | Query | String | - | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| type | Query | Enum | - | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| host_header | Query | String | - | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|



#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitors | Body | Array | 헬스 모니터 정보 객체 목록 |
| healthmonitors.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitors.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitors.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다. |
| healthmonitors.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitors.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitors.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitors.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitors.pools.id | Body | UUID | 풀 ID |
| healthmonitors.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitors.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitors.id | Body | UUID | 헬스 모니터 ID |
| healthmonitors.host_header | Body | String | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|



<details><summary>예시</summary>
<p>

```json
{
  "healthmonitors": [
    {
      "admin_state_up": true,
      "health_check_port": 80,
      "delay": 30,
      "expected_codes": "200",
      "max_retries": 2,
      "http_method": "GET",
      "timeout": 5,
      "pools": [
        {
          "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
        }
      ],
      "url_path": "/",
      "type": "HTTP",
      "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
    }
  ]
}
```

</p>
</details>


### 헬스 모니터 보기

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스 모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitors.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |
| healthmonitors.host_header | Body | String | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|



<details><summary>예시</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>



---
### 헬스 모니터 생성하기

```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitor | Body | Object | O | 헬스 모니터 정보 객체 |
| healthmonitor.pool_id | Body | UUID | O | 헬스 모니터가 연결될 풀 ID |
| healthmonitor.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | - | 헬스 체크의 대상이 되는 멤버 포트 |
| healthmonitor.delay | Body | Integer | O | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드. 생략하면 200으로 설정됨.<br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.max_retries | Body | Integer | O | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | - | 상태 확인에 사용할 HTTP Method. 생략하면 `GET`이 사용됨. <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.timeout | Body | Integer | O | 상태 확인 응답 대기 시간(초) |
| healthmonitor.url_path | Body | String | - | 상태 확인 요청 URL. 생략하면 `/`가 설정됨. <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.type | Body | Enum  | O | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitors.host_header | Body | String | - | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|





<details><summary>예시</summary>
<p>

```json
{
  "healthmonitor": {
    "pool_id": "872dc92f-777b-4e0f-9413-0132b98bc60b",
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "url_path": "/",
    "type": "HTTP"
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드. 생략하면 200으로 설정됨.<br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |
| healthmonitors.host_header | Body | String | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|



<details><summary>예시</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>

---
### 헬스 모니터 수정하기

```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitorId | URL | UUID | O | 헬스 모니터 ID |
| healthmonitor | Body | Object | O | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | - | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드<br>단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.max_retries | Body | Integer | - | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.timeout | Body | Integer | - | 상태 확인 응답 대기 시간(초) |
| healthmonitor.url_path | Body | String | - | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitors.host_header | Body | String | - | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|


<details><summary>예시</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "url_path": "/"
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드<br>단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |
| healthmonitors.host_header | Body | String | 상태 확인에 사용할 호스트 헤더의 필드값<br> 상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시됩니다.|


<details><summary>예시</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>

---
### 헬스 모니터 삭제하기

```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스 모니터 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.






























## 멤버
### 멤버 목록 보기

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| id | Query | UUID | - | 멤버 ID |
| weight | Query | Integer | - | 멤버 가중치 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| subnet_id | Query | UUID | - | 멤버의 서브넷 ID |
| tenant_id | Query | String | - | 테넌트 ID |
| address | Query | String | - | 멤버의 IP 주소 |
| protocol_port | Query | Integer | - | 멤버의 포트 |
| operating_status | Query | Enum | - | 멤버의 운영 상태 |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| members | Body | Array | 멤버 정보 객체 목록 |
| members.weight | Body | Integer | 멤버 가중치 |
| members.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| members.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| members.tenant_id | Body | String | 테넌트 ID |
| members.address | Body | String | 멤버의 IP 주소 |
| members.protocol_port | Body | Integer | 멤버의 포트 |
| members.id | Body | UUID | 멤버 ID |
| members.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
<p>

```json
{
  "members": [
    {
      "weight": 1,
      "admin_state_up": true,
      "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "address": "192.168.0.188",
      "protocol_port": 80,
      "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
      "operating_status": "INACTIVE"
    }
  ]
}
```

</p>
</details>


### 멤버 보기

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### 멤버 생성하기

```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| member | Body | Object | O | 멤버 정보 객체 |
| member.weight | Body | Integer | - | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | -| 관리자 제어 상태 |
| member.subnet_id | Body | UUID | O | 멤버의 서브넷 ID |
| member.address | Body | String | O | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | O | 멤버의 포트 |


<details><summary>예시</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "address": "192.168.0.188",
    "protocol_port": 80
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### 멤버 수정하기

```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |
| member | Body | Object | O | 멤버 정보 객체 |
| member.weight | Body | Integer | - | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |

<details><summary>예시</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### 멤버 삭제하기

```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

























## 시크릿

시크릿 API는 `key-manager` 타입 앤드포인트를 이용하여 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| key-manager | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 노출될 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.


### 시크릿 목록 보기

시크릿 목록을 반환합니다.

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| offset | Query | Integer | - | 응답 목록의 오프셋, 기본값: 0 |
| limit | Query | Integer| - | 응답 목록에 노출할 최대 개수, 기본값: 10 |
| name | Query | String | - | 시크릿 이름 |
| alg | Query | String | - | 시크릿 알고리즘 |
| mode | Query | String| - | 블록 암호 운용 방식 |
| bits | Query | Integer| - | 암호화 키 길이 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secrets | Body | Array | 시크릿 객체 목록 |
| secrets.secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |
| secrets.secret_type | Body | Enum | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` 중 하나 |
| secrets.status | Body | Enum | 시크릿 상태 |
| secrets.content_types | Body | Array | 시크릿 페이로드의 콘텐츠 타입 목록 |
| secrets.content_types.default | Body | String | 콘텐츠 타입 기본값 |
| secrets.creator_id | Body | String | 시크릿을 생성한 사용자 ID |
| secrets.mode | Body | String | 블록 암호 운용 방식. 사용자 입력 메타데이터 |
| secrets.algorithm | Body | String | 암호화 알고리즘. 사용자 입력 메타데이터 |
| secrets.bit_length | Body | Integer | 암호화 키 길이. 사용자 입력 메타데이터 |
| secrets.expiration | Body | Datetime | 만료일. 사용자 입력 메타데이터 <br>`YYYY-MM-DDThh:mm:ss`<br> 만료일이 지난 시크릿은 자동으로 삭제 처리됨 |
| secrets.name| Body | String | 시크릿 이름 |
| secrets.created | Body | Datetime | 생성 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | 수정 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | 요청 쿼리의 총 시크릿 개수 |
| next | Body | String | 현재 조회된 목록의 다음 목록 URL |
| previous | Body | String | 현재 조회된 목록의 이전 목록 URL |

<details><summary>예시</summary>
<p>

```json
{
  "secrets": [
    {
      "algorithm": null,
      "bit_length": null,
      "content_types": {
        "default": "text/plain"
      },
      "created": "2019-12-17T08:50:39",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "expiration": null,
      "mode": null,
      "name": "certificate",
      "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
      "secret_type": "certificate",
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39"
    },
    {
      "algorithm": null,
      "bit_length": null,
      "content_types": {
        "default": "text/plain"
      },
      "created": "2019-12-17T08:50:39",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "expiration": null,
      "mode": null,
      "name": "private_key",
      "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
      "secret_type": "private",
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39"
    }
  ],
  "total": 10,
  "next": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets?limit=1&offset=2",
  "previous": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets?limit=1&offset=0"
}

```

</p>
</details>


### 시크릿 보기
지정한 시크릿 정보를 반환합니다.
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secret | Body | Object | 시크릿 객체 |
| secret.secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |
| secret.secret_type | Body | Enum | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` 중 하나 |
| secret.status | Body | Enum | 시크릿 상태 |
| secret.content_types | Body | Array | 시크릿 페이로드의 콘텐츠 타입 목록 |
| secret.content_types.default | Body | String | 콘텐츠 타입 기본값 |
| secret.creator_id | Body | String | 시크릿을 생성한 사용자 ID |
| secret.mode | Body | String | 블록 암호 운용 방식. 사용자 입력 메타데이터 |
| secret.algorithm | Body | String | 암호화 알고리즘. 사용자 입력 메타데이터 |
| secret.bit_length | Body | Integer | 암호화 키 길이. 사용자 입력 메타데이터 |
| secret.expiration | Body | Datetime | 만료일. 사용자 입력 메타데이터 <br>`YYYY-MM-DDThh:mm:ss`<br> 만료일이 지난 시크릿은 자동으로 삭제됨 |
| secret.name| Body | String | 시크릿 이름 |
| secret.created | Body | Datetime | 생성 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | 수정 시간 <br> `YYYY-MM-DDThh:mm:ss` |

<details><summary>예시</summary>
<p>

```json
{
  "status": "ACTIVE",
  "secret_type": "certificate",
  "updated": "2019-12-17T08:50:39",
  "name": "certificate",
  "algorithm": null,
  "created": "2019-12-17T08:50:39",
  "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
  "content_types": {
    "default": "text/plain"
  },
  "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
  "mode": null,
  "bit_length": null,
  "expiration": null
}
```
</p>
</details>

---
### 시크릿 생성하기
새로운 시크릿을 생성합니다.
```
POST /v1/secrets
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| name | Body | String | - | 시크릿 이름 |
| expiration | Body | Datetime | - | 만료일. ISO8601형식으로 요청 |
| algorithm | Body | String | - | 암호화 알고리즘 |
| bit_length | Body | String | - | 암호화 키 길이|
| mode | Body | String | - | 블록 암호 운용 방식 |
| payload | Body | String | - | 암호화 키 페이로드 |
| payload_content_type | Body | String | - | 암호화 키 페이로드 콘텐츠 타입<br> payload를 입력할 시 필수로 입력해야 함 <br>지원하는 콘텐츠 타입 목록: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | 암호화 키 페이로드 인코딩 방식 <br>payload_content_type이 text/plain이 아닌 경우 필수로 입력해야 함<br> `base64` 만 지원 |
| secret_type | Body | Enum | - | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` 중 하나 |



<details><summary>예시</summary>
메타데이터만 생성
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode"
}
```

text로 페이로드 전송
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode",
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n",
    "payload_content_type": "text/plain"
}
```

base64로 페이로드 전송
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode",
    "payload": "ZXhhbXBsZQo=",
    "payload_content_type": "application/octet-stream",
    "payload_content_encoding": "base64"
}
```
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |

<details><summary>예시</summary>
<p>

```json
{
    "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### 시크릿 수정하기
기존에 메타데이터만 입력한 시크릿의 페이로드 데이터를 입력합니다.
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ConetentType}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |
| ContentType| Header | Enum | O | `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` 중 하나<br> 생략시 `text/plain` 으로 설정됨 |
| payload | Body | String | O | 암호화 키 페이로드 |

<details><summary>예시</summary>
```
{
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n"
}
```
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

---
### 시크릿 삭제하기
지정한 시크릿을 삭제합니다.
```
DELETE /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.






































## 시크릿 컨테이너

시크릿 컨테이너 API는 `key-manager` 타입 앤드포인트를 이용하여 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| key-manager | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 노출될 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지없이 변경될 수 있으므로 사용하지 않습니다.


### 시크릿 컨테이너 목록 보기

시크릿 컨테이너 목록을 반환합니다.

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| offset | Query | Integer | - | 응답 목록의 오프셋, 기본값: 0 |
| limit | Query | Integer | - | 응답 목록에 노출할 최대 개수, 기본값: 10 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| containers | Body | Array | 컨테이너 객체 목록 |
| containers.status | Body | Enum | 컨테이너 상태 |
| containers.updated | Body | Datetime | 수정 시간 `YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | 컨테이너 이름 |
| containers.consumers | Body | Array | 컨슈머 목록 |
| containers.consumers.URL | Body | String | 컨슈머 URL |
| containers.consumers.name | Body | String | 컨슈머 이름 |
| containers.created | Body | Datetime | 생성 시간  `YYYY-MM-DDThh:mm:ss`|
| containers.container_ref | Body | String | 컨테이너 주소 |
| containers.creator_id | Body | String | 컨테이너를 생성한 사용자 ID |
| containers.secret_refs | Body | Array | 시크릿 목록 |
| containers.secret_refs.secret_ref | Body | String | 시크릿 주소 |
| containers.secret_refs.name | Body | String | 컨테이너가 지정한 시크릿 이름<br> 컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |
| containers.type | Body | Enum | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나|
| total | Body | Integer | 요청 쿼리의 시크릿 컨테이너의 총 개수 |
| next | Body | String | 현재 조회된 목록의 다음 목록 URL |
| previous | Body | String | 현재 조회된 목록의 이전 목록 URL |



<details><summary>예시</summary>
<p>

```json
{
  "total": 10,
  "previous": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=0",
  "next": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=2",
  "containers": [
    {
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39",
      "name": "The Certificate",
      "consumers": [],
      "created": "2019-12-17T08:50:39",
      "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "secret_refs": [
        {
          "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
          "name": "certificate"
        },
        {
          "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
          "name": "private_key"
        }
      ],
      "type": "certificate"
    }
  ]
}


```
</p>
</details>


### 시크릿 컨테이너 보기
지정한 시크릿 컨테이너 정보를 반환합니다.
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| containerId | URL | UUID | O | 시크릿 컨테이너 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| status | Body | Enum | 컨테이너 상태 |
| updated | Body | Datetime | 수정 시간 `YYYY-MM-DDThh:mm:ss` |
| name | Body | String | 컨테이너 이름 |
| consumers | Body | Array | 컨슈머 목록 |
| consumers.URL | Body | String | 컨슈머 URL |
| consumers.name | Body | String | 컨슈머 이름 |
| created | Body | Datetime | 생성 시간  `YYYY-MM-DDThh:mm:ss`|
| container_ref | Body | String | 컨테이너 주소 |
| creator_id | Body | String | 컨테이너를 생성한 사용자 ID |
| secret_refs | Body | Array | 컨테이너에 등록한 시크릿 목록 |
| secret_refs.secret_ref | Body | String | 시크릿 주소 |
| secret_refs.name | Body | String| 컨테이너가 지정한 시크릿 이름<br>컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |
| type | Body | Enum | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나 |


<details><summary>예시</summary>

```json
{
    "status": "ACTIVE",
    "updated": "2019-12-17T08:50:39",
    "name": "The Certificate",
    "consumers": [],
    "created": "2019-12-17T08:50:39",
    "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
    "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
    "secret_refs": [
        {
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
            "name": "private_key"
        },
        {
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
            "name": "certificate"
        }
    ],
    "type": "certificate"
}
```
</details>

---
### 시크릿 컨테이너 생성하기
새로운 시크릿 컨테이너 생성합니다.
```
POST /v1/containers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| type | Body | Enum | O | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나 |
| name | Body | String | - | 컨테이너 이름 |
| secret_refs | Body | Array | - | 컨테이너에 등록할 시크릿 목록 |
| secret_refs.secret_ref | Body | String | - | 시크릿 주소 |
| secret_refs.name | Body | String | - | 컨테이너가 지정한 시크릿 이름<br> 컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |


<details><summary>예시</summary>
<p>

```json
{
    "type": "certificate",
    "name": "test cert",
    "secret_refs": [
        {
            "name": "private_key",
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/cf11edcf-f475-47f3-92c3-29de8bcdd639"
        }
    ]
}
```
</p>
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| container_ref | Body | String | 시크릿 컨테이너 주소 |

<details><summary>예시</summary>
<p>

```json
{
    "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/ea2e90fc-1ba2-412b-b7a0-61da4402bf58"
}
```
</p>
</details>

---
### 시크릿 컨테이너 삭제하기
지정한 시크릿 컨테이너 삭제합니다.
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| containerId | URL | UUID | 시크릿 컨테이너 ID |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.



































## IP ACL 그룹

### IP ACL 그룹 목록 보기

IP ACL 그룹 목록을 반환합니다.

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | IP ACL 그룹 ID |
| name | Query | String | - | IP ACL 그룹 이름 |
| description | Query | String | - | IP ACL 그룹 설명 |
| action | Query | Enum | - | IP ACL 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_groups | Body | Array | IP ACL 그룹 객체 목록 |
| ipacl_groups.ipacl_target_count | Body | String | IP ACL 그룹에 포함된 타깃 개수 |
| ipacl_groups.description | Body | String | IP ACL 그룹 설명 |
| ipacl_groups.loadbalancers | Body | Object | IP ACL 그룹이 적용된 로드 밸런서 객체 목록 |
| ipacl_groups.loadbalancers.loadbalancer_id | Body | String | 로드 밸런서 ID |
| ipacl_groups.tenant_id | Body | String | 테넌트 ID |
| ipacl_groups.action | Body | Enum | IP 접근제어 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |
| ipacl_groups.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_groups.name | Body | String | IP ACL 그룹명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_groups": [
      {
      "ipacl_target_count": "1",
      "description": "",
      "loadbalancers": [
        {
          "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
        }
      ],
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "action": "DENY",
      "id": "04570ec5-456a-48ac-85ee-38adcc83ee70",
      "name": "ip-acl-group-1"
    }
  ]
}
```
</p>
</details>

### IP ACL 그룹 보기

지정한 IP ACL 그룹을 반환합니다.

```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 포함된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | Object | IP ACL 그룹이 적용된 로드 밸런서 객체 목록 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | 로드 밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "1",
    "description": "",
    "loadbalancers": [
      {
        "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "action": "DENY",
    "id": "04570ec5-456a-48ac-85ee-38adcc83ee70",
    "name": "ip-acl-group-1"
  }
}
```
</p>
</details>

- - -

### IP ACL 그룹 생성하기

새로운 IP ACL 그룹을 생성합니다.

```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipacl_group | Body | Object | O | IP ACL 그룹 객체 |
| ipacl_group.description | Body | String | -  | IP ACL 그룹 설명 |
| ipacl_group.action | Body | Enum | O | IP ACL 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |
| ipacl_group.name | Body | String | -  | IP ACL 그룹명 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACL 타깃 객체, 값 입력 시 타깃도 함께 생성함 |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targets 객체가 추가된 경우) | IP ACL 타깃 CIDR<br>단독 IP 주소, 또는 CIDR 형식의 IP RANGE 입력 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACL 타깃 설명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_group": {
    "action": "ALLOW",
    "name": "example",
    "description": "description",
    "ipacl_targets": [
			{
				"cidr_address" : "192.168.0.5",
				"description": "My Friend"
			},
			{
				"cidr_address" : "10.10.22.3/24",
				"description": "Your Friends"
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
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 포함된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드 밸런서 객체 목록 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | 로드 밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "0",
    "description": "description",
    "loadbalancers": [],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "action": "ALLOW",
    "id": "e5e2627e-c1fc-4deb-a96d-f1213bb8227e",
    "name": "example"
  }
}
```
</p>
</details>

- - -

### IP ACL 그룹 수정하기

기존 IP ACL 그룹을 수정합니다.
ipacl_group.action은 변경할 수 없습니다.
하위 IP ACL 타깃 목록을 전체적으로 교체할 때에 이 API를 사용할 수 있습니다. 
단, IP ACL 그룹에 속했던 모든 기존의 타깃이 삭제되고 입력한 타깃 목록으로 대체됩니다. 
입력한 타깃의 cidr_address는 중복되지 않아야 합니다.

```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | URL | UUID | O | IP ACL 그룹 ID |
| ipacl_group | Body | String | O | IP ACL 그룹 객체 |
| ipacl_group.name | Body | String | - | IP ACL 그룹명 |
| ipacl_group.description | Body | String | - | IP ACL 그룹 설명 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACL 타깃 객체, 값 입력 시 타깃도 함께 생성함 |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targets 객체가 추가된 경우) | IP ACL 타깃 CIDR<br>단독 IP 주소, 또는 CIDR 형식의 IP RANGE 입력 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACL 타깃 설명 |


<details><summary>예시</summary>
<p>

``` json
{
    "ipacl_group" : {
    "name" : "HouseLannister",
    "description" : "A Lannister always pays his debts",
    "ipacl_targets" : [
        {
            "cidr_address" : "11.11.11.11",
            "description" : "Jamie"
        },
        {
            "cidr_address" : "22.22.22.22",
            "description" : "Cercei"
        },
        {
            "cidr_address" : "33.33.33.33",
            "description" : "Tyrion"
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
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 포함된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드 밸런서 객체 목록 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | 로드 밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 그룹의 제어 동작<br>`ALLOW`, `DENY`중 하나 |
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "3",
    "description": "A Lannister always pays his debts",
    "loadbalancers": [],
    "tenant_id": "18717b5d8a9d45b9af440c75d61235c7",
    "action": "DENY",
    "id": "acc655d4-4735-4892-b32b-669cc21925ff",
    "name": "HouseLannister"
  }
}
```
</p>
</details>

- - -

### IP ACL 그룹 삭제하기

지정한 IP ACL 그룹을 삭제합니다.

```
DELETE /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

IP ACL 그룹 삭제시 하위의 IP ACL 타깃도 모두 삭제됩니다. 
삭제되는 IP ACL 그룹을 사용하는 모든 로드 밸런서에서 이 IP ACL 그룹 관련된 룰이 삭제됩니다.

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | URL | UUID | O | IP ACL 그룹 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

- - -


### 로드 밸런서에 IP ACL 그룹 적용

로드 밸런서에 IP ACL 그룹을 적용합니다.
IP ACL 그룹을 적용받은 로드 밸런서에는 그룹에 포함된 IP ACL 타겟 룰이 적용됩니다.
여러 개의 그룹을 로드 밸런서에 적용할 수 있습니다. 단, 그룹들의 action은 모두 동일해야 합니다.
기존에 로드 밸런서에 적용되어 있던 IP ACL 그룹은 모두 삭제되고 입력된 그룹 목록으로 재적용됩니다.

```
PUT /v2.0/lbaas/loadbalancers/{lb_id}/bind_ipacl_groups
X-auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| lb_id | URL | UUID | O | 로드 밸런서 ID |
| ipacl_groups_binding | Body | Object | O | IP ACL 바인딩 객체 |
| ipacl_groups_binding.ipacl_group_id | Body | UUID | O | 로드 밸런서에 적용할 IP ACL 그룹 ID |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_groups_binding": [
    {
      "ipacl_group_id": "acc655d4-4735-4892-b32b-669cc21925ff"
    },
    {
      "ipacl_group_id": "ef33c087-2dc9-4be6-a0d2-d24c9d84e66e"
    }
  ]
}
```

</p>
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| loadbalancer_id | Body | UUID | 로드 밸런서 ID |
| ipacl_group_id | Body | UUID | IP ACL 그룹 ID |

<details><summary>예시</summary>
<p>

``` json
[
  {
    "loadbalancer_id": "096ddfbf-aaf9-42d6-b93d-0036ec219479",
    "ipacl_group_id": "acc655d4-4735-4892-b32b-669cc21925ff"
  },
  {
    "loadbalancer_id": "096ddfbf-aaf9-42d6-b93d-0036ec219479",
    "ipacl_group_id": "ef33c087-2dc9-4be6-a0d2-d24c9d84e66e"
  }
]
```

</p>
</details>







































## IP ACL 타깃

### IP ACL 타깃 목록 보기

IP ACL 타깃 목록을 반환합니다.

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | IP ACL 타깃 ID |
| cidr_address | Query | String | - | IP ACL 타깃 CIDR<br>단독 IP 주소 또는 CIDR 형식의 IP RANGE |
| ipacl_group_id | Query | String | - | IP ACL 그룹 id |
| description | Query | String | - | IP ACL 그룹 설명 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_targets | Body | Array | IP ACL 타깃 정보 객체 목록 |
| ipacl_targets.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_targets.tenant_id | Body | String | 테넌트 ID |
| ipacl_targets.cidr_address | Body | String | IP ACL 타깃 CIDR |
| ipacl_targets.description | Body | String | IP ACL 타깃 설명 |
| ipacl_targets.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_targets": [
    {
      "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "cidr_address": "10.0.0.0/24",
      "description": "description",
      "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
    }
  ]
}
```

</p>
</details>

### IP ACL 타깃 보기

지정한 IP ACL 타깃 정보를 반환합니다.

```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_target | Body | Array | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR<br>단독 IP 주소 또는 CIDR 형식의 IP RANGE |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### IP ACL 타깃 생성하기

IP ACL 타깃을 생성합니다.

```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipacl_target | Body | Object | O | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | O | IP ACL 그룹 ID |
| ipacl_target.cidr_address | Body | String | O | IP ACL 타깃 CIDR<br>단독 IP 주소 또는 CIDR 형식의 IP RANGE |
| ipacl_target.description | Body | String | - | IP ACL 타깃 설명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "cidr_address": "10.0.0.0/24",
    "description": "description"
  }
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR<br>단독 IP 주소 또는 CIDR 형식의 IP RANGE |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### IP ACL 타깃 수정하기

기존 IP ACL 타깃을 변경합니다.
description만 변경할 수 있습니다.

```
PUT /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID |
| ipacl_target | Body | Object | O | IP ACL 타깃 정보 객체 |
| ipacl_target.description | Body | String | - | IP ACL 타깃 설명 |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_target": {
    "description": "description"
  }
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR<br>단독 IP 주소 또는 CIDR 형식의 IP RANGE |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### IP ACL 타깃 삭제하기

지정한 로드 밸런서를 삭제합니다.

```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

- - -

