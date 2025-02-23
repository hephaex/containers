---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 이미지에서 컨테이너 빌드
{: #images}

Docker 이미지는 작성하는 모든 컨테이너의 기초가 됩니다. 이미지는 이미지를 빌드하는 지시사항이 포함된 파일인 Dockerfile에서 작성됩니다. Dockerfile은 앱, 해당 앱의 구성 및 그 종속 항목과 같이 개별적으로 저장되는 해당 지시사항의 빌드 아티팩트를 참조할 수 있습니다.
{:shortdesc}


## 이미지 레지스트리 계획
{: #planning}

일반적으로 이미지는 공용으로 액세스 가능한 레지스트리(공용 레지스트리) 또는 소규모 사용자 그룹에 대한 제한된 액세스 권한으로 설정된 레지스트리(개인용 레지스트리)에 저장됩니다. 공용 레지스트리(예: Docker Hub)는 클러스터에서 첫 번째 컨테이너화된 앱을 작성하기 위해 Docker 및 Kubernetes를 시작하는 데 사용될 수 있습니다. 그러나 엔터프라이즈 애플리케이션인 경우에는 {{site.data.keyword.registryshort_notm}}에 제공된 레지스트리와 같은 개인용 레지스트리를 사용하여 권한 없는 사용자가 이미지를 사용하고 변경하지 못하도록 보호하십시오. 
개인용 레지스트리에 액세스하기 위한 신임 정보를 클러스터 사용자가 사용할 수 있도록 보장하기 위해,
개인용 레지스트리는 클러스터 관리자에 의해 설정되어야 합니다.
{:shortdesc}

{{site.data.keyword.containershort_notm}}에서 다중 레지스트리를 사용하여 클러스터에 앱을 배치할 수 있습니다.

|레지스트리|설명|이점|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|이 옵션을 사용하면 이미지를 안전하게 저장하고 이를 클러스터 사용자 간에 공유할 수 있는 {{site.data.keyword.registryshort_notm}}의 사용자 고유의 보안 Docker 이미지 저장소를 설정할 수 있습니다.|<ul><li>계정에서 이미지에 대한 액세스 권한을 관리합니다.</li><li>{{site.data.keyword.IBM_notm}}이 제공하는 이미지와 샘플 앱 {{site.data.keyword.IBM_notm}} Liberty를 상위 이미지로서 사용하고 이에 자체 앱 코드를 추가합니다.</li><li>Vulnerability Advisor에 의해 잠재적 취약점에 대한 이미지의 자동 스캐닝(이를 해결하기 위한 OS 특정 권장사항 포함).</li></ul>|
|기타 개인용 레지스트리|[imagePullSecret ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/containers/images/)을 작성하여 기존 개인용 레지스트리를 클러스터에 연결합니다. 시크릿은 Kubernetes 시크릿에 레지스트리 URL과 신임 정보를 안전하게 저장하는 데 사용됩니다.|<ul><li>자체 소스(Docker Hub, 조직이 소유하는 레지스트리 또는 기타 프라이빗 클라우드 레지스트리)와는 무관하게 기존 개인용 레지스트리를 사용합니다.</li></ul>|
|공용 Docker Hub|Dockerfile 변경사항이 필요 없을 때 Docker Hub에서 기존 공용 이미지를 직접 사용하려면 이 옵션을 사용합니다. <p>**참고:** 이 옵션이 조직의 보안 요구사항을 충족하지 않을 수 있음을 유념하십시오(예: 액세스 관리, 취약성 스캐닝 또는 앱 개인정보 보호정책).</p>|<ul><li>클러스터에 대해 추가 설정이 필요하지 않습니다.</li><li>다양한 오픈 소스 애플리케이션이 포함됩니다.</li></ul>|
{: caption="표. 공용 및 개인용 이미지 레지스트리 옵션" caption-side="top"}

이미지 레지스트리를 설정한 후에 클러스터 사용자는 클러스터에 자신의 앱 배치를 위한 이미지를 사용할 수 있습니다.


<br />



## {{site.data.keyword.registryshort_notm}}의 네임스페이스 액세스
{: #namespace}

{{site.data.keyword.registryshort_notm}}의 네임스페이스에 저장된 개인용 이미지나 IBM 제공 공용 이미지에서 클러스터로 컨테이너를 배치할 수 있습니다.
{:shortdesc}

시작하기 전에:

1. [{{site.data.keyword.Bluemix_notm}} 퍼블릭 또는 {{site.data.keyword.Bluemix_dedicated_notm}}의 {{site.data.keyword.registryshort_notm}}에 네임스페이스를 설정하고 이 네임스페이스에 이미지를 푸시](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)하십시오.
2. [클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
3. [클러스터에 CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

클러스터를 작성할 때 만료되지 않는 레지스트리 토큰과 시크릿이 [가장 근접한 지역 레지스트리와 글로벌 레지스트리](/docs/services/Registry/registry_overview.html#registry_regions) 둘 다에 대해 자동으로 작성됩니다. 글로벌 레지스트리는 개별 지역 레지스트리에 저장된 이미지에 대한 서로 다른 참조를 가지는 대신 배치 전반에서 참조할 수 있는 공용 IBM 제공 이미지를 저장합니다. 지역 레지스트리는 글로벌 레지스트리에 저장된 동일한 공용 이미지 외에도 사용자의 개인용 Docker 이미지를 안전하게 저장합니다. 토큰을 사용하여 이러한 공용(글로벌 레지스트리) 및 개인용(지역 레지스트리) 이미지로 작업할 수 있도록 {{site.data.keyword.registryshort_notm}}에서 사용자가 설정한 임의의 네임스페이스에 대한 읽기 전용 액세스 권한을 부여합니다. 

개별 토큰은 컨테이너화된 앱을 배치할 때 Kubernetes 클러스터에 액세스할 수 있도록 Kubernetes `imagePullSecret`에 저장되어야 합니다. 클러스터가 작성되면 {{site.data.keyword.containershort_notm}}에서 Kubernetes 이미지 풀 시크릿의 글로벌(IBM 제공 공용 이미지) 및 지역 레지스트리에 대한 토큰을 자동으로 저장합니다. 이미지 풀 시크릿은 `기본` Kubernetes 네임스페이스, 해당 네임스페이스의 `ServiceAccount`에 있는 기본 시크릿 목록 및 `kube-system` 네임스페이스에 추가됩니다.

**참고:** 이 초기 설정을 사용하면 {{site.data.keyword.Bluemix_notm}} 계정의 네임스페이스에서 사용할 수 있는 이미지에서 클러스터의 **기본** 네임스페이스로 컨테이너를 배치할 수 있습니다. 클러스터의 다른 네임스페이스로 컨테이너를 배치하려는 경우 또는 다른 {{site.data.keyword.Bluemix_notm}} 지역 또는 다른 {{site.data.keyword.Bluemix_notm}} 계정에 저장된 이미지를 사용하려는 경우, [클러스터에 대해 사용자 고유의 imagePullSecret을 작성](#other)해야 합니다.

클러스터의 **기본** 네임스페이스에 컨테이너를 배치하려면 구성 파일을 작성하십시오.

1.  `mydeployment.yaml`이라는 이름의 배치 구성 파일을 작성하십시오.
2.  {{site.data.keyword.registryshort_notm}}의 네임스페이스에서 사용하려는 배치 및 이미지를 정의하십시오.

    {{site.data.keyword.registryshort_notm}}의 네임스페이스에서 개인용 이미지 사용:

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **팁:** 네임스페이스 정보를 검색하려면 `bx cr namespace-list`를 실행하십시오.

3.  클러스터에 배치를 작성하십시오.

    ```
     kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **팁:** IBM 제공 공용 이미지 중 하나와 같이 기존 구성 파일도 배치할 수 있습니다. 이 예에서는 미국 남부 지역에서 **ibmliberty** 이미지를 사용합니다.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## 기타 Kubernetes 네임스페이스, {{site.data.keyword.Bluemix_notm}} 지역 및 계정의 이미지 액세스
{: #other}

고유 imagePullSecret을 작성하여 컨테이너를 다른 Kubernetes 네임스페이스에 배치하거나 다른 {{site.data.keyword.Bluemix_notm}} 지역 또는 계정에 저장된 이미지를 사용하거나 {{site.data.keyword.Bluemix_dedicated_notm}}에 저장된 이미지를 사용할 수 있습니다.
{:shortdesc}

시작하기 전에:

1.  [{{site.data.keyword.Bluemix_notm}} 퍼블릭 또는 {{site.data.keyword.Bluemix_dedicated_notm}}의 {{site.data.keyword.registryshort_notm}}에 네임스페이스를 설정하고 이 네임스페이스에 이미지를 푸시](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)하십시오.
2.  [클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
3.  [클러스터에 CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

고유 imagePullSecret을 작성하려면 다음을 수행하십시오.

**참고:** ImagePullSecrets는 사용하도록 지정된 Kubernetes 네임스페이스에만 유효합니다. 컨테이너를 배치하려는 모든 네임스페이스에 대해 이러한 단계를 반복하십시오. [DockerHub](#dockerhub)의 이미지에는 ImagePullSecret이 필요하지 않습니다.

1.  토큰이 없는 경우 [액세스할 레지스트리의 토큰을 작성](/docs/services/Registry/registry_tokens.html#registry_tokens_create)하십시오.
2.  {{site.data.keyword.Bluemix_notm}} 계정의 토큰을 나열하십시오.

    ```
     bx cr token-list
    ```
    {: pre}

3.  사용하려는 토큰 ID를 기록해 놓으십시오.
4.  토큰의 값을 검색하십시오. <em>&lt;token_id&gt;</em>를 이전 단계에서 검색한 토큰의 ID로 대체하십시오.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    사용자의 토큰 값이 CLI 출력의 **토큰** 필드에 표시됩니다.

5.  토큰 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>표. 이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>필수. 시크릿을 사용하고 컨테이너를 배치하려는 클러스터의 Kubernetes 네임스페이스입니다. 클러스터의 모든 네임스페이스를 나열하려면 <code>kubectl get namespaces</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>필수. imagePullSecret에 사용하려는 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>필수. 네임스페이스가 설정된 이미지 레지스트리에 대한 URL입니다.<ul><li>미국 남부 및 미국 동부에 설정된 네임스페이스: registry.ng.bluemix.net</li><li>미국 남부에 설정된 네임스페이스: registry.eu-gb.bluemix.net</li><li>중앙 유럽(프랑크푸르트)에 설정된 네임스페이스: registry.eu-de.bluemix.net</li><li>호주(시드니)에 설정된 네임스페이스: registry.au-syd.bluemix.net</li><li>{{site.data.keyword.Bluemix_dedicated_notm}} 레지스트리에 설정된 네임스페이스의 경우.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>필수. 개인용 레지스트리에 로그인하기 위한 사용자 이름입니다. {{site.data.keyword.registryshort_notm}}의 경우 사용자 이름이 <code>token</code>으로 설정됩니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>필수. 이전에 검색한 레지스트리 토큰의 값입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: example a@b.c)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하기 위해서는 반드시 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>

6.  시크릿이 작성되었는지 확인하십시오. <em>&lt;kubernetes_namespace&gt;</em>를 imagePullSecret을 작성한 네임스페이스의 이름으로 대체하십시오.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  imagePullSecret을 참조하는 포드를 작성하십시오.
    1.  `mypod.yaml`이라는 이름의 포드 구성 파일을 작성하십시오.
    2.  개인용 {{site.data.keyword.Bluemix_notm}} 레지스트리에 액세스하기 위해 사용하려는 포드 및 imagePullSecret을 정의하십시오.

        네임스페이스의 개인용 이미지:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/<my_namespace>/<my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        {{site.data.keyword.Bluemix_notm}} 퍼블릭 이미지:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>표. YAML 파일 컴포넌트 이해</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>클러스터에 배치하려는 컨테이너의 이름입니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_namespace&gt;</em></code></td>
        <td>이미지가 저장된 네임스페이스입니다. 사용 가능한 네임스페이스를 나열하려면 `bx cr namespace-list`를 실행하십시오.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>사용하려는 이미지의 이름입니다. {{site.data.keyword.Bluemix_notm}} 계정에서 사용 가능한 이미지를 나열하려면 `bx cr image-list`을 실행하십시오.</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>사용하려는 이미지의 버전입니다. 태그가 지정되지 않은 경우, 기본적으로 <strong>latest</strong>로 태그가 지정된 이미지가 사용됩니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>이전에 작성한 imagePullSecret의 이름입니다.</td>
        </tr>
        </tbody></table>

   3.  변경사항을 저장하십시오.
   4.  클러스터에 배치를 작성하십시오.

        ```
         kubectl apply -f mypod.yaml
        ```
        {: pre}


<br />



## Docker Hub에서 공용 이미지에 액세스
{: #dockerhub}

Docker Hub에 저장된 공용 이미지를 사용하여 추가 구성 없이 컨테이너를 클러스터에 배치할 수 있습니다.
{:shortdesc}

시작하기 전에:

1.  [클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
2.  [클러스터에 CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

배치 구성 파일을 작성하십시오.

1.  `mydeployment.yaml`이라는 이름의 구성 파일을 작성하십시오.
2.  사용하려는 Docker Hub로부터 배치 및 공용 이미지를 정의하십시오. 다음 구성 파일은 Docker Hub에서 사용 가능한 공용 NGINX 이미지를 사용합니다.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
    ```
    {: codeblock}

3.  클러스터에 배치를 작성하십시오.

    ```
     kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **팁:** 또는, 기존 구성 파일을 배치하십시오. 다음 예에서는 동일한 공용 NGINX 이미지를 사용하지만 클러스터에 직접 적용됩니다.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-nginx.yaml
    ```
    {: pre}

<br />



## 다른 개인용 레지스트리에 저장된 이미지에 액세스
{: #private_images}

사용하려는 개인용 레지스트리가 이미 있는 경우, 레지스트리 신임 정보를 Kubernetes imagePullSecret에 저장하고 구성 파일에서 이 시크릿을 참조해야 합니다.
{:shortdesc}

시작하기 전에:

1.  [클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
2.  [클러스터에 CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

imagePullSecret을 작성하려면 다음을 수행하십시오.

**참고:** ImagePullSecrets는 사용하도록 지정된 Kubernetes 네임스페이스에만 유효합니다. {{site.data.keyword.Bluemix_notm}} 레지스트리의 이미지에서 컨테이너를 배치하려는 모든 네임스페이스에 이러한 단계를 반복하십시오.

1.  개인용 레지스트리 신임 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>표. 이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>필수. 시크릿을 사용하고 컨테이너를 배치하려는 클러스터의 Kubernetes 네임스페이스입니다. 클러스터의 모든 네임스페이스를 나열하려면 <code>kubectl get namespaces</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>필수. imagePullSecret에 사용하려는 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>필수. 개인용 이미지가 저장된 레지스트리에 대한 URL입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>필수. 개인용 레지스트리에 로그인하기 위한 사용자 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>필수. 이전에 검색한 레지스트리 토큰의 값입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: example a@b.c)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하기 위해서는 반드시 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>

2.  시크릿이 작성되었는지 확인하십시오. <em>&lt;kubernetes_namespace&gt;</em>를 imagePullSecret을 작성한 네임스페이스의 이름으로 대체하십시오.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  imagePullSecret을 참조하는 포드를 작성하십시오.
    1.  `mypod.yaml`이라는 이름의 포드 구성 파일을 작성하십시오.
    2.  개인용 {{site.data.keyword.Bluemix_notm}} 레지스트리에 액세스하기 위해 사용하려는 포드 및 imagePullSecret을 정의하십시오. 개인용 레지스트리의 개인용 이미지를 사용하려면 다음을 작성하십시오.

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: <my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>표. YAML 파일 컴포넌트 이해</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;pod_name&gt;</em></code></td>
        <td>작성하려는 포드의 이름입니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>클러스터에 배치하려는 컨테이너의 이름입니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>사용하려는 개인용 레지스트리의 이미지에 대한 전체 경로입니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>사용하려는 이미지의 버전입니다. 태그가 지정되지 않은 경우, 기본적으로 <strong>latest</strong>로 태그가 지정된 이미지가 사용됩니다.</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>이전에 작성한 imagePullSecret의 이름입니다.</td>
        </tr>
        </tbody></table>

  3.  변경사항을 저장하십시오.
  4.  클러스터에 배치를 작성하십시오.

        ```
         kubectl apply -f mypod.yaml
        ```
        {: pre}

