## Network > Security Groups > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

보안 그룹 API는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.


## 보안 그룹
### 보안 그룹 목록 보기
```
GET /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 보안 그룹 ID |
| tenant_id | Query | String | - | 조회할 보안 그룹의 테넌트 ID |
| name | Query | String | - | 조회할 보안 그룹의 이름 |
| sort_dir | Query | Enum | - | 조회할 보안 그룹의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 보안 그룹의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 보안 그룹의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_groups | Body | Array | 보안 그룹 목록 객체 |
| security_groups.tenant_id | Body | String | 테넌트 ID |
| security_groups.description | Body | String | 보안 그룹 설명 |
| security_groups.id | Body | UUID | 보안 그룹 ID |
| security_groups.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_groups.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "security_groups": [
    {
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "description": "Default security group",
      "id": "877b092c-2a31-4509-8d23-deeb02d95633",
      "security_group_rules": [
        {
          "direction": "ingress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "0c7279cd-657e-43cd-a635-6886ca3a8acd",
          "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv4"
        },
        {
          "direction": "egress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "4c5ae06e-44f0-4ea9-a999-29473873bca2",
          "remote_group_id": null,
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv6"
        },
        {
          "direction": "egress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "4e21389a-bb3c-469c-8139-29da582bc3c5",
          "remote_group_id": null,
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv4"
        },
        {
          "direction": "ingress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "72af180f-c425-4ec4-b7a3-90f86d4ce145",
          "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv6"
        }
      ],
      "name": "default"
    }
  ]
}
```

</p>
</details>

---

### 보안 그룹 보기
```
GET /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 예시 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupId | Query | UUID | O | 조회할 보안 그룹 ID |
| tokenId | Header | String | O | 토큰 ID |
| fields | Query | String | - | 조회할 보안 그룹의 필드 이름<br>지정한 필드만 응답에 반환<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 예시 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "Default security group",
    "id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "security_group_rules": [
      {
        "direction": "ingress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "0c7279cd-657e-43cd-a635-6886ca3a8acd",
        "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "4c5ae06e-44f0-4ea9-a999-29473873bca2",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "4e21389a-bb3c-469c-8139-29da582bc3c5",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      }
      {
        "direction": "ingress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "72af180f-c425-4ec4-b7a3-90f86d4ce145",
        "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "default"
  }
}
```

</p>
</details>

---

### 보안 그룹 생성하기

새로운 보안 그룹을 생성합니다. 새로 생성된 보안 그룹은 나가는 방향의 보안 그룹 규칙을 기본적으로 포함합니다.

```
POST /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| security_group | Body | Object | O | 보안 그룹 생성 요청 객체 |
| description | Body | String | - | 보안 그룹 설명 |
| name | Body | String | - | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
    "security_group": {
        "name": "webservers",
        "description": "security group for webservers"
    }
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "security group for webservers",
    "id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
    "security_group_rules": [
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "057e031a-ec2a-4bee-859a-6bc1d88c57d0",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "cd37d4a7-75ac-4246-95cf-e93b37b75a9f",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "webservers"
  }
}
```

</p>
</details>

---

### 보안 그룹 수정하기
기존 보안 그룹을 수정합니다.
```
PUT /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| securityGroupId | URL | UUID | O | 보안 그룹 ID |
| security_group | Body | Object | O | 보안 그룹 수정 요청 객체 |
| description | Body | String | - | 보안 그룹 설명 |
| name | Body | String | - | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
    "security_group": {
        "name": "new-webservers",
        "description": "security group for new webservers"
    }
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
<p>

```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "security group for new webservers",
    "id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
    "security_group_rules": [
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "057e031a-ec2a-4bee-859a-6bc1d88c57d0",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "cd37d4a7-75ac-4246-95cf-e93b37b75a9f",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "new-webservers"
  }
}
```

</p>
</details>

---

### 보안 그룹 삭제하기
지정한 보안 그룹을 삭제합니다.
```
DELETE /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupId | URL | UUID | O | 보안 그룹 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 보안 규칙
### 보안 규칙 목록 보기
```
GET /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 보안 규칙 ID |
| remote_group_id | Query | UUID | - | 조회할 보안 규칙의 원격 보안 그룹 ID |
| protocol | Query | String | - | 조회할 보안 규칙의 프로토콜 |
| direction | Query | Enum | - | 조회할 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| ethertype | Query | Enum | - | 조회할 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| port_range_max | Query | Integer | - | 조회할 보안 규칙의 포트 범위 최댓값 |
| port_range_min | Query | Integer | - | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_id | Query | UUID | - | 조회할 보안 규칙이 속한 보안 그룹 ID |
| tenant_id | Query | String | - | 조회할 보안 규칙의 테넌트 ID |
| remote_ip_prefix | Query | String | - | 조회할 보안 규칙의 목적지 IP 접두사 |
| description | Query | String | - | 조회할 보안 규칙의 설명 |
| sort_dir | Query | Enum | - | 조회할 보안 규칙의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 보안 규칙의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 보안 규칙의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rules | Body | Array | 보안 규칙 객체 목록 |
| security_group_rules.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rules.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rules.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rules.description | Body | String | 보안 규칙 설명 |
| security_group_rules.port_range_max | Body | Integer | 보안 규칙의 포트 범위 최댓값 |
| security_group_rules.port_range_min | Body | Integer | 보안 규칙의 포트 범위 최솟값 |
| security_group_rules.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rules.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rules.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rules.tenant_id | Body | String | 테넌트 ID |
| security_group_rules.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
<p>

```json
{
  "security_group_rules": [
    {
      "direction": "ingress",
      "protocol": "tcp",
      "description": "",
      "port_range_max": 65535,
      "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
      "remote_group_id": null,
      "remote_ip_prefix": "0.0.0.0/0",
      "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "port_range_min": 1,
      "ethertype": "IPv4"
    }
  ]
}
```

</p>
</details>

---

### 보안 규칙 보기
```
GET /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | 보안 규칙 ID |
| tokenId | Header | String | O | 토큰 ID |
| fields | Query | String | - | 조회할 보안 규칙의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rule | Body | Object | 보안 규칙 객체 |
| security_group_rule.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rule.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rule.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rule.description | Body | String | 보안 규칙 설명 |
| security_group_rule.port_range_max | Body | Integer | 조회할 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.tenant_id | Body | String | 테넌트 ID |
| security_group_rule.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
<p>

```json
{
  "security_group_rule": {
    "direction": "ingress",
    "protocol": "tcp",
    "description": "",
    "port_range_max": 65535,
    "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
    "remote_group_id": null,
    "remote_ip_prefix": "0.0.0.0/0",
    "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "port_range_min": 1,
    "ethertype": "IPv4"
  }
}
```

</p>
</details>

---

### 보안 규칙 생성하기

새로운 보안 그룹 규칙을 생성합니다. IPv4에 대한 보안 규칙만 생성할 수 있습니다.

```
POST /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| security_group_rule | Body | Object | O | 보안 규칙 생성 요청 객체 |
| security_group_rule.remote_group_id | Body | UUID | - | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.direction | Body | Enum | O | 보안 규칙이 적용되는 패킷 방향<br>**ingress**, **egress** |
| security_group_rule.ethertype | Body | Enum | - | `IPv4`로 지정. 생략 시에 `IPv4`로 지정 |
| security_group_rule.protocol | Body | String | - | 보안 규칙의 프로토콜 이름. 생략 시에 모든 프로토콜에 적용. |
| security_group_rule.port_range_max | Body | Integer | - | 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | - | 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.security_group_id | Body | UUID | O | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | - | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.description | Body | String | - | 보안 규칙 설명 |

<details><summary>예시</summary>
<p>

```json
{
    "security_group_rule": {
        "direction": "ingress",
        "port_range_min": "80",
        "ethertype": "IPv4",
        "port_range_max": "80",
        "protocol": "tcp",
        "remote_group_id": "85cc3048-abc3-43cc-89b3-377341426ac5",
        "security_group_id": "a7734e61-b545-452d-a3cd-0189cbd9747a"
    }
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rule | Body | Object | 보안 규칙 객체 |
| security_group_rule.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rule.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rule.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rule.description | Body | String | 보안 규칙 설명 |
| security_group_rule.port_range_max | Body | Integer | 조회할 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.tenant_id | Body | String | 테넌트 ID |
| security_group_rule.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
<p>

```json
{
  "security_group_rule": {
    "direction": "ingress",
    "protocol": "tcp",
    "description": "",
    "port_range_max": 65535,
    "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
    "remote_group_id": null,
    "remote_ip_prefix": "0.0.0.0/0",
    "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "port_range_min": 1,
    "ethertype": "IPv4"
  }
}
```

</p>
</details>

---

### 보안 규칙 삭제하기
지정한 보안 규칙을 삭제합니다.
```
DELETE /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | 보안 규칙 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.
