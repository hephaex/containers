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


# 클러스터 문제점 해결
{: #cs_troubleshoot}

{{site.data.keyword.containershort_notm}}를 사용할 때 문제점을 해결하고 도움을 얻으려면 이러한 기술을 고려하십시오. [{{site.data.keyword.Bluemix_notm}} 시스템의 상태 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/bluemix/support/#status)를 확인할 수도 있습니다.

몇 가지 일반 단계를 수행하여 클러스터가 최신 상태인지 확인할 수 있습니다.
- IBM에서 자동으로 운영 체제에 배치하는 업데이트 및 보안 패치가 설치될 수 있도록 정기적으로 [작업자 노드를 다시 부팅](cs_cli_reference.html#cs_worker_reboot)하십시오.
- 클러스터를 {{site.data.keyword.containershort_notm}}용 [Kubernetes의 최신 기본 버전](cs_versions.html)으로 업데이트하십시오.

{: shortdesc}

<br />




## 클러스터 디버깅
{: #debug_clusters}

클러스터를 디버그하기 위한 옵션을 검토하고 실패의 근본 원인을 찾습니다.

1.  클러스터를 나열하고 클러스터의 `상태`를 찾으십시오.

  ```
   bx cs clusters
  ```
  {: pre}

2.  클러스터의 `상태`를 검토하십시오.

  <table summary="모든 테이블 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 클러스터의 상태, 2열에는 설명이 있어야 합니다.">
    <thead>
    <th>클러스터 상태</th>
    <th>설명</th>
    </thead>
    <tbody>
  
  <tr>
      <td>중요</td>
      <td>Kubernetes 마스터에 도달할 수 없거나 클러스터의 모든 작업자 노드가 작동 중지되었습니다.</td>
     </tr>
  
      <tr>
        <td>배치</td>
        <td>Kubernetes 마스터가 아직 완전히 배치되지 않았습니다. 클러스터에 액세스할 수 없습니다.</td>
       </tr>
       <tr>
        <td>정상</td>
        <td>클러스터의 모든 작업자 노드가 시작되어 실행 중입니다. 클러스터에 액세스하고 클러스터에 앱을 배치할 수 있습니다.</td>
     </tr>
       <tr>
        <td>보류 중</td>
        <td>Kubernetes 마스터가 배치됩니다. 작업자 노드를 프로비저닝 중이므로 아직 클러스터에서 사용할 수 없습니다. 클러스터에 액세스할 수 있지만 클러스터에 앱을 배치할 수 없습니다.</td>
      </tr>
  
     <tr>
        <td>경고</td>
        <td>클러스터에 있는 하나 이상의 작업자 노드를 사용할 수 없지만, 다른 작업자 노드가 사용 가능하며 워크로드를 인계받을 수 있습니다.</td>
     </tr>  
    </tbody>
  </table>

3.  클러스터가 **경고**, **위험** 또는 **삭제에 실패함** 상태이거나 오랜 기간 동안 **보류 중** 상태에 있으면 작업자 노드의 상태를 검토하십시오. 클러스터가 **배치 중** 상태이면 클러스터가 완전히 배치될 때까지 기다렸다가 클러스터의 상태를 검토하십시오. **정상** 상태의 클러스터에는 현재 조치가 필요하지 않습니다. 
<p>작업자 노드의 상태를 검토하십시오.</p>

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

  <table summary="모든 테이블 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 클러스터의 상태, 2열에는 설명이 있어야 합니다.">
    <thead>
    <th>작업자 노드 상태</th>
    <th>설명</th>
    </thead>
    <tbody>
      <tr>
       <td>알 수 없음</td>
       <td>다음 이유 중 하나로 인해 Kubernetes 마스터에 연결할 수 없습니다.<ul><li>Kubernetes 마스터의 업데이트를 요청했습니다. 업데이트 중에 작업자 노드의 상태를 검색할 수 없습니다.</li><li>작업자 노드를 보호 중이거나 최근에 방화벽 설정을 변경한 추가 방화벽이 있을 수 있습니다. {{site.data.keyword.containershort_notm}}에서는 작업자 노드와 Kubernetes 마스터 간의 양방향 통신을 허용하기 위해 특정 IP 주소와 포트를 열도록 요구합니다. 자세한 정보는 [방화벽으로 인해 작업자 노드를 연결할 수 없음](#cs_firewall)을 참조하십시오.</li><li>Kubernetes 마스터가 작동 중단되었습니다. [{{site.data.keyword.Bluemix_notm}} 지원 티켓](/docs/get-support/howtogetsupport.html#getting-customer-support)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</li></ul></td>
      </tr>
      <tr>
        <td>프로비저닝</td>
        <td>작업자 노드를 프로비저닝 중이므로 아직 클러스터에서 사용할 수 없습니다. CLI 출력의 **상태** 열에서 프로비저닝 프로세스를 모니터할 수 있습니다. 작업자 노드가 오랫동안 이 상태로 고착되어 있고 **상태** 열에 진행상태가 표시되지 않으면 다음 단계로 진행하여 프로비저닝 중에 문제점이 발생했는지 확인하십시오.</td>
      </tr>
      <tr>
        <td>프로비저닝_실패</td>
        <td>작업자 노드를 프로비저닝할 수 없습니다. 다음 단계로 진행하여 실패에 관한 세부사항을 찾으십시오.</td>
      </tr>
      <tr>
        <td>다시 로드 중</td>
        <td>작업자 노드를 다시 로드 중이므로 클러스터에서 사용할 수 없습니다. CLI 출력의 **상태** 열에서 다시 로드 프로세스를 모니터할 수 있습니다. 작업자 노드가 오랫동안 이 상태로 고착되어 있고 **상태** 열에 진행상태가 표시되지 않으면 다음 단계로 진행하여 다시 로드 중에 문제점이 발생했는지 확인하십시오.</td>
       </tr>
       <tr>
        <td>다시 로드_실패</td>
        <td>작업자 노드를 다시 로드할 수 없습니다. 다음 단계로 진행하여 실패에 관한 세부사항을 찾으십시오.</td>
      </tr>
      <tr>
        <td>정상</td>
        <td>작업자 노드가 완전히 프로비저닝되어 클러스터에서 사용할 준비가 되었습니다.</td>
     </tr>
     <tr>
        <td>경고</td>
        <td>작업자 노드가 메모리 또는 디스크 공간의 한계에 도달하고 있습니다.</td>
     </tr>
     <tr>
      <td>중요</td>
      <td>작업자 노드의 디스크 공간이 부족합니다.</td>
     </tr>
    </tbody>
  </table>

4.  작업자 노드의 세부사항을 나열하십시오.

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

5.  일반 오류 메시지를 검토하고 해결 방법을 알아보십시오.

  <table>
    <thead>
    <th>오류 메시지</th>
    <th>설명 및 해결
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 현재 사용자의 계정을 통해 '컴퓨팅 인스턴스'를 주문하지 못하게 금지되어 있습니다.</td>
        <td>IBM Cloud 인프라(SoftLayer) 계정이 컴퓨팅 리소스를 주문하지 못하게 제한되었을 수 있습니다. [{{site.data.keyword.Bluemix_notm}} 지원 티켓](/docs/get-support/howtogetsupport.html#getting-customer-support)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 주문할 수 없습니다. 'router_name' 라우터의 리소스가 충분하지 않아 'worker_id' 게스트의 요청을 이행할 수 없습니다.</td>
        <td>선택한 VLAN이 작업자 노드를 프로비저닝하기에는 공간이 부족한 데이터센터의 포드와 연관되어 있습니다. 선택할 수 있는 옵션은 다음과 같습니다.<ul><li>다른 데이터센터를 사용하여 작업자 노드를 프로비저닝하십시오. <code>bx cs locations</code>를 실행하여 사용 가능한 데이터센터를 나열하십시오.<li>데이터센터의 다른 포드와 연관된 기존 퍼블릭 및 프라이빗 VLAN 쌍이 있으면 이 VLAN 쌍을 대신 사용하십시오.<li>[{{site.data.keyword.Bluemix_notm}} 지원 티켓](/docs/get-support/howtogetsupport.html#getting-customer-support)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: ID가 &lt;vlan id&gt;인 네트워크를 얻을 수 없습니다.</td>
        <td>다음 이유 중 하나로 인해 선택된 VLAN ID를 찾을 수 없으므로 작업자 노드를 프로비저닝할 수 없습니다.<ul><li>VLAN ID가 아니라 VLAN 번호를 지정했을 수 있습니다. VLAN 번호는 3 또는 4자리 숫자이지만 VLAN ID는 7자리 숫자입니다. <code>bx cs vlans &lt;location&gt;</code>을 실행하여 VLAN ID를 검색하십시오.<li>사용하는 IBM Cloud 인프라(SoftLayer) 계정과 VLAN ID가 연관되지 않았을 수 있습니다. <code>bx cs vlans &lt;location&gt;</code>을 실행하여 계정의 사용 가능한 VLAN ID를 나열하십시오. IBM Cloud 인프라(SoftLayer) 계정을 변경하려면 [bx cs credentials-set](cs_cli_reference.html#cs_credentials_set)를 참조하십시오. </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: 이 주문을 위해 제공된 위치가 올바르지 않습니다 (HTTP 500).</td>
        <td>선택된 데이터센터에서 컴퓨팅 리소스를 주문하도록 IBM Cloud 인프라(SoftLayer)가 설정되지 않았습니다. 계정이 올바르게 설정되었는지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 지원](/docs/get-support/howtogetsupport.html#getting-customer-support)에 문의하십시오.</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 사용자에게 서버를 추가하는 데 필요한 {{site.data.keyword.Bluemix_notm}} 인프라 권한이 없습니다.

        </br></br>
        {{site.data.keyword.Bluemix_notm}} 인프라 예외: 'Item'은 권한을 갖고 주문해야 합니다.</td>
        <td>IBM Cloud 인프라(SoftLayer) 포트폴리오에서 작업자 노드를 프로비저닝하는 데 필요한 권한이 없을 수 있습니다. [표준 Kubernetes 클러스터를 작성하기 위해 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 구성](cs_infrastructure.html#unify_accounts)을 참조하십시오.</td>
      </tr>
    </tbody>
  </table>

<br />




## 앱 배치 디버깅
{: #debug_apps}

앱 배치를 디버그해야 하는 옵션을 검토하고 실패의 근본 원인을 찾습니다.

1. `describe` 명령을 실행하여 서비스 또는 배치 리소스의 비정상 항목을 찾으십시오.

 예:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [컨테이너가 ContainerCreating 상태인지 확인](#stuck_creating_state)하십시오.

3. 클러스터가 `Critical` 상태인지 확인하십시오. 클러스터가 `Critical` 상태이면 방화벽 규칙을 검사하고 마스터가 작업자 노드와 통신할 수 있는지 확인하십시오.

4. 서비스가 올바른 포트에서 청취하는지 확인하십시오.
   1. 포드의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 컨테이너 내에서 앱을 curl하십시오. 포트에 액세스할 수 없는 경우 서비스가 올바른 포트에서 청취 중이지 않거나 앱에 문제가 있을 수 있습니다. 서비스의 구성 파일을 올바른 포트로 업데이트하고 앱에 대한 잠재적인 문제를 재배치하거나 조사하십시오.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 서비스가 포드에 올바르게 링크되었는지 확인하십시오.
   1. 포드의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 서비스의 클러스터 IP 주소 및 포트를 curl하십시오. IP 주소 및 포트에 액세스할 수 없는 경우 서비스의 엔드포인트를 살펴보십시오. 엔드포인트가 없으면 서비스의 선택기가 포드를 일치시키지 않습니다. 엔드포인트가 있으면 서비스의 대상 포트 필드를 살펴보고 대상 포트가 포드에 사용된 것과 동일한지 확인하십시오.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Ingress 서비스의 경우 클러스터 내에서 서비스에 액세스할 수 있는지 확인하십시오.
   1. 포드의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Ingress 서비스에 대해 지정된 URL을 curl하십시오. URL에 액세스할 수 없는 경우 클러스터와 외부 엔드포인트 간의 방화벽 문제를 확인하십시오. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />


## 인프라 계정에 연결할 수 없음
{: #cs_credentials}

{: tsSymptoms}
새 Kubernetes 클러스터를 작성할 때 다음과 같은 메시지를 수신합니다.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.Bluemix_notm}}
Container Service CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

{: tsCauses}
연결되지 않은 {{site.data.keyword.Bluemix_notm}} 계정이 있는 사용자는 새 종량과금제 계정을 작성하거나 {{site.data.keyword.Bluemix_notm}} CLI를 사용하여 IBM Cloud 인프라(SoftLayer) API 키를 수동으로 추가해야 합니다.

{: tsResolve}
{{site.data.keyword.Bluemix_notm}} 계정의 신임 정보를 추가하려면 다음을 수행하십시오.

1.  IBM Cloud 인프라(SoftLayer) 관리자에게 문의하여 IBM Cloud 인프라(SoftLayer) 사용자 이름과 API 키를 얻으십시오.

    **참고:** 표준 클러스터를 작성하려면 사용하는 IBM Cloud 인프라(SoftLayer) 계정을 수퍼유저 권한으로 설정해야 합니다.

2.  신임 정보를 추가하십시오.

  ```
  bx cs credentials-set --infrastructure-username <username> --infrastructure-api-key <api_key>
  ```
  {: pre}

3.  표준 클러스터를 작성하십시오.

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

<br />


## 방화벽으로 인해 CLI 명령을 실행할 수 없음
{: #ts_firewall_clis}

{: tsSymptoms}
CLI에서 `bx`, `kubectl` 또는 `calicoctl` 명령을 실행하면 실패합니다.

{: tsCauses}
로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 회사 네트워크 정책이 있을 수 있습니다.

{: tsResolve}
[CLI 명령에 대한 TCP 액세스가 작동하도록 허용](cs_firewall.html#firewall)하십시오. 이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.


## 방화벽으로 인해 클러스터를 리소스에 연결할 수 없음
{: #cs_firewall}

{: tsSymptoms}
작업자 노드를 연결할 수 없는 경우 다양한 증상이 나타날 수 있습니다. kubectl proxy가 실패하거나 클러스터의 서비스에 액세스하려고 할 때 다음 메시지 중 하나가 표시되면서 연결에 실패합니다.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

kubectl exec, attach 또는 logs를 실행하는 경우 다음 메시지가 표시될 수 있습니다.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

kubectl proxy가 성공하지만 대시보드를 사용할 수 없는 경우 다음 메시지가 표시될 수 있습니다.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
IBM Cloud 인프라(SoftLayer) 계정에서 기존 방화벽 설정을 사용자 정의했거나 추가 방화벽을 설정했을 수 있습니다. {{site.data.keyword.containershort_notm}}에서는 작업자 노드와 Kubernetes 마스터 간의 양방향 통신을 허용하기 위해 특정 IP 주소와 포트를 열도록 요구합니다. 다른 이유는 작업자 노드가 다시 로드 루프에서 고착 상태에 빠졌기 때문일 수 있습니다.

{: tsResolve}
[클러스터가 인프라 리소스 및 기타 서비스에 액세스하도록 허용](cs_firewall.html#firewall_outbound)하십시오. 이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.

<br />



## SSH로 작업자 노드에 액세스하는 데 실패
{: #cs_ssh_worker}

{: tsSymptoms}
SSH 연결을 사용하여 작업자 노드에 액세스할 수 없습니다.

{: tsCauses}
비밀번호를 통한 SSH가 작업자 노드에서 사용되지 않습니다.

{: tsResolve}
실행해야 하는 일회성 조치에 대한 모든 노드 또는 작업에 대해 실행해야 하는 전체 항목에 대해 [DaemonSets![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)를 사용하십시오.

<br />



## 서비스를 클러스터에 바인딩하면 동일한 이름 오류가 발생함
{: #cs_duplicate_services}

{: tsSymptoms}
`bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` 실행 시 다음 메시지가 표시됩니다.

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
서로 다른 지역에 동일한 이름의 서비스 인스턴스가 여러 개 있습니다.

{: tsResolve}
`bx cs cluster-service-bind` 명령에서 서비스 인스턴스 이름 대신 서비스 GUID를 사용하십시오.

1. [바인드할 서비스 인스턴스가 포함된 지역에 로그인하십시오.](cs_regions.html#bluemix_regions)

2. 서비스 인스턴스의 GUID를 가져오십시오.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  출력:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 서비스를 다시 클러스터에 바인드하십시오.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />



## 작업자 노드를 업데이트하거나 다시 로드한 후 중복 노드 및 포드가 표시됨
{: #cs_duplicate_nodes}

{: tsSymptoms}
`kubectl get nodes`를 실행할 때 **NotReady** 상태의 중복된 작업자 노드가 표시됩니다. **NotReady** 상태의 작업자 노드에는 공인 IP 주소가 있지만 **Ready** 상태의 작업자 노드에는 사설 IP 주소가 있습니다.

{: tsCauses}
이전 클러스터에서는 작업 노드가 클러스터의 공인 IP 주소별로 나열되었습니다. 이제 작업자 노드가 클러스터의 사설 IP 주소별로 나열됩니다. 노드를 다시 로드하거나 업데이트할 때 IP 주소가 변경되지만 공인 IP 주소에 대한 참조는 그대로 유지됩니다.

{: tsResolve}
이러한 중복 항목으로 인해 서비스 중단이 발생하지는 않지만 API 서버에서 이전 작업자 노드 참조를 제거해야 합니다.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />




## 제한시간 초과로 인해 새 작업자 노드에서 포드에 액세스하는 데 실패
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가했습니다. 포드 또는 Kubernetes 서비스를 배치한 경우 리소스에서 새로 작성한 작업자 노드에 액세스할 수 없으며 연결 제한시간이 초과됩니다.

{: tsCauses}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가하면 새 작업자 노드를 삭제한 작업자 노드의 사설 IP 주소에 할당할 수 있습니다. Calico에서는 이 사설 IP 주소를 태그로 사용하고 계속하여 삭제된 노드에 연결하려고 합니다.

{: tsResolve}
정확한 노드를 가리키도록 사설 IP 주소의 참조를 수동으로 업데이트하십시오.

1.  **사설 IP** 주소가 동일한 작업자 노드가 두 개 있는지 확인하십시오. 삭제된 작업자의 **사설 IP** 및 **ID**를 참고하십시오.

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

2.  [Calico CLI](cs_network_policy.html#adding_network_policies)를 설치하십시오.
3.  Calico에서 사용 가능한 작업자 노드를 나열하십시오. <path_to_file>을 Calico 구성 파일의 로컬 경로로 대체하십시오.

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

4.  Calico에서 중복 작업자 노드를 삭제하십시오. NODE_ID를 작업자 노드 ID로 대체하십시오.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  삭제되지 않은 작업자 노드를 다시 부팅하십시오.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


삭제된 노드는 더 이상 Calico에 나열되지 않습니다.

<br />




## 포드가 보류 상태를 유지함
{: #cs_pods_pending}

{: tsSymptoms}
`kubectl get pods`를 실행할 때 **보류** 상태를 유지하는 포드를 볼 수 있습니다.

{: tsCauses}
Kubernetes 클러스터를 방금 작성한 경우, 작업자 노드가 여전히 구성 중일 수 있습니다. 이 클러스터가 기존 클러스터인 경우, 클러스터에 포드를 배치할 만큼 충분한 용량이 없을 수 있습니다.

{: tsResolve}
이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.

방금 Kubernetes 클러스터를 작성한 경우에는 다음 명령을 실행하고 작업자 노드가 초기화될 때까지 대기하십시오.

```
kubectl get nodes
```
{: pre}

이 클러스터가 기존 클러스터인 경우, 클러스터 용량을 확인하십시오.

1.  기본 포트 번호로 프록시를 설정하십시오.

  ```
   kubectl proxy
  ```
   {: pre}

2.  Kubernetes 대시보드를 여십시오.

  ```
   http://localhost:8001/ui
  ```
  {: pre}

3.  포드를 배치하기에 충분한 용량이 클러스터에 있는지 확인하십시오.

4.  클러스터에 충분한 용량이 없으면 클러스터에 다른 작업자 노드를 추가하십시오.

  ```
  bx cs worker-add <cluster name or id> 1
  ```
  {: pre}

5.  작업자 노드를 완전히 배치한 후에도 포드가 계속 **보류** 상태를 유지하면 [Kubernetes 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending)를 검토하여 보류 상태인 포드의 추가 문제점을 해결할 수 있습니다.

<br />




## 포드가 계속 작성 중 상태에 있음
{: #stuck_creating_state}

{: tsSymptoms}
`kubectl get pods -o wide`를 실행할 때 동일한 작업자 노드에서 실행 중인 여러 포드가 계속 `ContainerCreating` 상태에 있음을 알게 됩니다.

{: tsCauses}
작업자 노드의 파일 시스템은 읽기 전용입니다.

{: tsResolve}
1. 작업자 노드 또는 컨테이너에 저장될 수 있는 데이터를 백업하십시오.
2. 다음 명령을 실행하여 작업 노드를 다시 빌드하십시오.

<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_id&gt;</code></pre>

<br />




## 컨테이너가 시작되지 않음
{: #containers_do_not_start}

{: tsSymptoms}
포드가 클러스터에 배치되지만 컨테이너가 시작되지 않습니다.

{: tsCauses}
레지스트리 할당량에 도달하면 컨테이너가 시작되지 않을 수 있습니다.

{: tsResolve}
[{{site.data.keyword.registryshort_notm}}의 스토리지를 비우십시오.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />




## 로그가 나타나지 않음
{: #cs_no_logs}

{: tsSymptoms}
Kibana 대시보드에 액세스할 때 로그는 표시되지 않습니다.

{: tsResolve}
로그가 표시되지 않는 이유와 해당 문제점 해결 단계를 검토하십시오.

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>발생 원인</th>
 <th>해결 방법</th>
 </thead>
 <tbody>
 <tr>
 <td>로깅 구성이 설정되지 않았습니다.</td>
 <td>로그를 전송하려면 우선 로그를 {{site.data.keyword.loganalysislong_notm}}로 전달하기 위한 로깅 구성을 작성해야 합니다. 로깅 구성을 작성하려면 <a href="cs_health.html#log_sources_enable">로그 전달 사용</a>을 참조하십시오.</td>
 </tr>
 <tr>
 <td>클러스터가 <code>Normal</code> 상태가 아닙니다.</td>
 <td>클러스터의 상태를 확인하려면 <a href="cs_troubleshoot.html#debug_clusters">클러스터 디버깅</a>을 참조하십시오.</td>
 </tr>
 <tr>
 <td>로그 스토리지 할당량에 도달했습니다.</td>
 <td>로그 스토리지 한계를 늘리려면 <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} 문서</a>를 참조하십시오.</td>
 </tr>
 <tr>
 <td>클러스터 작성시 영역을 지정한 경우, 계정 소유자에게 해당 영역에 대한 관리자, 개발자 또는 감사자 권한이 없습니다.</td>
 <td>계정 소유자의 액세스 권한을 변경하려면 다음을 수행하십시오.<ol><li>클러스터의 계정 소유자를 찾으려면 <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>를 실행하십시오.</li><li>계정 소유자에게 영역에 대한 관리자, 개발자 또는 감사자 {{site.data.keyword.containershort_notm}} 액세스 권한을 부여하려면 <a href="cs_users.html#managing">클러스터 액세스 관리</a>를 참조하십시오.</li><li>권한이 변경된 후 로깅 토큰을 새로 고치려면 <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>를 실행하십시오.</li></ol></td>
 </tr>
 </tbody></table>

문제점 해결 중 작성된 변경사항을 테스트하려면 여러 로그 이벤트를 생성하는 샘플 포드 Noisy를 클러스터의 작업자 노드에 배치할 수 있습니다.

  1. 로그 생성을 시작할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

  2. `deploy-noisy.yaml` 구성 파일을 작성하십시오.

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

  3. 클러스터의 컨텍스트에서 구성 파일을 실행하십시오.

        ```
        kubectl apply -f <filepath_to_noisy>
        ```
        {:pre}

  4. 몇 분 뒤에 Kibana 대시보드에 로그가 표시됩니다. Kibana 대시보드에 액세스하려면 다음 URL 중 하나로 이동하여 클러스터를 작성한 {{site.data.keyword.Bluemix_notm}} 계정을 선택하십시오. 클러스터 작성 시 영역을 지정한 경우 대신 해당 영역으로 이동하십시오.
      - 미국 남부 및 미국 동부: https://logging.ng.bluemix.net
      - 영국 남부: https://logging.eu-gb.bluemix.net
      - 중앙 유럽: https://logging.eu-fra.bluemix.net
      - AP 남부: https://logging.au-syd.bluemix.net

<br />




## Kubernetes 대시보드에 사용률 그래프가 표시되지 않음
{: #cs_dashboard_graphs}

{: tsSymptoms}
Kubernetes 대시보드에 액세스할 때 사용률 그래프는 표시되지 않습니다.

{: tsCauses}
때때로 클러스터 업데이트 또는 작업자 노드 재부팅 후에 `kube-dashboard` 포드가 업데이트되지 않습니다.

{: tsResolve}
강제로 다시 시작하려면 `kube-dashboard` 포드를 삭제하십시오. 사용률 정보에 대한 heapster에 액세스하기 위해 RBAC 정책으로 포드가 다시 작성됩니다.

  ```
    kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## 로드 밸런서 서비스를 통해 앱에 연결하는 데 실패
{: #cs_loadbalancer_fails}

{: tsSymptoms}
클러스터에서 로드 밸런서 서비스를 작성하여 공용으로 앱을 노출했습니다. 로드 밸런서의 공인 IP 주소를 통해 앱에 연결하려고 할 때 연결에 실패했거나 제한시간이 초과되었습니다.

{: tsCauses}
다음과 이유 중 하나로 인해 로드 밸런서 서비스가 제대로 작동하지 않을 수 있습니다.

-   클러스터는 작업자 노드가 하나뿐인 표준 클러스터 또는 무료 클러스터입니다.
-   클러스터가 아직 완전히 배치되지 않았습니다.
-   로드 밸런서 서비스의 구성 스크립트에 오류가 포함되어 있습니다.

{: tsResolve}
로드 밸런서 서비스의 문제점을 해결하려면 다음을 수행하십시오.

1.  로드 밸런서 서비스의 고가용성을 위해 완전히 배치된 표준 클러스터를 설정하고 두 개 이상의 작업자 노드가 있는지 확인하십시오.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    CLI 출력에서 작업자 노드의 **상태**에 **준비**가 표시되며 **시스템 유형**에 **무료** 이외의 시스템 유형이 표시되는지 확인하십시오.

2.  로드 밸런서 서비스의 구성 파일이 정확한지 확인하십시오.

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

    1.  **LoadBalancer**를 서비스 유형으로 정의했는지 확인하십시오.
    2.  앱을 배치할 때 **label/metadata** 섹션에서 사용한 내용과 동일한 **<selectorkey>** 및 **<selectorvalue>**를 사용했는지 확인하십시오.
    3.  앱에서 청취하는 **port**를 사용했는지 확인하십시오.

3.  로드 밸런서 서비스를 확인하고 **이벤트** 섹션을 검토하여 잠재적 오류를 찾으십시오.

  ```
  kubectl describe service <myservice>
  ```
  {: pre}

    다음 오류 메시지를 찾으십시오.

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>로드 밸런서 서비스를 사용하려면 두 개 이상의 작업자 노드가 있는 표준 클러스터가 있어야 합니다.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>이 오류 메시지는 로드 밸런서 서비스에 할당할 포터블 공인 IP 주소가 남아 있지 않음을 나타냅니다. 클러스터의 포터블 공인 IP 주소를 요청하는 방법에 대한 정보는 <a href="cs_subnets.html#subnets">클러스터에 서브넷 추가</a>를 참조하십시오. 포터블 공인 IP 주소를 클러스터에 사용할 수 있게 되면 로드 밸런서 서비스가 자동으로 작성됩니다.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>**loadBalancerIP** 섹션을 사용하여 로드 밸런서 서비스의 포터블 공인 IP 주소를 정의했지만, 이 포터블 공인 IP 주소는 포터블 공인 서브넷에서 사용할 수 없습니다. 사용 가능한 포터블 공인 IP 주소를 자동으로 할당할 수 있도록 로드 밸런서 서비스 구성 스크립트를 변경하고 사용 가능한 포터블 공인 IP 주소 중 하나를 선택하거나 스크립트에서 **loadBalancerIP** 섹션을 제거하십시오.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>로드 밸런서 서비스를 배치하는 데 충분한 작업자 노드가 없습니다. 그 이유 중 하나는 작업자 노드가 두 개 이상인 표준 클러스터를 배치했지만 작업자 노드의 프로비저닝에 실패했기 때문일 수 있습니다.</li>
    <ol><li>사용 가능한 작업자 노드를 나열하십시오.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>두 개 이상의 사용 가능한 작업자 노드를 발견하면 작업자 노드 세부사항을 나열하십시오.</br><pre class="screen"><code>bx cs worker-get [&lt;cluster_name_or_id&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li><code>kubectl get nodes</code> and the <code>bx cs [&lt;cluster_name_or_id&gt;] worker-get</code> 명령에서 리턴되는 작업자 노드의 퍼블릭 및 프라이빗 VLAN ID가 일치하는지 확인하십시오.</li></ol></li></ul>

4.  사용자 정의 도메인을 사용하여 로드 밸런서 서비스에 연결하는 경우 사용자 정의 도메인이 로드 밸런서 서비스의 공인 IP 주소에 맵핑되었는지 확인하십시오.
    1.  로드 밸런서 서비스의 공인 IP 주소를 찾으십시오.

      ```
      kubectl describe service <myservice> | grep "LoadBalancer Ingress"
      ```
      {: pre}

    2.  사용자 정의 도메인이 포인터 레코드(PTR)에 있는 로드 밸런서 서비스의 포터블 공인 IP 주소에 맵핑되었는지 확인하십시오.

<br />


## Ingress를 통해 앱에 연결하는 데 실패
{: #cs_ingress_fails}

{: tsSymptoms}
클러스터에 앱의 Ingress 리소스를 작성하여 공용으로 앱을 노출했습니다. Ingress 애플리케이션 로드 밸런서의 하위 도메인 또는 공인 IP 주소를 통해 앱에 연결하려고 할 때 연결에 실패했거나 제한시간이 초과되었습니다.

{: tsCauses}
다음과 같은 이유로 인해 Ingress가 제대로 작동하지 않을 수 있습니다.
<ul><ul>
<li>클러스터가 아직 완전히 배치되지 않았습니다.
<li>작업자 노드가 하나뿐인 표준 클러스터 또는 무료 클러스터로 클러스터가 설정되었습니다.
<li>Ingress 구성 스크립트에 오류가 포함되어 있습니다.
</ul></ul>

{: tsResolve}
Ingress 문제점을 해결하려면 다음을 수행하십시오.

1.  Ingress 애플리케이션 로드 밸런서의 고가용성을 위해 완전히 배치된 표준 클러스터를 설정하고 두 개 이상의 작업자 노드가 있는지 확인하십시오.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    CLI 출력에서 작업자 노드의 **상태**에 **준비**가 표시되며 **시스템 유형**에 **무료** 이외의 시스템 유형이 표시되는지 확인하십시오.

2.  Ingress 애플리케이션 로드 밸런서 하위 도메인 및 공인 IP 주소를 검색한 다음 각각에 대해 ping을 실행하십시오.

    1.  Ingress 제어기 하위 도메인을 검색하십시오.

      ```
      bx cs cluster-get <cluster_name_or_id> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Ingress 애플리케이션 로드 밸런서 하위 도메인에 대해 ping을 실행하십시오.

      ```
      ping <ingress_controller_subdomain>
      ```
      {: pre}

    3.  Ingress 애플리케이션 로드 밸런서의 공인 IP 주소를 검색하십시오.

      ```
      nslookup <ingress_controller_subdomain>
      ```
      {: pre}

    4.  Ingress 애플리케이션 로드 밸런서 공인 IP 주소에 대해 ping을 실행하십시오.

      ```
      ping <ingress_controller_ip>
      ```
      {: pre}

    CLI에서 Ingress 애플리케이션 로드 밸런서의 하위 도메인 또는 공인 IP 주소의 제한시간을 리턴하고 작업자 노드를 보호하는 사용자 정의 방화벽을 설정한 경우 [방화벽](#cs_firewall)에서 추가 포트 및 네트워킹 그룹을 열어야 할 수도 있습니다.

3.  사용자 정의 도메인을 사용하는 경우 사용자 정의 도메인이 도메인 이름 서비스(DNS) 제공자로 IBM에서 제공한 Ingress 애플리케이션 로드 밸런서의 하위 도메인 또는 공인 IP 주소에 맵핑되었는지 확인하십시오.
    1.  Ingress 애플리케이션 로드 밸런서 하위 도메인을 사용한 경우 표준 이름 레코드(CNAME)를 확인하십시오.
    2.  Ingress 애플리케이션 로드 밸런서 공인 IP 주소를 사용한 경우 사용자 정의 도메인이 포인터 레코드(PTR)의 포터블 공인 IP 주소에 맵핑되었는지 확인하십시오.
4.  Ingress 리소스 구성 파일을 확인하십시오.

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

    1.  Ingress 애플리케이션 로드 밸런서 하위 도메인 및 TLS 인증서가 올바른지 확인하십시오. IBM 제공 하위 도메인 및 TLS 인증서를 찾으려면 bx cs cluster-get <cluster_name_or_id>를 실행하십시오.
    2.  Ingress의 **path** 섹션에 구성된 동일한 경로에서 앱이 청취하는지 확인하십시오. 루트 경로에서 청취하도록 앱을 설정하는 경우 **/**를 경로로 포함하십시오.
5.  Ingress 배치를 확인하고 잠재적 오류 메시지를 검색하십시오.

  ```
  kubectl describe ingress <myingress>
  ```
  {: pre}

6.  Ingress 제어기의 로그를 확인하십시오.
    1.  클러스터에서 실행 중인 Ingress 포드의 ID를 검색하십시오.

      ```
      kubectl get pods -n kube-system | grep alb1
      ```
      {: pre}

    2.  각 Ingress 포드의 로그를 검색하십시오.

      ```
      kubectl logs <ingress_pod_id> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Ingress 제어기 로그에서 오류 메시지를 검색하십시오.

<br />



## Ingress 애플리케이션 로드 밸런서 시크릿 문제
{: #cs_albsecret_fails}

{: tsSymptoms}
Ingress 애플리케이션 로드 밸런서 시크릿을 클러스터에 배치한 후에는 {{site.data.keyword.cloudcerts_full_notm}}에서 인증서를 볼 때 `Description` 필드가 시크릿 이름으로 업데이트되지 않습니다.

애플리케이션 로드 밸런서에 관한 정보 목록을 볼 때 상태는 `*_failed`입니다. 예: `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
애플리케이션 로드 밸런서 시크릿의 실패 이유와 해당 문제점 해결 단계를 검토하십시오.

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>발생 원인</th>
 <th>해결 방법</th>
 </thead>
 <tbody>
 <tr>
 <td>인증서 데이터 다운로드와 업데이트에 필요한 액세스 역할이 없습니다.</td>
 <td>계정 관리자에게 문의하여 {{site.data.keyword.cloudcerts_full_notm}} 인스턴스에 대한 **Operator** 및 **Editor** 역할을 받으십시오. 세부사항은 {{site.data.keyword.cloudcerts_short}}의 <a href="/docs/services/certificate-manager/about.html#identity-access-management">ID 및 액세스 관리</a>를 참조하십시오.</td>
 </tr>
 <tr>
 <td>작성, 업데이트 또는 제거 시 제공한 인증서 CRN이 클러스터와 동일한 계정에 속하지 않습니다.</td>
 <td>제공한 인증서 CRN을 클러스터와 동일한 계정에 배치된 {{site.data.keyword.cloudcerts_short}} 서비스 인스턴스로 가져왔는지 확인하십시오.</td>
 </tr>
 <tr>
 <td>작성 시 제공한 인증서 CRN이 잘못되었습니다.</td>
 <td><ol><li>제공한 인증서 CRN이 정확한지 확인하십시오.</li><li>인증서 CRN이 정확하지 않은 경우, 시크릿을 업데이트하십시오. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>이 명령을 실행하여 <code>update_failed</code> 상태가 되면 시크릿을 제거하십시오. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>시크릿을 다시 배치하십시오. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>업데이트 시 제공한 인증서 CRN이 잘못되었습니다.</td>
 <td><ol><li>제공한 인증서 CRN이 정확한지 확인하십시오.</li><li>인증서 CRN이 정확하지 않은 경우, 시크릿을 제거하십시오. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>시크릿을 다시 배치하십시오. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>시크릿을 업데이트하십시오. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} 서비스에서 가동 중단이 발생합니다.</td>
 <td>{{site.data.keyword.cloudcerts_short}} 서비스가 시작되어 실행 중인지 확인하십시오.</td>
 </tr>
 </tbody></table>

<br />



## Calico CLI 구성의 ETCD URL 검색 실패
{: #cs_calico_fails}

{: tsSymptoms}
`<ETCD_URL>`을 검색하여 [네트워크 정책을 추가](cs_network_policy.html#adding_network_policies)할 때 `calico-config not found` 오류 메시지가 수신됩니다.

{: tsCauses}
클러스터가 [Kubernetes 버전 1.7](cs_versions.html) 이상이 아닙니다.

{: tsResolve}
[클러스터를 업데이트](cs_cluster_update.html#master)하거나 이전 버전의 Kubernetes와 호환 가능한 명령을 사용하여 `<ETCD_URL>`을 검색하십시오.

`<ETCD_URL>`을 검색하려면 다음 명령 중 하나를 실행하십시오.

- Linux 및 OS X:

    ```
              kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
    ```
    {: pre}

- Windows:
    <ol>
    <li> kube-system 네임스페이스에서 포드의 목록을 가져오고 Calico 제어기 포드를 찾으십시오. </br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>예:</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
    <li> Calico 제어기 포드의 세부사항을 보십시오.</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
    <li> ETCD 엔드포인트 값을 찾으십시오. 예: <code>https://169.1.1.1:30001</code>
    </ol>

`<ETCD_URL>`을 검색할 때 (네트워크 정책 추가)[cs_network_policy.html#adding_network_policies]에 나열된 단계를 계속하십시오.

<br />




## 도움 및 지원 받기
{: #ts_getting_help}

어디에서부터 컨테이너 문제점 해결을 시작하십니까?

-   {{site.data.keyword.Bluemix_notm}}가 사용 가능한지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조](https://developer.ibm.com/bluemix/support/#status)하십시오.
-   [{{site.data.keyword.containershort_notm}} Slack에 질문을 게시하십시오. ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com) 팁: {{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생하는지 여부를 확인하십시오. 포럼을 사용하여 질문을 할 때는 {{site.data.keyword.Bluemix_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

    -   {{site.data.keyword.containershort_notm}}로 클러스터 또는 앱을 개발하거나 배치하는 데 대한 기술적 질문이 있으면 [스택 오버플로우![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)에 질문을 게시하고 질문에 `ibm-cloud`, `kubernetes` 및 `containers` 태그를 지정하십시오.
    -   시작하기 지시사항과 서비스에 대한 질문은 [IBM developerWorks dW 응답![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `containers` 태그를 포함하십시오.
    포럼 사용에 대한 세부사항은 [도움 받기](/docs/get-support/howtogetsupport.html#using-avatar)를 참조하십시오.

-   IBM 지원 센터에 문의하십시오. IBM 지원 티켓 열기에 대한 정보나 지원 레벨 및 티켓 심각도에 대한 정보는 [지원 문의](/docs/get-support/howtogetsupport.html#getting-customer-support)를 참조하십시오.

{:tip}
문제를 보고할 때 클러스터 ID를 포함하십시오. 클러스터 ID를 가져오려면 `bx cs clusters`를 실행하십시오.
