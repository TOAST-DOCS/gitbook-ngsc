## Compute > System Monitoring > Advanced Monitoring(OpenMetrics) > 개요
**Compute > System Monitoring > Advanced Monitoring(OpenMetrics)** 서비스는 사용자의 인스턴스에서 동작하는 다양한 플랫폼과 애플리케이션을 모니터링할 수 있는 확장 모니터링 도구입니다.
기본적인 시스템 지표 외에도 사용자의 상황에 필요한 여러 지표를 수집할 수 있는 방법을 소개합니다.

## Prometheus 소개
* [Prometheus](https://prometheus.io/)는 CNCF(Cloud Native Computing Foundation)의 프로젝트 중 하나로, 시스템과 서비스를 모니터링하는 데 사용하는 도구입니다.
* Prometheus에 모니터링 대상을 HTTP URL로 지정하면 Prometheus는 이 경로로 요청을 보내고 응답값을 기초로 모니터링 정보를 수집합니다.
* 아래의 방법들을 통해 출력되는 [Prometheus 설명 형식](https://prometheus.io/docs/instrumenting/exposition_formats/)의 지표 데이터를 수집할 수 있습니다.
    * 지표 추출 도구(Exporter)를 설치하거나 시스템이나 플랫폼 자체적으로 제공하는 도구를 사용하는 방법
        * [Third-party exporters](https://prometheus.io/docs/instrumenting/exporters/#third-party-exporters)
        * [Software exposing Prometheus metrics](https://prometheus.io/docs/instrumenting/exporters/#software-exposing-prometheus-metrics)
    * 형식에 맞게 지표를 출력해주는 프로그램을 직접 작성하여 원하는 지표를 내보내는 방법
        * [Client Libraries](https://prometheus.io/docs/instrumenting/clientlibs/#client-libraries)

## OpenMetrics 소개
* [OpenMetrics](https://github.com/OpenObservability/OpenMetrics/blob/master/OpenMetrics.md)는 Prometheus 설명 형식과 이를 기반으로 한 통합 시스템들에서 사용하기 위해 추진되고 있는 표준화 작업입니다.
* System Monitoring에서도 OpenMetrics 기반의 모니터링 도구를 제공합니다.
* 일반적으로 Prometheus를 구축하여 모니터링하려면 추가적인 자원 소모와 네트워크 구성 관리가 필요합니다.
    * ![Prometheus](https://static.toastoven.net/prod_system_monitoring/console_guide/open-metrics-overview-1.png)
* System Monitoring에서는 각 인스턴스 내에서 지표를 추출할 경로만 준비되면, 에이전트가 대신 지표를 수집하고 사용자는 별도로 서비스를 구축하지 않고 모니터링 도구를 사용할 수 있습니다.
    * ![System Monitoring Advanced Monitoring(OpenMetrics)](https://static.toastoven.net/prod_system_monitoring/console_guide/open-metrics-overview-2.png)

## 제공 기능
### OpenMetrics 기반의 커스텀 지표 수집 및 대시보드 제공
**Compute > Instance**에서 생성한 서버 인스턴스에서 모니터링할 대상 시스템의 Exporter를 설치하고 실행하면 그 시스템의 지표 정보를 수집할 수 있습니다.
Exporter가 준비되면 System Monitoring 서비스를 통해 모니터링 기능을 활성화하고 수집된 지표를 조회할 수 있습니다. 시스템 지표와 마찬가지로 차트를 생성하고 원하는 레이아웃으로 배치할 수 있으며, 레이아웃을 여러 개 생성해 목적에 따라 관리할 수 있습니다. 또한 서버 기본 모니터링과 유사하게, 관찰 대상 지표와 임계치를 지정하여 서비스의 이상 징후를 파악할 수 있습니다. 설정한 감시 조건을 충족하는 상황이 발생하면 메일 또는 SMS로 사용자들에게 메시지를 전달할 수 있습니다.


### 지표 수집 주기 및 보관 기간
지표는 1분 단위로 수집되며, 최대 5년간 보관됩니다. 지표 데이터는 5분, 30분, 2시간, 1일 단위로 집계됩니다. 집계 단위별로 보장하는 보관 기간은 아래와 같습니다.

집계 단위|보관 기간
---|---
1분|1년

> 최대 보관 기간은 사정에 따라 변경될 수 있으며, 변경되는 경우 별도로 회원에게 고지합니다.