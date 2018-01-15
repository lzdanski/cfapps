---



copyright:

  years: 2015, 2017

lastupdated: "2017-12-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Apps in {{site.data.keyword.Bluemix_notm}} hosten
{: #hosting_apps}

In {{site.data.keyword.Bluemix}} können Sie Apps erstellen und vorhandene Apps hosten. Sie können Ihre Apps nach {{site.data.keyword.Bluemix_notm}} verschieben, sofern diese für die Cloud geeignet sind. Von {{site.data.keyword.Bluemix_notm}} werden zahlreiche Methoden zum Ausführen von Apps bereitgestellt, z. B. Cloud Foundry, IBM Containers und virtuelle Maschinen.

## Apps für die Cloud vorbereiten
{: #cloud-readyapps}

Wenn alle nachfolgenden Prinzipien in einer App beachtet werden, ist die App für die Cloud geeignet und kann nach {{site.data.keyword.Bluemix_notm}} migriert werden. Wenn in der App gegen ein Prinzip verstoßen wird, können Sie die App in der Regel so ändern, dass sie den Prinzipien entspricht.

* Codieren Sie die App nicht direkt für eine bestimmte Topologie. 

  In einer Umgebung ohne Cloud kann von der App eine bestimmte Bereitstellungstopologie verwendet werden. Die Apptopologie kann sich in Cloud-Apps jedoch ändern, weil für die Cloud geeignete Apps und Services sofortige Änderungen der Skalierbarkeit zulassen. Die Änderungen der Skalierbarkeit umfassen dynamisches Skalieren und eine manuelle Größenänderung der Instanzenanzahl einer App. 

  Erstellen Sie Ihre App so generisch und statusunabhängig wie möglich, um zu verhindern, dass die App durch Änderungen der Skalierbarkeit beeinträchtigt wird. 

* Gehen Sie nicht davon aus, dass das lokale Dateisystem ein permanentes System ist.

  Eine App-Instanz kann in der Cloud jederzeit verschoben, gelöscht oder kopiert werden; verlassen Sie sich deswegen nicht auf die Dateien, die in das Dateisystem geschrieben werden. Wenn eine App das lokale Dateisystem als Cache für häufig verwendete Informationen (einschließlich von App-Protokollen) verwendet, gehen diese Informationen verloren, wenn die Instanz beendet wird und an einer anderen Position oder auf einer anderen virtuellen Maschine erneut gestartet wird. 

  Sie können Informationen anstatt im lokalen Dateisystem in einem Service speichern, z. B. in einer SQL- oder NoSQL-Datenbank. In einer dynamischen Cloudumgebung ist es darüber hinaus wichtig, dass die Protokolle durch einen Service bereitgestellt werden, der länger verfügbar ist als die App-Instanzen, für die die Protokolle erstellt werden. 

* Speichern Sie den Sitzungsstatus nicht in der App. 

  Der Status Ihres Systems wird durch die Datenbanken und den gemeinsam genutzter Speicher definiert und nicht durch jede einzelne aktive App-Instanz. Statusangaben jeder Art schränken die Skalierbarkeit einer App ein. Versuchen Sie, die Auswirkung des Sitzungsstatus dadurch zu minimieren, dass er an einer zentralen Position auf dem Server gespeichert wird.

  Wenn Sie den Sitzungsstatus nicht vollständig ignorieren können, verlagern Sie ihn in einen Speicher mit hoher Verfügbarkeit, der sich außerhalb Ihres App-Servers befindet. Solche Speicher sind zum Beispiel IBM WebSphere Extreme Scale, Redis, Memcached oder eine externe Datenbank.

* Geben Sie keine konkreten Infrastrukturabhängigkeiten an.

  Hierbei handelt es sich um ein generelles Prinzip, das sich mehrfach auswirkt. Gehen Sie zum Beispiel nicht davon aus, dass den Services, die von der App verwendet werden, bestimmte Hostnamen oder IP-Adressen zugeordnet sind. Die Services in der Cloudumgebung können verlagert oder neu generiert worden sein und auch die Hostnamen und IP-Adressen können sich ändern. 

  Das Extrahieren von umgebungsspezifischen Abhängigkeiten in eine Reihe von Eigenschaftendateien ist zwar eine Verbesserung, aber trotzdem unzulänglich. Ein bewährtes Verfahren ist das Verwenden einer externen Service-Registry zum Auflösen von Serviceendpunkten oder das Delegieren der gesamten Routingfunktion an einen Service-Bus oder eine Lastausgleichsfunktion mit einem virtuellen Namen.

* Verwenden Sie in Ihrer App keine Infrastruktur-APIs. 

  Wenn Sie in Ihrer App eine bestimmte Infrastruktur-API benötigen, ist eine Änderung der Infrastruktur eine Herausforderung, weil von einer Infrastruktur-API auf viele verschiedene Schichten im Software-Stack verwiesen werden kann.

  Sie können stattdessen auf vorhandene Open Source-Produkte oder kostenpflichtige Produkte zurückgreifen und PaaS-Lösungen (PaaS - Platform as a Service) in der PaaS-Schicht belassen, damit sie nicht zu einem Bestandteil Ihres App-Codes werden. 

* Verwenden Sie nicht eingeschränkt lesbare Protokolle.

  Verwenden Sie nicht eingeschränkt lesbare Protokolle, für die eine zusätzliche Konfiguration erforderlich ist, damit sie dauerhaft verfügbar sind.

  Apps, die auf Standardprotokollen basieren, sind ausfallsicherer, wenn die Konfigurationselemente an die Plattform delegiert werden. Zu den Standardprotokollen gehören HTTP, SSL, Standarddatenbanken, Warteschlangensteuerung und Web-Service-Verbindungen.

* Verlassen Sie sich nicht auf betriebssystemspezifische Funktionen. 

  Wenn Sie bereits betriebssystemspezifische Funktionen verwendet haben, können Sie dieses Problem mithilfe von Kompatibilitätsbibliotheken, z. B. Cygwin oder Mono, beheben. Cygwin ist eine Kompatibilitätsbibliothek, von der eine Reihe von Linux-Tools in einer Windows-Umgebung bereitgestellt werden. Mono ist eine Kompatibilitätsbibliothek, von der Windows .NET-Funktionen in Linux bereitgestellt werden.

  Vermeiden Sie betriebssystemspezifische Abhängigkeiten; verwenden Sie stattdessen Services, die von der Middlewareinfrastruktur oder von Service-Providern bereitgestellt werden.

* Installieren Sie die App nicht manuell. 

  Es kann vorkommen, dass die App in der dynamischen Cloudumgebung häufig bedarfsgesteuert installiert wird. Der Installationsprozess muss scriptgesteuert und zuverlässig sein, die Konfigurationsdaten müssen aus den Scripts externalisiert werden.

  Erfassen Sie die App-Installation mindestens als einheitlichen Satz aus Scripts, die vom Betriebssystem unabhängig sind. Halten Sie die App-Installation klein und portierbar, damit sie an abweichende Automatisierungsverfahren angepasst werden kann. Minimieren Sie auch die Abhängigkeiten, die für die App-Installation erforderlich sind. 

Weitere Informationen zu für die Cloud geeigneten Apps finden Sie in dem Abschnitt zu [12-Faktor-Apps![Symbol für externen Link](../icons/launch-glyph.svg)](http://12factor.net/){: new_window}. 

##Apps migrieren
{: #ht_hostapp}

Sie können Ihre Apps schrittweise nach {{site.data.keyword.Bluemix_notm}} migrieren, anstatt sie vollständig in die Cloudumgebung zu verschieben. Sie können zuerst einen Teil der App migrieren und danach mit dem Service 'Cloud Integration' eine Verbindung zu vorhandenen Daten oder einem System of Record herstellen. 

Es kann erforderlich sein, in den Cloud-Apps auf die Back-End-Daten oder -Services zuzugreifen, zum Beispiel auf ein System of Record (SOR, Kerndatensystem). In {{site.data.keyword.Bluemix_notm}} können Sie den Service 'Secure Gateway' zum Herstellen eines sicheren Tunnels zwischen einer {{site.data.keyword.Bluemix_notm}}-Organisation und dem Back-End-Netz des Unternehmens verwenden. Mithilfe des Service können die Apps in {{site.data.keyword.Bluemix_notm}} auf die Daten und Services des Back-End-Netzes zugreifen. Details hierzu finden Sie unter [Reaching enterprise backend with Bluemix Secure Gateway via console ![Symbol für externen Link](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window}.

Wenn Sie eine App in {{site.data.keyword.Bluemix_notm}} als Cloud Foundry-App bereitstellen möchten, wählen Sie eine Laufzeit aus dem {{site.data.keyword.Bluemix_notm}}-Katalog aus. Bestandteil der Laufzeit ist die Startanwendung Hello World, die Sie durch eine eigene App ersetzen können. Wenn Sie keine Startanwendung finden, von der die gewünschte Laufzeit bereitgestellt wird, können Sie ein angepasstes Cloud Foundry-kompatibles Buildpack zu {{site.data.keyword.Bluemix_notm}} hinzufügen; verwenden Sie hierzu den Befehl 'cf push' mit der Option '-b'. Details hierzu finden Sie unter [Community-Buildpacks verwenden](/docs/cfapps/byob.html).

Sie können die folgenden Tools und Services verwenden, die von {{site.data.keyword.Bluemix_notm}} bereitgestellt werden:

| Tool | Methode |
|:------|:--------|
| Cloud Foundry-Befehlszeilenschnittstelle (Befehlszeilenschnittstelle 'cf') | Verwalten Sie den Code auf einem lokalen Client und verwenden Sie die Cloud Foundry-Befehlszeilenschnittstelle, um Ihre App mit einer Push-Operation manuell zu {{site.data.keyword.Bluemix_notm}} zu übertragen. Weitere Informationen finden Sie unter [Apps hochladen](/docs/starters/upload_app.html). |
| Eclipse | Verwalten Sie den Code in Eclipse und verwenden Sie IBM Eclipse Tools for {{site.data.keyword.Bluemix_notm}}, um Ihre App mit einer Push-Operation zu übertragen. |
| Git-Integration | Verwalten Sie Ihren Code auf GitHub und integrieren Sie Git in {{site.data.keyword.Bluemix_notm}}. Sie können online mit anderen Entwicklern zusammenarbeiten. Ihre App wird automatisch in {{site.data.keyword.Bluemix_notm}} bereitgestellt, wenn Sie Änderungen im Code festschreiben. Eine manuelle Push-Operation für die App ist nicht erforderlich. |
| {{site.data.keyword.Bluemix_notm}} DevOps Delivery Pipeline | Verwalten Sie Ihren Code im DevOPs GitHub-Repository und stellen Sie Ihre App mit der DevOps Delivery Pipeline erfolgreich in {{site.data.keyword.Bluemix_notm}} bereit. |
{: caption="Tabelle 1. {{site.data.keyword.Bluemix_notm}}-Tools" caption-side="top"}

Wenn die Cloud Foundry-Plattform nicht die Anforderungen der App erfüllt, können Sie einen Container oder eine virtuelle Maschine verwenden, für den bzw. die eine Laufzeit mit entsprechend angepassten Optionen konfiguriert ist und verwaltet wird.

## Entwicklung und Bereitstellung Ihrer Apps mithilfe von Toolchains in Continuous Delivery
{:ht_cd}

Fügen Sie eine [Toolchain zur App](/docs/services/ContinuousDelivery/toolchains_working.html#creating_a_toolchain_from_an_app) hinzu und verwenden Sie dann die [Toolchain-Benutzerschnittstelle von Continuous Delivery](/docs/services/ContinuousDelivery/toolchains_using.html#toolchains-using) zum Entwickeln und Bereitstellen der App.

## Apps mithilfe der Befehlszeilenschnittstelle 'cf' hochladen
{: #ht_cfcli}

Sie können Code auf einem lokalen Client verwalten und die App manuell mithilfe der Cloud Foundry-Befehlszeilenschnittstelle in {{site.data.keyword.Bluemix_notm}} hochladen. Wenn Sie den Code ändern, müssen Sie die App erneut mit einer Push-Operation zu {{site.data.keyword.Bluemix_notm}} übertragen, um den aktualisierten Code auszuführen. 

Führen Sie die folgenden Schritte aus, um die App zu migrieren: 

Installieren Sie die Cloud Foundry-Befehlszeilenschnittstelle. Stellen Sie sicher, dass Sie die neueste Version der Befehlszeilenschnittstelle 'cf' verwenden.
  1. Laden Sie das Installationsprogramm für Ihr Betriebssystem herunter.</li>
  2. Befolgen Sie die Anweisungen im Assistenten des Tools, um die Befehlszeile zu installieren.</li>
  3. Verwenden Sie den folgenden Befehl, um die Version der Befehlszeilenschnittstelle 'cf' zu überprüfen: `cf -v` 

Optional: Wenn Sie die Bereitstellungsdetails angeben und speichern möchten, bevor Sie eine App mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}} übertragen, können Sie das App-Manifest wie folgt hinzufügen: 
  1. Wechseln Sie in das Arbeitsverzeichnis der App und erstellen Sie eine Datei mit dem Namen 'manifest.yml' (der Standardname). </li>
  2. Geben Sie Bereitstellungsdetails in der Manifestdatei an. Im folgenden Beispiel wird eine Manifestdatei für eine Java™-App dargestellt.

  ```applications:
  - disk_quota: 1024M
  host: myjavatest
  name: MyJavaTest
  path: webStarterApp.war
  domain: mybluemix.net
  instances: 1
  memory: 512M
  ```
Weitere Informationen zu den unterstützten Optionen, die Sie in dieser Datei verwenden können, finden Sie unter
[App-Manifest](../manageapps/depapps.html#appmanifest).

Übertragen Sie Ihre App mit einer Push-Operation. Sie können die App mit dem Befehl 'cf push' hochladen. 
  1. Stellen Sie eine Verbindung zu {{site.data.keyword.Bluemix_notm}} her und melden Sie sich mit dem folgenden Befehl an. Wählen Sie Ihre Organisation und Ihren Bereich aus, wenn Sie dazu aufgefordert werden.
  ```
  cf login -a https://api.ng.bluemix.net
  ```
  Fügen Sie das Flag `-sso` hinzu, wenn Ihre Organisation Single Sign-on verwendet.

  2. Geben Sie über Ihr App-Verzeichnis den Befehl 'cf push' mit dem App-Namen ein. Der App-Name muss in der {{site.data.keyword.Bluemix_notm}}-Umgebung eindeutig sein.
  ```
  cf push App-Name
  ```
  3. Optional: Wenn Sie ein externes Buildpack verwenden, müssen Sie die Option '-b' mit dem Befehl 'cf push' verwenden. Beispiel:
  ```
  cf push App-Name -b Buildpack-URL
  ```
  Ausführliche Informationen finden Sie unter [Community-Buildpacks verwenden](../cfapps/byob.html).

Optional: Wenn Sie Ihre App ändern, müssen Sie diese Änderungen hochladen, indem Sie den Befehl 'cf push' erneut eingeben. Die Befehlszeilenschnittstelle 'cf' verwendet Ihre vorherigen Optionen und Ihre Antworten auf die Eingabeaufforderungen, um alle aktiven Instanzen Ihrer App mit den neuen Code-Bits zu aktualisieren. 

**Hinweise:**

* Wenn Sie den Befehl 'cf push' verwenden, kopiert die Befehlszeilenschnittstelle 'cf' alle Dateien und Verzeichnisse aus Ihrem aktuellen Verzeichnis in {{site.data.keyword.Bluemix_notm}}. Stellen Sie sicher, dass sich in Ihrem App-Verzeichnis nur die erforderlichen Dateien befinden. 
* Stellen Sie sicher, dass von Ihrer Organisation ausreichend Speicherplatz für alle Instanzen der App bereitgestellt wird. Das Speicherkontingent für Ihre Organisation können Sie mit 'cf org Organisationsname' anzeigen.
* Weitere Informationen zum Befehl 'cf push' finden Sie unter ['cf'-Befehle](../cli/reference/cfcommands/index.html).

## Daten migrieren und Services verwenden
{: #ht_service}

Nach dem Upload Ihrer App in {{site.data.keyword.Bluemix_notm}} wählen Sie den Service, mit dem die App verbunden ist, im {{site.data.keyword.Bluemix_notm}}-Katalog aus, erstellen Sie eine Serviceinstanz, binden Sie die Instanz an die App und starten Sie dann die App erneut. 

Bei der Umgebungsvariablen VCAP_SERVICES Ihrer App handelt es sich um ein JSON-Objekt mit Informationen zur Interaktion mit einer Serviceinstanz in {{site.data.keyword.Bluemix_notm}}. Die Informationen umfassen den Namen der Serviceinstanz, die Berechtigungsnachweise und die URL für die Verbindung zu der Serviceinstanz.

Wenn Sie den Code in {{site.data.keyword.Bluemix_notm}} ausführen möchten, müssen Sie die Codelogik zum Analysieren der Variablen VCAP_SERVICES hinzufügen, damit Sie Informationen zur Serviceverbindung erhalten. Ändern Sie die App, um den dynamisch zugewiesenen Host und Port der Serviceinstanz über die Umgebungsvariable abzurufen. Am folgenden Beispiel wird veranschaulicht, wie die Berechtigungsnachweise der PostgreSQL-Serviceinstanz in einer Ruby-App abgerufen werden: 

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

Wenn Sie sicherstellen möchten, dass Ihre App nach der Änderung für {{site.data.keyword.Bluemix_notm}} in der lokalen Umgebung ausgeführt werden kann, überprüfen Sie, ob die Variable VCAP_SERVICES vorhanden ist, die für alle {{site.data.keyword.Bluemix_notm}} Cloud Foundry-Apps festgelegt ist. 


# Zugehörige Links
{: #rellinks}

## Zugehörige Links
{: #general}

* [IBM Containers](/docs/containers/container_index.html)
* [Virtual Machines](/docs/virtualmachines/vm_index.html)
* [Einführung in Delivery Pipeline](/docs/services/DeliveryPipeline/index.html)
* [Apps mit IBM Eclipse Tools for Bluemix bereitstellen](/docs/manageapps/eclipsetools/eclipsetools.html)
* [The twelve-factor app ![Symbol für externen Link](../icons/launch-glyph.svg)](http://12factor.net/){: new_window}
* [Reaching enterprise backend with Bluemix Secure Gateway via console ![Symbol für externen Link](../icons/launch-glyph.svg)](https://developer.ibm.com/bluemix/2015/04/01/reaching-enterprise-backend-bluemix-secure-gateway/){: new_window}
