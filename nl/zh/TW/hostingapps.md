---



copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# 在 {{site.data.keyword.Bluemix_notm}} 中管理應用程式
{: #hosting_apps}

使用 {{site.data.keyword.Bluemix}}，您可以建立應用程式，以及管理現有應用程式。只要應用程式具有雲端功能，就可以將應用程式移至 {{site.data.keyword.Bluemix_notm}}。{{site.data.keyword.Bluemix_notm}} 提供許多讓您執行應用程式的方法，例如，Cloud Foundry、IBM Containers 及 Virtual Machines。

## 使應用程式具有雲端功能
{: #cloud-readyapps}

如果在應用程式中觀察到下列所有原則，即表示應用程式具有雲端功能，而且可以移轉至 {{site.data.keyword.Bluemix_notm}}。如果應用程式違反原則，則通常可以修改應用程式以遵守原則。

* 不要直接將應用程式撰寫為特定拓蹼。

  在非雲端環境中，應用程式可能會使用特定部署拓蹼。不過，雲端應用程式中的應用程式拓蹼可能會變更，因為具有雲端功能的應用程式及服務容許立即的可擴充性變更。可擴充性變更包括動態擴充及手動重新調整應用程式的實例數量。

  盡可能將應用程式建置為通用且無狀態，以避免應用程式受到可擴充性變更的影響。

* 不要假設本端檔案系統是永久性的。

  因為隨時都可能在雲端上移動、刪除或複製應用程式實例，所以請不要依賴寫入至檔案系統的檔案。如果應用程式使用本端檔案系統作為常用資訊（包括應用程式日誌）的快取，則當實例關閉並在不同的位置或不同的 VM 重新啟動時，資訊便會遺失。

  您可以將資訊儲存至服務（例如 SQL 或 NoSQL 資料庫），而非本端檔案系統。在動態雲端環境中，也一定要讓日誌存在於存活時間比產生日誌的應用程式實例還要久的服務上。

* 不要在應用程式中儲存階段作業狀態。

  系統狀態是由資料庫及共用儲存空間所定義，而非由每一個個別執行中應用程式實例所定義。任何種類的有狀態性都會限制應用程式的可擴充性。請嘗試將階段作業狀態儲存在伺服器上的集中位置，讓階段作業狀態的影響降到最低。

  如果您無法完全刪除階段作業狀態，請將它推送到應用程式伺服器以外的高可用性儲存庫。儲存庫包括 IBM WebSphere Extreme Scale、Redis 或 Memcached，或是外部資料庫。

* 不要使用特定基礎架構相依關係。

  這是具有數種呈現的一般原則。例如，請不要假設應用程式所使用的服務已配置特定主機名稱或 IP 位址。因為可能在雲端環境中重新定位或重新產生服務，所以主機名稱及 IP 位址也可能會變更。

  將環境特有相依關係擷取成一組內容檔，可以有所改善，但仍然不足。最佳作法是使用外部服務登錄來解析服務端點，或將整個遞送功能委派給具有虛擬名稱的服務匯流排或負載平衡器。

* 不要在應用程式中使用基礎架構 API。

  如果您在應用程式中依賴特定的基礎架構 API，則變更基礎架構會更具挑戰性，因為基礎架構 API 可能參照軟體堆疊中的許多不同層。

  您可以改為依賴現有的開放程式碼或商業產品，並將 PaaS 解決方案保留在 PaaS 層，讓它們獨立在您的應用程式碼之外。

* 不要使用模糊通訊協定。

  請不要使用需要額外配置的模糊通訊協定進行備援。

  根據標準通訊協定的應用程式在配置項目委派給平台時會較具復原力。標準通訊協定包括 HTTP、SSL、標準資料庫、佇列作業及 Web 服務連線。

* 不要依賴 OS 特有特性。

  如果您已使用 OS 特有的特性，則可以使用相容性程式庫（例如 Cygwin 及 Mono）來修正此問題。Cygwin 是一個相容性程式庫，可在 Windows 環境中提供一組 Linux 工具。Mono 是一個相容性程式庫，可在 Linux 中提供 Windows .NET 功能。

  請避免 OS 特有的相依關係；請改用中介軟體基礎架構或服務提供者所提供的服務。

* 不要手動安裝應用程式。

  您的應用程式可能會經常要視需求安裝在動態雲端環境上。安裝處理程序必須 Script 化且可靠，而且必須將配置資料與 Script 分開 (externalize)。

  最起碼要將應用程式安裝擷取為一組統一且與作業系統無關的 Script。請維持應用程式安裝短小精幹並且可攜，以適合不同的自動化技術。此外，也請將應用程式安裝所需的相依關係降到最少。

如需具有雲端功能的應用程式相關資訊，請參閱 [The 12-factor app ![外部鏈結圖示](../icons/launch-glyph.svg)](http://12factor.net/){: new_window}。

##移轉應用程式
{: #ht_hostapp}

您可以漸進式地將應用程式移轉至 {{site.data.keyword.Bluemix_notm}}，而不要將應用程式全面轉移至雲端環境。您可以先移轉應用程式的某個部分，並使用 Cloud Integration 服務以連接至現有資料或記錄系統。

在您的雲端應用程式中，您可能需要存取後端資料或服務（例如，記錄系統）。在 {{site.data.keyword.Bluemix_notm}} 中，您可以使用 Secure Gateway 服務以在 {{site.data.keyword.Bluemix_notm}} 組織與企業後端網路之間建立安全通道。服務可讓 {{site.data.keyword.Bluemix_notm}} 上的應用程式存取後端網路的資料及服務。如需詳細資料，請參閱 [Reaching enterprise backend with Bluemix Secure Gateway via console ![外部鏈結圖示](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window}。

若要將應用程式部署至 {{site.data.keyword.Bluemix_notm}} 以作為 Cloud Foundry 應用程式，請從 {{site.data.keyword.Bluemix_notm}}「型錄」中選取運行環境。此運行環境包含入門範本 Hello World 應用程式，您可以將它取代為自己的應用程式。如果您找不到提供您想要之運行環境的入門範本，則可以使用 -b 選項與 cf push 指令搭配，將自訂 Cloud Foundry 相容建置套件帶到 {{site.data.keyword.Bluemix_notm}}。如需詳細資料，請參閱[使用社群建置套件](/docs/cfapps/byob.html)。

您可以使用 {{site.data.keyword.Bluemix_notm}} 所提供的下列工具及服務：

| 工具| 方法|
|:------|:--------|
| Cloud Foundry 指令行介面 (cf cli)| 在本端用戶端上管理您的程式碼，並且使用 Cloud Foundry 指令行介面，手動將應用程式推送至 {{site.data.keyword.Bluemix_notm}}。如需相關資訊，請參閱[上傳應用程式](/docs/starters/upload_app.html)。|
| Eclipse| 在 Eclipse 中管理您的程式碼，並且使用 IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} 來推送應用程式。|
| Git 整合| 在 GitHub 上管理程式碼，並且 將 Git 整合至 {{site.data.keyword.Bluemix_notm}}。您可以與其他開發人員分工合作。確定程式碼中的變更時，會自動將您的應用程式部署至 {{site.data.keyword.Bluemix_notm}}。您不需要手動推送應用程式。|
| {{site.data.keyword.Bluemix_notm}} DevOps Delivery Pipeline | 在 DevOps GitHub 儲存庫上管理程式碼，並且使用 DevOps Delivery Pipeline 將應用程式部署至 {{site.data.keyword.Bluemix_notm}}。|
{: caption="表 1. {{site.data.keyword.Bluemix_notm}} 工具" caption-side="top"}

如果 Cloud Foundry 平台不支援您的應用程式需求，您可以使用容器或 VM，在其中利用其他自訂的選項來設定、配置及維護運行環境。

## 在 Continuous Delivery 中使用工具鏈開發及部署應用程式
{:ht_cd}

將[工具鏈新增至應用程式](/docs/services/ContinuousDelivery/toolchains_working.html#creating_a_toolchain_from_an_app)，然後使用 [Continuous Delivery 工具鏈使用者介面](/docs/services/ContinuousDelivery/toolchains_using.html#toolchains-using)來開發及部署應用程式。

## 使用 cf cli 上傳應用程式
{: #ht_cfcli}

您可以在本端用戶端上管理您的程式碼，並且使用 Cloud Foundry 指令行介面，手動將應用程式上傳至 {{site.data.keyword.Bluemix_notm}}。如果您變更程式碼，則必須重新將應用程式推送至 {{site.data.keyword.Bluemix_notm}}，以執行更新過的程式碼。

請採取下列步驟來移轉應用程式：

安裝 Cloud Foundry 指令行介面。請確定使用最新版本的 cf 指令行介面。
  1. 下載適用於您作業系統的安裝程式。</li>
  2. 遵循工具精靈以安裝指令行。</li>
  3. 使用下列指令來驗證 cf 指令行介面的版本：`cf -v`

選用項目：如果您要先指定並儲存部署詳細資料，再將應用程式推送至 {{site.data.keyword.Bluemix_notm}}，則可以採取下列步驟來新增應用程式資訊清單：
  1. 移至應用程式的工作目錄，並建立標題為 manifest.yml（此為預設名稱）的檔案。</li>
  2. 在資訊清單檔中，指定部署詳細資料。下列範例顯示 Java™ 應用程式的資訊清單檔。
  ```applications:
  - disk_quota: 1024M
  host: myjavatest
  name: MyJavaTest
  path: webStarterApp.war
  domain: mybluemix.net
  instances: 1
  memory: 512M
  ```
  如需可在此檔案中使用之受支援選項的相關資訊，請參閱[應用程式資訊清單](../manageapps/depapps.html#appmanifest)。

推送應用程式。您可以使用 cf push 指令來上傳應用程式。
  1. 執行下列指令，以連接並登入 {{site.data.keyword.Bluemix_notm}}。請在提示時選取您的組織及空間。
  ```
  cf login -a https://api.ng.bluemix.net
  ```
  如果您的組織使用「單一登入」，請新增 `-sso` 旗標。

  2. 從您的應用程式目錄，輸入 cf push 指令與應用程式名稱。應用程式名稱在 {{site.data.keyword.Bluemix_notm}} 環境中必須是唯一的。
  ```
  cf push appname
  ```
  3. 選用項目：如果您使用外部建置套件，則必須使用 -b 選項與 cf push 指令搭配。例如：
  ```
  cf push appname -b buildpack_URL
  ```
  如需詳細資料，請參閱[使用社群建置套件](../cfapps/byob.html)。

選用項目：如果您變更應用程式，則必須重新輸入 cf push 指令來上傳那些變更。cf 指令行介面會使用您先前的選項以及對提示的回應，以新的程式碼片段來更新應用程式的任何執行中實例。

**附註：**

* 使用 cf push 指令時，cf 指令行介面會將所有檔案及目錄從現行目錄複製到 {{site.data.keyword.Bluemix_notm}}。確定應用程式目錄中只有所需的檔案。
* 確定您的組織有足夠的記憶體可供應用程式的所有實例使用。若要檢視您組織的記憶體配額，請使用 cf org org_name。
* 如需 cf push 的相關資訊，請參閱 [cf 指令](../cli/reference/cfcommands/index.html)。

## 移轉資料及使用服務
{: #ht_service}

將應用程式上傳至 {{site.data.keyword.Bluemix_notm}} 之後，請從 {{site.data.keyword.Bluemix_notm}} 型錄中選取您應用程式所連接的服務、建立服務實例、將實例連結至應用程式，然後重新啟動應用程式。

您應用程式的 VCAP_SERVICES 環境變數是 JSON 物件，而此物件包含如何與 {{site.data.keyword.Bluemix_notm}} 中的服務實例互動的相關資訊。此資訊包括服務實例名稱、認證，以及與服務實例的連線 URL。

若要在 {{site.data.keyword.Bluemix_notm}} 中執行程式碼，您必須新增程式碼邏輯，以剖析 VCAP_SERVICES 變數來取得服務連線的相關資訊。請修改應用程式，以透過環境變數來取得服務實例的動態指派主機及埠。下列範例顯示如何在 Ruby 應用程式中取得 Postgre SQL 服務實例的認證：

```
services = JSON.parse(ENV['VCAP_SERVICES'], :symbolize_names => true)
        url = services.values.map do |srvs|
          srvs.map do |srv|
            if srv[:credentials][:uri] =~ /^postgres/
              srv[:credentials][:uri]
            else
              []
            end
          end
        end.flatten!.first
```
{:codeblock}

若要確定在修改 {{site.data.keyword.Bluemix_notm}} 的應用程式之後可以在本端環境中執行您的應用程式，請檢查 VCAP_SERVICES 環境變數是否存在（此環境變數是針對所有 {{site.data.keyword.Bluemix_notm}} Cloud Foundry 應用程式所設定）。


# 相關鏈結
{: #rellinks}

## 相關鏈結
{: #general}

* [IBM Containers](/docs/containers/container_index.html)
* [Virtual Machines](/docs/virtualmachines/vm_index.html)
* [開始使用 Delivery Pipeline](/docs/services/DeliveryPipeline/index.html)
* [使用 IBM Eclipse Tools for Bluemix 來部署應用程式](/docs/manageapps/eclipsetools/eclipsetools.html)
* [The Twelve-Factor App ![外部鏈結圖示](../icons/launch-glyph.svg)](http://12factor.net/){: new_window}
* [Reaching enterprise backend with Bluemix Secure Gateway via console ![外部鏈結圖示](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window}
