---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 클러스터의 서브넷 구성
{: #subnets}

클러스터에 서브넷을 추가하여 사용 가능한 포터블 공인 또는 사설 IP 주소의 풀을 변경하십시오.
{:shortdesc}

{{site.data.keyword.containershort_notm}}에서 사용자는 클러스터에 네트워크 서브넷을 추가하여 Kubernetes 서비스에 대한 안정적인 포터블 IP를 추가할 수 있습니다. 이 경우 서브넷은 하나 이상의 클러스터 전체에 걸쳐 연결성을 작성하기 위해 넷마스킹과 함께 사용되지 않습니다. 대신 서브넷은 클러스터에서 해당 서비스에 액세스할 때 사용될 수 있는 영구적인 고정 IP를 서비스에 제공하는 데 사용됩니다.

표준 클러스터를 작성하면 {{site.data.keyword.containershort_notm}}가 포터블 공인 서브넷에 5개의 공인 IP 주소를, 포터블 사설 서브넷에 5개의 사설 IP 주소를 자동으로 프로비저닝합니다. 포터블 공인 및 사설 IP 주소는 정적이며 작업자 노드 또는 클러스터가 제거되더라도 변경되지 않습니다. 각 서브넷에 대해 포터블 공인 IP 주소 중 하나와 포터블 사설 IP 주소 중 하나는 클러스터에서 다중 앱을 노출하는 데 사용할 수 있는 [애플리케이션 로드 밸런서](cs_ingress.html)에 사용됩니다. 나머지 네 개의 포터블 공인 IP 주소 및 네 개의 포터블 사설 IP 주소는 [로드 밸런서 서비스 작성](cs_loadbalancer.html)을 통해 단일 앱을 공용으로 노출하는 데 사용될 수 있습니다.

**참고:** 포터블 공인 IP 주소는 매월 비용이 청구됩니다. 클러스터가 프로비저닝된 후에 포터블 공인 IP 주소를 제거하도록 선택한 경우, 비록 짧은 시간 동안만 사용했어도 월별 비용을 계속 지불해야 합니다.

## 클러스터에 대한 추가 서브넷 요청
{: #request}

클러스터에 서브넷을 지정하여 클러스터에 안정적인 포터블 공인 또는 사설 IP를 추가할 수 있습니다.

**참고:** 클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에
{{site.data.keyword.containershort_notm}}의 외부에서
다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.

시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

IBM Cloud 인프라(SoftLayer) 계정에서 서브넷을 작성하고 지정된 클러스터에서 사용 가능하도록 설정하려면 다음을 수행하십시오.

1. 새 서브넷을 프로비저닝하십시오.

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>클러스터를 위한 서브넷을 프로비저닝하는 명령입니다.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td><code>&lt;cluster_name_or_id&gt;</code>를 클러스터의 이름 또는 ID로 대체하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td><code>&lt;subnet_size&gt;</code>를 포터블 서브넷에서 추가할 IP 주소의 수로 대체하십시오. 허용되는 값은 8, 16, 32 또는 64입니다. <p>**참고:** 서브넷에 대한 포터블 IP 주소를 추가할 때 세 개의 IP 주소를 사용하여 클러스터 내부 네트워킹을 설정하므로 이들을 애플리케이션 로드 밸런서에 대해 사용하거나 로드 밸런서 서비스를 작성하는 데 사용할 수 없습니다. 예를 들어, 8개의 포터블 공인 IP 주소를 요청하는 경우 이 중에서 5개를 사용하여 앱을 공용으로 노출할 수 있습니다.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td><code>&lt;VLAN_ID&gt;</code>를 포터블 공인 또는 사설 IP 주소를 할당할 퍼블릭 또는 프라이빗 VLAN의 ID로 대체하십시오. 기존 작업자 노드가 연결되어 있는 퍼블릭 또는 프라이빗 VLAN을 선택해야 합니다. 작업자 노드의 퍼플릭 또는 프라이빗 VLAN을 검토하려면 <code>bx cs worker-get &lt;worker_id&gt;</code> 명령을 실행하십시오. </td>
    </tr>
    </tbody></table>

2.  서브넷이 정상적으로 작성되어 클러스터에 추가되었는지 확인하십시오. 서브넷 CIDR이 **VLAN** 섹션에 나열됩니다.

    ```
    bx cs cluster-get --showResources <cluster name or id>
    ```
    {: pre}

<br />


## Kubernetes 클러스터에 사용자 정의 및 기존 서브넷 추가
{: #custom}

Kubernetes 클러스터에 기존의 포터블 공인 또는 사설 서브넷을 추가할 수 있습니다.

시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

사용할 사용자 정의 방화벽 규칙이나 사용 가능한 IP 주소와 함께 IBM Cloud 인프라(SoftLayer) 포트폴리오에 기존 서브넷이 있는 경우 서브넷 없이 클러스터를 작성하고 클러스터가 프로비저닝할 때 클러스터에서 기존 서브넷을 사용할 수 있도록 하십시오.

1.  사용할 서브넷을 식별하십시오. 서브넷의 ID 및 VLAN ID를 기록해 두십시오. 이 예제에서 서브넷 ID는 807861이며 VLAN ID는 1901230입니다.

    ```
     bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network                                      Gateway                                   VLAN ID   Type      Bound Cluster   
    553242    203.0.113.0/24                               10.87.15.00                               1565280   private      
    807861    192.0.2.0/24                                 10.121.167.180                            1901230   public

    ```
    {: screen}

2.  VLAN이 있는 위치를 확인하십시오. 이 예제에서 위치는 dal10입니다.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name                  Number   Type      Router   
    1900403   vlan                    1391     private   bcr01a.dal10   
    1901230   vlan                    1180     public   fcr02a.dal10
    ```
    {: screen}

3.  식별된 위치와 VLAN ID를 사용하여 클러스터를 작성하십시오. 새 포터블 공인 IP 서브넷 및 새 포터블 사설 IP 서브넷이 자동으로 작성되지 않도록 `--no-subnet` 플래그를 포함하십시오.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 1901230 --private-vlan 1900403 --workers 3 --name my_cluster
    ```
    {: pre}

4.  클러스터 작성이 요청되었는지 확인하십시오.

    ```
         bx cs clusters
    ```
    {: pre}

    **참고:** 작업자 노드 시스템을 정렬하고, 클러스터를 설정하고 계정에 프로비저닝하는 데 최대 15분이 소요될 수 있습니다.

    클러스터의 프로비저닝이 완료되면 클러스터의 상태가 **deployed**로 변경됩니다.

    ```
    Name         ID                                   State      Created          Workers   
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   3
    ```
    {: screen}

5.  작업자 노드의 상태를 확인하십시오.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    작업자 노드가 준비되면 상태(state)가 **정상**으로 변경되며, 상태(status)는 **준비**입니다. 노드 상태(status)가 **준비**인 경우에는 클러스터에 액세스할 수 있습니다.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status  
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  서브넷 ID를 지정하여 클러스터에 서브넷을 추가하십시오. 클러스터가 서브넷을 사용할 수 있도록 하는 경우에는 사용자가 사용할 수 있는 모든 사용 가능한 포터블 공인 IP 주소가 포함된 Kubernetes 구성 맵이 사용자를 위해 작성됩니다. 클러스터를 위한 애플리케이션 로드 밸런서가 아직 존재하지 않는 경우, 하나의 포터블 공인 IP 주소와 하나의 포터블 사설 IP 주소가 자동으로 사용되어 공인 및 사설 애플리케이션 로드 밸런서를 작성합니다. 기타 모든 포터블 공인 및 사설 IP 주소를 사용하여 사용자 앱에 대한 로드 밸런서 서비스를 작성할 수 있습니다.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

<br />


## Kubernetes 클러스터에 사용자가 관리하는 서브넷 및 IP 주소 추가
{: #user_managed}

{{site.data.keyword.containershort_notm}}에서 액세스하도록 하려는 온프레미스 네트워크의 고유한 서브넷을 제공하십시오. 그런 다음 해당 서브넷의 사설 IP 주소를 Kubernetes 클러스터의 로드 밸런서 서비스에 추가할 수 있습니다.

요구사항:
- 사용자가 관리하는 서브넷은 프라이빗 VLAN에만 추가할 수 있습니다.
- 서브넷 접두부 길이 한계는 /24 - /30입니다. 예를 들어, `203.0.113.0/24`는 253개의 사용 가능한 사설 IP 주소를 지정하지만 `203.0.113.0/30`은 1개의 사용 가능한 사설 IP 주소를 지정합니다.
- 서브넷의 첫 번째 IP 주소는 서브넷에 대한 게이트웨이로 사용되어야 합니다.

시작하기 전에:
- 외부 서브넷에 들어오고 나가는 네트워크 트래픽의 라우팅을 구성하십시오.
- 온프레미스 데이터센터 게이트웨이 디바이스와 IBM Cloud 인프라(SoftLayer) 포트폴리오의 사설 네트워크 Vyatta 또는 클러스터에서 실행되는 Strongswan VPN 서비스 간의 VPN 연결이 있는지 확인하십시오. Vyatta를 사용하려면, 이 [블로그 게시물 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/)을 참조하십시오. Strongswan을 사용하려면, [Strongswan IPSec VPN 서비스와 VPN 연결 설정](cs_vpn.html)을 참조하십시오.

1. 클러스터 프라이빗 VLAN의 ID를 보십시오. **VLAN** 섹션을 찾으십시오. **사용자 관리** 필드에서 _false_인 VLAN ID를 식별하십시오.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    ```
    {: screen}

2. 프라이빗 VLAN에 외부 서브넷을 추가하십시오. 포터블 사설 IP 주소가 클러스터의 구성 맵에 추가됩니다.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    예:

    ```
    bx cs cluster-user-subnet-add my_cluster 203.0.113.0/24 1555505
    ```
    {: pre}

3. 사용자 제공 서브넷이 추가되었는지 확인하십시오. **사용자 관리** 필드가 _true_입니다.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    1555505   203.0.113.0/24      false        true
    ```
    {: screen}

4. 사설 네트워크를 통해 앱에 액세스하려면 사설 로드 밸런서 서비스 또는 사설 Ingress 애플리케이션 로드 밸런서를 추가하십시오. 서브넷에서 사설 로드 밸런서 또는 사설 Ingress 애플리케이션 로드 밸런서를 작성할 때 추가한 사설 IP 주소를 사용하려면 IP 주소를 지정해야 합니다. 그렇지 않으면 IBM Cloud 인프라(SoftLayer) 서브넷 또는 프라이빗 VLAN의 사용자 제공 서브넷에서 랜덤으로 IP 주소가 선택됩니다. 자세한 정보는 [로드 밸런서 서비스 유형을 사용하여 앱에 대한 액세스 구성](cs_loadbalancer.html#config) 또는 [사설 애플리케이션 로드 밸런서 사용](cs_ingress.html#private_ingress)을 참조하십시오.

<br />


## IP 주소 및 서브넷 관리
{: #manage}

포터블 공인 및 사설 서브넷과 IP 주소를 사용하여 앱을 클러스터에 노출시켜 인터넷 또는 사설 네트워크에서 액세스 가능하도록 설정할 수 있습니다.
{:shortdesc}

{{site.data.keyword.containershort_notm}}에서 사용자는 클러스터에 네트워크 서브넷을 추가하여 Kubernetes 서비스에 대한 안정적인 포터블 IP를 추가할 수 있습니다. 표준 클러스터를 작성하면 {{site.data.keyword.containershort_notm}}에서 포터블 공인 서브넷에 5개의 포터블 공인 IP 주소를, 포터블 사설 서브넷에 5개의 포터블 사설 IP 주소를 자동으로 프로비저닝합니다. 포터블 IP 주소는 정적이며 작업자 노드 또는 클러스터가 제거되더라도 변경되지 않습니다.

포터블 IP 주소 중 2개(하나는 공인, 하나는 사설)는 클러스터에서 다중 앱을 노출하는 데 사용할 수 있는 [Ingress 애플리케이션 로드 밸런서](cs_ingress.html)에 사용됩니다. [로드 밸런서 서비스를 작성](cs_loadbalancer.html)하여 앱을 노출시키는 데 4개의 포터블 공인 IP 주소와 4개의 포터블 사설 IP 주소를 사용할 수 있습니다.

**참고:** 포터블 공인 IP 주소는 매월 비용이 청구됩니다. 클러스터가 프로비저닝된 후에 포터블 공인 IP 주소를 제거하도록 선택한 경우, 비록 짧은 시간 동안만 사용했어도 월별 비용을 계속 지불해야 합니다.



1.  `myservice.yaml`이라는 이름의 Kubernetes 서비스 구성 파일을 작성하고 더미 로드 밸런서 IP 주소를 사용하여 `LoadBalancer` 유형의 서비스를 정의하십시오. 다음 예는 로드 밸런서 IP 주소로 IP 주소 1.1.1.1을 사용합니다.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  클러스터에 서비스를 작성하십시오.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  서비스를 검사하십시오.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **참고:** Kubernetes 마스터가 Kubernetes 구성 맵에서 지정된 로드 밸런서 IP 주소를 찾을 수 없기 때문에 이 서비스의 작성에 실패합니다. 이 명령을 실행하면 오류 메시지 및 클러스터에 사용 가능한 공인 IP 주소의 목록을 볼 수 있습니다.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IPs are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />




## 사용된 IP 주소 해제
{: #free}

포터블 IP 주소를 사용 중인 로드 밸런서 서비스를 삭제하여 사용된 포터블 공인 IP 주소를 해제할 수 있습니다.

시작하기 전에, [사용할 클러스터의 컨텍스트를 설정하십시오.](cs_cli_install.html#cs_cli_configure)

1.  클러스터에서 사용 가능한 서비스를 나열하십시오.

    ```
     kubectl get services
    ```
    {: pre}

2.  공인 또는 사설 IP 주소를 사용하는 로드 밸런서 서비스를 제거하십시오.

    ```
    kubectl delete service <myservice>
    ```
    {: pre}
