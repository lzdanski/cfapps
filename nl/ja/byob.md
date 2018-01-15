---


copyright:

  years: 2015, 2017

lastupdated: "2015-12-15"


---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# コミュニティー・ビルドパックの使用
{: #using_buildpacks}

使用したいランタイムを提供するスターターが {{site.data.keyword.Bluemix}} カタログにない場合、外部ビルドパックを {{site.data.keyword.Bluemix_notm}} に持ち込むことができます。 cf push コマンドを使用してアプリをデプロイするときに、Cloud Foundry と互換のカスタム・ビルドパックを指定することができます。
{:shortdesc}

外部ビルドパックは、ユーザーが独自のビルドパックとして使用できるよう、Cloud Foundry コミュニティーによって提供されます。 アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする前に、cf コマンド・ライン・インターフェースをインストールしてください。

**注:** 外部ビルドパックは IBM によって提供されるものではありません。サポートについては、Cloud Foundry コミュニティーにお問い合わせください。

## 組み込みのコミュニティー・ビルドパック

{{site.data.keyword.Bluemix_notm}} では、Cloud Foundry コミュニティーによって提供される組み込みビルドパックを使用できます。 組み込みのコミュニティー・ビルドパックを表示するには、以下のように `cf buildpacks` コマンドを実行します。

```
cf buildpacks
Getting buildpacks...

buildpack      position   enabled   locked   filename
...
java_buildpack     7      true      false    buildpack_java_v2.0.2.zip
ruby_buildpack     8      true      false    buildpack_ruby_v46-245-g2fc4ad8.zip
nodejs_buildpack   9      true      false    buildpack_nodejs_v8-177-g2b0a5cf.zip
```
{:screen}


同じランタイムまたはフレームワークでは、IBM 製ビルドパックがコミュニティー・ビルドパックより優先されます。 コミュニティー・ビルドパックを使用して IBM 製ビルドパックを上書きしたい場合、cf push コマンドで -b オプションを使用してビルドパックを指定する必要があります。
例えば、以下のようにして Java™ Web アプリ用のコミュニティー・ビルドパックを使用できます。

```
cf push app_name -b java_buildpack -p app_path
```
{:pre}

また、以下のようにして Node.js アプリ用のコミュニティー・ビルドパックを使用することもできます。

```
cf push app_name -b nodejs_buildpack -p app_path
```
{:pre}

IBM 製ビルドパックではサポートされないが組み込みコミュニティー・ビルドパックではサポートされるランタイムまたはフレームワークの場合、cf push コマンドで -b オプションを使用する必要はありません。</p><p>例えば、Ruby アプリ用の IBM 製のビルドパックはありません。 以下のコマンドを入力することによって、組み込みのコミュニティー・ビルドパックを使用できます。

```
cf push app_name -p app_path
```
{:pre}

## 外部ビルドパック

{{site.data.keyword.Bluemix_notm}} では、外部ビルドパックやカスタム・ビルドパックを使用できます。 **cf push** コマンドの -b オプションでビルドパックの URL を指定し、`-s` オプションでスタックを指定する必要があります。 例えば、静的ファイル用の外部コミュニティー・ビルドパックを使用するには、以下のコマンドを実行します。

```
cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{:pre}

Ruby アプリに組み込みのコミュニティー・ビルドパックを使用したくない場合は、以下のコマンドを入力して、外部ビルドパックを使用できます。

```
cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{:pre}

アプリケーション用にカスタム・ビルドパックを使用することもできます。 例えば、Cloud Foundry コミュニティーによって提供されているオープン・ソース PHP ビルドパックを使用するには、PHP アプリを Bluemix にデプロイするときに、以下のコマンドを入力してビルドパックの Git リポジトリー URL を指定します。

```
cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

また、プロジェクトの `manifest.yml` ファイルを編集して次のような `buildpack` 行を追加することも可能です。

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{:pre}


## Java ビルドパックのバージョンの指定

<ul>
<li>
<strong>cf set-env</strong> コマンドを使用します。 例えば、Java バージョンを 1.7.0 に設定するには、次のコマンドを入力します。
<pre class="pre"><code>cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;</code></pre>
<p>その後、変更を有効にするためにアプリを再ステージします。</p>
<pre class="pre"><code>cf restage app_name</code></pre>
</li>
<li>
<code>manifest.yml</code> ファイルを使用します。 指定したい環境変数と値を直接このファイルに追加することができます。 詳細については、<a href="https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block">環境変数</a>を参照してください。</li></ul>
