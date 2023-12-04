
## CloudTrail > API 가이드

> CloudTrail에 RESTful API를 호출해, 사용자가 설정한 조건에 맞는 이벤트를 조회할 수 있습니다.

## URL & Appkey
RESTful API를 사용하려면 AppKey가 필요합니다.
[CONSOLE]의 우측 상단에서 발급된 Key 정보를 확인할 수 있습니다.
![[그림 1] AppKey & SecretKey 확인](http://static.toastoven.net/prod_cloudtrail/cloudtrail_20190924.png)
<center>[그림 1] AppKey 확인</center>

## RESTful API 가이드

### Common Response Body

모든 API 요청에 대해 HTTP 응답 코드는 200입니다.
자세한 응답 결과는 Response Body의 header 항목을 참고합니다.

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|header|	Object|	응답 헤더|
|header.isSuccessful|	boolean|	성공 여부|
|header.resultCode|	int|	응답 코드. 성공 시 0, 실패 시 오류 코드 반환|
|header.resultMessage|	String|	응답 메시지. 성공 시 "SUCCESS", 실패 시 오류 메시지 반환|

### 1. Event 조회
* 발생한 이벤트를 조회합니다. 
* 이벤트 조회 시, 사용자가 설정한 검색 조건으로 조회됩니다.
* Request Body에 이 검색 조건을 포함해야 합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/cloud-trail/v1.0/appkeys/{appKey}/events/search|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]에서 발급받은 AppKey|

**[Request Body]**

```json
{
    "idNo" : "string",
    "member" : {
      "memberType" : "string",    /* TOAST, IAM 중 선택 */
      "userCode" : "string",      /* IAM member type일 경우 작성 */
      "emailAddress" : "string",   /* TOAST member type일 경우 작성 */
      "idNo" : "string" 
    },
    "eventId" : "string",
    "startDate": "2019-09-01T02:00:00.000Z",
    "endDate": "2019-09-12T03:13:00.000Z",
    "page": {
       "sortBy": "string",
       "limit": 20,
       "page": 0
    }
} 
```
* member에 값을 지정하지 않으면 전체 이벤트 목록을 조회할 수 있습니다.
* NHN Cloud memberType일 경우, emailAddress의 값은 필수이고, userCode의 값은 존재하지 않아야 합니다. 
* 반대로 IAM memberType일 경우, userCode의 값은 필수이고, emailAddress의 값은 존재하지 않아야 합니다.
* idNo 값이 있을 경우, memberType과 userCode, emailAddress 값과 관계없이 우선으로 적용됩니다.
* 이벤트 ID에 대한 자세한 정보는 매뉴얼을 참고합니다 : [링크](/CloudTrail/ko/event-list-ngsc/)

| Key | Type | Required  | Description |
| --- | --- | --- | --- |
| idNo | String | X | 이벤트를 발생시킨 회원 아이디 번호(uuid) |
| member | Object | X | 이벤트를 발생시킨 회원 |
| member.memberType | String | X | 이벤트를 발생시킨 회원의 타입(NHN Cloud, IAM) |
| member.userCode | String | X | 이벤트를 발생시킨 회원의 userCode(IAM 회원일 경우) |
| member.emailAddress | String | X | 이벤트를 발생시킨 회원의 이메일 주소(NHN Cloud 회원일 경우)|
| member.idNo | String | X | 이벤트를 발생시킨 회원 IdNo (uuid) |
| eventId | String | O | 조회할 이벤트의 ID |
| startDate | Date | O | 조회할 기간의 시작 날짜 |정
| endDate | Date |O  | 조회할 기간의 끝나는 날짜 |
| page | Object | O | 조회 결과의 페이지 조건 |
| page.sortBy | String | X | 조회 결과의 사이즈 정렬 조건 (ex. startDate:desc, idNo:asc)|
| page.limit | Integer | O | 조회 결과의 사이즈 조건 |
| page.page | Integer | O | 조회 결과의 페이지 중 조회할 페이지 조건 |


**[Response Body]**
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "page": {
        "content": [
            {
                "eventTime": "2019-11-01T10:31:49.348+0000",
                "userIdNo": "String",
                "userIp":"10.00.00.000",
                "userAgent":"ReactorNetty/0.8.4.RELEASE",
                "userName": "홍길동",
                "userId": "test_email@nhn.com",
                "eventSourceType": "API",
                "productId": "M0XnzOFE",
                "region": "String",
                "orgId": "String",
                "projectId": "String",
                "projectName": "String",
                "appKey": "String",
                "tenantId": "String",
                "eventId": "event_id.iam.member.role.update",
                "request": "{\n\t\"id\" : \"2\",\n\t\"productId\" : \"M0XnzOFE\",\n\t\"uuid\" : \"String\"\n\t\n}",
                "response": "{\"header\":{\"resultCode\":0,\"resultMessage\":\"SUCCESS\",\"isSuccessful\":true}}",
                "eventTarget": {
                    "targetMembers": [
                        {
                            "idNo": "String",
                            "name": "임꺽정",
                            "userCode": "test_user",
                            "emailAddress": "test_email2@nhn.com"
                        }
                    ]
                }
            }
        ]
    }
}
```
| Key | Type | Description |
| --- | --- | --- |
| eventTime | Date | 이벤트 발생 시간 |
| userIdNo | Object | 이벤트를 발생시킨 회원의 uuid |
| userName | String |  이벤트를 발생시킨 회원의 이름|
| UserId | String | 이벤트를 발생시킨 회원의 ID (NHN Cloud 계정일 경우, 이메일 형식) |
| userIp | String | 이벤트를 발생시킨 회원의 IP |
| userAgent | String | 이벤트를 발생시킨 회원의 에이전트 |
| eventSourceType | String | 이벤트를 발생시킨 주체의 타입 |
| productId | String | 이벤트가 발생한 상품 ID |
| region | String | 이벤트가 발생한 리전 |
| orgId | String | 이벤트가 발생한 조직 ID |
| projectId | String | 이벤트가 발생한 프로젝트 ID |
| projectName | String | 이벤트가 발생한 프로젝트 이름 |
| appKey | String | 이벤트가 발생한 앱키 |
| tenantId | String | 이벤트가 발생한 테넌트 ID |
| eventId | String | 이벤트 ID |
| request | String | 발생한 이벤트의 요청 내용 |
| response | String | 발생한 이벤트의 응답 내용 |
| eventTarget | Object | 발생한 이벤트의 대상 |
| eventTarget.targetMembers | Object | 발생한 이벤트의 대상 회원 |
| targetMembers.idNo | String | 발생한 이벤트의 대상 회원의 uuid |
| targetMembers.name | String | 발생한 이벤트의 대상 회원의 이름 |
| targetMembers.userCode | Integer | 발생한 이벤트의 대상 회원의 ID (IAM 회원일 경우) |
| targetMembers.emailAddress | String | 발생한 이벤트의 대상 회원의 이메일 주소 (NHN Cloud 회원일 경우) |
