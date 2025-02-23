---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Limitazione del traffico di rete ai nodi di lavoro edge
{: #edge}

I nodi di lavoro edge possono migliorare la sicurezza del tuo cluster consentendo a un minor numero di nodi di lavoro di essere accessibili esternamente e isolando il carico di lavoro della rete. Quando questi nodi di lavoro sono contrassegnati solo per la rete, gli altri carichi di lavoro non possono consumare la CPU o la memoria del nodo di lavoro e interferire con la rete.
{:shortdesc}



## Etichetta nodi di lavoro come nodi edge
{: #edge_nodes}

Aggiungi l'etichetta `dedicated=edge` a due o più nodi di lavoro nel tuo cluster per garantire che i programmi di bilanciamento del carico e Ingress vengano distribuiti solo a quei nodi di lavoro.

Prima di iniziare:

- [Crea un cluster standard.](cs_clusters.html#clusters_cli)
- Assicurati che il tuo cluster abbia almeno una VLAN pubblica. I nodi di lavoro edge non sono disponibili per i cluster con solo le VLAN private.
- [Indirizza la CLI Kubernetes CLI al
cluster](cs_cli_install.html#cs_cli_configure).

Passi:

1. Elenca tutti i nodi di lavoro nel cluster. Utilizza l'indirizzo IP privato dalla colonna **NAME** per identificare i nodi. Seleziona almeno due nodi di lavoro per impostarli come nodi di lavoro edge. L'utilizzo di due o più nodi di lavoro migliora la disponibilità delle risorse di rete.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. Etichetta i nodi di lavoro con `dedicated=edge`. Una volta che un nodo di lavoro è contrassegnato con `dedicated=edge`, tutti i successivi programmi di bilanciamento del carico e Ingress vengono distribuiti a un nodo di lavoro edge.

  ```
  kubectl label nodes <node_name> <node_name2> dedicated=edge
  ```
  {: pre}

3. Richiama tutti i servizi di bilanciamento del carico esistenti nel cluster.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Output:

  ```
  kubectl get service -n <namespace> <name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. Utilizzando l'output dal passo precedente, copia e incolla ogni riga `kubectl get service`. Questo comando ridistribuisce il programma di bilanciamento del carico a un nodo di lavoro edge. Solo i programmi di bilanciamento del carico pubblici devono essere ridistribuiti.

  Output:

  ```
  service "<name>" configured
  ```
  {: screen}

Hai etichettato i nodi di lavoro con `dedicated=edge` e hai ridistribuito tutti i programmi di bilanciamento del carico e Ingress esistenti ai nodi di lavoro edge. Successivamente, impedisci [l'esecuzione di altri carichi di lavoro sui nodi di lavoro edge](#edge_workloads) e [blocca il traffico in entrata verso le porte del nodo sui nodi di lavoro](cs_network_policy.html#block_ingress).

<br />


## Impedisci l'esecuzione dei carichi di lavoro sui nodi di lavoro edge
{: #edge_workloads}

Uno dei vantaggi dei nodi di lavoro edge è che possono essere specificati per eseguire solo i servizi di rete. L'utilizzo della tolleranza `dedicated=edge` indica che tutti i servizi di bilanciamento del carico e Ingress vengono distribuiti solo ai nodi di lavoro etichettati. Tuttavia, per impedire che altri carichi di lavoro vengano eseguiti sui nodi di lavoro edge e consumino le risorse dei nodi di lavoro, devi utilizzare le [corruzioni Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).
{:shortdesc}

1. Elenca tutti i nodi di lavoro con l'etichetta `edge`.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Applica una corruzione a ciascun nodo di lavoro che impedisce l'esecuzione dei pod sul nodo di lavoro e rimuove i pod che non hanno l'etichetta `edge` dal nodo di lavoro. I pod che vengono rimossi vengono ridistribuiti su altri nodi di lavoro con capacità.

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```

Adesso, solo i pod con la tolleranza `dedicated=edge` vengono distribuiti ai tuoi nodi di lavoro edge.
