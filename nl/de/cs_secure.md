---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Sicherheit für {{site.data.keyword.containerlong_notm}}
{: #security}

Sie können die integrierten Sicherheitsfunktionen für die Risikoanalyse und den Sicherheitsschutz verwenden. Diese Funktionen helfen Ihnen, die Clusterinfrastruktur und Netzkommunikation zu schützen, die Berechnungsressourcen zu isolieren und die Einhaltung von Sicherheitsbestimmungen über die einzelnen Infrastrukturkomponenten und Containerbereitstellungen hinweg sicherzustellen.
{: shortdesc}

## Sicherheit nach Clusterkomponente
{: #cluster}

Jeder {{site.data.keyword.containerlong_notm}}-Cluster verfügt über Sicherheitsfunktionen, die in seine [Master](#master)- und [Worker](#worker)knoten integriert sind. Wenn Sie eine Firewall haben, auf den Lastausgleich von außerhalb des Clusters zugreifen müssen oder `kubectl`-Befehle aus Ihrem lokalen System ausführen möchten, aber Unternehmensnetzrichtlinien den Zugriff auf öffentliche Internetendpunkte verhindern, [öffnen Sie Ports in Ihrer Firewall](cs_firewall.html#firewall). Wenn Sie Apps in Ihrem Cluster mit einem lokalen Netz oder mit anderen Apps, die sich außerhalb Ihres Netzes befinden, verbinden möchten, [richten Sie VPN-Konnektivität ein](cs_vpn.html#vpn).
{: shortdesc}

Das folgende Diagramm zeigt die Sicherheitsfunktionen gruppiert nach Kubernetes-Master, Workerknoten und Container-Images.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}}-Clustersicherheit" style="width:400px; border-style: none"/>


  <table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Der Rest der Zeilen sollte von links nach rechts gelesen werden, wobei die Serverposition in der ersten Spalte und die passenden IP-Adressen in der zweiten Spalte angegeben sind.">
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Integrierte Clustersicherheitseinstellungen in {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes-Master</td>
      <td>Der Kubernetes-Master in jedem Cluster wird von IBM verwaltet und ist hoch verfügbar. Er enthält Sicherheitseinstellungen für {{site.data.keyword.containershort_notm}}, mit denen die Einhaltung der Sicherheitsbestimmungen und eine sichere Kommunikation von und zu den Workerknoten sichergestellt wird. Aktualisierungen werden von IBM bei Bedarf durchgeführt. Vom dedizierten Kubernetes-Master werden alle Kubernetes-Ressourcen im Cluster zentral gesteuert und überwacht. Abhängig von den Bereitstellungsvoraussetzungen und der Kapazität im Cluster werden die containerisierten Apps vom Kubernetes-Master automatisch zur Bereitstellung für die verfügbaren Workerknoten geplant. Weitere Informationen finden Sie unter [Sicherheit des Kubernetes-Masters](#master).</td>
    </tr>
    <tr>
      <td>Workerknoten</td>
      <td>Container werden auf Workerknoten bereitgestellt, die der Nutzung durch einen Cluster vorbehalten sind und gegenüber IBM Kunden die Berechnungs-, Netz- und Speicherisolation sicherstellen. {{site.data.keyword.containershort_notm}} stellt integrierte Sicherheitsfunktionen zur Verfügung, um die Sicherheit
Ihrer Workerknoten im privaten und im öffentlichen Netz sicherzustellen und für die Einhaltung von Sicherheitsbestimmungen für Workerknoten zu sorgen. Weitere Informationen enthält [Sicherheit von Workerknoten](#worker). Zusätzlich können Sie [Calico-Netzrichtlinien](cs_network_policy.html#network_policies) hinzufügen, um den Netzverkehr anzugeben, den Sie zu und von einem Pod in einem Workerknoten zulassen oder blockieren. </td>
     </tr>
     <tr>
      <td>Images</td>
      <td>Als Clusteradministrator können Sie ein eigenes sicheres Docker-Image-Repository in {{site.data.keyword.registryshort_notm}} einrichten, in dem Sie Docker-Images sicher speichern und zur gemeinsamen Nutzung durch Ihre Clusterbenutzer freigeben können. Jedes Image in Ihrer privaten Registry wird von Vulnerability Advisor überprüft, um die Bereitstellung sicherer Container sicherzustellen. Vulnerability Advisor ist eine Komponente von {{site.data.keyword.registryshort_notm}}, die nach potenziellen Sicherheitslücken sucht, Sicherheitsempfehlungen ausgibt und Anweisungen zur Schließung von Sicherheitslücken bereitstellt. Weitere Informationen finden Sie unter [Imagesicherheit in {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Kubernetes-Master
{: #master}

Prüfen Sie die integrierten Sicherheitsfeatures, die den Kubernetes-Master schützt und die Netzkommunikation für den Cluster sichert.
{: shortdesc}

<dl>
  <dt>Vollständig verwalteter und dedizierter Kubernetes-Master</dt>
    <dd>Jeder Kubernetes-Cluster in {{site.data.keyword.containershort_notm}} wird von einem dedizierten Kubernetes-Master gesteuert, der von IBM in einem IBM eigenen Konto von IBM Cloud Infrastructure (SoftLayer) verwaltet wird. Der Kubernetes-Master ist mit den folgenden dedizierten Komponenten konfiguriert, die nicht mit anderen IBM Kunden gemeinsam genutzt werden.
    <ul><li>Datenspeicher 'etcd': Speichert alle Kubernetes-Ressourcen eines Clusters, z. B. Services, Bereitstellungen und Pods. Kubernetes-Konfigurationsübersichten und geheime Kubernetes-Schlüssel sind App-Daten, die in Form von Schlüssel/Wert-Paaren gespeichert werden, damit sie von einer in einem Pod ausgeführten App verwendet werden können. Daten in 'etcd' werden auf einem verschlüsselten
Datenträger gespeichert, der von IBM verwaltet wird, und mit TLS verschlüsselt, wenn sie an einen Pod gesendet werden, um Datenschutz und die Datenintegrität sicherzustellen.</li>
    <li>'kube-apiserver': Dient als Haupteinstiegspunkt für alle Anforderungen vom Workerknoten zum Kubernetes-Master. Die Komponente 'kube-apiserver' validiert und verarbeitet Anforderungen und besitzt Lese- und Schreibberechtigungen für den Datenspeicher 'etcd'.</li>
    <li>'kube-scheduler': Entscheidet unter Berücksichtigung von Kapazitätsbedarf und Leistungsanforderungen, Beschränkungen durch Hardware- und Softwarerichtlinien, Anti-Affinitäts-Spezifikationen und Anforderungen für Arbeitslasten darüber, wo die Bereitstellung von Pods erfolgt. Wird kein Workerknoten gefunden, der mit den Anforderungen übereinstimmt, so wird der Pod nicht im Cluster bereitgestellt.</li>
    <li>'kube-controller-manager': Verantwortlich für die Überwachung von Replikatgruppen und für die Erstellung entsprechender Pods, um den gewünschten Zustand (Soll-Status) zu erreichen.</li>
    <li>'OpenVPN': Für {{site.data.keyword.containershort_notm}} spezifische Komponente, die sichere Netzkonnektivität für die gesamte Kommunikation zwischen dem Kubernetes-Master und dem Workerknoten bereitstellt.</li></ul></dd>
  <dt>Mit TLS geschützte Netzkonnektivität für die gesamte Kommunikation von den Workerknoten zum Kubernetes-Master</dt>
    <dd>{{site.data.keyword.containershort_notm}} schützt die Netzkommunikation zum Kubernetes-Master durch Generieren von TLS-Zertifikaten, mit denen für jeden Cluster die Kommunikation zu und von den Komponenten 'kube-apiserver' und 'etcd' verschlüsselt wird. Diese Zertifikate werden zu keinem Zeitpunkt von mehreren Clustern oder Komponenten des Kubernetes-Masters gemeinsam genutzt.</dd>
  <dt>Mit OpenVPN geschützte Netzkonnektivität für die gesamte Kommunikation vom Kubernetes-Master zu den Workerknoten</dt>
    <dd>Kubernetes schützt die Kommunikation zwischen dem Kubernetes-Master und den Workerknoten zwar durch die Verwendung des Protokolls `https`, doch auf dem Workerknoten wird keine standardmäßige Authentifizierung bereitgestellt. Zum Schutz dieser Kommunikation richtet {{site.data.keyword.containershort_notm}} bei der Erstellung des Clusters automatisch eine OpenVPN-Verbindung zwischen dem Kubernetes-Master und den Workerknoten ein.</dd>
  <dt>Kontinuierliche Überwachung des Kubernetes-Master-Netzes</dt>
    <dd>Jeder Kubernetes-Master wird kontinuierlich von IBM überwacht, um Denial-of-Service-Attacken (DoS) auf Prozessebene zu steuern und zu korrigieren.</dd>
  <dt>Einhaltung der Sicherheitsbestimmungen durch die Kubernetes-Master-Knoten</dt>
    <dd>{{site.data.keyword.containershort_notm}} überprüft automatisch jeden Knoten, auf dem der Kubernetes-Master bereitgestellt ist, auf Schwachstellen bzw. Sicherheitslücken, die in Kubernetes festgestellt wurden, und auf betriebssystemspezifische Korrekturen (Fixes) für die Sicherheit, die gegebenenfalls angewendet werden müssen, um den Schutz des Masterknotens sicherzustellen. Werden Schwachstellen bzw. Sicherheitslücken festgestellt, wendet {{site.data.keyword.containershort_notm}} automatisch entsprechende Korrekturen (Fixes) und beseitigt Schwachstellen bzw. Sicherheitslücken zugunsten des Benutzers.</dd>
</dl>

<br />


## Workerknoten
{: #worker}

Prüfen Sie die integrierten Sicherheitsfeatures für Workerknoten, die die Workerknotenumgebung schützen und die Ressourcen-, Netz- und
Speicherisolation sicherstellen.
{: shortdesc}

<dl>
  <dt>Eigentumsrechte für Workerknoten</dt>
    <dd>Die Eigentumsrechte für Workerknoten sind vom Typ des Clusters abhängig, den Sie erstellen.<p> Workerknoten in kostenlosen Clustern werden im Konto der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, dessen Kontoeigner IBM ist. Benutzer können zwar Apps in den Workerknoten bereitstellens, sie können in den Workerknoten jedoch weder Einstellungen ändern noch zusätzliche Software installieren. </p> <p>Workerknoten in Standardclustern werden im Konto der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, das dem öffentlichen oder dedizierten IBM Cloud-Konto des Kunden zugeordnet ist. Die Workerknoten sind Eigentum des Kunden, aber IBM verwaltet den Zugriff auf die Workerknoten, um Aktualisierungen und Sicherheitspatches für das Betriebssystem bereitzustellen. Die Kunden können bei Bedarf Sicherheitseinstellungen ändern oder zusätzliche Software auf den Workerknoten installieren, die von IBM Cloud Container Service bereitgestellt werden.</p> </dd>
  <dt>Isolation der Berechnungs-, Netz- und Speicherinfrastruktur</dt>
    <dd>Beim Erstellen eines Clusters werden Workerknoten von IBM als virtuelle Maschinen im Portfolio der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt. Workerknoten sind einem Cluster zugeordnet und hosten nicht die Arbeitslast anderer Cluster.<p> Jedes {{site.data.keyword.Bluemix_notm}}-Konto wird mit VLANs von IBM Cloud Infrastructure (SoftLayer) eingerichtet, um die Qualität der Netzleistung und Isolation auf den Workerknoten sicherzustellen. Darüber hinaus können Sie Workerknoten als privat deklarieren, indem Sie sie nur mit einem privaten VLAN verbinden. </p> <p>Damit Daten in Ihrem Cluster als persistent erhalten bleiben, können Sie den dedizierten NFS-basierten Dateispeicher von IBM Cloud Infrastructure (SoftLayer) einrichten und die integrierten Funktionen für Datensicherheit dieser Plattform nutzen.</p></dd>
  <dt>Einrichtung geschützter Workerknoten</dt>
    <dd>Jeder Workerknoten wird mit dem Betriebssystem Ubuntu eingerichtet, das vom Eigner des Workerknotens nicht geändert werden kann. Um das Betriebssystem der Workerknoten gegen potenzielle Angriffe zu schützen, wird jeder Workerknoten mit extrem fortgeschrittenen Firewalleinstellungen konfiguriert, die durch iptables-Regeln von Linux umgesetzt werden.<p> Alle Container, die auf Kubernetes ausgeführt werden, sind durch vordefinierte Calico-Netzrichtlinieneinstellungen geschützt, die bei der Clustererstellung auf jedem Workerknoten konfiguriert werden. Diese Konstellation stellt die sichere Netzkommunikation zwischen Workerknoten und Pods sicher. Um die Aktionen, die ein Container auf einem Workerknoten in einem Standardcluster ausführen kann, weiter einzuschränken, können die Benutzer [AppArmor-Richtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tutorials/clusters/apparmor/) auf den Workerknoten konfigurieren.</p><p> SSH-Zugriff ist auf dem Workerknoten inaktiviert. Wenn Sie über einen Standardcluster verfügen und zusätzliche Features auf Ihrem Workerknoten installieren möchten, können Sie [Kubernetes-Dämon-Sets ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) für Vorgänge verwenden, die auf jedem Workerknoten ausgeführt werden sollen, bzw. [Kubernetes-Jobs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) für alle einmaligen Aktionen, die Sie ausführen müssen.</p></dd>
  <dt>Einhaltung der Sicherheitsbestimmungen für die Kubernetes-Workerknoten</dt>
    <dd>IBM arbeitet mit internen und externen Beratungsteams für Sicherheit zusammen, um potenzielle Schwachstellen in Bezug auf die Einhaltung von Sicherheitsbestimmungen zu beseitigen. IBM verwaltet den Zugriff auf die Workerknoten, um Aktualisierungen und Sicherheitspatches für das Betriebssystem bereitzustellen.<p> <b>Wichtig</b>: Führen Sie regelmäßig einen Warmstart für Ihre Workerknoten durch, um sicherzustellen, dass die Installation von Aktualisierungen und Sicherheitspatches, die automatisch für das Betriebssystem bereitgestellt werden, auch durchgeführt wird. IBM führt für Ihre Workerknoten keinen Warmstart durch.</p></dd>
  <dt id="encrypted_disks">Verschlüsselte Platte</dt>
  <dd>Standardmäßig stellt {{site.data.keyword.containershort_notm}} zwei lokale SSD-verschlüsselte Datenpartitionen für alle Workerknoten bereit, wenn diese Workerknoten bereitgestellt werden. Die erste Partition ist nicht verschlüsselt, und die zweite Partition, die an _/var/lib/docker_ angehängt ist, wird mithilfe von LUKS-Verschlüsselungsschlüsseln entsperrt. Alle Worker in den einzelnen Kubernetes-Clustern haben ihren eigenen eindeutigen LUKS-Verschlüsselungsschlüssel, der von {{site.data.keyword.containershort_notm}} verwaltet wird. Wenn Sie einen Cluster erstellen oder einen Workerknoten zu einem vorhandenen Cluster hinzufügen, werden die Schlüssel sicher extrahiert und nach dem Entsperren der verschlüsselten Platte gelöscht.
  <p><b>Hinweis</b>: Verschlüsselung kann sich negativ auf die Platten-E/A-Leistung auswirken. Für Arbeitslasten, die eine eine leistungsfähige Platten-E/A erfordern, sollten Sie einen Cluster mit aktivierter und mit inaktivierter Verschlüsselung testen, um besser entscheiden zu können, ob die Verschlüsselung ausgeschaltet werden muss.</p>
  </dd>
  <dt>Unterstützung für Netzfirewalls von IBM Cloud Infrastructure (SoftLayer)</dt>
    <dd>{{site.data.keyword.containershort_notm}} ist mit allen [Firewallangeboten von IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") kompatibel](https://www.ibm.com/cloud-computing/bluemix/network-security). Sie können unter {{site.data.keyword.Bluemix_notm}} Public eine Firewall mit angepassten Netzrichtlinien einrichten, um für Ihren Standardcluster dedizierte Netzsicherheit bereitzustellen und unbefugten Zugriff zu erkennen und zu unterbinden. Sie können beispielsweise [Vyatta ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://knowledgelayer.softlayer.com/topic/vyatta-1) als Ihre Firewall und zum Blockieren unerwünschten Datenverkehrs einrichten. Wenn Sie eine Firewall einrichten, [müssen Sie auch die erforderlichen Ports und IP-Adressen für die einzelnen Regionen öffnen](cs_firewall.html#firewall), damit der Master und die Workerknoten kommunizieren können.</dd>
  <dt>Services privat belassen oder Service und Apps selektiv dem öffentlichen Internet zugänglich machen</dt>
    <dd>Sie können entscheiden, ob Sie Ihre Services und Apps privat belassen wollen. Durch Nutzung der in diesem Abschnitt beschriebenen integrierten
Sicherheitsfeatures können Sie außerdem die geschützte Kommunikation zwischen Workerknoten und Pods sicherstellen. Um Services und Apps dem öffentlichen Internet zugänglich zu machen, können Sie die Ingress-Unterstützung und die
Unterstützung der Lastausgleichsfunktion nutzen, um Ihre Services auf sichere Weise öffentlich verfügbar zu machen.</dd>
  <dt>Sichere Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum</dt>
  <dd>Um eine Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum einzurichten, können Sie einen VPN-IPSec-Endpunkt mit einem Strongswan-Service oder mit einer Vyatta-Gateway-Appliance bzw. einer Fortigate-Appliance konfigurieren.<br><ul><li><b>Strongswan-IPSec-VPN-Service</b>: Sie können einen [Strongswan-IPSec-VPN-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.strongswan.org/) konfigurieren, der Ihren Kubernetes-Cluster sicher mit einem lokalen Netz verbindet. Der Strongswan-IPSec-VPN-Service stellt einen sicheren End-to-End-Kommunikationskanal über das Internet bereit, der auf der standardisierten IPsec-Protokollsuite (IPsec - Internet Protocol Security) basiert. Um eine sichere Verbindung zwischen Ihrem Cluster und einem lokalen Netz einrichten zu können, muss in Ihrem Rechenzentrum vor Ort ein IPsec-VPN-Gateway oder eine IBM Cloud Infrastructure (SoftLayer)-Server installiert sein. Dann können Sie den [Strongswan-IPSec-VPN-Service](cs_vpn.html#vpn) in einem Kubernetes-Pod konfigurieren und bereitstellen.</li><li><b>Vyatta-Gateway-Appliance oder Fortigate-Appliance</b>: Wenn Ihr Cluster größer ist, können Sie eine Vyatta-Gateway-Appliance oder eine Fortigate-Appliance einrichten, um einen IPSec-VPN-Endpunkt zu konfigurieren. Weitere Informationen finden Sie in diesem Blogbeitrag zum Thema [Verbinden eines Clusters mit einem lokalen Rechenzentrum ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).</li></ul></dd>
  <dt>Kontinuierliche Überwachung und Protokollierung der Clusteraktivität</dt>
    <dd>Bei Standardclustern können alle clusterbezogenen Ereignisse wie das Hinzufügen eines Workerknotens, der Fortschritt bei einer rollierenden Aktualisierung oder Informationen zur Kapazitätsnutzung von {{site.data.keyword.containershort_notm}} protokolliert, überwacht und an {{site.data.keyword.loganalysislong_notm}} und {{site.data.keyword.monitoringlong_notm}} gesendet werden. Weitere Informationen zum Einrichten der Protokollierung und Überwachung finden Sie unter [Clusterprotokollierung konfigurieren](/docs/containers/cs_health.html#logging) und [Clusterüberwachung konfigurieren](/docs/containers/cs_health.html#monitoring).</dd>
</dl>

<br />


## Images
{: #images}

Verwenden Sie integrierte Sicherheitsfunktionen, um die Sicherheit und Integrität Ihrer Images zu verwalten.
{: shortdesc}

<dl>
<dt>Geschütztes privates Docker-Image-Repository in {{site.data.keyword.registryshort_notm}}</dt>
<dd>Sie können ein eigenes Docker-Image-Repository in einer hoch verfügbaren, skalierbaren, privaten Multi-Tenant-Image-Registry einrichten, die von IBM gehostet und verwaltet wird, um Docker-Images zu erstellen, sicher zu speichern und gemeinsam mit anderen Clusterbenutzern zu nutzen.</dd>

<dt>Einhaltung von Sicherheitsbestimmungen für Images</dt>
<dd>Wenn Sie {{site.data.keyword.registryshort_notm}} verwenden, können Sie die integrierte Sicherheitsfunktion zum Scannen einsetzen, die Vulnerability Advisor bereitstellt. Jedes Image, das mit einer Push-Operation an Ihren Namensbereich übertragen wird, wird automatisch auf Schwachstellen und Sicherheitslücken gescannt. Dieser Scanvorgang erfolgt im Abgleich mit einer Datenbank mit bekannten CentOS-, Debian-, Red Hat- und Ubuntu-Problemen. Werden derartige Schwachstellen oder Sicherheitslücken festgestellt, stellt Vulnerability Advisor Anweisung dazu bereit, wie Sie diese beseitigen bzw. schließen, um die Integrität und Sicherheit des Image sicherzustellen.</dd>
</dl>

Informationen darüber, wie Sie die Schwachstellenanalyse für Ihre Images anzeigen, finden Sie in der [Dokumentation zu Vulnerability Advisor](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Netzbetrieb in Clustern
{: #in_cluster_network}

Die geschützte clusterinterne Netzkommunikation zwischen Workerknoten und Pods erfolgt über private virtuelle LANs (VLANs). Ein VLAN konfiguriert eine Gruppe von Workerknoten und Pods so, als wären diese an dasselbe physische Kabel angeschlossen.
{:shortdesc}

Wenn Sie einen Cluster erstellen, wird jeder Cluster automatisch mit einem privaten VLAN von IBM verbunden. Das private VLAN bestimmt, welche private IP-Adresse einem Workerknoten bei der Clustererstellung zugewiesen wird.

|Clustertyp|Manager des privaten VLANs für den Cluster|
|------------|-------------------------------------------|
|Kostenlose Cluster in {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Standardcluster in {{site.data.keyword.Bluemix_notm}}|Sie bei Ihrem Konto von IBM Cloud Infrastructure (SoftLayer) <p>**Tipp:** Für den Zugriff auf alle VLANs in Ihrem Konto aktivieren Sie [VLAN-Spanning ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://knowledgelayer.softlayer.com/procedure/enable-or-disable-vlan-spanning).</p>|

Allen Pods, die auf einem Workerknoten bereitgestellt werden, wird ebenfalls eine private IP-Adresse zugewiesen. Pods wird eine
IP im privaten Adressbereich '172.30.0.0/16' zugewiesen und eine Weiterleitung findet nur zwischen Workerknoten statt. Vermeiden Sie Konflikte, indem Sie diesen IP-Bereich nicht auf Knoten verwenden, die mit Ihren Workerknoten kommunizieren. Workerknoten und Pods können im privaten Netz durch die Verwendung der privaten IP-Adressen sicher kommunizieren. Wenn ein Pod ausfällt oder ein Workerknoten neu erstellt werden muss, wird jedoch eine neue private IP-Adresse zugewiesen.

Da es schwierig ist, sich ändernde private IP-Adressen für Apps nachzuverfolgen, die hoch verfügbar sein müssen,
können Sie die integrierten Erkennungsfunktionen des Kubernetes-Service nutzen und Apps als Cluster-IP-Services auf dem privaten Netz im Cluster zugänglich machen. Ein Kubernetes-Service fassen eine Gruppe von Pods zusammen und stellen diesen Pods eine Netzverbindung für andere Services im Cluster bereit, ohne hierbei die tatsächlichen IP-Adressen der einzelnen Pods preiszugeben. Wenn Sie einen Cluster-IP-Service erstellen, wird dem Service eine private IP-Adresse aus dem privaten Adressbereich '10.10.10.0/24' zugewiesen. Verwenden Sie, wie beim privaten Pod-Adressbereich, diesen IP-Bereich nicht auf Knoten, die mit Ihren Workerknoten kommunizieren. Auf diese IP-Adresse kann nur aus dem Cluster selbst heraus zugegriffen werden. Vom Internet aus ist der Zugriff auf diese IP-Adresse nicht möglich. Zur gleichen Zeit wird für den Service ein Eintrag für die DNS-Suche erstellt und in der Komponente 'kube-dns' des Clusters gespeichert. Der DNS-Eintrag enthält den Namen des Service, den Namensbereich, in dem der Service erstellt wurde und den Link zur zugewiesenen privaten IP-Adresse des Clusters.

Wenn eine App im Cluster Zugriff auf einen Pod benötigt, der sich hinter einem Cluster-IP-Service befindet, kann sie entweder die private Cluster-IP-Adresse verwenden, die dem Service zugewiesen wurde, oder eine Anforderung unter Nennung des Servicenamens senden. Wenn Sie den Namen des Service nennen, wird dieser in der 'kube-dns'-Komponente gesucht und an die private Cluster-IP-Adresse des Service weitergeleitet. Wenn
eine Anforderung den Service erreicht, stellt der Service sicher, dass alle Anforderungen im gleichen Maße an die Pods weitergeleitet werden, und zwar unabhängig von ihren jeweiligen privaten IP-Adressen und dem Workerknoten, auf dem sie bereitgestellt sind.

Weitere Informationen zum Erstellen eines Service vom Typ 'Cluster-IP' finden Sie unter [Kubernetes-Services ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

Informationen über sichere Verbindungen von Apps in einem Kubernetes-Cluster zu einem lokalen Netz finden Sie in [VPN-Konnektivität einrichten](cs_vpn.html#vpn). Informationen zum Bereitstellen Ihrer Apps für die externe Netzkommunikation finden Sie in [Externen Netzbetrieb planen](cs_network_planning.html#planning).
