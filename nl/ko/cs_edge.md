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

# 네트워크 트래픽을 에지 작업자 노드로 제한
{: #edge}

에지 작업자 노드는 외부에서 적은 수의 작업자 노드에 액세스할 수 있게 하고 네트워크 워크로드를 격리함으로써 클러스터의 보안을 향상시킬 수 있습니다. 이러한 작업자 노드가 네트워킹 전용으로 표시되는 경우 다른 워크로드는 작업자 노드의 CPU 또는 메모리를 이용할 수 없고 네트워킹을 방해할 수도 없습니다.
{:shortdesc}



## 에지 노드로 작업자 노드에 레이블 지정
{: #edge_nodes}

`dedicated=edge` 레이블을 클러스터의 둘 이상의 작업자 노드에 추가하여 Ingress와 로드 밸런서가 해당 작업자 노드에만 배치되도록 하십시오.

시작하기 전에:

- [표준 클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
- 클러스터에 하나 이상의 퍼블릭 VLAN을 가지고 있는지 확인하십시오. 에지 작업자 노드는 프라이빗 VLAN만 있는 클러스터에서는 사용할 수 없습니다.
- [Kubernetes CLI를 클러스터에 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

단계:

1. 클러스터의 작업자 노드를 나열하십시오. **NAME** 열에서 사설 IP 주소를 사용하여 노드를 식별하십시오. 에지 작업자 노드가 될 둘 이상의 작업자 노드를 선택하십시오. 둘 이상의 작업자 노드를 사용하면 네트워킹 리소스 가용성이 향상됩니다.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. `dedicated=edge`로 작업자 노드의 레이블을 지정하십시오. 작업자 노드가 `dedicated=edge`와 함께 표시되면 모든 후속 Ingress 및 로드 밸런서가 에지 작업자 노드에 배치됩니다.

  ```
  kubectl label nodes <node_name> <node_name2> dedicated=edge
  ```
  {: pre}

3. 클러스터에서 기존 로드 밸런서 서비스를 모두 검색하십시오.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  출력:

  ```
  kubectl get service -n <namespace> <name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. 이전 단계의 출력을 사용하여 각 `kubectl get service` 행을 복사하여 붙여넣으십시오. 이 명령은 에지 작업자 노드에 로드 밸런서를 다시 배치합니다. 공용 로드 밸런서만 다시 배치해야 합니다.

  출력:

  ```
  service "<name>" configured
  ```
  {: screen}

`dedicated=edge`로 작업자 노드의 레이블을 지정했으며 기존 모든 로드 밸런서와 Ingress를 에지 작업자 노드에 다시 배치했습니다. 그 다음 다른 [워크로드가 에지 작업자 노드에서 실행](#edge_workloads)되지 않도록 하고 [작업자 노드에서 노드 포트에 대한 인바운드 트래픽을 차단](cs_network_policy.html#block_ingress)하십시오.

<br />


## 워크로드가 에지 작업자 노드에서 실행되지 않도록 함
{: #edge_workloads}

에지 작업자 노드의 이점 중 하나는 네트워킹 서비스만 실행하도록 이러한 작업자 노드를 지정할 수 있다는 것입니다. `dedicated=edge` 결함 허용을 사용하면 모든 로드 밸런서와 Ingress 서비스가 레이블 지정된 작업자 노드에만 배치됩니다. 하지만 다른 워크로드가 에지 작업자 노드에서 실행되지 않고 작업자 노드 리소스를 이용하지 못하도록 하려면 [Kubernetes 오염 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)을 사용해야 합니다.
{:shortdesc}

1. `edge` 레이블이 있는 모든 작업자 노드를 나열하십시오.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. 포드가 작업자 노드에서 실행되지 않도록 하고 작업자 노드에서 `edge` 레이블이 없는 포드를 제거하는 오염을 각 작업자 노드에 적용하십시오. 제거된 포드는 용량이 있는 다른 작업자 노드에 다시 배치됩니다.

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```

이제 `dedicated=edge` 결함 허용을 사용하는 포드만 에지 작업자 노드에 배치됩니다.
