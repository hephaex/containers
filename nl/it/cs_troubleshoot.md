---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Risoluzione dei problemi dei cluster
{: #cs_troubleshoot}

Come quando utilizzi {{site.data.keyword.containershort_notm}}, tieni presenti queste tecniche per la risoluzione dei problemi e su come ottenere supporto. Puoi anche verificare lo [stato del sistema {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/support/#status).

Puoi eseguire alcuni passi per assicurarti che i tuoi cluster siano aggiornati:
- [Riavvia i tuoi nodi di lavoro](cs_cli_reference.html#cs_worker_reboot) regolarmente per garantire l'installazione degli aggiornamenti e delle patch di sicurezza che IBM distribuisce automaticamente al sistema operativo.
- Aggiorna il tuo cluster con [l'ultima versione di Kubernetes](cs_versions.html) per {{site.data.keyword.containershort_notm}}

{: shortdesc}

<br />




## Debug dei cluster
{: #debug_clusters}

Rivedi le opzioni per il debug dei tuoi cluster e per trovare le cause principali dei malfunzionamenti.

1.  Elenca il tuo cluster e trova lo `Stato` del cluster.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Riesamina lo `Stato` del cluster.

  <table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
    <thead>
    <th>Stato cluster</th>
    <th>Descrizione</th>
    </thead>
    <tbody>
  
  <tr>
      <td>Critico</td>
      <td>Il master Kubernetes non può essere raggiunto o tutti i nodi di lavoro nel cluster sono inattivi.</td>
     </tr>
  
      <tr>
        <td>Distribuzione</td>
        <td>Il master Kubernetes non è ancora stato completamente distribuito. Non puoi accedere al tuo cluster.</td>
       </tr>
       <tr>
        <td>Normale</td>
        <td>Tutti i nodi di lavoro in un cluster sono attivi e in esecuzione. Puoi accedere al cluster e distribuire le applicazioni al cluster.</td>
     </tr>
       <tr>
        <td>In sospeso</td>
        <td>Il master Kubernetes è stato distribuito. Sta venendo eseguito il provisioning dei nodi di lavoro e non sono ancora disponibili nel cluster. Puoi accedere al cluster, ma non puoi distribuire le applicazioni al cluster.</td>
      </tr>
  
     <tr>
        <td>Avvertenza</td>
        <td>Almeno un nodo di lavoro nel cluster non è disponibile, ma altri lo sono e possono subentrare nel carico di lavoro.</td>
     </tr>  
    </tbody>
  </table>

3.  Se il tuo cluster è nello stato **Avvertenza**, **Critico** o **Eliminazione non riuscita** o è bloccato nello stato **In sospeso** per lungo tempo, controlla lo stato dei tuoi nodi di lavoro. Se il tuo cluster è nello stato **Distribuzione**, attendi finché non è stato completamente distribuito per controllarne l'integrità. I cluster nello stato **Normale** non richiedono un'azione al momento.  
<p>Per controllare lo stato dei tuoi nodi di lavoro: </p>

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

  <table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
    <thead>
    <th>Stato nodo di lavoro</th>
    <th>Descrizione</th>
    </thead>
    <tbody>
      <tr>
       <td>Sconosciuto</td>
       <td>Il master Kubernetes non è raggiungibile per uno dei seguenti motivi:<ul><li>Hai richiesto un aggiornamento del tuo master Kubernetes. Lo stato del nodo di lavoro non può essere richiamato durante l'aggiornamento.</li><li>Potresti avere un ulteriore firewall che protegge i tuoi nodi di lavoro o hai modificato le impostazioni del firewall recentemente. {{site.data.keyword.containershort_notm}}
richiede che alcuni indirizzi IP e porte siano aperti per consentire la comunicazione dal nodo di lavoro al master Kubernetes e viceversa. Per ulteriori informazioni, vedi [Il firewall impedisce la connessione dei nodi di lavoro](#cs_firewall).</li><li>Il master Kubernetes è inattivo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [ticket di supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support).</li></ul></td>
      </tr>
      <tr>
        <td>Provisioning</td>
        <td>Sta venendo eseguito il provisioning del tuo nodo di lavoro e non è ancora disponibile nel cluster. Puoi monitorare il processo di provisioning nella colonna **Stato** del tuo output CLI. Se il tuo nodo di lavoro è bloccato in questo stato per lungo tempo e non riesci a visualizzare alcun progresso nella colonna **Stato**, continua con il passo successivo per controllare se si è verificato un problema durante il provisioning.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
      </tr>
      <tr>
        <td>Ricaricamento</td>
        <td>Il tuo nodo di lavoro sta venendo ricaricato e non è disponibile nel cluster. Puoi monitorare il processo di ricaricamento nella colonna **Stato** del tuo output CLI. Se il tuo nodo di lavoro è bloccato in questo stato per lungo tempo e non riesci a visualizzare alcun progresso nella colonna **Stato**, continua con il passo successivo per controllare se si è verificato un problema durante il ricaricamento.</td>
       </tr>
       <tr>
        <td>Reloading_failed</td>
        <td>Impossibile ricaricare il tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
      </tr>
      <tr>
        <td>Normale</td>
        <td>È stato eseguito il provisioning completo del tuo nodo di lavoro ed è pronto per essere utilizzato nel cluster.</td>
     </tr>
     <tr>
        <td>Avvertenza</td>
        <td>Il tuo nodo di lavoro ha raggiunto il limite di memoria o spazio disco.</td>
     </tr>
     <tr>
      <td>Critico</td>
      <td>Il tuo nodo di lavoro ha esaurito lo spazio disco.</td>
     </tr>
    </tbody>
  </table>

4.  Elenca i dettagli del nodo di lavoro.

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

5.  Controlla i messaggi di errore comuni e impara come risolverli.

  <table>
    <thead>
    <th>Messaggio di errore</th>
    <th>Descrizione e risoluzione
    </thead>
    <tbody>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: al tuo account è al momento vietato ordinare le 'Istanze di calcolo'.</td>
        <td>Al tuo account dell'infrastruttura IBM Cloud (SoftLayer) potrebbe essere stato impedito di ordinare le risorse di calcolo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [ticket di supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support).</td>
      </tr>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile inserire l'ordine. Le risorse dietro il router 'router_name' non sono sufficienti a soddisfare la richiesta per i seguenti guest: 'worker_id'.</td>
        <td>La VLAN che hai selezionato è associata a un pod nel data center con insufficiente spazio per il provisioning del tuo nodo di lavoro. Puoi scegliere tra le seguenti opzioni:<ul><li>Utilizza un data center differente per eseguire il provisioning del tuo nodo di lavoro. Esegui <code>bx cs locations</code> per elencare i data center disponibili.<li>Se disponi di una coppia di VLAN privata e pubblica esistente che è associata a un altro pod nel data center, utilizza invece questa coppia di VLAN.<li>Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [ticket di supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support).</ul></td>
      </tr>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile ottenere la VLAN di rete con ID: &lt;vlan id&gt;.</td>
        <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro perché non è stato possibile trovare l'ID della VLAN selezionato per uno dei seguenti motivi:<ul><li>Potresti aver specificato un numero della VLAN invece dell'ID. Il numero della VLAN è di 3 o 4 cifre, mentre l'ID è di 7. Esegui <code>bx cs vlans &lt;location&gt;</code> per richiamare l'ID della VLAN.<li>L'ID VLAN potrebbe non essere associato all'account dell'infrastruttura IBM Cloud (SoftLayer) da te utilizzato. Esegui <code>bx cs vlans &lt;location&gt;</code> per elencare gli ID della VLAN disponibili per il tuo account. Per modificare l'account dell'infrastruttura IBM Cloud (SoftLayer), vedi [bx cs credentials-set](cs_cli_reference.html#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: l'ubicazione fornita per questo ordine non è valida. (HTTP 500)</td>
        <td>La tua infrastruttura IBM Cloud (SoftLayer) non è configurata per ordinare le risorse di calcolo nel data center selezionato. Contatta il [supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support) per verificare se il tuo account è configurato correttamente.</td>
       </tr>
       <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: l'utente non dispone della autorizzazioni dell'infrastruttura {{site.data.keyword.Bluemix_notm}} necessarie per aggiungere i server.

        </br></br>
        Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: 'Elemento' deve essere ordinato con l'autorizzazione.</td>
        <td>Potresti non disporre delle autorizzazioni necessarie per eseguire il provisioning di un nodo di lavoro dal portfolio dell'infrastruttura IBM Cloud (SoftLayer). Vedi [Configurazione dell'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer) per creare cluster Kubernetes standard](cs_infrastructure.html#unify_accounts).</td>
      </tr>
    </tbody>
  </table>

<br />




## Debug delle distribuzioni dell'applicazione
{: #debug_apps}

Rivedi le opzioni di cui disponi per il debug delle tue distribuzioni dell'applicazione e per trovare le cause principali dei malfunzionamenti.

1. Ricerca anomalie nelle risorse di distribuzione o nel servizio eseguendo il comando `describe`.

 Esempio:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Verifica se i contenitori sono bloccati nello stato ContainerCreating](#stuck_creating_state).

3. Verifica se il cluster è nello stato `Critical`. Se il cluster è in uno stato `Critical`, controlla le regole del firewall e verifica che il master possa comunicare con i nodi di lavoro.

4. Verifica che il servizio sia in ascolto sulla porta corretta.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per l'applicazione dall'interno del contenitore. Se la porta non è accessibile, il servizio potrebbe non essere in ascolto sulla porta corretta o l'applicazione potrebbe avere dei problemi. Aggiorna il file di configurazione per il servizio con la porta corretta e ridistribuisci o investiga sui problemi potenziali dell'applicazione.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verifica che il servizio sia collegato correttamente ai pod.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per la porta e l'indirizzo IP del cluster del servizio. Se la porta o l'indirizzo IP non sono accessibili, ricerca gli endpoint del servizio. Se non sono presenti endpoint, il selettore del servizio non corrisponde ai pod. Se sono presenti gli endpoint, ricerca il campo della porta di destinazione nel servizio e assicurati che sia la stessa di quella utilizzata dai pod.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Per i servizi Ingress, verifica che il servizio sia accessibile dall'interno del cluster.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Esegui curl dell'URL specificato per il servizio Ingress. Se l'URL non è accessibile, controlla se è presente un errore con il firewall tra il cluster e l'endpoint esterno. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />


## Impossibile collegarsi al tuo account dell'infrastruttura
{: #cs_credentials}

{: tsSymptoms}
Quando crei un nuovo cluster Kubernetes, ricevi il seguente messaggio.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.Bluemix_notm}}
Container Service CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

{: tsCauses}
Gli utenti con un account {{site.data.keyword.Bluemix_notm}} non collegato, devono creare un nuovo account Pagamento a consumo o aggiungere manualmente le chiavi API dell'infrastruttura IBM Cloud (SoftLayer) utilizzando la CLI {{site.data.keyword.Bluemix_notm}}.

{: tsResolve}
Per aggiungere le credenziali al tuo account {{site.data.keyword.Bluemix_notm}}:

1.  Contattata l'amministratore della tua infrastruttura IBM Cloud (SoftLayer) per ottenere il tuo nome utente e chiave API.

    **Nota:** l'account dell'infrastruttura IBM Cloud (SoftLayer) da te utilizzato deve essere configurato con autorizzazioni di SuperUser per creare correttamente i cluster standard.

2.  Aggiungi le credenziali.

  ```
  bx cs credentials-set --infrastructure-username <username> --infrastructure-api-key <api_key>
  ```
  {: pre}

3.  Crea un cluster standard.

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

<br />


## Il firewall impedisce l'esecuzione dei comandi della CLI 
{: #ts_firewall_clis}

{: tsSymptoms}
Quando esegui i comandi `bx`, `kubectl` o `calicoctl` dalla CLI, hanno esito negativo.

{: tsCauses}
Puoi avere delle politiche di rete aziendali che impediscono l'accesso dal tuo sistema locale agli endpoint pubblici tramite i proxy o i firewall 

{: tsResolve}
[Consenti l'accesso TCP per il funzionamento dei comandi della CLI](cs_firewall.html#firewall). Questa attività richiede una [politica di accesso Amministratore](cs_users.html#access_policies). Verifica la tua [politica di accesso](cs_users.html#infra_access) corrente.


## Il firewall impedisce al cluster di collegarsi alle risorse 
{: #cs_firewall}

{: tsSymptoms}
Quando il nodo di lavoro non può collegarsi, potresti visualizzare molti sintomi diversi. Potresti visualizzare uno dei seguenti messaggi quando kubectl proxy ha esito negativo o tenti di accedere a un servizio nel tuo cluster e la connessione ha un malfunzionamento.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Impossibile collegarsi al server: net/http: timeout handshake TLS
  ```
  {: screen}

Se esegui kubectl exec, attach o logs, potresti visualizzare il seguente messaggio.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

Se kubectl proxy ha esito positivo, ma il dashboard non è disponibile, potresti visualizzare il seguente messaggio.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
Potresti aver configurato un firewall aggiuntivo o personalizzato le tue impostazioni firewall esistenti nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). {{site.data.keyword.containershort_notm}}
richiede che alcuni indirizzi IP e porte siano aperti per consentire la comunicazione dal nodo di lavoro al master Kubernetes e viceversa. Un altro motivo potrebbe essere che i tuoi nodi di lavoro sono bloccati in un loop di ricaricamento.

{: tsResolve}
[Consenti al cluster di accedere alle risorse dell'infrastruttura e ad altri servizi](cs_firewall.html#firewall_outbound). Questa attività richiede una [politica di accesso Amministratore](cs_users.html#access_policies). Verifica la tua [politica di accesso](cs_users.html#infra_access) corrente.

<br />



## L'accesso al tuo nodo di lavoro con SSH non riesce
{: #cs_ssh_worker}

{: tsSymptoms}
Non è possibile accedere al tuo nodo di lavoro utilizzando la connessione SSH.

{: tsCauses}
SSH tramite password è disabilitata per i nodi di lavoro.

{: tsResolve}
Utilizza le [Serie daemon
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)per qualsiasi operazione che devi eseguire su ogni nodo o lavoro
per qualsiasi azione monouso che devi eseguire.

<br />



## L'associazione di un servizio a un cluster provoca un errore di stessa denominazione
{: #cs_duplicate_services}

{: tsSymptoms}
Quando esegui `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` visualizzi il seguente messaggio.

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Più istanze del servizio potrebbero avere lo stesso nome in regioni differenti.

{: tsResolve}
Utilizza la GUI del servizio invece del nome dell'istanza del servizio nel comando `bx cs cluster-service-bind`.

1. [Accedi alla regione che include l'istanza del servizio da associare.](cs_regions.html#bluemix_regions)

2. Ottieni il GUID per l'istanza del servizio.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  Output:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Associa nuovamente il servizio al cluster.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />



## Dopo aver aggiornato o ricaricato un nodo di lavoro, vengono visualizzati i i pod e i nodi duplicati
{: #cs_duplicate_nodes}

{: tsSymptoms}
Quando esegui `kubectl get nodes` visualizzi i nodi di lavoro duplicati con lo stato **NotReady**. I nodi di lavoro con lo stato **NotReady** hanno indirizzi IP pubblici, mentre i nodi di lavoro con **Ready** hanno indirizzi IP privati.

{: tsCauses}
I cluster meno recenti avevano i nodi di lavoro elencati per indirizzo IP pubblico del cluster. Ora, i nodi di lavoro sono elencati per indirizzo IP privato del cluster. Quando ricarichi o aggiorni un nodo, l'indirizzo IP viene modificato, ma il riferimento all'indirizzo IP pubblico rimane.

{: tsResolve}
Non sono presenti interruzioni del servizio dovute a questi duplicati, ma devi rimuovere i riferimenti al nodo di lavoro precedente dal server API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />




## L'accesso a un pod su un nodo di lavoro non è riuscito a causa di un timeout
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Hai eliminato un nodo di lavoro nel tuo cluster e quindi aggiunto un nodo di lavoro. Quando distribuisci un pod o un servizio Kubernetes, la risorsa non può accedere al nodo di lavoro appena creato e la connessione va in timeout.

{: tsCauses}
Se elimini un nodo di lavoro dal tuo cluster e quindi ne aggiungi un altro, è possibile che al nuovo nodo di lavoro venga assegnato l'indirizzo IP privato nel nodo di lavoro eliminato. Calico utilizza questo indirizzo IP come una tag e continua a provare a raggiungere il nodo eliminato.

{: tsResolve}
Aggiorna manualmente i riferimenti all'indirizzo IP privato in modo che puntino al nodo corretto.

1.  Conferma di avere due nodi di lavoro con lo stesso indirizzo **IP privato**. Prendi nota di **IP privato** e **ID** del nodo di lavoro eliminato.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   192.0.2.0.12   203.0.113.144   b2c.4x16       normal    Ready
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   192.0.2.0.16   203.0.113.144   b2c.4x16       deleted    -
  ```
  {: screen}

2.  Installa la [CLI Calico](cs_network_policy.html#adding_network_policies).
3.  Elenca i nodi di lavoro disponibili in Calico. Sostituisci <path_to_file> con il percorso locale al file di configurazione Calico.

  ```
  calicoctl get nodes --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Elimina il nodo di lavoro duplicato in Calico. Sostituisci NODE_ID con l'ID del nodo di lavoro.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Riavvia il nodo di lavoro che non è stato eliminato.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


Il nodo di lavoro non è più elencato in Calico.

<br />




## I pod rimangono nello stato in sospeso
{: #cs_pods_pending}

{: tsSymptoms}
Quando esegui `kubectl get pods`, puoi visualizzare i pod
che rimangono in uno stato **In sospeso**.

{: tsCauses}
Se hai appena creato il cluster Kubernetes, i nodi di lavoro potrebbero essere ancora in fase di configurazione. Se questo cluster è un cluster che già esiste, potresti non avere abbastanza capacità nel tuo cluster per distribuire il pod.

{: tsResolve}
Questa attività richiede una [politica di accesso Amministratore](cs_users.html#access_policies). Verifica la tua [politica
di accesso](cs_users.html#infra_access) corrente.

Se hai appena creato il cluster Kubernetes, immetti il seguente comando e attendi che il nodo
di lavoro venga inizializzato.

```
kubectl get nodes
```
{: pre}

Se questo cluster è un cluster che già esiste, controlla la capacità del tuo cluster.

1.  Imposta il proxy con il numero di porta predefinito.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Apri il dashboard Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Controlla di avere abbastanza capacità nel tuo cluster per distribuire il tuo pod.

4.  Se non hai abbastanza capacità nel tuo cluster, aggiungi un altro nodo di lavoro al cluster.

  ```
  bx cs worker-add <cluster name or id> 1
  ```
  {: pre}

5.  Se i tuoi pod sono ancora nello stato **in attesa** dopo che il nodo di lavoro è stato completamente distribuito,
controlla la [ Documentazione Kubernetes
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) per ulteriore risoluzione dei problemi sullo stato in attesa del tuo pod.

<br />




## I pod sono bloccati nello stato di creazione
{: #stuck_creating_state}

{: tsSymptoms}
Quando esegui `kubectl get pods -o wide`, visualizzi che molti pod in esecuzione nello stesso nodo di lavoro sono bloccati nello stato `ContainerCreating`.

{: tsCauses}
Il file system nel nodo di lavoro è in sola lettura.

{: tsResolve}
1. Esegui il backup di tutti i dati che possono venire archiviati nel nodo di lavoro o nei tuoi contenitori.
2. Ricrea il nodo di lavoro eseguendo il seguente comando.

<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_id&gt;</code></pre>

<br />




## I contenitori non si avviano
{: #containers_do_not_start}

{: tsSymptoms}
I pod sono stati distribuiti correttamente ai cluster, ma i contenitori non sono avviati.

{: tsCauses}
I contenitori potrebbero non essere stati avviati quando è stata raggiunta la quota del registro.

{: tsResolve}
[Libera memoria in {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />




## I log non vengono visualizzati
{: #cs_no_logs}

{: tsSymptoms}
Quando accedi al dashboard Kibana, i log non vengono visualizzati.

{: tsResolve}
Controlla i seguenti motivi sul perché i log non vengono visualizzati e i passi di risoluzione del problema corrispondenti:

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>Perché sta succedendo</th>
 <th>Come porvi rimedio </th>
 </thead>
 <tbody>
 <tr>
 <td>Non è impostata alcuna configurazione di registrazione.</td>
 <td>Per poter inviare i log, devi prima creare una configurazione di registrazione per inoltrare i log a {{site.data.keyword.loganalysislong_notm}}. Per creare una configurazione di registrazione, consulta <a href="cs_health.html#log_sources_enable">Abilitazione dell'inoltro dei log</a>.</td>
 </tr>
 <tr>
 <td>Il cluster non è in uno stato <code>Normal</code>. </td>
 <td>Per controllare lo stato del tuo cluster, vedi <a href="cs_troubleshoot.html#debug_clusters">Debug dei cluster</a>. </td>
 </tr>
 <tr>
 <td>La quota di archiviazione dei log è stata raggiunta. </td>
 <td>Per aumentare i limiti della tua archiviazione di log, consulta la <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">documentazione {{site.data.keyword.loganalysislong_notm}}</a>.</td>
 </tr>
 <tr>
 <td>Se hai specificato uno spazio durante la creazione del cluster, il proprietario dell'account non dispone delle autorizzazioni di Gestore, Sviluppatore o Revisore in quello spazio. </td>
 <td>Per modificare le autorizzazioni di accesso per il proprietario dell'account: <ol><li>Per trovare chi è il proprietario dell'account del cluster, esegui <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>.</li><li>Per concedere a tale proprietario dell'account le autorizzazioni di accesso allo spazio di gestore, sviluppatore o revisore {{site.data.keyword.containershort_notm}}, consulta <a href="cs_users.html#managing">Gestione dell'accesso al cluster</a>.</li><li>Per aggiornare il token di accesso dopo che sono state modificate le autorizzazioni, esegui <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
 </tr>
 </tbody></table>

Per verificare le modifiche che hai effettuato durante la risoluzione del problema, puoi distribuire Noisy, un pod di esempio che produce diversi eventi di log, su un nodo di lavoro nel cluster.

  1. [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) a un cluster in cui vuoi iniziare a produrre i log.

  2. Crea il file di configurazione `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Esegui il file di configurazione nel contesto del cluster.

        ```
        kubectl apply -f <filepath_to_noisy>
        ```
        {:pre}

  4. Dopo alcuni minuti, puoi visualizzare i tuoi log nel dashboard Kibana. Per accedere al dashboard Kibana, vai a uno dei seguenti URL e seleziona l'account {{site.data.keyword.Bluemix_notm}} in cui hai creato il cluster. Se hai specificato uno spazio durante la creazione del cluster, vai a tale spazio.
      - Stati Uniti Sud e Stati Uniti Est: https://logging.ng.bluemix.net
      - Regno Unito-Sud: https://logging.eu-gb.bluemix.net
      - EU-Central: https://logging.eu-fra.bluemix.net
      - AP-South: https://logging.au-syd.bluemix.net

<br />




## Il dashboard Kubernetes non visualizza i grafici di utilizzo
{: #cs_dashboard_graphs}

{: tsSymptoms}
Quando accedi al dashboard Kubernetes, i grafici di utilizzo non vengono visualizzati.

{: tsCauses}
A volte, dopo un aggiornamento del cluster o un riavvio del nodo di lavoro, il pod `kube-dashboard` non viene aggiornato.

{: tsResolve}
Elimina il pod `kube-dashboard` per forzare un riavvio. Il pod viene ricreato con le politiche RBAC in modo da accedere all'heapster per le informazioni di utilizzo.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Impossibile collegarsi a un'applicazione tramite un servizio del programma di bilanciamento del carico 
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Hai esposto pubblicamente la tua applicazione creando un servizio del programma di bilanciamento del carico nel tuo cluster. Quando tenti di collegarti alla tua applicazione tramite l'indirizzo IP pubblico del programma di bilanciamento del carico, la connessione non riesce o va in timeout.

{: tsCauses}
Il tuo servizio del programma di bilanciamento del carico potrebbe non funzionare correttamente per uno dei seguenti motivi:

-   Il cluster è un cluster gratuito o standard con solo un nodo di lavoro. 
-   Il cluster non è ancora stato completamente distribuito.
-   Lo script di configurazione per il tuo servizio del programma di bilanciamento del carico include degli errori.

{: tsResolve}
Per risolvere i problemi del tuo servizio del programma di bilanciamento del carico:

1.  Verifica di aver configurato un cluster standard che è stato completamente distribuito e che abbia almeno due nodi di lavoro per garantire l'elevata disponibilità al tuo servizio del programma di bilanciamento del carico.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    Nel tuo output della CLI, assicurati che lo **Stato** del tuo nodo di lavoro visualizzi **Pronto** e che il **Tipo di macchina** sia diverso da **gratuito**.

2.  Verifica l'accuratezza del file di configurazione del tuo servizio del programma di bilanciamento del carico.

  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myservice
  spec:
    type: LoadBalancer
    selector:
      <selectorkey>:<selectorvalue>
    ports:
     - protocol: TCP
       port: 8080
  ```
  {: pre}

    1.  Verifica di aver definito **LoadBalancer** come il tipo del tuo servizio.
    2.  Assicurati di aver utilizzato gli stessi **<selectorkey>** e **<selectorvalue>** che hai utilizzato nella sezione **label/metadata** di quando hai distribuito la tua applicazione.
    3.  Verifica di aver utilizzato la **porta** su cui è in ascolto la tua applicazione.

3.  Verifica il tuo servizio del programma di bilanciamento del carico e controlla la sezione **Eventi** per trovare errori potenziali.

  ```
  kubectl describe service <myservice>
  ```
  {: pre}

    Ricerca i seguenti messaggi di errore:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Per utilizzare il servizio del programma di bilanciamento del carico, devi disporre di un cluster standard con almeno due nodi di lavoro.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>Questo messaggio di errore indica che non è stato lasciato alcun indirizzo IP pubblico portatile che può essere assegnato al tuo servizio del programma di bilanciamento del carico. Fai riferimento a <a href="cs_subnets.html#subnets">Aggiunta di sottoreti ai cluster</a> per trovare informazioni su come richiedere gli indirizzi IP pubblici portatili per il tuo cluster. Dopo che gli indirizzi IP pubblici portatili sono disponibili per il cluster, il servizio del programma di bilanciamento del carico viene creato automaticamente.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Hai definito un indirizzo IP pubblico portatile per il tuo servizio del programma di bilanciamento del carico utilizzando la sezione **loadBalancerIP**, ma questo indirizzo IP pubblico portatile non è disponibile nella tua sottorete pubblica portatile. Modifica il tuo script di configurazione del servizio del programma di bilanciamento del carico e scegli uno degli indirizzi IP pubblici portatile o rimuovi la sezione **loadBalancerIP** dal tuo script in modo che possa venirne assegnato uno automaticamente.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>Non disponi di abbastanza nodi di lavoro da distribuire a un servizio del programma di bilanciamento del carico. Un motivo potrebbe essere che hai distribuito un cluster standard con più di un nodo di lavoro ma il provisioning ha avuto esito negativo.</li>
    <ol><li>Elenca i nodi di lavoro disponibili.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>Se vengono trovati almeno due nodi di lavoro, elenca i dettagli del nodo di lavoro.</br><pre class="screen"><code>bx cs worker-get [&lt;cluster_name_or_id&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Assicurati che gli ID delle VLAN privata e pubblica dei nodi di lavoro restituiti dai comandi <code>kubectl get nodes</code> e <code>bx cs [&lt;cluster_name_or_id&gt;] worker-get</code> corrispondano.</li></ol></li></ul>

4.  Se stai utilizzando un dominio personalizzato per collegarti al tuo servizio del programma di bilanciamento del carico, assicurati che sia associato all'indirizzo IP pubblico del tuo servizio del programma di bilanciamento del carico.
    1.  Trova l'indirizzo IP pubblico del servizio di bilanciamento del carico.

      ```
      kubectl describe service <myservice> | grep "LoadBalancer Ingress"
      ```
      {: pre}

    2.  Verifica che il tuo dominio personalizzato sia associato all'indirizzo IP pubblico portatile del tuo servizio di bilanciamento del carico nel record di puntatore (PTR).

<br />


## Impossibile collegarsi a un'applicazione tramite Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
Hai esposto pubblicamente la tua applicazione creando una risorsa Ingress per la tua applicazione nel tuo cluster. Quando tenti di collegarti alla tua applicazione tramite il dominio secondario o l'indirizzo IP pubblico del programma di bilanciamento del carico dell'applicazione Ingress, la connessione non riesce o va in timeout. 

{: tsCauses}
Ingress potrebbe non funzionare correttamente per i seguenti motivi:
<ul><ul>
<li>Il cluster non è ancora stato completamente distribuito.
<li>Il cluster è stato configurato come un cluster gratuito o standard con solo un nodo di lavoro.
<li>Lo script di configurazione Ingress include degli errori.
</ul></ul>

{: tsResolve}
Per risolvere i problemi con Ingress:

1.  Verifica di aver configurato un cluster standard che è stato completamente distribuito e che abbia almeno due nodi di lavoro per garantire l'elevata disponibilità al tuo programma di bilanciamento del carico dell'applicazione Ingress. 

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    Nel tuo output della CLI, assicurati che lo **Stato** del tuo nodo di lavoro visualizzi **Pronto** e che il **Tipo di macchina** sia diverso da **gratuito**.

2.  Richiama l'indirizzo IP pubblico e il dominio secondario del programma di bilanciamento del carico dell'applicazione Ingress e quindi esegui il ping ad entrambi. 

    1.  Richiama il dominio secondario del controller Ingress.

      ```
      bx cs cluster-get <cluster_name_or_id> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Esegui il ping del dominio secondario del programma di bilanciamento del carico dell'applicazione Ingress. 

      ```
      ping <ingress_controller_subdomain>
      ```
      {: pre}

    3.  Richiama l'indirizzo IP pubblico del tuo programma di bilanciamento del carico dell'applicazione Ingress. 

      ```
      nslookup <ingress_controller_subdomain>
      ```
      {: pre}

    4.  Esegui il ping dell'indirizzo IP pubblico del programma di bilanciamento del carico dell'applicazione Ingress. 

      ```
      ping <ingress_controller_ip>
      ```
      {: pre}

    Se la CLI restituisce un timeout per il dominio secondario o per l'indirizzo IP pubblico del programma di bilanciamento del carico dell'applicazione Ingress e hai configurato un firewall personalizzato che protegge i tuoi nodi di lavoro, potresti dover aprire ulteriori porte e gruppi di rete nel tuo [firewall](#cs_firewall). 

3.  Se stai utilizzando un dominio personalizzato, assicurati che sia associato al dominio secondario o all'indirizzo IP pubblico del programma di bilanciamento del carico dell'applicazione Ingress fornito da IBM con il tuo provider DNS (Domain Name Service). 
    1.  Se hai utilizzato il dominio secondario del programma di bilanciamento del carico dell'applicazione Ingress, verifica il tuo record di nome canonico (CNAME). 
    2.  Se hai utilizzato l'indirizzo IP pubblico del programma di bilanciamento del carico dell'applicazione Ingress, verifica che il tuo dominio personalizzato sia associato all'indirizzo IP pubblico portatile nel record di puntatore (PTR). 
4.  Verifica il tuo file di configurazione della risorsa Ingress. 

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tlssecret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  Verifica che il dominio secondario del programma di bilanciamento del carico dell'applicazione Ingress e il certificato TLS siano corretti. Per trovare il dominio secondario e il certificato TLS, esegui bx cs cluster-get <cluster_name_or_id>.
    2.  Assicurati che la tua applicazione sia in ascolto sullo stesso percorso configurato nella sezione **percorso** del tuo Ingress. Se la tua applicazione è configurata per essere in ascolto nel percorso root, includi **/** al tuo percorso.
5.  Verifica la tua distribuzione Ingress e cerca un messaggio di errore potenziale,

  ```
  kubectl describe ingress <myingress>
  ```
  {: pre}

6.  Verifica i log del tuo controller Ingress.
    1.  Richiama l'ID dei pod Ingress in esecuzione nel tuo cluster.

      ```
      kubectl get pods -n kube-system | grep alb1
      ```
      {: pre}

    2.  Richiama i log per ogni pod Ingress.

      ```
      kubectl logs <ingress_pod_id> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Ricerca messaggi di errore nei log del controller Ingress.

<br />



## Problemi con il segreto del programma di bilanciamento del carico dell'applicazione Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Dopo aver distribuito un segreto del programma di bilanciamento del carico dell'applicazione Ingress al tuo cluster, il campo `Description` non viene aggiornato con il nome del segreto quando visualizzi il tuo certificato in {{site.data.keyword.cloudcerts_full_notm}}.

Quando elenchi le informazioni sul segreto del programma di bilanciamento del carico dell'applicazione, lo stato è `*_failed`. Ad esempio, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Controlla i seguenti motivi sul perché il segreto del programma di bilanciamento del carico dell'applicazione può avere un malfunzionamento e i passi di risoluzione del problema corrispondenti: 

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>Perché sta succedendo</th>
 <th>Come porvi rimedio </th>
 </thead>
 <tbody>
 <tr>
 <td>Non disponi dei ruoli di accesso necessari per scaricare e aggiornare i dati del certificato.</td>
 <td>Controlla con il tuo amministratore dell'account che ti vengano assegnati i ruoli **Operatore** e **Editor** per la tua istanza {{site.data.keyword.cloudcerts_full_notm}}. Per ulteriori dettagli, consulta <a href="/docs/services/certificate-manager/about.html#identity-access-management">Identity and Access Management</a> per {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento della creazione, dell'aggiornamento o della rimozione non appartiene allo stesso account del cluster.</td>
 <td>Controlla che il CRN del certificato che hai fornito sia importato in un'istanza del servizio {{site.data.keyword.cloudcerts_short}} che viene distribuita nello stesso account del tuo cluster.</td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento della creazione non è corretto. </td>
 <td><ol><li>Controlla l'accuratezza della stringa CRN del certificato che fornisci.</li><li>Se il CRN del certificato è accurato, prova ad aggiornare il segreto. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>Se questo comando provoca lo stato <code>update_failed</code>, rimuovi il segreto. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>Distribuisci nuovamente il segreto. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento dell'aggiornamento non è corretto. </td>
 <td><ol><li>Controlla l'accuratezza della stringa CRN del certificato che fornisci.</li><li>Se il CRN del certificato è accurato, prova a rimuovere il segreto. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>Distribuisci nuovamente il segreto. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>Tenta di aggiornare il segreto. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>Il servizio {{site.data.keyword.cloudcerts_long_notm}} sta riscontrando un tempo di inattività.</td>
 <td>Controlla che il tuo servizio {{site.data.keyword.cloudcerts_short}} sia attivo e in esecuzione.</td>
 </tr>
 </tbody></table>

<br />



## Impossibile richiamare l'URL ETCD per la configurazione della CLI Calico
{: #cs_calico_fails}

{: tsSymptoms}
Quando richiami `<ETCD_URL>` per [aggiungere le politiche di rete](cs_network_policy.html#adding_network_policies), ricevi un messaggio di errore `calico-config not found`.

{: tsCauses}
Il tuo cluster non è della [versione Kubernetes 1.7](cs_versions.html) o successiva.

{: tsResolve}
[Aggiorna il tuo cluster](cs_cluster_update.html#master) o richiama `<ETCD_URL>` con i comandi compatibili con le versioni precedenti di Kubernetes.

Per richiamare `<ETCD_URL>`, esegui uno dei seguenti comandi:

- Linux e OS X:

    ```
    kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
    ```
    {: pre}

- Windows:
    <ol>
    <li> Ottieni un elenco di pod nello spazio dei nomi kube-system e individua il pod del
controller Calico. </br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>Esempio:</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
    <li> Visualizza i dettagli del pod del controller
Calico.</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
    <li> Individua il valore dell'endpoint ETCD. Esempio: <code>https://169.1.1.1:30001</code>
    </ol>

Quando richiami `<ETCD_URL>`, continua con le istruzioni elencate in (Aggiunta delle politiche di rete)[cs_network_policy.html#adding_network_policies].

<br />




## Come ottenere aiuto e supporto
{: #ts_getting_help}

Da dove iniziare a risolvere i problemi per un contenitore?

-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la pagina sugli stati
{{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/support/#status).
-   Pubblica una domanda in [{{site.data.keyword.containershort_notm}} Slack. ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com) Suggerimento: se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo slack.
-   Rivedi i forum per controllare se altri utenti hanno riscontrato gli stessi problemi. Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.

    -   Se hai domande tecniche sullo sviluppo o la distribuzione di cluster o applicazioni con
{{site.data.keyword.containershort_notm}}, inserisci la tua domanda in
[Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e contrassegnala con le tag `ibm-cloud`, `kubernetes` e `containers`.
    -   Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum
[IBM developerWorks dW Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Includi le tag `ibm-cloud`
e `containers`. Consulta [Come ottenere supporto](/docs/get-support/howtogetsupport.html#using-avatar) per ulteriori dettagli sull'utilizzo dei forum.

-   Contatta il supporto IBM. Per informazioni su come aprire un ticket di supporto IBM o sui livelli di supporto e sulla gravità dei ticket,
consulta [Come contattare il supporto](/docs/get-support/howtogetsupport.html#getting-customer-support).

{:tip}
Quando riporti un problema, includi il tuo ID del cluster. Per ottenere il tuo ID del cluster, esegui `bx cs clusters`.
