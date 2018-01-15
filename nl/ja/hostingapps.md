---



copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# {{site.data.keyword.Bluemix_notm}} でのアプリのホスティング
{: #hosting_apps}

{{site.data.keyword.Bluemix}} を使用して、アプリを作成することや、既存のアプリをホストすることができます。クラウド対応のアプリであれば、{{site.data.keyword.Bluemix_notm}} に移動できます。 {{site.data.keyword.Bluemix_notm}} では、アプリを実行するための方法として、例えば Cloud Foundry、IBM Containers、Virtual Machines など、多数の方法が用意されています。

## アプリをクラウド対応にする
{: #cloud-readyapps}

以下のすべての基本原則がアプリ内で守られている場合、そのアプリはクラウド対応であり、{{site.data.keyword.Bluemix_notm}} にマイグレーションできます。 アプリ内でいずれかの基本原則に対する違反がある場合、通常は基本原則に従うようにアプリを変更できます。

* アプリを、直接特定のトポロジー用にコーディングしないでください。

  非クラウド環境では、アプリは特定のデプロイメント・トポロジーを使用する場合があります。 しかし、クラウド対応のアプリおよびサービスは即時のスケーラビリティーの変更を許容するため、アプリ・トポロジーはクラウド・アプリでは変わる可能性があります。 スケーラビリティーの変更には、動的なスケーリングと、手動でのアプリのインスタンス数の変更があります。

  スケーラビリティー変更によってアプリが影響を受けないように、アプリをできるだけ汎用的かつステートレスになるようビルドしてください。

* ローカル・ファイル・システムが永続することを前提としないでください。

  アプリ・インスタンスはクラウド上でいつでも移動、削除、複製される可能性があるため、ファイル・システムに書き込まれたファイルに依存しないようにしてください。 頻繁に使用される情報 (アプリ・ログなど) のキャッシュとしてアプリがローカル・ファイル・システムを使用する場合、その情報は、インスタンスがシャットダウンされ、別の場所または別の VM で再始動されると失われます。

  ローカル・ファイル・システムの代わりに、SQL または NoSQL データベースなどのサービスに情報を保管できます。 動的クラウド環境では、ログが生成されるアプリ・インスタンスよりも長く存続するサービスでログを使用可能にしておくことも重要です。

* アプリ内にセッション状態を保管しないでください。

  システムの状態は、実行中の個々のアプリ・インスタンスではなく、データベースおよび共有ストレージによって定義されます。 ステートフルであるという性質はどのような種類であっても、アプリのスケーラビリティーを制限します。 サーバー上の集中管理される場所にセッション状態を保管することによって、セッション状態の影響を最小化するようにしてください。

  セッション状態を完全に除去できない場合は、アプリ・サーバーの外部にある可用性の高い保管場所にセッション状態を格納してください。 その保管場所としては、IBM WebSphere Extreme Scale、Redis、Memcached、外部データベースなどがあります。

* 特定のインフラストラクチャー依存関係を使用しないでください。

  これは 1 つの一般原則ですが、いくつかの現れ方があります。 例えば、アプリが使用するサービスに特定のホスト名または IP アドレスが割り振られていると想定しないでください。 それらのサービスはクラウド環境では再配置されたり再生成されたりする可能性があり、ホスト名および IP アドレスも変わる可能性があります。

  環境特有の依存関係を一連のプロパティー・ファイルに抽出することは改善ではありますが、まだ不十分です。 ベスト・プラクティスは、外部サービス・レジストリーを使用してサービス・エンドポイントを解決することか、または、ルーティング機能全体を仮想名を持つ 1 つのサービス・バスまたはロード・バランサーに委任することです。

* アプリ内でインフラストラクチャー API を使用しないでください。

  インフラストラクチャー API はソフトウェア・スタック内の多くのさまざまなレイヤーを参照することがあるため、アプリ内で特定のインフラストラクチャー API に依存している場合、そのインフラストラクチャーの変更がより難しいものになります。

  代わりに、既存のオープン・ソース製品または市販製品に依存し、PaaS ソリューションは PaaS レイヤーに残したままにしてアプリ・コードの外に保持するようにできます。

* あいまいなプロトコルを使用しないでください。

  回復力のために余分な構成を必要とするようなあいまいなプロトコルを使用しないでください。

  標準プロトコルに基づくアプリのほうが、構成項目をプラットフォームに委任でき、より回復力があります。 標準プロトコルには、HTTP、SSL、標準データベース、キューイング、Web サービス接続などがあります。

* OS 固有の機能に依存しないでください。

  既に OS 固有の機能を使用している場合は、例えば Cygwin および Monoなどの互換ライブラリーを使用して、この問題を修正できます。 Cygwin は、Windows 環境で Linux ツールのセットを提供する互換ライブラリーです。 Mono は、Linux で Windows .NET 機能を提供する互換ライブラリーです。

  OS 固有の依存関係を避けて、代わりにミドルウェア・インフラストラクチャーまたはサービス・プロバイダーによって提供されるサービスを使用してください。

* アプリを手動でインストールしないでください。

  動的クラウド環境では、アプリがオンデマンドで頻繁にインストールされる可能性があります。 インストール・プロセスはスクリプト化されて信頼できるものでなければならず、構成データはスクリプトの外部に置く必要があります。

  少なくとも、アプリのインストールを、オペレーティング・システムから独立した一様なスクリプトのセットとして捉えてください。 さまざまな自動化技法に適応できるよう、アプリのインストールを小さくポータブルなものにしてください。 また、アプリのインストールに必要な依存関係を最小限にしてください。

クラウド対応のアプリについて詳しくは、[The 12-factor app ![「外部リンク」アイコン](../icons/launch-glyph.svg)](http://12factor.net/){: new_window} を参照してください。

##アプリのマイグレーション
{: #ht_hostapp}

アプリを完全にクラウド環境に移す代わりに、アプリを {{site.data.keyword.Bluemix_notm}} にインクリメンタルにマイグレーションできます。 アプリの一部を先にマイグレーションし、Cloud Integration サービスを使用して、既存のデータまたは SoR (Systems of Record、定型業務処理システム) に接続することができます。

クラウド・アプリ内で、バックエンドのデータまたはサービス (例えば、SoR) にアクセスすることが必要になる場合があります。 {{site.data.keyword.Bluemix_notm}} では、Secure Gateway サービスを使用して、{{site.data.keyword.Bluemix_notm}} 組織とエンタープライズ・バックエンド・ネットワークの間にセキュア・トンネルを確立できます。 このサービスによって、{{site.data.keyword.Bluemix_notm}} 上のアプリがバックエンド・ネットワークのデータおよびサービスにアクセスできるようになります。 詳細については、[Reaching enterprise backend with Bluemix Secure Gateway via console ![「外部リンク」アイコン](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window} を参照してください。

アプリを Cloud Foundry アプリとして {{site.data.keyword.Bluemix_notm}} にデプロイするには、{{site.data.keyword.Bluemix_notm}} カタログからランタイムを選択します。 ランタイムには、スターター・アプリ Hello World が含まれていて、これを自分のアプリに置き換えることができます。 使用したいランタイムを提供するスターターが見つからない場合は、cf push コマンドで -b オプションを使用して、Cloud Foundry 互換のカスタム・ビルドパックを {{site.data.keyword.Bluemix_notm}} に持ち込むことができます。 詳しくは、『[コミュニティー・ビルドパックの使用](/docs/cfapps/byob.html)』を参照してください。

{{site.data.keyword.Bluemix_notm}} が提供する以下のツールおよびサービスを使用できます。

| ツール | 方法 |
|:------|:--------|
| Cloud Foundry コマンド・ライン・インターフェース (cf cli) | ローカル・クライアントでコードを管理し、Cloud Foundry コマンド・ライン・インターフェースを使用してアプリを手動で {{site.data.keyword.Bluemix_notm}} にプッシュします。 詳しくは、『[アプリケーションのアップロード](/docs/starters/upload_app.html)』を参照してください。 |
| Eclipse | Eclipse でコードを管理し、IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}} を使用してアプリをプッシュします。 |
| Git 統合 | GitHub でコードを管理し、Git を {{site.data.keyword.Bluemix_notm}} に統合します。 他の開発者と共同作業することができます。 コードの変更をコミットすると、アプリは自動的に {{site.data.keyword.Bluemix_notm}} にデプロイされます。 アプリを手動でプッシュする必要はありません。 |
| {{site.data.keyword.Bluemix_notm}} DevOps Delivery Pipeline | DevOps GitHub リポジトリーでコードを管理し、DevOps Delivery Pipeline を使用してアプリを {{site.data.keyword.Bluemix_notm}} にデプロイします。 |
{: caption="表 1. {{site.data.keyword.Bluemix_notm}} のツール" caption-side="top"}

アプリの要件が Cloud Foundry プラットフォームではサポートされない場合、カスタマイズされた追加のオプションを使用してランタイムがセットアップされ、構成され、保守される、コンテナーまたは VM を使用できます。

## Continuous Delivery でのツールチェーンを使用したアプリの開発およびデプロイ
{:ht_cd}

[アプリにツールチェーン](/docs/services/ContinuousDelivery/toolchains_working.html#creating_a_toolchain_from_an_app)を追加し、[Continuous Delivery ツールチェーン UI](/docs/services/ContinuousDelivery/toolchains_using.html#toolchains-using) を使用して、アプリの開発およびデプロイを行います。

## cf CLI を使用したアプリのアップロード
{: #ht_cfcli}

ローカル・クライアントでコードを管理し、Cloud Foundry コマンド・ライン・インターフェースを使用してアプリを手動で {{site.data.keyword.Bluemix_notm}} にアップロードすることができます。 コードを変更した場合、更新後のコードを実行するには、アプリを再度 {{site.data.keyword.Bluemix_notm}} にプッシュする必要があります。

アプリをマイグレーションするには、以下の手順を実行します。

Cloud Foundry コマンド・ライン・インターフェースをインストールします。 必ず最新バージョンの cf コマンド・ライン・インターフェースを使用するようにしてください。
  1. ご使用のオペレーティング・システム用のインストール・プログラムをダウンロードします。</li>
  2. ツールのウィザードに従ってコマンド・ラインをインストールします。</li>
  3. 次のコマンドを使用して、cf コマンド・ライン・インターフェースのバージョンを確認します。`cf -v`

オプション: アプリを {{site.data.keyword.Bluemix_notm}} にプッシュする前にデプロイメントの詳細を指定して保存したい場合は、以下の手順を実行してアプリ・マニフェストを追加できます。
  1. アプリの作業ディレクトリーに移動し、manifest.yml という名前 (これはデフォルト名です) のファイルを作成します。</li>
  2. このマニフェスト・ファイル内にデプロイメント詳細を指定します。 以下は、Java™ アプリ用のマニフェスト・ファイルの例です。
  ```applications:
  - disk_quota: 1024M
  host: myjavatest
  name: MyJavaTest
  path: webStarterApp.war
  domain: mybluemix.net
  instances: 1
  memory: 512M
  ```
  このファイル内で使用できるサポートされるオプションについて詳しくは、『[アプリ・マニフェスト](../manageapps/depapps.html#appmanifest)』を参照してください。

アプリをプッシュします。 cf push コマンドを使用してアプリをアップロードできます。
  1. 以下のコマンドを実行して、{{site.data.keyword.Bluemix_notm}} に接続し、ログインします。 プロンプトが出されたら、組織およびスペースを選択します。
  ```
  cf login -a https://api.ng.bluemix.net
  ```
  組織がシングル・サインオンを使用している場合、`-sso` フラグを追加します。

  2. アプリ・ディレクトリーから、アプリ名を指定して cf push コマンドを入力します。 アプリ名は、{{site.data.keyword.Bluemix_notm}} 環境内で固有でなければなりません。
  ```
  cf push appname
  ```
  3. オプション: 外部ビルドパックを使用する場合、cf push コマンドで -b オプションを使用する必要があります。 例えば次のようにします。
  ```
  cf push appname -b buildpack_URL
  ```
  詳しくは、『[コミュニティー・ビルドパックの使用](../cfapps/byob.html)』を参照してください。

オプション: アプリを変更した場合、再度 cf push コマンドを入力して、変更をアップロードする必要があります。 cf コマンド・ライン・インターフェースは、アプリの実行中のインスタンスを新規コードで更新するようプロンプトを出されたときに、ユーザーの過去のオプションとユーザーの応答を使用します。

**注:**

* cf コマンド・ライン・インターフェースは、cf push コマンドが使用されると、すべてのファイルおよびディレクトリーを現行ディレクトリーから {{site.data.keyword.Bluemix_notm}} にコピーします。 アプリ・ディレクトリーには必要なファイルだけを保管しておくようにしてください。
* 組織のメモリーがアプリのすべてのインスタンスにとって十分であることを確認してください。 組織のメモリー割り当て量を表示するには、cf org org_name を使用します。
* cf push について詳しくは、『[cf コマンド](../cli/reference/cfcommands/index.html)』を参照してください。

## データのマイグレーションおよびサービスの使用
{: #ht_service}

アプリを {{site.data.keyword.Bluemix_notm}} にアップロードした後、アプリが接続されるサービスを {{site.data.keyword.Bluemix_notm}} カタログから選択し、サービス・インスタンスを作成し、そのインスタンスをアプリにバインドし、その後でアプリを再始動します。

アプリの VCAP_SERVICES 環境変数は、{{site.data.keyword.Bluemix_notm}} でサービス・インスタンスと対話する方法についての情報を含んでいる JSON オブジェクトです。 その情報には、サービス・インスタンス名、資格情報、およびサービス・インスタンスへの接続 URL が含まれます。

{{site.data.keyword.Bluemix_notm}} でコードを実行するには、VCAP_SERVICES 変数を解析してサービス接続についての情報を取得するためのコード・ロジックを追加する必要があります。 サービス・インスタンスの動的に割り当てられたホストおよびポートを環境変数を通して取得するように、アプリを変更してください。 次の例は、Ruby アプリで Postgre SQL サービス・インスタンスの資格情報を取得する方法を示します。

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

{{site.data.keyword.Bluemix_notm}} 用にアプリを変更した後、アプリをローカル環境で確実に実行できるようにするため、すべての {{site.data.keyword.Bluemix_notm}} Cloud Foundry アプリに対して設定される VCAP_SERVICES 環境変数があるかどうかをチェックしてください。


# 関連リンク
{: #rellinks}

## 関連リンク
{: #general}

* [IBM Containers](/docs/containers/container_index.html)
* [仮想マシン](/docs/virtualmachines/vm_index.html)
* [Delivery Pipeline 概説](/docs/services/DeliveryPipeline/index.html)
* [IBM Eclipse Tools for Bluemix を使用したアプリのデプロイ](/docs/manageapps/eclipsetools/eclipsetools.html)
* [The twelve-factor app ![「外部リンク」アイコン](../icons/launch-glyph.svg)](http://12factor.net/){: new_window}
* [Reaching enterprise backend with Bluemix Secure Gateway via console ![「外部リンク」アイコン](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window}
