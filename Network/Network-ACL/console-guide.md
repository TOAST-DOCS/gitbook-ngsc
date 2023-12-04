## Network > Network ACL > 콘솔 사용 가이드

## ACL
ACL 기능에 대한 자세한 사항은 [Network ACL](/Network/Network%20ACL/ko/overview/) 문서를 참고합니다.

#### ACL 생성
ACL을 생성하려면 **ACL 생성** 버튼을 클릭하고 다음 값을 입력합니다.

* 이름: ACL의 이름을 입력합니다.
* 설명: ACL의 설명을 입력합니다.

**확인**을 클릭하면 ACL이 생성됩니다.

#### ACL 변경
ACL의 속성 중 이름과 설명을 변경할 수 있습니다.

#### ACL 삭제
선택한 ACL을 삭제할 수 있습니다.
ACL에 바인딩된 네트워크가 없을 때만 삭제할 수 있습니다.

#### ACL Rule 추가
ACL을 선택하면 하단에 **ACL Rule** 메뉴가 나타납니다.
ACL Rule을 추가하면, 이 ACL을 사용하는 모든 네트워크에 추가된 규칙(rule)이 반영됩니다.

* 프로토콜: TCP, UDP, ICMP 등의 프로토콜을 선택합니다.
* src cidr: src IP 혹은 대역을 입력합니다.
* src port: src port 혹은 범위를 입력합니다.
* dst cidr: dst IP 혹은 대역을 입력합니다.
* dst port: dst port 혹은 범위를 입력합니다.
* 순서: 적용 순서를 입력합니다. 해당 규칙이 적용되는 우선순위를 의미합니다. 값이 작은 것부터 먼저 적용됩니다.
* 적용 방법: 규칙이 허용인지 차단인지를 입력합니다.
* 설명: ACL Rule의 설명을 입력합니다.

**확인**을 클릭하면 ACL Rule이 생성됩니다.

> [참고] src와 dst 짝으로 설정하기
>
> 예를 들어 192.168.0.10 주소에서만 192.168.0.13 주소의 tcp 22번으로 접속하게 하려면, 다음과 같이 규칙(rule)을 추가합니다.
> "protocol"="tcp", "src cidr"="192.168.0.10/32", "dst cidr"="192.168.0.13/32", "dst_port_range_min"=22, "policy"="allow"
> "protocol"="tcp", "src cidr"="192.168.0.13/32", "src_port_range_min"=22, "dst cidr"="192.168.0.10/32", "policy"="allow"
> 해당 주소로 패킷이 들어왔다 나가야 하는데, 첫 번째 규칙만 있을 경우 해당 인스턴스 입장에서는 dst는 들어오는 것만 허용하는 것이고, 두 번째 규칙이 있어야 src로부터 나가는 것이 허용되기 때문입니다.

> [참고] src와 dst 주소
>
> 예를 들어 NHN Cloud 인스턴스에서 고정 IP 192.168.0.10에 플로팅 IP 133.186.237.10을 연결하면, 같은 VPC에서의 접근이 목적인 경우 
> ACL Rule은 고정 IP 192.168.0.10을 주소로 설정하는 것이 편리합니다.
> 192.168.0.10(fip:133.186.237.10)과  192.168.0.20(fip:133.186.237.20) 두 개의 인스턴스 간에 10번에서 20번으로 80번 포트 접속을 허용하는 규칙(rule)을 설정한다고 가정하면
> 
> 고정 IP로 설정할 경우, 다음과 같이 설정하고 192.168.0.10에서 curl http://192.168.0.20처럼 고정 IP로 접근하면 됩니다.
> "protocol"="tcp", "src cidr"="192.168.0.10/32", "dst cidr"="192.168.0.20/32", "dst_port_range_min"=80, "policy"="allow"
> "protocol"="tcp", "src cidr"="192.168.0.20/32", "src_port_range_min"=80, "dst cidr"="192.168.0.10/32", "policy"="allow"
> 
> 플로팅 IP로 설정할 경우, 다음과 같이 설정하고 133.186.237.10에서 curl http://133.186.237.20처럼 플로팅 IP로 접근하면 됩니다.
> "protocol"="tcp", "src cidr"="133.186.237.10/32", "dst cidr"="192.168.0.20/32", "dst_port_range_min"=80, "policy"="allow"
> "protocol"="tcp", "src cidr"="192.168.0.20/32", "src_port_range_min"=80, "dst cidr"="133.186.0.10/32", "policy"="allow"
> "protocol"="tcp", "src cidr"="192.168.0.10/32", "dst cidr"="133.186.237.20/32", "dst_port_range_min"=80, "policy"="allow"
> "protocol"="tcp", "src cidr"="133.186.237.20/32", "src_port_range_min"=80, "dst cidr"="192.168.0.10/32", "policy"="allow"
>
> 다른 VPC로부터의 접근일 경우
>
> 133.186.237.30(VPC1)로부터 192.168.0.40(fip:133.186.237.40, VPC2)으로 80번 접속을 허용하려면
> VPC2와 바인딩된 ACL Rule에 다음과 같이 설정하면 됩니다. (VPC1은 ACL Rule 설정없음)
> "protocol"="tcp", "src cidr"="133.186.237.30/32", "dst cidr"="192.168.0.40/32", "dst_port_range_min"=80, "policy"="allow"
> "protocol"="tcp", "src cidr"="192.168.0.40/32", "src_port_range_min"=80, "dst cidr"="133.186.237.30/32", "policy"="allow"

> [참고] ACL Rule order
>
> ACL 생성 시 기본적으로 order number 101번에 모두 허용하는 규칙이 적용되고, order number 32765에 모두 거부하는 규칙이 적용됩니다.
> 따라서 기본값(default) 상태는 모든 것이 허용되는 상태가 되고, 이후 사용자가 101번을 지우고 선택적으로 규칙을 추가할 수 있습니다. 기본 사용 방식은 화이트리스트를 관리하는 방식입니다.
> 각 ACL의 마지막 규칙은 32765의 order number를 가지고 있고 정책은 모두 거부이며, 삭제가 불가능한 것입니다.
> order number 101번부터 32765번 사이의 값을 사용해서 원하는 규칙을 추가할 수 있습니다.
> 1~100번은 내부 시스템용으로 예약되어 있습니다.
> order를 변경하려면 수정하는 것은 안 되고 삭제 후 다시 넣어야 해서 규칙을 설정할 때 10~100 정도의 order number 간격을 두고 설정하는 것이 좋습니다.
> 블랙리스트를 관리하는 방식으로 바꾸려면, order number 32764에 모든 것을 허용하는 규칙을 추가하고, 앞쪽 order number에 블랙리스트 규칙을 관리하면 됩니다.

> [참고] ACL과 ACL Rule 개수
>
> 프로젝트별 ACL을 최대 10개까지 생성할 수 있습니다. 
> 프로젝트별 ACL Rule을 최대 100개까지 생성할 수 있습니다.
#### ACL Rule 변경
규칙의 속성 중 설명만 변경할 수 있습니다.

#### ACL Rule 삭제
ACL을 선택하면 하단에 **ACL Rule** 메뉴가 나타납니다.
ACL Rule을 삭제하면, 이 ACL을 사용하는 모든 네트워크에서 해당 규칙이 삭제됩니다.

#### ACL 바인딩
ACL을 적용할 네트워크를 선택하고 **확인**을 클릭합니다.
하나의 ACL은 여러 네트워크에 바인딩할 수 있습니다.
하나의 네트워크에는 하나의 ACL만 바인딩할 수 있습니다.

#### ACL 바인딩 해제
ACL 적용을 해제할 네트워크를 선택하고 **확인**을 클릭합니다.

