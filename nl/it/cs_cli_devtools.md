---

copyright:
  years: 2014, 2018
lastupdated: "2017-01-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Riferimenti CLI per la gestione dei cluster
{: #cs_cli_reference}

Fai riferimento a questi comandi per creare e gestire i cluster.
{:shortdesc}

**Suggerimento:** Stai cercando i comandi `bx cr`? Consulta i riferimenti CLI [{{site.data.keyword.registryshort_notm}} ](/docs/cli/plugins/registry/index.html). Stai cercando i comandi `kubectl`? Consulta la [Documentazione Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).




<table summary="Comandi per la creazione dei cluster in {{site.data.keyword.Bluemix_notm}}">
 <thead>
    <th colspan=5>Comandi per la creazione dei cluster in {{site.data.keyword.Bluemix_notm}}</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs albs](#cs_albs)</td>
    <td>[bx cs alb-configure](#cs_alb_configure)</td>
    <td>[bx cs alb-get](#cs_alb_get)</td>
    <td>[bx cs alb-types](#cs_alb_types)</td>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
  </tr>
 <tr>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
 </tr>
 <tr>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
 </tr>
 <tr>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[      bx cs clusters
      ](#cs_clusters)</td>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[  bx cs credentials-unset
  ](#cs_credentials_unset)</td>
    <td>[  bx cs help
  ](#cs_help)</td>
 </tr>
 <tr>
    <td>[        bx cs init
        ](#cs_init)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
    <td>[  bx cs locations
  ](#cs_datacenters)</td>
    <td>[bx cs logging-config-create](#cs_logging_create)</td>
    <td>[bx cs logging-config-get](#cs_logging_get)</td>
 </tr>
 <tr>
    <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    <td>[bx cs logging-config-update](#cs_logging_update)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[    bx cs subnets
    ](#cs_subnets)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
 </tr>
 <tr>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
    <td>[bx cs worker-add](#cs_worker_add)</td>
    <td>[bx cs worker-get](#cs_worker_get)</td>
    <td>[bx cs worker-rm](#cs_worker_rm)</td>
    <td>[bx cs worker-update](#cs_worker_update)</td>
 </tr>
 <tr>
    <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
    <td>[bx cs worker-reload](#cs_worker_reload)</td>
    <td>[bx cs workers](#cs_workers)</td>
 </tr>
 </tbody>
 </table>

**Suggerimento:** per visualizzare la versione del plugin {{site.data.keyword.containershort_notm}}, esegui il seguente comando.

```
bx plugin list
```
{: pre}

## Comandi bx cs
{: #cs_commands}

### bx cs albs --cluster CLUSTER
{: #cs_albs}

Visualizza lo stato di tutti i programmi di bilanciamento del carico dell'applicazione in un cluster. Se non viene restituito alcun ID del programma di bilanciamento del carico dell'applicazione, il cluster non ha una sottorete portatile. Puoi [creare](#cs_cluster_subnet_create) o [aggiungere](#cs_cluster_subnet_add) le sottoreti a un cluster.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>Il nome o l'ID del cluster in cui elenchi i programmi di bilanciamento del carico dell'applicazione disponibili. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs albs --cluster mycluster
  ```
  {: pre}



### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

Abilita o disabilita un programma di bilanciamento del carico dell'applicazione nel tuo cluster standard. Il programma di bilanciamento del carico dell'applicazione pubblico è abilitato per impostazione predefinita.

**Opzioni comando**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>L'ID di un programma di bilanciamento del carico dell'applicazione. Esegui <code>bx cs albs <em>--cluster </em>CLUSTER</code> per visualizzare gli ID dei programmi di bilanciamento del carico dell'applicazione in un cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--enable</code></dt>
   <dd>Includi questo indicatore per abilitare un programma di bilanciamento del carico dell'applicazione in un cluster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Includi questo indicatore per disabilitare un programma di bilanciamento del carico dell'applicazione in un cluster.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>Questo parametro è disponibile solo per un programma di bilanciamento del carico dell'applicazione privato</li>
    <li>Il programma di bilanciamento del carico dell'applicazione privato viene distribuito con un indirizzo IP da una sottorete privata fornita dall'utente. Se non viene fornito alcun indirizzo IP, il programma di bilanciamento del carico dell'applicazione viene distribuito con un indirizzo IP privato dalla sottorete privata portatile che è stata fornita automaticamente quando hai creato il cluster.</li>
   </ul>
   </dd>
   </dl>

**Esempi**:

  Esempio per abilitare un programma di bilanciamento del carico dell'applicazione:

  ```
  bx cs alb-configure --albID my_alb_id --enable
  ```
  {: pre}

  Esempio per disabilitare un programma di bilanciamento del carico dell'applicazione:

  ```
  bx cs alb-configure --albID my_alb_id --disable
  ```
  {: pre}

  Esempio per abilitare un programma di bilanciamento del carico dell'applicazione con un indirizzo IP fornito dall'utente:

  ```
  bx cs alb-configure --albID my_private_alb_id --enable --user-ip user_ip
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

Visualizza i dettagli di un programma di bilanciamento del carico dell'applicazione.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>L'ID di un programma di bilanciamento del carico dell'applicazione. Esegui <code>bx cs albs --cluster <em>CLUSTER</em></code> per visualizzare gli ID dei programmi di bilanciamento del carico dell'applicazione in un cluster. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs alb-get --albID ALB_ID
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

Visualizza i tipi di programmi di bilanciamento del carico dell'applicazione che sono supportati nella regione.

<strong>Opzioni comando</strong>:

   Nessuno

**Esempio**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Imposta un'opzione per una configurazione del server API Kubernetes del cluster. Questo comando deve essere combinato con uno dei seguenti comandi secondari per l'opzione di configurazione che vuoi impostare.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

Visualizza l'URL del servizio di registrazione remoto a cui stai inviando i log di controllo del server API. L'URL è stato specificato quando hai creato il backend webhook per la configurazione del server API.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Imposta il backend webhook per la configurazione del server API. Il backend webhook inoltra i log di controllo del server API a un server remoto. Una configurazione webhook viene creata in base alle informazioni che fornisci negli indicatori di questo comando. Se non fornisci alcuna informazione negli indicatori, viene utilizzata una configurazione webhook predefinita.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>L'URL o l'indirizzo IP del servizio di registrazione remoto a cui vuoi inviare i log di controllo. Se fornisci un URL del server non sicuro, vengono ignorati tutti i certificati. Questo valore è facoltativo.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>Il percorso del file di un certificato CA utilizzato per verificare il servizio di registrazione remoto. Questo valore è facoltativo.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>Il percorso del file di un certificato client utilizzato per l'autenticazione con il servizio di registrazione remoto. Questo valore è facoltativo.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>Il percorso del file della chiave client corrispondente utilizzata per il collegamento al servizio di registrazione remoto. Questo valore è facoltativo.</dd>
   </dl>

**Esempio**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Disabilita la configurazione backend webhook per il server API del cluster. La disabilitazione del backend webhook arresta l'inoltro dei log di controllo del server API a un server remoto.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

Riavvia il master Kubernetes nel cluster per applicare le modifiche alla configurazione del server API.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

Dopo aver eseguito l'accesso, scarica i certificati e i dati di configurazione Kubernetes per collegare il tuo cluster ed eseguire i comandi `kubectl`. I file vengono scaricati in `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Opzioni comando**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--admin</code></dt>
   <dd>Scarica i certificati TLS e i file di autorizzazione per il ruolo Super utente. Puoi utilizzare i certificati per automatizzare le attività senza doverti riautenticare. I file vengono scaricati in `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Questo valore è facoltativo.</dd>

   <dt><code>--export</code></dt>
   <dd>Scarica i certificati e i dati di configurazione Kubernetes senza un messaggio diverso dal comando export. Poiché non viene visualizzato alcun messaggio, puoi utilizzare questo indicatore quando crei gli script automatizzati. Questo valore è facoltativo.</dd>
   </dl>

**Esempio**:

```
bx cs cluster-config my_cluster
```
{: pre}



### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt]
{: #cs_cluster_create}

Per creare un cluster nella tua organizzazione.

<strong>Opzioni comando</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>Il percorso del file YAML per creare il tuo cluster standard. Invece di definire le caratteristiche del cluster utilizzando le opzioni fornite in questo comando, puoi utilizzare un file YAML.  Questo valore è facoltativo per i cluster standard e non è disponibile per i cluster gratuiti.

<p><strong>Nota:</strong> se fornisci la stessa opzione nel comando come parametro nel file YAML, il valore nel comando ha la precedenza rispetto al valore nel YAML. Ad esempio, definisci un'ubicazione nel tuo file YAML e utilizzi l'opzione <code>--location</code> nel comando, il valore che hai immesso nell'opzione del comando sovrascrive il valore nel file YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_vlan&gt;</em>
public-vlan: <em>&lt;public_vlan&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>

</code></pre>


<table>
    <caption>Tabella 1.Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Sostituisci <code><em>&lt;cluster_name&gt;</em></code> con un nome per il tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>Sostituisci <code><em>&lt;location&gt;</em></code> con l'ubicazione in cui vuoi creare il tuo cluster. Le ubicazioni disponibili dipendono dalla regione in cui ha eseguito l'accesso. Per elencare le ubicazioni disponibili, esegui <code>bx cs locations</code>. </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>Per impostazione predefinita, sulla rete VLAN associata al cluster vengono create sottoreti portatili sia pubbliche che private. Sostituisci <code><em>&lt;no-subnet&gt;</em></code> con <code><em>true</em></code> per evitare di creare sottoreti con il cluster. Puoi [creare](#cs_cluster_subnet_create) o [aggiungere](#cs_cluster_subnet_add) le sottoreti a un cluster in seguito.</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>Sostituisci <code><em>&lt;machine_type&gt;</em></code> con il tipo di macchina che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare tutti i tipi di macchina disponibili per la tua ubicazione, esegui <code>bx cs machine-types <em>&lt;location&gt;</em></code>.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>Sostituisci <code><em>&lt;private_vlan&gt;</em></code> con l'ID della VLAN privata che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare le VLAN disponibili, esegui <code>bx cs vlans <em>&lt;location&gt;</em></code> e ricerca i router VLAN che iniziano con <code>bcr</code> (router di back-end).</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>Sostituisci <code><em>&lt;public_vlan&gt;</em></code> con l'ID della VLAN pubblica che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare le VLAN disponibili, esegui <code>bx cs vlans <em>&lt;location&gt;</em></code> e ricerca i router VLAN che iniziano con <code>fcr</code> (router di front-end).</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>Il livello di isolamento hardware per il nodo di lavoro. Utilizza dedicato se vuoi avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è
<code>condiviso</code>.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>Sostituisci <code><em>&lt;number_workers&gt;</em></code> con il numero di nodi di lavoro che vuoi distribuire.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>La versione di Kubernetes per il nodo master del cluster. Questo valore è facoltativo. Se non specificato, il cluster viene creato con l'impostazione predefinita delle versioni di Kubernetes supportate. Per visualizzare le versioni disponibili. esegui <code>bx cs kube-versions</code>.</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Codifica del disco della funzione dei nodi di lavoro predefinita; [learn more](cs_secure.html#worker). Per disabilitare la codifica, includi questa opzione e imposta il valore su <code>false</code>.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Il livello di isolamento hardware per il nodo di lavoro. Utilizza dedicato per avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è condiviso.  Questo valore è facoltativo per i cluster standard e non è disponibile per i cluster gratuiti.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>L'ubicazione in cui vuoi creare il cluster. Le ubicazioni disponibili
dipendono dalla regione {{site.data.keyword.Bluemix_notm}} in cui hai
eseguito l'accesso. Per
prestazioni ottimali, seleziona la regione fisicamente più vicina a te.  Questo valore è obbligatorio per i cluster standard ed è facoltativo per i cluster gratuiti.

<p>Controlla le [ubicazioni disponibili](cs_regions.html#locations).
</p>

<p><strong>Nota:</strong> quando selezioni una posizione ubicata al di fuori del tuo paese, tieni presente che potresti aver bisogno di un'autorizzazione legale prima di poter fisicamente archiviare i dati in un paese straniero.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Il tipo di macchina che scegli influisce sulla quantità di memoria e spazio disco
disponibile per i contenitori distribuiti al tuo nodo di lavoro. Per elencare i tipi di macchina disponibili, consulta [bx cs machine-types <em>LOCATION</em>](#cs_machine_types).  Questo valore è obbligatorio per i cluster standard e non è disponibile per i cluster gratuiti.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Il nome del cluster.  Questo valore è obbligatorio.</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>La versione di Kubernetes per il nodo master del cluster. Questo valore è facoltativo. Se non specificato, il cluster viene creato con l'impostazione predefinita delle versioni di Kubernetes supportate. Per visualizzare le versioni disponibili. esegui <code>bx cs kube-versions</code>.</dd>

<dt><code>--no-subnet</code></dt>
<dd>Per impostazione predefinita, sulla rete VLAN associata al cluster vengono create sottoreti portatili sia pubbliche che private. Includi l'indicatore <code>--no-subnet</code> per evitare di creare sottoreti con il cluster. Puoi [creare](#cs_cluster_subnet_create) o [aggiungere](#cs_cluster_subnet_add) le sottoreti a un cluster in seguito.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Questo parametro non è disponibile per i cluster gratuiti.</li>
<li>Se si tratta del primo cluster standard che crei in questa ubicazione, non includere questo indicatore. Alla creazione dei cluster, viene creata per te una VLAN privata.</li>
<li>Se hai creato prima un cluster standard in questa ubicazione o hai creato prima una VLAN privata nell'infrastruttura IBM Cloud (SoftLayer), devi specificare tale VLAN privata.

<p><strong>Nota:</strong> le VLAN pubbliche e private che specifichi con il comando create devono corrispondere. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). La combinazione di numeri e lettere
dopo questi prefissi devono corrispondere per utilizzare tali VLAN durante la creazione di un cluster. Per creare un cluster, non utilizzare
VLAN pubbliche e private che non corrispondono.</p></li>
</ul>

<p>Per scoprire se già hai una VLAN privata per una specifica ubicazione o per trovare il nome di una
VLAN privata esistente, esegui <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Questo parametro non è disponibile per i cluster gratuiti.</li>
<li>Se si tratta del primo cluster standard che crei in questa ubicazione, non utilizzare questo indicatore. Alla creazione
del cluster, viene creata per te una VLAN pubblica.</li>
<li>Se hai creato prima un cluster standard in questa ubicazione o hai creato prima una VLAN pubblica nell'infrastruttura IBM Cloud (SoftLayer), devi specificare tale VLAN pubblica.

<p><strong>Nota:</strong> le VLAN pubbliche e private che specifichi con il comando create devono corrispondere. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). La combinazione di numeri e lettere
dopo questi prefissi devono corrispondere per utilizzare tali VLAN durante la creazione di un cluster. Per creare un cluster, non utilizzare
VLAN pubbliche e private che non corrispondono.</p></li>
</ul>

<p>Per scoprire se già hai una VLAN pubblica per una specifica ubicazione o per trovare il nome di una
VLAN pubblica esistente, esegui <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>Il numero di nodi di lavoro che vuoi distribuire nel tuo cluster. Se non specifici
questa opzione, viene creato un cluster con 1 nodo di lavoro. Questo valore è facoltativo per i cluster standard e non è disponibile per i cluster gratuiti.

<p><strong>Nota:</strong> a ogni nodo di lavoro viene assegnato
un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del
cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo
cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Codifica del disco della funzione dei nodi di lavoro predefinita; [learn more](cs_secure.html#worker). Per disabilitare la codifica, includi questa opzione.</dd>
</dl>

**Esempi**:

  

  Esempio per un cluster standard:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  Esempio per un cluster gratuito:

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  Esempio per un ambiente {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}


### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

Visualizzare le informazioni su un cluster nella tua organizzazione.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Mostra le VLAN e le sottoreti di un cluster.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-get my_cluster
  ```
  {: pre}


### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

Rimuovere un cluster dalla tua organizzazione.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare la rimozione di un cluster senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_bind}

Aggiungi un servizio {{site.data.keyword.Bluemix_notm}} a un cluster.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Il nome dello spazio di nomi Kubernetes. Questo valore è obbligatorio.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>L'ID dell'istanza del servizio {{site.data.keyword.Bluemix_notm}}
di cui vuoi eseguire il bind. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

Rimuovi un servizio {{site.data.keyword.Bluemix_notm}} da un cluster.

**Nota:** quando rimuovi un servizio {{site.data.keyword.Bluemix_notm}}, le credenziali del servizio vengono rimosse dal cluster. Se un pod sta ancora utilizzando il servizio,
si verificherà un errore perché non è possibile trovare le credenziali del servizio.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Il nome dello spazio di nomi Kubernetes. Questo valore è obbligatorio.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>L'ID dell'istanza del servizio {{site.data.keyword.Bluemix_notm}}
che vuoi rimuovere. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

Elencare i servizi associati a uno o a tutti gli spazi dei nomi Kubernetes in un cluster. Se non viene specificata
alcuna opzione, vengono visualizzati i servizi per lo spazio dei nomi predefinito.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Includi i servizi associati a uno specifico spazio dei nomi in un cluster. Questo valore è facoltativo.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Includi i servizi associati a tutti gli spazi dei nomi in un cluster. Questo valore è facoltativo.</dd>
    </dl>

**Esempio**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}


### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

Rendere disponibile una sottorete in un account dell'infrastruttura IBM Cloud (SoftLayer) per un cluster specificato.

**Nota:** quando rendi disponibile una sottorete a un cluster, gli indirizzi IP
di questa sottorete vengono utilizzati per scopi di rete cluster. Per evitare conflitti di indirizzi IP, assicurati
di utilizzare una sottorete con un solo cluster. Non utilizzare una sottorete per più cluster o per altri
scopi al di fuori di {{site.data.keyword.containershort_notm}}
contemporaneamente.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>L'ID della sottorete. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-subnet-add my_cluster subnet
  ```
  {: pre}

### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

Crea una sottorete in un account dell'infrastruttura IBM Cloud (SoftLayer) e la rende disponibile per un cluster specificato in {{site.data.keyword.containershort_notm}}.

**Nota:** quando rendi disponibile una sottorete a un cluster, gli indirizzi IP
di questa sottorete vengono utilizzati per scopi di rete cluster. Per evitare conflitti di indirizzi IP, assicurati
di utilizzare una sottorete con un solo cluster. Non utilizzare una sottorete per più cluster o per altri
scopi al di fuori di {{site.data.keyword.containershort_notm}}
contemporaneamente.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio. Per elencare i tuoi cluster, utilizza il [comando](#cs_clusters) `bx cs clusters`.</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>Il nome di indirizzi IP della sottorete. Questo valore è obbligatorio. I valori possibili sono 8, 16, 32 o 64.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>La VLAN in cui creare la sottorete. Questo valore è obbligatorio. Per elencare le VLAN, utilizza il [comando](#cs_vlans) `bx cs vlans <location>`.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}

### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

Porta la tua sottorete privata nei cluster {{site.data.keyword.containershort_notm}}.

Questa sottorete privata non è una di quelle fornite dall'infrastruttura IBM Cloud (SoftLayer). In quanto tale, devi configurare l'instradamento del traffico di rete in entrata e in uscita per la sottorete. Per aggiungere una sottorete dell'infrastruttura IBM Cloud (SoftLayer), utilizza il [comando](#cs_cluster_subnet_add) `bx cs cluster-subnet-add`.

**Nota**: quando aggiungi una sottorete utente privata a un cluster, gli indirizzi IP di questa sottorete vengono utilizzati per i programmi di bilanciamento del carico privati nel cluster. Per evitare conflitti di indirizzi IP, assicurati
di utilizzare una sottorete con un solo cluster. Non utilizzare una sottorete per più cluster o per altri
scopi al di fuori di {{site.data.keyword.containershort_notm}}
contemporaneamente.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>La sottorete CIDR (Classless InterDomain Routing). Questo valore è obbligatorio e non deve essere in conflitto con una delle sottoreti utilizzate dall'infrastruttura IBM Cloud (SoftLayer).

   L'intervallo di prefissi supportati è compreso tra `/30` (1 indirizzo IP) e `/24` (253 indirizzi IP). Se configuri il CIDR con una lunghezza di prefisso e successivamente devi modificarla, aggiungi prima il nuovo CIDR e quindi [rimuovi il vecchio CIDR](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>L'ID della VLAN privata. Questo valore è obbligatorio. Deve corrispondere all'ID della VLAN privata di uno o più nodi di lavoro nel cluster.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-user-subnet-add my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

Rimuovi la tua propria sottorete privata da un cluster specificato.

**Nota:** ogni servizio distribuito a un indirizzo IP dalla tua propria sottorete privata rimane attivo dopo la rimozione della sottorete.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>La sottorete CIDR (Classless InterDomain Routing). Questo valore è obbligatorio e deve corrispondere al CIDR configurato dal [commando](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>L'ID della VLAN privata. Questo valore è obbligatorio e deve corrispondere all'ID della VLAN configurato dal [commando](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

Aggiorna il master Kubernetes alla versione API predefinita. Durante l'aggiornamento, non è possibile accedere o modificare il cluster. I nodi di lavoro, le applicazioni e le risorse che sono state distribuite dall'utente non vengono modificate e continueranno ad essere in esecuzione.

Potresti dover modificare i tuoi file YAML per distribuzioni future. Controlla questa [nota sulla release](cs_versions.html) per i dettagli.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>La versione Kubernetes del cluster. Se questo indicatore non è specificato, il master Kubernetes viene aggiornato alla versione API predefinita. Per visualizzare le versioni disponibili. esegui [bx cs kube-versions](#cs_kube_versions). Questo valore è facoltativo.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare l'aggiornamento di un master senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tenta l'aggiornamento anche se la modifica è maggiore di due versioni secondarie. Questo valore è facoltativo.</dd>
   </dl>

**Esempio**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}

### bx cs clusters
{: #cs_clusters}

Visualizzare un elenco di cluster nella tua organizzazione.

<strong>Opzioni comando</strong>:

  Nessuno

**Esempio**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Imposta le credenziali di account dell'infrastruttura IBM Cloud (SoftLayer) per il tuo account {{site.data.keyword.Bluemix_notm}}. Queste credenziali ti consentono di accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer) attraverso il tuo account {{site.data.keyword.Bluemix_notm}}.

**Nota:** non configurare più credenziali per un account {{site.data.keyword.Bluemix_notm}}. Ogni account {{site.data.keyword.Bluemix_notm}} è collegato a un solo portfolio dell'infrastruttura IBM Cloud (SoftLayer).

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nome utente dell'account dell'infrastruttura IBM Cloud (SoftLayer). Questo valore è obbligatorio.</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Chiave API dell'account dell'infrastruttura IBM Cloud (SoftLayer). Questo valore è obbligatorio.

 <p>
  Per generare una chiave API:

  <ol>
  <li>Accedi al [portale dell'infrastruttura IBM Cloud (SoftLayer) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).</li>
  <li>Seleziona <strong>Account</strong> e quindi <strong>Utenti</strong>.</li>
  <li>Fai clic su <strong>Genera</strong> per generare una chiave API dell'infrastruttura IBM Cloud (SoftLayer) per il tuo account.</li>
  <li>Copia la chiave API da utilizzare in questo comando.</li>
  </ol>

  Per visualizzare la tua chiave API esistente:
  <ol>
  <li>Accedi al [portale dell'infrastruttura IBM Cloud (SoftLayer) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).</li>
  <li>Seleziona <strong>Account</strong> e quindi <strong>Utenti</strong>.</li>
  <li>Fai clic su <strong>Visualizza</strong> per vedere la tua chiave API esistente.</li>
  <li>Copia la chiave API da utilizzare in questo comando.</li>
  </ol>
  </p></dd>
  </dl>

**Esempio**:

  ```
  bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Rimuovi le credenziali di account dell'infrastruttura IBM Cloud (SoftLayer) dal tuo account {{site.data.keyword.Bluemix_notm}}. Dopo aver rimosso le credenziali, non potrai più accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer) attraverso il tuo account {{site.data.keyword.Bluemix_notm}}.

<strong>Opzioni comando</strong>:

   Nessuno

**Esempio**:

  ```
  bx cs credentials-unset
  ```
  {: pre}



### bx cs help
{: #cs_help}

Visualizzare un elenco di comandi e parametri supportati.

<strong>Opzioni comando</strong>:

   Nessuno

**Esempio**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

Inizializza il plugin {{site.data.keyword.containershort_notm}} o specificare la regione in cui desideri creare o accedere ai cluster Kubernetes.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>L'endpoint API {{site.data.keyword.containershort_notm}} da utilizzare.  Questo valore è facoltativo. [Visualizza i valori dell'endpoint API disponibili.](cs_regions.html#container_regions)</dd>
   </dl>



```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}

### bx cs kube-versions
{: #cs_kube_versions}

Visualizza un elenco di versioni di Kubernetes supportate in {{site.data.keyword.containershort_notm}}. Aggiorna i tuoi [master cluster](#cs_cluster_update) e [nodi di lavoro](#cs_worker_update) alla versione predefinita per le funzionalità stabili più recenti.

**Opzioni comando**:

  Nessuno

**Esempio**:

  ```
  bx cs kube-versions
  ```
  {: pre}

### bx cs locations
{: #cs_datacenters}

Visualizzare un elenco di ubicazioni disponibili in cui creare un cluster.

<strong>Opzioni comando</strong>:

   Nessuno

**Esempio**:

  ```
  bx cs locations
  ```
  {: pre}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_create}

Crea una configurazione di registrazione. Puoi utilizzare questo comando per inoltrare i log dei contenitori, delle applicazioni, dei nodi di lavoro, dei cluster Kubernetes e dei programmi di bilanciamento del carico dell'applicazione Ingress a {{site.data.keyword.loganalysisshort_notm}} o a un server syslog esterno.

<strong>Opzioni comando</strong>:

<dl>
<dt><code><em>CLUSTER</em></code></dt>
<dd>Il nome o l'ID del cluster.</dd>
<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>L'origine del log per la quale desideri abilitare l'inoltro di log. I valori accettati sono <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> e <code>ingress</code>. Questo valore è obbligatorio.</dd>
<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>Lo spazio dei nomi del contenitore Docker dal quale desideri inoltrare i log. L'inoltro del log non è supportato per gli spazi dei nomi Kubernetes <code>ibm-system</code> e <code>kube-system</code>. Questo valore è valido solo per l'origine del log del contenitore ed è facoltativo. Se non specifichi uno spazio dei nomi, tutti gli spazi dei nomi nel contenitore utilizzeranno questa configurazione.</dd>
<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>Quando il tipo di registrazione è <code>syslog</code>, il nome host o l'indirizzo IP del server di raccolta del log. Questo valore è obbligatorio per <code>syslog</code>. Quando il tipo di registrazione è <code>ibm</code>, l'URL di inserimento {{site.data.keyword.loganalysislong_notm}}. Puoi trovare l'elenco degli URL di inserimento disponibili [qui](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Se non specifichi un URL di inserimento, viene utilizzato l'endpoint della regione in cui è stato creato il tuo cluster.</dd>
<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>La porta del server di raccolta del log. Questo valore è facoltativo. Se non specifichi una porta, viene utilizzata la porta standard <code>514</code> per <code>syslog</code> e <code>9091</code> per <code>ibm</code>.</dd>
<dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
<dd>Il nome dello spazio a cui vuoi inviare i log. Questo valore è valido solo per il tipo di log <code>ibm</code> ed è facoltativo. Se non specifichi uno spazio, i log vengono inviati al livello dell'account.</dd>
<dt><code>--org <em>CLUSTER_ORG</em></code></dt>
<dd>Il nome dell'organizzazione in cui si trova lo spazio. Questo valore è valido solo per il tipo di log <code>ibm</code> ed è obbligatorio se hai specificato uno spazio.</dd>
<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>Il protocollo di inoltro del log che desideri utilizzare. Al momento, sono supportati <code>syslog</code> e <code>ibm</code>. Questo valore è obbligatorio.</dd>
<dt><code>--json</code></dt>
<dd>Stampa facoltativamente l'output del comando nel formato JSON.</dd>
</dl>

**Esempi**:

Esempio del tipo di log `ibm` che inoltra da un'origine log `container` sulla porta predefinita 9091:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Esempio del tipo di log `syslog` che inoltra da un'origine log `container` sulla porta predefinita 514:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname my_hostname-or-IP --type syslog
  ```
  {: pre}

  Esempio di un tipo di log `syslog` che inoltra i log da un'origine `ingress` su una porta differente dalla predefinita:

    ```
    bx cs logging-config-create my_cluster --logsource container --hostname my_hostname-or-IP --port 5514 --type syslog
    ```
    {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

Visualizza tutte le configurazioni di inoltro di un cluster o filtrale in base all'origine del log.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
   <dd>Il tipo di origine del log per la quale desideri eseguire il filtro. Vengono restituite solo le configurazioni di registrazione di questa origine del log nel cluster. I valori accettati sono <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> e <code>ingress</code>. Questo valore è facoltativo.</dd>
   <dt><code>--json</code></dt>
   <dd>Stampa facoltativamente l'output del comando nel formato JSON.</dd>
   </dl>

**Esempio**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER LOG_CONFIG_ID
{: #cs_logging_rm}

Elimina una configurazione di inoltro del log. Questo arresta l'inoltro dei log a un server syslog o a {{site.data.keyword.loganalysisshort_notm}}.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   <dt><code><em>LOG_CONFIG_ID</em></code></dt>
   <dd>L'ID di configurazione di registrazione che desideri rimuovere dall'origine del log. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs logging-config-rm my_cluster f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER LOG_CONFIG_ID [--hostname LOG_SERVER_HOSTNAME_OR_IP][--port LOG_SERVER_PORT] [--space CLUSTER_SPACE][--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_update}

Aggiorna i dettagli di una configurazione di inoltro del log.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>
   <dt><code><em>LOG_CONFIG_ID</em></code></dt>
   <dd>L'ID di configurazione di registrazione che desideri aggiornare. Questo valore è obbligatorio.</dd>
   <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Quando il tipo di registrazione è <code>syslog</code>, il nome host o l'indirizzo IP del server di raccolta del log. Questo valore è obbligatorio per <code>syslog</code>. Quando il tipo di registrazione è <code>ibm</code>, l'URL di inserimento {{site.data.keyword.loganalysislong_notm}}. Puoi trovare l'elenco degli URL di inserimento disponibili [qui](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Se non specifichi un URL di inserimento, viene utilizzato l'endpoint della regione in cui è stato creato il tuo cluster.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>La porta del server di raccolta del log. Questo valore è facoltativo quando il tipo di registrazione è <code>syslog</code>. Se non specifichi una porta, viene utilizzata la porta standard <code>514</code> per <code>syslog</code> e <code>9091</code> per <code>ibm</code>.</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>Il nome dello spazio a cui vuoi inviare i log. Questo valore è valido solo per il tipo di log <code>ibm</code> ed è facoltativo. Se non specifichi uno spazio, i log vengono inviati al livello dell'account.</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>Il nome dell'organizzazione in cui si trova lo spazio. Questo valore è valido solo per il tipo di log <code>ibm</code> ed è obbligatorio se hai specificato uno spazio.</dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>Il protocollo di inoltro del log che desideri utilizzare. Al momento, sono supportati <code>syslog</code> e <code>ibm</code>. Questo valore è obbligatorio.</dd>
   <dt><code>--json</code></dt>
   <dd>Stampa facoltativamente l'output del comando nel formato JSON.</dd>
   </dl>

**Esempio di tipo di log `ibm`**:

  ```
  bx cs logging-config-update my_cluster f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Esempio di tipo di log `syslog`**:

  ```
  bx cs logging-config-update my_cluster f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs machine-types LOCATION
{: #cs_machine_types}

Visualizzare un elenco di tipi di macchina disponibili per i tuoi nodi di lavoro. Ogni tipo di macchina include
la quantità di CPU virtuale, memoria e spazio su disco per ogni nodo di lavoro nel cluster.
- I tipi di macchina con `u2c` o `b2c` nel nome utilizzano il disco locale invece di SAN (storage area networking) per garantire l'affidabilità. I vantaggi dell'affidabilità includono una velocità di elaborazione più elevata durante la serializzazione dei byte sul disco locale e una riduzione del danneggiamento del file system dovuto a errori di rete. Questi tipi di macchina contengono un'archiviazione disco locale da 25 GB per il file system del sistema operativo e un'archiviazione disco locale da 100 GB per `/var/lib/docker`, la directory in cui sono scritti tutti i dati del contenitore.
- I tipi di macchina che includono `encrypted` nel nome codificano i dati Docker dell'host. La directory `/var/lib/docker`, in cui vengono memorizzati tutti i dati del contenitore, è codificata con la crittografia LUKS.
- I tipi di macchina con `u1c` o `b1c` nel nome sono obsoleti, ad esempio `u1c.2x4`. Per iniziare a utilizzare i tipi di macchina `u2c` e `b2c`, usa il comando `bx cs worker-add` per aggiungere nodi di lavoro con il tipo di macchina aggiornato. Quindi, rimuovi i nodi di lavoro che utilizzano i tipi di macchina obsoleti utilizzando il comando `bx cs worker-rm`.
</p>


<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Immetti l'ubicazione in cui vuoi elencare i tipi di macchina disponibili. Questo valore è obbligatorio. Controlla le [ubicazioni disponibili](cs_regions.html#locations).</dd></dl>

**Esempio**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

### bx cs region
{: #cs_region}

Trova la regione {{site.data.keyword.containershort_notm}} in cui sei al momento. Crei e gestisci i cluster specifici della regione. Utilizza il comando `bx cs region-set` per modificare le regioni.

**Esempio**:

```
bx cs region
```
{: pre}

**Output**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Imposta la regione per {{site.data.keyword.containershort_notm}}. Crei e gestisci i cluster specifici della regione e puoi volere i cluster in più regioni per l'elevata disponibilità. 

Ad esempio, puoi accedere a {{site.data.keyword.Bluemix_notm}} nella regione Stati Uniti Sud e creare un cluster. Successivamente, puoi utilizzare `bx cs region-set eu-central` per specificare la regione Europa Centrale e creare un altro cluster. Infine, puoi utilizzare `bx cs region-set us-south` per ritornare alla regione Stati Uniti Sud per gestire il tuo cluster in tale regione.

**Opzioni comando**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Immetti la regione che vuoi specificare. Questo valore è facoltativo. Se non fornisci la regione, puoi selezionarla dall'elenco nell'output.

Per un elenco di regioni disponibili, controlla [regioni e ubicazioni](cs_regions.html) o utilizza il comando `bx cs regions` [](#cs_regions).</dd></dl>

**Esempio**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Output**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Elenca le regioni disponibili. `Region Name` è il nome {{site.data.keyword.containershort_notm}} e `Region Alias` è il nome {{site.data.keyword.Bluemix_notm}} generale della regione.

**Esempio**:

```
bx cs regions
```
{: pre}

**Output**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}

### bx cs subnets
{: #cs_subnets}

Visualizza un elenco di sottoreti disponibili in un account dell'infrastruttura IBM Cloud (SoftLayer).

<strong>Opzioni comando</strong>:

   Nessuno

**Esempio**:

  ```
  bx cs subnets
  ```
  {: pre}


### bx cs vlans LOCATION
{: #cs_vlans}

Elenca le VLAN pubbliche e private disponibili per un'ubicazione nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Per elencare le VLAN disponibili,
devi avere un account a pagamento.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Immetti l'ubicazione in cui vuoi elencare le tue VLAN pubbliche e private. Questo valore è obbligatorio. Controlla le [ubicazioni disponibili](cs_regions.html#locations).</dd>
   </dl>

**Esempio**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --URL URL
{: #cs_webhook_create}

Creare webhook.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>Il livello di notifica, come ad esempio <code>Normal</code> o
<code>Warning</code>. <code>Warning</code> è il valore predefinito. Questo valore è facoltativo.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Il tipo di webhook, come ad esempio slack. È supportato solo slack. Questo valore è obbligatorio.</dd>

   <dt><code>--URL <em>URL</em></code></dt>
   <dd>L'URL del webhook. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --URL http://github.com/<mywebhook>
  ```
  {: pre}


### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

Aggiungere i nodi di lavoro al tuo cluster standard.

<strong>Opzioni comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>Il percorso al file YAML per aggiungere i nodi di lavoro al tuo cluster. Invece di definire ulteriori nodi di lavoro utilizzando le opzioni fornite in questo comando, puoi utilizzare un file YAML. Questo valore è facoltativo.

<p><strong>Nota:</strong> se fornisci la stessa opzione nel comando come parametro nel file YAML, il valore nel comando ha la precedenza rispetto al valore nel YAML. Ad esempio, definisci un tipo di macchina nel tuo file YAML e utilizzi l'opzione --machine-type nel comando, il valore che hai immesso nell'opzione del comando sovrascrive il valore nel file YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_id&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_vlan&gt;</em>
public-vlan: <em>&lt;public_vlan&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
</code></pre>

<table>
<caption>Tabella 2.Descrizione dei componenti del file YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Sostituisci <code><em>&lt;cluster_name_or_id&gt;</em></code> con il nome o l'ID del cluster in cui desideri aggiungere nodi di lavoro.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>Sostituisci <code><em>&lt;location&gt;</em></code> con l'ubicazione in cui vuoi distribuire i tuoi nodi di lavoro. Le ubicazioni disponibili dipendono dalla regione in cui ha eseguito l'accesso. Per elencare le ubicazioni disponibili, esegui <code>bx cs locations</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Sostituisci <code><em>&lt;machine_type&gt;</em></code> con il tipo di macchina che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare tutti i tipi di macchina disponibili per la tua ubicazione, esegui <code>bx cs machine-types <em>&lt;location&gt;</em></code>.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Sostituisci <code><em>&lt;private_vlan&gt;</em></code> con l'ID della VLAN privata che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare le VLAN disponibili, esegui <code>bx cs vlans <em>&lt;location&gt;</em></code> e ricerca i router VLAN che iniziano con <code>bcr</code> (router di back-end).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Sostituisci <code>&lt;public_vlan&gt;</code> con l'ID della VLAN pubblica che vuoi utilizzare per i tuoi nodi di lavoro. Per elencare le VLAN disponibili, esegui <code>bx cs vlans &lt;location&gt;</code> e ricerca i router VLAN che iniziano con <code>fcr</code> (router di front-end).</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Il livello di isolamento hardware per il nodo di lavoro. Utilizza dedicato se vuoi avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è condiviso.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Sostituisci <code><em>&lt;number_workers&gt;</em></code> con il numero di nodi di lavoro che vuoi distribuire.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Codifica del disco della funzione dei nodi di lavoro predefinita; [learn more](cs_secure.html#worker). Per disabilitare la codifica, includi questa opzione e imposta il valore su <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Il livello di isolamento hardware per il nodo di lavoro. Utilizza dedicato se vuoi avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è condiviso. Questo valore è facoltativo.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Il tipo di macchina che scegli influisce sulla quantità di memoria e spazio disco
disponibile per i contenitori distribuiti al tuo nodo di lavoro. Questo valore è obbligatorio. Per elencare i tipi di macchina disponibili, consulta [bx cs machine-types LOCATION](#cs_machine_types).</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>Un numero intero che rappresenta il numero di nodi di lavoro da creare nel cluster. Il valore predefinito è 1. Questo valore è facoltativo.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>La VLAN privata specificata quando è stato creato il cluster. Questo valore è obbligatorio.

<p><strong>Nota:</strong> le VLAN pubbliche e private che specifichi devono corrispondere. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). La combinazione di numeri e lettere
dopo questi prefissi devono corrispondere per utilizzare tali VLAN durante la creazione di un cluster. Per creare un cluster, non utilizzare
VLAN pubbliche e private che non corrispondono.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>La VLAN pubblica specificata quando è stato creato il cluster. Questo valore è facoltativo.

<p><strong>Nota:</strong> le VLAN pubbliche e private che specifichi devono corrispondere. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). La combinazione di numeri e lettere
dopo questi prefissi devono corrispondere per utilizzare tali VLAN durante la creazione di un cluster. Per creare un cluster, non utilizzare
VLAN pubbliche e private che non corrispondono.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Codifica del disco della funzione dei nodi di lavoro predefinita; [learn more](cs_secure.html#worker). Per disabilitare la codifica, includi questa opzione.</dd>
</dl>

**Esempi**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Esempio per {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}


### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

Visualizzare i dettagli di un nodo di lavoro.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>Il nome o l'ID del cluster del nodo di lavoro. Questo valore è facoltativo.</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>L'ID per un nodo di lavoro. Esegui <code>bx cs workers <em>CLUSTER</em></code> per visualizzare gli ID per i nodi di lavoro in un cluster. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
  ```
  {: pre}


### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

Riavviare i nodi di lavoro in un cluster. Se è presente un problema con un nodo di lavoro, tenta prima
di riavviarlo. Se il riavvio non risolve il problema, tenta il comando
`worker-reload` . Lo stato dei nodi di lavoro non cambia durante il riavvio. Lo stato di avanzamento rimane `deployed`, ma lo stato viene aggiornato.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare il riavvio del nodo di lavoro senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>

   <dt><code>--hard</code></dt>
   <dd>Utilizza questa opzione per effettuare un riavvio forzato di un nodo di lavoro togliendo l'alimentazione
al nodo di lavoro. Utilizza questa opzione se il nodo di lavoro non risponde o ha un blocco
Docker. Questo valore è facoltativo.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Il nome o l'ID di uno o più nodi di lavoro. Utilizza uno spazio per elencare più
nodi di lavoro. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs worker-reboot my_cluster my_node1 my_node2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

Ricaricare i nodi di lavoro in un cluster. Se è presente un problema con un nodo di lavoro, tenta prima
di riavviarlo. Se il riavvio non risolve il problema, tenta il comando
`worker-reload`, che riavvia tutte le configurazioni necessarie per il nodo di lavoro.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare il ricaricamento di un nodo di lavoro senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Il nome o l'ID di uno o più nodi di lavoro. Utilizza uno spazio per elencare più
nodi di lavoro. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs worker-reload my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

Rimuovere uno o più nodi di lavoro da un cluster.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Il nome o l'ID del cluster. Questo valore è obbligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare la rimozione di un nodo di lavoro senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Il nome o l'ID di uno o più nodi di lavoro. Utilizza uno spazio per elencare più
nodi di lavoro. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs worker-rm my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

Aggiorna i nodi di lavoro con l'ultima versione di Kubernetes. L'esecuzione di `bx cs worker-update` può causare tempi di inattività per le applicazioni e i servizi. Durante l'aggiornamento, tutti i pod vengono ripianificati in altri nodi di lavoro e i dati vengono eliminati se non archiviati al di fuori del pod. Per evitare il tempo di inattività, assicurati di disporre di abbastanza nodi di lavoro per gestire il tuo carico di lavoro mentre vengono aggiornati i nodi di lavoro selezionati.

Potresti dover modificare i tuoi file YAML per le distribuzioni prima dell'aggiornamento. Controlla questa [nota sulla release](cs_versions.html) per i dettagli.

<strong>Opzioni comando</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>Il nome o l'ID del cluster in cui elenchi i nodi di lavoro disponibili. Questo valore è obbligatorio.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>La versione Kubernetes del cluster. Se questo indicatore non è specificato, il nodo di lavoro viene aggiornato alla versione predefinita. Per visualizzare le versioni disponibili. esegui [bx cs kube-versions](#cs_kube_versions). Questo valore è facoltativo.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilizza questa opzione per forzare l'aggiornamento di un master senza richiedere l'intervento dell'utente. Questo valore è facoltativo.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tenta l'aggiornamento anche se la modifica è maggiore di due versioni secondarie. Questo valore è facoltativo.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>L'ID di uno o più nodi di lavoro. Utilizza uno spazio per elencare più
nodi di lavoro. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs worker-update my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs workers CLUSTER
{: #cs_workers}

Visualizzare un elenco di nodi di lavoro e lo stato di ciascun cluster.

<strong>Opzioni comando</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>Il nome o l'ID del cluster in cui elenchi i nodi di lavoro disponibili. Questo valore è obbligatorio.</dd>
   </dl>

**Esempio**:

  ```
  bx cs workers mycluster
  ```
  {: pre}

<br />

