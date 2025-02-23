---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Aggiornamento dei cluster e dei nodi di lavoro
{: #update}

## Aggiornamento del master Kubernetes
{: #master}

Periodicamente, Kubernetes rilascia gli aggiornamenti. Questo può essere un [aggiornamento maggiore, minore o patch](cs_versions.html#version_types). A seconda del tipo di aggiornamento, potresti essere responsabile dell'aggiornamento del tuo master Kubernetes. Sei sempre responsabile di mantenere i tuoi nodi di lavoro aggiornati. Quando effettui gli aggiornamenti, il master Kubernetes viene aggiornato prima dei tuoi nodi di lavoro.
{:shortdesc}

Per impostazione predefinita, limitiamo la tua capacità di aggiornare un master Kubernetes a più di due versioni secondarie in avanti rispetto alla tua versione corrente. Ad esempio, se il master corrente è la versione 1.5 e vuoi aggiornare a 1.8, devi prima aggiornare a 1.7. Puoi forzare l'aggiornamento, ma l'aggiornamento di più di due versioni secondarie potrebbe causare risultati imprevisti. 

Il seguente diagramma illustra il processo che puoi seguire per aggiornare il tuo master.

![Procedura consigliata per l'aggiornamento del master](/images/update-tree.png)

Figura 1. Diagramma del processo di aggiornamento del master Kubernetes

**Attenzione**: non puoi eseguire il rollback di un cluster a una versione precedente dopo aver eseguito il processo di aggiornamento. Assicurati di utilizzare un cluster di test e segui le istruzioni per risolvere i problemi potenziali prima di aggiornare il tuo master di produzione.

Per gli aggiornamenti _maggiore_ o _minore_, completa la seguente procedura:

1. Controlla le [modifiche Kubernetes](cs_versions.html) ed effettua tutti gli aggiornamenti contrassegnati come _Aggiorna prima master_.
2. Aggiorna il tuo master Kubernetes utilizzando la GUI o eseguendo il comando della CLI [](cs_cli_reference.html#cs_cluster_update). Quando aggiorni il master Kubernetes, il master non è attivo per circa 5 - 10 minuti. Durante l'aggiornamento, non è possibile accedere o modificare il cluster. Tuttavia, i nodi di lavoro, le applicazioni e le risorse che gli utenti del cluster hanno distribuito non vengono modificate e continuano ad essere eseguite.
3. Conferma che l'aggiornamento è stato completato. Controlla la versione di Kubernetes nel dashboard {{site.data.keyword.Bluemix_notm}} o esegui `bx cs clusters`.

Una volta completato l'aggiornamento del master Kubernetes, puoi aggiornare i tuoi nodi di lavoro.

<br />


## Aggiornamento dei nodi di lavoro
{: #worker_node}

Dunque, hai ricevuto una notifica di aggiornare il tuo nodo di lavoro. Cosa significa? I tuoi dati sono archiviati nei pod all'interno dei tuoi nodi di lavoro. Quando sono disponibili patch e aggiornamenti di sicurezza per il master Kubernetes, devi assicurati che i tuoi nodi di lavoro rimangano sincronizzati. Il master del nodo di lavoro non può essere superiore al master Kubernetes.
{: shortdesc}

<ul>**Attenzione**:</br>
<li>Gli aggiornamenti ai nodi di lavoro possono causare tempi di inattività per applicazioni e servizi. </li>
<li>I dati vengono eliminati se non archiviati al di fuori del pod.</li>
<li>Utilizza le [repliche ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) nelle tue distribuzioni per ripianificare i pod sui nodi disponibili.</li></ul>

Ma se non posso avere tempi di inattività?

Come parte del processo di aggiornamento, alcuni nodi specifici vengono disabilitati per un periodo di tempo. Per evitare l'inattività della tua applicazione, puoi definire delle chiavi univoche in una mappa di configurazione che specifica le percentuali della soglia per tipi specifici di nodi durante il processo di upgrade. Definendo le regole basate sulle etichette Kubernetes standard e fornendo una percentuale della quantità massima di nodi che è consentito non siano disponibili, puoi assicurarti che la tua applicazione rimanga attiva e in esecuzione. Un nodo viene considerato indisponibile se ha già completato il processo di distribuzione.

Come vengono definite le chiavi?

Nella sezione delle informazioni sui dati della mappa di configurazione, puoi definire fino a 10 regole separate da eseguire in qualsiasi momento. Per essere aggiornati, i nodi di lavoro devono passare ogni regola definita.

Le chiavi sono definite. E adesso? 

Dopo aver definito le tue regole, esegui il comando `worker-upgrade`. Se viene restituita una risposta positiva, i nodi di lavoro sono in coda per essere aggiornati. Tuttavia, i nodi non si sottopongono al processo di upgrade finché non vengono soddisfatte tutte le regole. Mentre sono in coda, le regole vengono controllate a intervalli per vedere se è possibile aggiornare i nodi.

Se scelgo di non definire una mappa di configurazione?

Quando la mappa di configurazione non è definita, viene utilizzato il valore predefinito. Per impostazione predefinita, un massimo del 20% di tutti i nodi di lavoro in ogni cluster non sono disponibili durante il processo di aggiornamento.

Per aggiornare i tuoi nodi di lavoro:

1. Installa la versione della [`cli kubectl` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/tools/install-kubectl/) che corrisponde alla versione di Kubernetes del master Kubernetes.

2. Apporta tutte le modifiche contrassegnate come _Aggiorna dopo il master_ in [Modifiche Kubernetes](cs_versions.html).

3. Facoltativo: definisci la tua mappa di configurazione.
    Esempio:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
    ...
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="La prima riga nella tabella si estende su entrambe le colonne. Le rimanenti righe devono essere lette da sinistra a destra, con il parametro nella colonna uno e la descrizione corrispondente nella colonna due.">
    <thead>
      <th colspan=2><img src="images/idea.png"/> Descrizione dei componenti </th>
    </thead>
    <tbody>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> Come valore predefinito, se la mappa ibm-cluster-update-configuration non viene definita in un modo valido, solo il 20% dei tuoi cluster possono non essere disponibili in una sola volta. Se una o più regole valide sono definite senza un valore predefinito globale, il nuovo valore predefinito è di consentire al 100% dei nodi di lavoro di non essere disponibili in una sola volta. Puoi controllarlo creando una percentuale predefinita. </td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Esempi di chiavi univoche per cui vuoi impostare le regole. I nomi delle chiavi possono essere qualsiasi cosa tu voglia; le informazioni vengono analizzate dalle configurazioni impostate nella chiave. Per ogni chiave che definisci, puoi impostare solo un valore per <code>NodeSelectorKey</code> e <code>NodeSelectorValue</code>. Se vuoi impostare le regole per più di una regione o ubicazione (data center), crea una nuova voce chiave. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> La quantità massima di nodi che è consentito non siano disponibili per una chiave specificata, specificata come una percentuale. Un nodo non è disponibile quando è nel processo di distribuzione, ricaricamento o provisioning. I nodi di lavoro in coda sono bloccati dall'essere aggiornati se superano le percentuali di non disponibilità massime definite. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> Il tipo di etichetta per la quale desideri impostare una regola per una chiave specificata. Puoi impostare le regole per le etichette predefinite fornite da IBM, così come per le etichette che hai creato. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> Il sottoinsieme di nodi in una chiave specificata che la regola deve valutare. </td>
      </tr>
    </tbody>
  </table>

    **Nota**: può essere definito un massimo di 10 regole. Se aggiungi più di 10 chiavi a un file, vene analizzata solo una sottorete di informazioni.

3. Aggiorna i tuoi nodi di lavoro dalla GUI o eseguendo il comando CLI.
  * Per aggiornare dal dashboard {{site.data.keyword.Bluemix_notm}}, passa alla sezione `Nodi di lavoro` del tuo cluster e fai clic su `Aggiorna nodo di lavoro`.
  * Per ottenere gli ID del nodo di lavoro, esegui `bx cs workers <cluster_name_or_id>`. Se selezioni più nodi di lavoro, vengono posizionati in una coda per la valutazione dell'aggiornamento. Se vengono considerati pronti dopo la valutazione, saranno aggiornati in base alle regole impostate nelle configurazioni.

    ```
    bx cs worker-update <cluster_name_or_id> <worker_node_id1> <worker_node_id2>
    ```
    {: pre}

4. Facoltativo: verifica gli eventi che vengono attivati dalla mappa di configurazione e tutti gli errori di convalida che si verificano immettendo il seguente comando e che ricercano **Eventi**.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Conferma che l'aggiornamento sia stato completato:
  * Controlla la versione di Kubernetes nel dashboard {{site.data.keyword.Bluemix_notm}} o esegui `bx cs workers <cluster_name_or_id>`.
  * Controlla la versione di Kubernetes dei nodi di lavoro eseguendo `kubectl get nodes`.
  * In alcuni casi, i cluster meno recenti possono elencare nodi di lavoro duplicati con uno stato di **Non pronto** dopo un aggiornamento. Per rimuovere i duplicati, consulta [risoluzione dei problemi](cs_troubleshoot.html#cs_duplicate_nodes).

Passi successivi:
  - Ripeti il processo di aggiornamento con gli altri cluster.
  - Avvisa gli sviluppatori che lavorano nel cluster di aggiornare la loro CLI `kubectl` alla versione del master Kubernetes.
  - Se il dashboard Kubernetes non visualizza i grafici di utilizzo, [elimina il pod `kube-dashboard`](cs_troubleshoot.html#cs_dashboard_graphs).
<br />

