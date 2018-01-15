---

copyright:

  years: 2015, 2017

lastupdated: "2017-12-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# シナリオ: エンドツーエンド開発
{: #ee}


アプリを作成、実行、およびデプロイするときに、{{site.data.keyword.Bluemix}} コンソール、プラットフォーム、および各種ツールを使用できます。 このエンドツーエンド開発シナリオに従って作業を開始してください。
{:shortdesc}

## サインアップ
{: #ee_start}

開始する前に、[bluemix.net/ ![外部リンク・アイコン](../icons/launch-glyph.svg)](https://bluemix.net/){: new_window} で登録を行って IBM ID を取得する必要があります。その後、{{site.data.keyword.Bluemix_notm}} にログインして、30 日間の無料トライアルを開始します。{{site.data.keyword.Bluemix_notm}} の無料トライアルでは、2 GB のランタイム・メモリーと 10 個のサービス・インスタンスを使用できます。

アプリを開発するには、次の 3 つの方法があります。

* [継続的デリバリー・サービス](#ee_cd)
* [IBM Cloud ユーザー・インターフェース](#ee_appui)
* [Cloud Foundry コマンド・ライン](#ee_cf)

## ツールチェーンおよび {{site.data.keyword.contdelivery_short}} サービスを使用したアプリの開発およびデプロイ
{: #ee_cd}

{{site.data.keyword.contdelivery_full}} サービスをアプリに組み込む <a href="/docs/services/ContinuousDelivery/toolchains_working.html#creating_a_toolchain_from_an_app">ツールチェーンを追加します</a>。 次に、<a href="/docs/services/ContinuousDelivery/toolchains_using.html#toolchains-using">ツールチェーンを使用して</a>、アプリを開発しデプロイします。

## {{site.data.keyword.Bluemix_notm}} ユーザー・インターフェースを使用した Web アプリの作成
{: #ee_appui}

サインアップした後、最初のアプリの作成を {{site.data.keyword.Bluemix_notm}} カタログとダッシュボードを使用して開始します。

{{site.data.keyword.Bluemix_notm}} では、アプリは組織およびスペースに関連付けられます。 1 つの組織を複数のコラボレーターが所有し、使用します。 最初は、ユーザー名を元に名付けられたデフォルトの組織が 1 つ用意され、自身が唯一のコラボレーターになります。 この組織内にスペースも 1 つ用意されます。 スペースはアプリを実行するための環境です。例えば、開発環境として dev というスペース、テスト環境として test というスペース、実稼働環境として production というスペースを作成できます。 それぞれの環境は、地域に属しています。{{site.data.keyword.Bluemix_notm}} では、少ないネットワーク待ち時間、データ・プライバシー、高い可用性のために、アプリケーションを特定の地域にデプロイできます。

このシナリオでは、Node.js を使用して Web アプリを開発します。 開発者は米国にいて、このアプリのユーザーの大部分も米国にいるものとします。 ネットワーク待ち時間を少なくできるように、ユーザー・ベースの近くでアプリの作成と実行を行うことに決めます。 {{site.data.keyword.Bluemix_notm}} にログインした後に、ユーザー・アカウント設定リンクをクリックし、次に**「米国南部」**地域を選択します。 次に、以下の手順を実行してアプリを作成します。

  1. {{site.data.keyword.Bluemix_notm}} ツールバーの**「カタログ」**をクリックします。
  2. **「Cloud Foundry アプリ」**を選択します。
  3. **「SDK for Node.js」**を選択します。
  4. アプリ名として固有の名前を入力し、**「作成」**をクリックします。 このアプリ名は {{site.data.keyword.Bluemix_notm}} 環境全体で固有でなければなりません。

これで、**「概説チュートリアル」**が表示されます。指示に従って、アプリのスターター・コードをダウンロードし、変更して、デプロイすることができます。

アプリには、1 つのインスタンスと 512 MB のメモリー割り当て量がデフォルトで割り当てられます。 例えば、3 つのインスタンスで、インスタンス当たり 1 GB メモリーにするなど、メモリーを増やしたりインスタンスを追加したりしてアプリの可用性を高めることができます。 アプリのインスタンス数およびメモリー割り当て量を指定するには、**「アプリの概要の表示」**をクリックします。 例えば、インスタンス数に 3、メモリー割り当て量に 1 GB と入力し、**「保存」**をクリックします。 また、問題のトラブルシューティングのために、ファイル、ログ、および環境変数を表示することもできます。

### {{site.data.keyword.Bluemix_notm}} ユーザー・インターフェースを使用したサービスのバインド
{: #ee_bindui}

アプリの作成後に、データベースをアプリと接続します。データベース照会言語を使用してアプリ・データを保管および監視できます。このシナリオでは、{{site.data.keyword.Bluemix_notm}} によって提供されている {{site.data.keyword.cloudant}} サービスを使用します。

アプリケーション内でサービスを使用するには、以下の手順を実行して、サービス・インスタンスを作成し、そのサービス・インスタンスにアプリケーションをバインドします。

  1. {{site.data.keyword.Bluemix_notm}} カタログで、{{site.data.keyword.cloudant}} サービスを選択します。 Cloudant サービスの固有の名前を追加して、**「作成」**をクリックします。Cloudant 管理パネルで、**「起動」**をクリックしてサービスを起動します。
  2. **「接続」**をクリックします。 次に、**「接続の作成」**をクリックします。
  3. アプリの横にある**「接続」**をクリックします。
  4. 「アプリの再ステージ」ウィンドウが表示されます。 **「再ステージ」**をクリックします。

これで、アプリが {{site.data.keyword.cloudant}} サービスにバインドされました。 アプリケーションがサービス・インスタンスと通信するためのすべての必要なデータは VCAP_SERVICES 環境変数に入っています。 例えば、{{site.data.keyword.Bluemix_notm}} はいくつかのアプリケーションを同じ仮想マシン上でホストしているため、複数のアプリケーションが同じ HTTP ポート番号を使用して着信要求を受け取ることはできません。 競合を避けるため、各アプリケーションに固有のポート番号が与えられます。 このポート番号は VCAP_APP_PORT 変数の下にあります。

アプリに関連付けられた VCAP_SERVICES の全体のリストをダッシュボードに表示することができます。そのリストを表示するには、IBM Cloud ツールバーでメニューをクリックして、**「ダッシュボード」**をクリックします。アプリをクリックします。 次に**「ランタイム」**をクリックし、**「環境変数」**タブを選択します。

**注:** この環境変数は 1 つの JSON オブジェクトの直列化であり、アプリがバインドされているサービス・インスタンスごとに 1 つの項目があります。 各サービス・インスタンスが提供するデータの量とタイプはサービス固有です。 アプリがサービスを何も使用しない場合、VCAP_SERVICES は空の JSON オブジェクトです。 この環境変数は、サービスをアプリに追加する場合にのみ使用されます。

## cf cli を使用したアプリのビルド
{: #ee_cf}

{{site.data.keyword.Bluemix_notm}} では、例えば cf コマンド・ライン・インターフェースや Eclipse ツールなど、アプリでコーディングを始めるためのいくつかのツールが提供されています。 アプリでのコーディングを始めるために cf コマンド・ライン・インターフェースを選択できます。

  1. 最初に、アプリのコードをダウンロードして開発します。

    1. アプリのダッシュボードで「開始 (Getting started)」をクリックします。**「サンプル・コードのダウンロード」**リンクをクリックして、アプリのコードをダウンロードします。
    2. ダウンロードしたファイルをディレクトリー (例えば `C:&#xa5;test`) に解凍します。
    3. ローカル統合開発環境でコードを開発します。

  2. **cf** コマンド・ライン・インターフェース (CLI) をインストールします。

    1. ご使用のオペレーティング・システム向けの cf コマンド・ライン・ツール・インストール・プログラムをダウンロードします。
    2. ツールのウィザードに従ってインストールを実行します。
    3. `cf -v` コマンドを使用して、cf コマンド・ライン・インターフェースのバージョンを確認します。

    **要件:** 常に最新バージョンの cf コマンド・ライン・ツールを使用するようにしてください。

  3. **cf** コマンド・ライン・インターフェースをインストールした後、**cf api** コマンドを使用して、作業する {{site.data.keyword.Bluemix_notm}} 地域を指定する必要があります。 **cf** コマンド・ライン・インターフェースは *https://api.[Bluemix_URL]* を使用します (*Bluemix_URL* は地域の URL です)。米国南部地域の API エンドポイントは `ng.bluemix.net` です。次のコマンドを入力して、{{site.data.keyword.Bluemix_notm}} に接続します。

  ```
  cf api https://api.ng.bluemix.net
  ```

  他の API エンドポイントを探すには、『[地域](/docs/overview/cf.html#ov_intro_reg)』を参照してください。{{site.data.keyword.Bluemix_notm}} 地域を指定すると、指定したロケーション情報が保存されます。

  4. 次に、`cf login` コマンドを使用して、{{site.data.keyword.Bluemix_notm}} にログインします。

  ```
  cf login -u your_user_ID -p ***** -o your_org_name -s your_space_name
  ```

  組織がシングル・サインオンを使用している場合は、`cf login -sso` を使用します。

  5. {{site.data.keyword.Bluemix_notm}} にログインすると、アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする準備ができている状態になります。 アプリのディレクトリー `C:&#xa5;test` から、次のコマンドを入力します。

  ```
  cf push [your_appname]
  ```

  **cf push** コマンドについて詳しくは、『[アプリのアップロード](/docs/cfapps/hostingapps.html#ht_cfcli)』を参照してください。

  6. これで、ブラウザーに以下のアプリ URL を入力して、アプリにアクセスすることができます。
  ```
  http://your_app.stage1.mybluemix.net
  ```

アプリをビルドするために他のツール (Eclipse ツールなど) を選択することもできます。 詳しくは、{{site.data.keyword.Bluemix_notm}} ダッシュボードでアプリの「開始 (Getting started)」ページを参照してください。

### cf cli を使用したサービスのバインド
{: #ee_cfbind}

**cf** コマンド・ライン・インターフェースを使用することでもサービスをバインドできます。 cf コマンド・ライン・インターフェースを使用して {{site.data.keyword.cloudant}} サービスをアプリに追加したいと想定します。

アプリ内で {{site.data.keyword.cloudant}} サービスを使用するには、Cloudant サービス・インスタンスを作成し、そのサービス・インスタンスにアプリをバインドし、その後でそのサービス・インスタンスを使用します。他のすべてのサービスにも同じ手順が適用されます。

  1. Cloudant NoSQL DB サービス・インスタンスを作成します。

  cf create-service コマンドを使用して、サービスの新規インスタンスを作成します。 この例では、*Lite* はプランの名前です。例えば次のようにします。

  ```
  cf create-service cloudantNoSQLDB Lite [your_name_for_your_cloudant_service]
  ```

  cf services コマンドを使用して、作成したサービス・インスタンスのリストを表示することもできます。

  ```
  cf services
  ```

  サービス・インスタンスが作成されると、そのサービス・インスタンスは任意のアプリケーションでバインドおよび使用することができます。

  2. サービス・インスタンスをアプリにバインドします。

  サービス・インスタンスを使用するには、それをアプリケーションにバインドする必要があります。 アプリケーション名および作成したサービス・インスタンスを指定することによって、cf bind-service コマンドを使用してサービス・インスタンスをアプリケーションにバインドします。

  ```
  cf bind-service [your_app_name] [your_name_for_your_cloudant_service]
  ```

  サービス・インスタンスをアプリケーションにバインドすると、{{site.data.keyword.Bluemix_notm}} はサービスと通信できるようになり、新規アプリケーションがそのサービス・インスタンスと通信することを指定できます。 異なるサービスに対して、{{site.data.keyword.Bluemix_notm}} はバインド中のアプリケーションおよびサービス・インスタンスの処理方法を変えることがあります。 例えば、サービスによっては、サービス・インスタンスと通信する各アプリケーションごとに新規テナントを作成する場合があります。 サービスは {{site.data.keyword.Bluemix_notm}} に応答を返すときに、アプリケーションとサービスの間での通信のためにアプリケーションに渡される必要のある情報 (例えば資格情報) を渡します。

  **注:** サービス・インスタンスにバインドされるときにアプリケーションが実行中である場合、VCAP_SERVICES 環境変数はアプリケーションが再始動されるまで更新されません。 アプリケーションを再始動するには、cf restart コマンドを使用します。

  3. サービス・インスタンスを使用します。

  このシナリオでは、VCAP_SERVICES 環境変数には、アプリケーションが {{site.data.keyword.cloudant}} のこのインスタンスに接続するために使用できる以下の項目のような情報が含まれます。

  <dl><dt>username</dt>
  <dd>d72837bb-b341-4038-9c8e-7f7232916197-bluemix</dd>
  <dt>password</dt>
  <dd>secret</dd>
  <dt>url</dt>
  <dd>https://d72837bb-b341-4038-9c8e-7f7232916197-bluemix:b6fc4708942b70a88853177ee52a528d07a43fa8575a69abeb8e044a7b0a7424@d72837bb-b341-4038-9c8e-7f7232916197-bluemix.cloudant.com</dd></dl>

  例えば、Node.js アプリはこの情報に次のようにアクセスします。
  ```
  if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var cloudant = env['cloudantNoSQLDB'][0].credentials;
  } else {
        var cloudant = {
                "username" : "user1",
                "password" : "secret",
                "url" : "https://user1:secret@localhost:25002"
                }
        };
  ```

  **注:** サンプル・コードで示されているように、{{site.data.keyword.cloudant}} サービス・インスタンスに接続するために、まず最初に VCAP_SERVICES 環境変数が存在するかどうかを確認できます。 存在する場合、アプリケーションはこの cloudant 変数のプロパティーを使用してデータベースにアクセスできます。 しかし、VCAP_SERVICES 環境変数が存在しない場合、提供されたデフォルト値を持つローカル {{site.data.keyword.cloudant}} サービス・インスタンスが使用されます。

  4. サービス・インスタンスと対話します。

  資格情報を使用してサービス・インスタンスと対話することができます。 実行できるアクションには、読み取り、書き込み、更新などがあります。 次の例は、{{site.data.keyword.cloudant}} サービス・インスタンスに JSON オブジェクトを挿入する方法を示しています。

  ```
  // create a new message
var create_message = function(req, res) {
  require('cloudantdb').connect(cloudant.url, function(err, conn) {
    var collection = conn.collection('messages');

    // create message record
    var parsedUrl = require('url').parse(req.url, true);
    var queryObject = parsedUrl.query;
    var name = (queryObject["name"] || 'Bluemix');
    var message = { 'message': 'Hello, ' + name, 'ts': new Date()
};
    collection.insert(message, {safe:true}, function(err){
      if (err) { console.log(err.stack); }
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.write(JSON.stringify(message));
      res.end('\n');
    });
  });
}
  ```

  5. **オプション:** サービス・インスタンスをアンバインドまたは削除します。

  サービス・インスタンスが不要になった場合や、一部のスペースを解放したい場合に、サービス・インスタンスのアンバインドまたは削除が必要になることがあります。 サービス・インスタンスをアプリからアンバインドするには **cf unbind-service** コマンドを使用し、サービス・インスタンスを削除するには **cf delete-service** コマンドを使用します。

  サービスについて詳しくは、『サービス』を参照してください。 {{site.data.keyword.Bluemix_notm}} 環境でアプリケーションを管理するために使用できる **cf** オプションの詳細情報を表示するには、**cf** コマンド・ライン・インターフェースで **cf --help** を実行してください。

  **注:** サービス・インスタンスを削除する前に、もうそれが必要ではないことを確認してください。 サービス・インスタンスを削除すると、そのサービス・インスタンスに関連付けられたすべてのデータが消去されます。 削除されたサービスにバインドされたアプリケーションの VCAP_SERVICES 環境変数は、アプリケーションが再始動されるまで更新されません。

## アプリのコスト計算
{: #ee_billing}

30 日間の無料トライアルの期限が切れたが、{{site.data.keyword.Bluemix_notm}} を使用し続けることを望んでいるとします。 {{site.data.keyword.Bluemix_notm}} を使用し続けるためには、従量課金アカウントまたはサブスクリプション・アカウント用にクレジット・カード情報を追加する必要があります。 ただし、{{site.data.keyword.Bluemix_notm}} は、有料アカウントに切り替えた場合でも、ほとんどのランタイム・フレームワークおよびサービスについて無料枠を引き続き提供します。 使用量が無料枠を超えない限り、{{site.data.keyword.Bluemix_notm}} による課金はありません。

{{site.data.keyword.Bluemix_notm}} は、アプリのコストを確認できるように、見積もり機能と計算機能を備えています。 次のようにしてアプリのコストを確認できます。

  * ダッシュボードで、アプリをクリックします。 次に、「概要」ページで、**「このアプリのコストの見積もり」**をクリックして **SDK for Node.js** ランタイムおよびサポートの価格を確認し、アプリの月の合計価格を確認します。

  * あるいは、「価格設定シート」ページで、アプリのランタイムおよびサービスの月次使用量を入力します。 例えば、**SDK for Node.js** のインスタンスが 3 つあり、インスタンスごとに 1 GB のメモリーを使用しているといったケースが考えられます。 月額料金が計算され、表示されます。

アプリのコストは手動で計算することもできます。そうする場合は、ランタイムおよびサービスの価格を合算してから無料枠を差し引きます。 詳しくは、『手動によるコスト計算』を参照してください。

## アプリの削除
{: #ee_removing}

ビルドするアプリが増えるにつれて、割り当て量は限度に近づきます。 ただし、もう使用しそうにないアプリが割り当て量を占めているままになっている可能性があります。 {{site.data.keyword.Bluemix_notm}} では、いつでも簡単にアプリを削除してスペースを解放することができます。

{{site.data.keyword.Bluemix_notm}} ユーザー・インターフェースで、アプリの「概要」ページに移動し、**「メニュー」**アイコンをクリックし、もう使用しないアプリを削除します。 **cf delete** コマンドを使用してアプリを削除することもできます。
