---

copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Cloud Foundry 앱 작성
{: #creating_cloud_foundry_apps}

{{site.data.keyword.Bluemix}}를 사용하면 {{site.data.keyword.Bluemix_notm}} 콘솔에서 앱을 작성할 수 있습니다. 그런 다음 콘솔 또는 cf 명령 인터페이스를 계속 사용하거나 {{site.data.keyword.jazzhub_title}}를 사용하여 앱을 개발하고 추적하고 계획하고 배치할 수 있습니다.
{:shortdesc}

{{site.data.keyword.Bluemix_notm}}에서 앱을 작성할 때 스타터로 시작할 수 있습니다. *스타터*는 특정 빌드팩으로 구성되는 미리 정의된 서비스와 애플리케이션 코드를 포함하는 템플리트입니다. 표준 유형 및 런타임이라는 두 가지 유형의 스타터가 있습니다.

*표준 유형*은 특정 도메인에 대한 애플리케이션 및 그 연관된 런타임 환경과 사전 정의된 서비스의 컨테이너입니다. 예를 들어, 모바일 클라우드 표준 유형에는 Mobile Data, Mobile Application Security 및 푸시 서비스 외에 Node.js 런타임이 포함되어 있습니다. 또한 이러한 서비스에 액세스하는 모바일 앱 개발을 시작하기 위한 SDK 및 샘플 애플리케이션을 포함합니다. 

*런타임*은 애플리케이션을 실행하는 데 사용되는 리소스 세트입니다. {{site.data.keyword.Bluemix_notm}}에서는 다양한 유형의 애플리케이션을 위한 컨테이너로 런타임 환경을 제공합니다. 런타임 환경은 빌드팩으로 {{site.data.keyword.Bluemix_notm}}에 통합되며 사용 가능하도록 자동으로 구성되고 유지보수에 거의 노력이 필요하지 않습니다.

애플리케이션 작성을 시작하려면 다음 단계를 수행하십시오. 
  1. IBM Cloud 도구 모음에서 **카탈로그**를 클릭하십시오.
  2. **Cloud Foundry 앱**을 클릭하고 런타임을 선택하십시오. 유도된 체험에 따라 이름을 지정하고 코딩 방법을 선택하십시오. **작성**을 클릭하십시오.
  3. 유도된 체험을 완료하면 **개요**를 클릭하십시오.
  5. 대시보드의 개요 앱에서 **연결 작성**을 클릭하여 앱에 서비스를 추가할 수 있습니다. 카탈로그에서 서비스를 찾아보고 선택하거나 카탈로그의 끝으로 스크롤한 후에 **{{site.data.keyword.Bluemix_notm}} 시범 서비스**를 클릭하여 시범 서비스를 찾아보십시오. 또는 cf 명령행 인터페이스를 사용할 수 있습니다. 앱으로 작업하기 위한 옵션을 참조하십시오.
  6. 개요 페이지에서 "Continuous Delivery" 카드로 스크롤하고 **도구 체인 보기**를 클릭하십시오. 앱의 소스가 Bluemix에서 호스팅되는 저장소에 저장됩니다. 해당 저장소와 딜리버리 파이프라인을 사용하여 앱을 개발하고 배치하는 열린 도구 체인도 작성됩니다. Continuous Delivery 서비스에 대한 자세한 정보는 <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Continuous Delivery 시작하기</a>를 참조하십시오. 

**참고:** 앱에 바인딩하는 서비스가 충돌하는 경우 앱이 실행을 중지하거나 오류가 발생할 수 있습니다. {{site.data.keyword.Bluemix_notm}}에서는 이러한 문제점에서 복구하기 위해 앱을 자동으로 다시 시작하지 않습니다. 가동 중단, 예외 및 연결 실패를 식별하고 복구하도록 앱을 코딩하는 방법을 고려해 보십시오. 자세한 정보는 앱이 자동으로 다시 시작되지 않음 문제점 해결 주제를 참조하십시오.

## 앱으로 작업하기 위한 옵션

앱이 작성된 후에 앱에 계속 서비스를 추가하고 앱을 빌드 및 배치할 수 있는 몇 가지 옵션이 있습니다.

<dl><dt>cf 명령행 인터페이스</dt>
<dd>애플리케이션을 업데이트하거나 서비스 인스턴스를 작성하거나 서비스를 애플리케이션에 바인드하려면 <a href="https://github.com/cloudfoundry/cli#getting-started">cf 명령 인터페이스</a>를 사용하십시오. 또한 cloud-cli 명령행 인터페이스를 사용하는 방법으로도 서비스 오퍼링을 작성, 업데이트 및 삭제할 수 있습니다.</dd>
<dt>{{site.data.keyword.Bluemix_notm}} 사용자 인터페이스</dt>
<dd>{{site.data.keyword.Bluemix_notm}} <a href="https://console.bluemix.net/dashboard/apps">사용자 인터페이스</a>를 사용하여 비즈니스 문제점을 해결하기 위해 결합할 서비스 및 런타임을 선택하는 등 애플리케이션을 빌드하십시오.</dd>
<dt>{{site.data.keyword.contdelivery_full}}</dt>
<dd>{{site.data.keyword.contdelivery_short}}를 사용하여 빌드, 단위 테스트, 배치 등을 자동화합니다. 풍부한 웹 기반 IDE을 통해 코드를 편집하고 푸시하십시오. 개발, 배치 및 오퍼레이션 태스크를 지원하는 도구 통합을 사용할 수 있도록 도구 체인을 작성합니다.
Continuous Delivery 서비스에는 Delivery Pipeline, Eclipse Orion Web IDE, Git 저장소 및 문제 추적이 포함됩니다. 자세한 정보는 <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Continuous Delivery 시작하기</a>를 참조하십시오.</dd>
</dl>

## 팁

웹 앱 개발 시 다음 팁을 사용하십시오.

<dl><dt>지속성</dt>
<dd>애플리케이션에 대해 로컬 스토리지를 지정하지 마십시오. 애플리케이션의 각 인스턴스는 단 하나의 인스턴스만 실행 중인 경우에도 로드 밸런스를 조정하기 위해 언제든지 다른 가상 머신으로 이동되거나 다시 시작될 수 있습니다. 애플리케이션이 이동되거나 삭제될 때 로컬 스토리지에 저장된 내용도 모두 지워집니다. 지속성을 유지할 수 있도록 {{site.data.keyword.Bluemix_notm}} 데이터 저장소 서비스 중 하나를 사용하십시오. </dd>
<dt>리소스 한계</dt>
<dd>평가판 계정이 사용할 수 있는 리소스의 양에는 한계가 있음을 유념하십시오. 한계는 다음과 같습니다.
<table style="width:100%">
<caption>표 1. 평가판 계정에 대한 {{site.data.keyword.Bluemix_notm}} 리소스 한계</caption>
  <th>리소스 유형</th>	<th>수량 한계</th>
<tr><td>모든 앱에 걸쳐 사용되는 서비스의 수</td> <td>10</td>
<tr><td>모든 앱에 걸쳐 사용되는 메모리</td> <td>	2G</td>
<tr><td>라우트 수</td> <td>500</td>
</table>
</dd>
</dl>
