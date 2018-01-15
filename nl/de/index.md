---

copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Cloud Foundry-Apps erstellen
{: #creating_cloud_foundry_apps}

In {{site.data.keyword.Bluemix}} können Sie Ihre App in der {{site.data.keyword.Bluemix_notm}}-Konsole erstellen. Anschließend können Sie entscheiden, ob Sie weiterhin die Konsole, die Befehlszeilenschnittstelle 'cf' oder {{site.data.keyword.jazzhub_title}} verwenden möchten, um Ihre App zu entwickeln, zu verfolgen, zu planen und bereitzustellen.
{:shortdesc}

Wenn Sie eine App in {{site.data.keyword.Bluemix_notm}} erstellen, können Sie mit einem
Starter beginnen. Bei einem *Starter* handelt es sich um eine Vorlage, die vordefinierte Services sowie Anwendungscode enthält, der mit einem bestimmten Buildpack konfiguriert ist. Es gibt zwei Arten von Startern: Boilerplates und Laufzeiten. 

Eine *Boilerplate* ist ein Container für eine Anwendung und die zugehörige Laufzeitumgebung sowie vordefinierte Services für eine bestimmte Domäne. Beispiel: Die Boilerplate 'Mobile Cloud'
enthält eine Node.js-Laufzeit sowie die Services 'Mobile Data', 'Mobile Application Security' und 'Push'. Darüber hinaus sind auch ein SDK und Beispielanwendungen enthalten, um in die Entwicklung von mobilen Apps, die auf diese Services zugreifen, einsteigen
zu können.

Als *Laufzeit* wird das Set mit Ressourcen bezeichnet, das zur Ausführung einer Anwendung benutzt wird. {{site.data.keyword.Bluemix_notm}} stellt Laufzeitumgebungen als Container für verschiedene Anwendungstypen bereit. Die
Laufzeitumgebungen werden als Buildpacks in {{site.data.keyword.Bluemix_notm}} integriert,
automatisch für die Verwendung konfiguriert und müssen nur sehr wenig oder auch gar nicht gewartet werden.

Gehen Sie wie folgt vor, um mit der Erstellung Ihrer Anwendung zu beginnen:
  1. Klicken Sie in der IBM Cloud-Symbolleiste auf **Katalog**. 
  2. Klicken Sie auf **Cloud Foundry-Apps** und wählen Sie eine Laufzeitumgebung aus. Befolgen Sie die Anweisungen, um einen Namen anzugeben und zu entscheiden, welchen Code Sie verwenden möchten. Klicken Sie auf **Erstellen**. 
  3. Nachdem Sie die Anweisungen abgeschlossen haben, klicken Sie auf **Übersicht**. 
  5. Sie können Ihrer App einen Service hinzufügen, indem Sie in der App-Übersicht im Dashboard auf **Verbindung erstellen** klicken. Durchsuchen Sie den Katalog und wählen Sie Services aus oder blättern Sie zum Ende des Katalogs und klicken Sie auf **{{site.data.keyword.Bluemix_notm}} Experimental Services**, um die experimentellen Services zu durchsuchen. Sie haben auch die Möglichkeit, die Befehlszeilenschnittstelle 'cf' zu verwenden. Weitere Informationen finden Sie unter 'Optionen für das Arbeiten mit Apps'.
  6. Blättern Sie auf der Übersichtsseite zur Karte 'Continuous Delivery' und klicken Sie auf **Toolchain anzeigen**. Die Quelle der App wird in einem Repository gespeichert, das unter Bluemix gehostet wird. Außerdem wird eine offene Toolchain erstellt, mit der und unter Verwendung dieses Repositorys und einer Delivery Pipeline Ihre App entwickelt und bereitgestellt wird. Weitere Informationen zum Continuous Delivery-Service finden Sie in <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Einführung in Continuous Delivery</a>.

**Hinweis:** Wenn ein an eine App gebundener Service abstürzt, wird die App
möglicherweise gestoppt oder weist Fehler auf. {{site.data.keyword.Bluemix_notm}} startet die App nicht automatisch erneut, um eine Recovery für diese Probleme durchzuführen. Ziehen Sie in Betracht, die App so zu codieren, dass sie eine Recovery für Ausfallzeiten, Ausnahmen und Verbindungsfehler durchführt. Weitere Informationen finden Sie im Abschnitt, in dem beschrieben wird, dass Apps nicht automatisch neu gestartet werden.

## Optionen für das Arbeiten mit Apps

Nach dem Erstellen Ihrer App steht Ihnen eine Reihe von Optionen zur Verfügung, um Ihrer App weitere Services hinzuzufügen und um Ihre App
zu erstellen und bereitzustellen:

<dl><dt>Befehlszeilenschnittstelle 'cf'</dt>
<dd>Über die <a href="https://github.com/cloudfoundry/cli#getting-started">Befehlszeilenschnittstelle 'cf'</a> können Sie Ihre Anwendung aktualisieren, eine Serviceinstanz erstellen oder den Service an
Ihre Anwendung binden. Sie können auch die Befehlszeilenschnittstelle 'cloud-cli' verwenden, um Serviceangebote zu
erstellen, zu aktualisieren und zu löschen.</dd>
<dt>{{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle</dt>
<dd>Mit der {{site.data.keyword.Bluemix_notm}} <a href="https://console.bluemix.net/dashboard/apps">-Benutzerschnittstelle</a> können Sie Ihre Anwendung erstellen und dabei auswählen, welche Services und Laufzeiten kombiniert werden sollen, um Ihre geschäftsbezogenen Probleme zu lösen.</dd>
<dt>{{site.data.keyword.contdelivery_full}}</dt>
<dd>Mit {{site.data.keyword.contdelivery_short}} können Sie Builds, Komponententests, Bereitstellungen und weitere Tasks automatisieren. In der umfangreichen webbasierten IDE können Sie Code bearbeiten und mit einer Push-Operation übertragen. Durch die Erstellung von Toolchains werden Toolintegrationen ermöglicht, die Entwicklungs-, Bereitstellungs- und Betriebstasks unterstützen. Der Continuous Delivery Service enthält, Web-IDE für Eclipse Orion, Delivery Pipeline und Git-Repositorys und Issue Tracking. Weitere Informationen finden Sie unter <a href="https://console.ng.bluemix.net/docs/services/ContinuousDelivery/index.html#cd_getting_started">Einführung in Continuous Delivery</a>.</dd>
</dl>

## Tipps

Nutzen Sie bei der Entwicklung Ihrer Webanwendungen die folgenden Tipps:

<dl><dt>Persistenz</dt>
<dd>Geben Sie für Ihre Anwendungen keinen lokalen Speicher an. Jede Instanz Ihrer Anwendung kann jederzeit
erneut gestartet oder auf eine andere virtuelle Maschine verschoben werden. Dies gilt auch dann, wenn nur eine einzige Instanz aktiv ist. Dies geschieht
üblicherweise, um einen Lastausgleich zu erreichen. Wenn die Anwendung verschoben oder gelöscht wird, gehen sämtliche Daten im
lokalen Speicher verloren. Verwenden Sie zwecks Persistenz einen der Datenspeicherservices von {{site.data.keyword.Bluemix_notm}}.</dd>
<dt>Ressourcengrenzen</dt>
<dd>Berücksichtigen Sie die Grenzwerte im Hinblick auf die Ressourcenmengen, die bei einem Testkonto verwendet
werden können. Es gelten folgende Grenzwerte:
<table style="width:100%">
<caption>Tabelle 1. {{site.data.keyword.Bluemix_notm}}-Ressourcengrenzen für Testkonten</caption>
  <th>Ressourcentyp</th>	<th>Mengenbegrenzung</th>
<tr><td>Anzahl der in allen Apps verwendeten Services</td> <td>10</td>
<tr><td>Menge des in allen Apps belegten Hauptspeichers</td> <td>	2 GB</td>
<tr><td>Anzahl der Routen</td> <td>500</td>
</table>
</dd>
</dl>
