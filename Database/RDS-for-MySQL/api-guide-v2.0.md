## Database > RDS for MySQL > API 가이드

| 리전        | 엔드포인트                                     |
|-----------|-------------------------------------------|
| 한국(판교) 리전 | https://kr1-mysql.api.nhncloudservice.com |
| 한국(평촌) 리전 | https://kr2-mysql.api.nhncloudservice.com |
| 일본 리전     | https://jp1-mysql.api.nhncloudservice.com |

## Monitoring

### Metric 조회

- 통계 정보 조회에 필요한 통계 항목(metric)을 조회합니다.

```
GET /rds/api/v2.0/metrics
```

#### 요청 헤더

| 이름           | 종류  | 형식     | 필수 | 설명                          |
|--------------|-----|--------|----|-----------------------------|
| X-TC-APP-KEY | URL | String | O  | 상품 Appkey 또는 프로젝트 통합 Appkey |

#### 응답

```json
{
    "metrics": [
        {
            "measureName": "CPU_USAGE",
            "unit": "%"
        },
        {
            "measureName": "NETWORK_SENT",
            "unit": "Bytes/min"
        }
    ]
}
```

### 통계 정보 조회

- 일정 주기마다 수집된 통계 정보들을 조회합니다.

```
GET /rds/api/v2.0/metric-statistics
```

#### 요청 헤더

| 이름           | 종류  | 형식     | 필수 | 설명                          |
|--------------|-----|--------|----|-----------------------------|
| X-TC-APP-KEY | URL | String | O  | 상품 Appkey 또는 프로젝트 통합 Appkey |

#### 요청

| 이름          | 종류    | 형식       | 필수 | 설명               | 제약 사항                                       |
|-------------|-------|----------|----|------------------|---------------------------------------------|
| instanceId  | Query | Array    | O  | DB 인스턴스 ID 목록    | Min:1, Max: 20                              |
| measureName | Query | Array    | O  | 조회 지표(metric) 목록 | Min:1                                       |
| from        | Query | Datetime | O  | 시작 일시            | yyyy-MM-dd'T'HH:mm:ss.SSSXXX (ISO Datetime) |
| to          | Query | Datetime | O  | 종료 일시            | yyyy-MM-dd'T'HH:mm:ss.SSSXXX (ISO Datetime) |
| interval    | Query | Integer  | X  | 조회 간격            | 1, 5, 30, 120, 1440 (분)                     |

- interval: 기본값 사용 시 from/to 값에 따라 적절한 값을 자동 선택함
    - 날짜 범위가 1일 이하 and 시작 날짜가 8일 경과 전 - 1분 단위 raw 데이터
    - 날짜 범위가 7일 이하 and 시작 날짜가 40일 경과 전 - 5분 단위 평균 데이터
    - 날짜 범위가 30일 이하 and 시작 날짜가 186일 경과 전 - 30분 단위 평균 데이터
    - 날짜 범위가 180일 이하 and 시작 날짜가 730일 경과 전 - 2시간 단위 평균 데이터
    - 그 외 - 1일 단위 평균 데이터
- from, to: ISO Datetime 형식 예시
    - UTC: 2021-01-01T00:00:00.000Z
    - KST, JST: 2021-01-01T00:00:00.000+09:00

#### 응답

```json
{
    "metricStatistics": [
        {
            "instanceId": "9a978085-0dc4-4da6-974c-bc6822b06a7c",
            "metrics": [
                {
                    "measureName": "NETWORK_RECV",
                    "unit": "Bytes/min",
                    "values": [
                        [
                            1623817800,
                            "3949.0200000000004"
                        ],
                        [
                            1623819600,
                            "3951.3122222222228"
                        ],
                        [
                            1623821400,
                            "3955.8588888888894"
                        ]
                    ]
                },
                {
                    "measureName": "NETWORK_SENT",
                    "unit": "Bytes/min",
                    "values": [
                        [
                            1623817800,
                            "4356.027777777778"
                        ],
                        [
                            1623819600,
                            "4261.1322222222225"
                        ],
                        [
                            1623821400,
                            "4312.244444444445"
                        ]
                    ]
                }
            ]
        }
    ]
}
```
