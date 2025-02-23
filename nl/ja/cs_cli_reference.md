---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# クラスターを管理するための CLI リファレンス
{: #cs_cli_reference}

{{site.data.keyword.Bluemix_notm}} でクラスターの作成と管理を行うには、以下のコマンドを参照してください。
{:shortdesc}

## bx cs コマンド
{: #cs_commands}

**ヒント:** `bx cr` コマンドをお探しですか? [{{site.data.keyword.registryshort_notm}} CLI リファレンス](/docs/cli/plugins/registry/index.html)を参照してください。 `kubectl` コマンドをお探しですか? [Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) を参照してください。

**ヒント:** {{site.data.keyword.containershort_notm}} プラグインのバージョンを表示するには、以下のコマンドを実行します。

```
bx plugin list
```
{: pre}



<table summary="アプリケーション・ロード・バランサーのコマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>アプリケーション・ロード・バランサー・コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
    <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
    <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
    <td>[bx cs alb-certs](#cs_alb_certs)</td>
  </tr>
  <tr>
    <td>[bx cs alb-configure](#cs_alb_configure)</td>
    <td>[bx cs alb-get](#cs_alb_get)</td>
    <td>[bx cs alb-types](#cs_alb_types)</td>
    <td>[bx cs albs](#cs_albs)</td>
 </tr>
</tbody>
</table>

<br>

<table summary="API コマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI プラグインの使用方法を示すコマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI プラグインの使用方法を示すコマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="クラスター・コマンド: 管理">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>クラスター・コマンド: 管理</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="クラスター・コマンド: サービスと統合">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>クラスター・コマンド: サービスと統合</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="クラスター・コマンド: サブネット">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>クラスター・コマンド: サブネット</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="インフラストラクチャー・コマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>インフラストラクチャー・コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="ロギング・コマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>ロギング・コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs logging-config-create](#cs_logging_create)</td>
    <td>[bx cs logging-config-get](#cs_logging_get)</td>
    <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
    <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
  </tr>
  <tr>
    <td>[bx cs logging-config-update](#cs_logging_update)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="地域コマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>地域コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="ワーカー・ノード・コマンド">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>ワーカー・ノード・コマンド</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs worker-add](#cs_worker_add)</td>
    <td>[bx cs worker-get](#cs_worker_get)</td>
    <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
    <td>[bx cs worker-reload](#cs_worker_reload)</td>
  </tr>
  <tr>
    <td>[bx cs worker-rm](#cs_worker_rm)</td>
    <td>[bx cs worker-update](#cs_worker_update)</td>
    <td>[bx cs workers](#cs_workers)</td>
    <td></td>
  </tr>
</tbody>
</table>

## アプリケーション・ロード・バランサー・コマンド
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

クラスター内のアプリケーション・ロード・バランサーに対する、{{site.data.keyword.cloudcerts_long_notm}} インスタンスからの証明書のデプロイまたは更新を実行します。

**注:**
* 管理者アクセス役割を持つユーザーのみが、このコマンドを実行できます。
* 同じ {{site.data.keyword.cloudcerts_long_notm}} インスタンスからインポートされた証明書のみを更新できます。

<strong>コマンド・オプション</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--update</code></dt>
   <dd>クラスター内のアプリケーション・ロード・バランサー・シークレットの証明書を更新するには、このフラグを指定します。 この値はオプションです。</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>アプリケーション・ロード・バランサー・シークレットの名前。 この値は必須です。</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>証明書 CRN。 この値は必須です。</dd>
   </dl>

**例**:

アプリケーション・ロード・バランサー・シークレットをデプロイする場合の例:

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret_name --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

既存のアプリケーション・ロード・バランサー・シークレットを更新する場合の例:

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret_name --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

クラスター内のアプリケーション・ロード・バランサー・シークレットに関する情報を表示します。

**注:** 管理者アクセス役割を持つユーザーのみが、このコマンドを実行できます。

<strong>コマンド・オプション</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>クラスターの名前または ID。 この値は必須です。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>アプリケーション・ロード・バランサー・シークレットの名前。 この値は、クラスター内の特定のアプリケーション・ロード・バランサー・シークレットに関する情報を取得するときに必要です。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>証明書 CRN。 この値は、クラスター内の特定の証明書 CRN と一致する、すべてのアプリケーション・ロード・バランサー・シークレットに関する情報を取得するときに必要です。</dd>
  </dl>

**例**:

 アプリケーション・ロード・バランサー・シークレットに関する情報を取り出す場合の例:

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret_name
 ```
 {: pre}

 指定された証明書 CRN と一致する、すべてのアプリケーション・ロード・バランサー・シークレットに関する情報を取り出す場合の例:

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

クラスター内のアプリケーション・ロード・バランサー・シークレットを削除します。

**注:** 管理者アクセス役割を持つユーザーのみが、このコマンドを実行できます。

<strong>コマンド・オプション</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>クラスターの名前または ID。 この値は必須です。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB シークレットの名前。 この値は、クラスター内の特定のアプリケーション・ロード・バランサー・シークレットを削除するときに必要です。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>証明書 CRN。 この値は、クラスター内の特定の証明書 CRN と一致する、すべてのアプリケーション・ロード・バランサー・シークレットを削除するときに必要です。</dd>
  </dl>

**例**:

 アプリケーション・ロード・バランサー・シークレットを削除する場合の例:

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret_name
 ```
 {: pre}

 指定された証明書 CRN と一致する、すべてのアプリケーション・ロード・バランサー・シークレットを削除する場合の例:

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

クラスター内のアプリケーション・ロード・バランサー・シークレットのリストを表示します。

**注:** 管理者アクセス役割を持つユーザーのみが、このコマンドを実行できます。

<strong>コマンド・オプション</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}




### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

標準クラスターで、アプリケーション・ロード・バランサーを有効または無効にします。 パブリック・アプリケーションのロード・バランサーは、デフォルトで有効になります。

**コマンド・オプション**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>アプリケーション・ロード・バランサーの ID。 クラスター内のアプリケーション・ロード・バランサーの ID を表示するには、<code>bx cs albs <em>--cluster </em>CLUSTER</code> を実行します。 この値は必須です。</dd>

   <dt><code>--enable</code></dt>
   <dd>クラスター内のアプリケーション・ロード・バランサーを有効にするには、このフラグを指定します。</dd>

   <dt><code>--disable</code></dt>
   <dd>クラスター内のアプリケーション・ロード・バランサーを無効にするには、このフラグを指定します。</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>このパラメーターはプライベート・アプリケーションのロード・バランサーの場合にのみ使用できます。</li>
    <li>プライベート・アプリケーションのロード・バランサーは、ユーザー提供のプライベート・サブネットからの IP アドレスとともにデプロイされます。 IP アドレスが提供されない場合、アプリケーション・ロード・バランサーは、クラスターの作成時に自動的にプロビジョンされたポータブル・プライベート・サブネットのプライベート IP アドレスを使用してデプロイされます。</li>
   </ul>
   </dd>
   </dl>

**例**:

  アプリケーション・ロード・バランサーを有効にする場合の例:

  ```
  bx cs alb-configure --albID my_alb_id --enable
  ```
  {: pre}

  アプリケーション・ロード・バランサーを無効にする場合の例:

  ```
  bx cs alb-configure --albID my_alb_id --disable
  ```
  {: pre}

  ユーザー提供の IP アドレスを使用してアプリケーション・ロード・バランサーを有効にする場合の例:

  ```
  bx cs alb-configure --albID my_private_alb_id --enable --user-ip user_ip
  ```
  {: pre}



### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

アプリケーション・ロード・バランサーの詳細を表示します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>アプリケーション・ロード・バランサーの ID。 クラスター内のアプリケーション・ロード・バランサーの ID を表示するには、<code>bx cs albs --cluster <em>CLUSTER</em></code> を実行します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs alb-get --albID ALB_ID
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

この地域でサポートされているアプリケーション・ロード・バランサーのタイプを表示します。

<strong>コマンド・オプション</strong>:

   なし

**例**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

クラスター内のすべてのアプリケーション・ロード・バランサーの状態を表示します。 アプリケーション・ロード・バランサーの ID が返されない場合、クラスターには移植可能なサブネットはありません。 サブネットを[作成](#cs_cluster_subnet_create)したり、クラスターに[追加](#cs_cluster_subnet_add)したりすることができます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>使用可能なアプリケーション・ロード・バランサーをリストするクラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs albs --cluster mycluster
  ```
  {: pre}


<br />


## API コマンド
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

クラスターの IAM API キーの所有者の名前と E メール・アドレスを表示します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

API キーを置換します。クラスターを管理するためには API キーが必要です。サービスの中断を防ぐため、既存のキーが漏えいした場合以外は API キーを置換しないでください。

**例**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

クラスターの Kubernetes API サーバー構成のオプションに関する情報を取得します。このコマンドは、情報を取得する構成オプションを表す以下のいずれかのサブコマンドと一緒に指定する必要があります。

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

API サーバー監査ログの送信先となるリモート・ロギング・サービスの URL を表示します。 URL は、API サーバー構成の Web フック・バックエンドの作成時に指定されています。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

クラスターの Kubernetes API サーバー構成のオプションを設定します。 このコマンドは、設定する構成オプションに対する以下のいずれかのサブコマンドと結合させる必要があります。

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

API サーバー構成の Web フック・バックエンドを設定します。 Web フック・バックエンドは、API サーバー監査ログをリモート・サーバーに転送します。 Web フック構成は、このコマンドのフラグで指定する情報に基づいて作成されます。 どのフラグにも情報を指定しない場合、デフォルトの Web フック構成が使用されます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>監査ログの送信先となるリモート・ロギング・サービスの URL または IP アドレス。 非セキュアなサーバー URL を指定した場合、すべての証明書は無視されます。 この値はオプションです。</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>リモート・ロギング・サービスの検証に使用される CA 証明書のファイル・パス。 この値はオプションです。</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>リモート・ロギング・サービスに対する認証に使用されるクライアント証明書のファイル・パス。 この値はオプションです。</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>リモート・ロギング・サービスへの接続に使用される、対応するクライアント・キーのファイル・パス。 この値はオプションです。</dd>
   </dl>

**例**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

クラスターの Kubernetes API サーバー構成のオプションを無効にします。このコマンドは、設定を解除する構成オプションを表す以下のいずれかのサブコマンドと一緒に指定する必要があります。

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

クラスターの API サーバーの Web フック・バックエンド構成を無効にします。 Web フック・バックエンドを無効にすると、リモート・サーバーへの API サーバー監査ログの転送が停止します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

API サーバー構成への変更を適用するには、クラスター内の Kubernetes マスターを再始動します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## CLI プラグインの使用方法を示すコマンド
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

サポートされるコマンドとパラメーターのリストを表示します。

<strong>コマンド・オプション</strong>:

   なし

**例**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

{{site.data.keyword.containershort_notm}} プラグインを初期化するか、Kubernetes クラスターを作成またはアクセスする地域を指定します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>使用する {{site.data.keyword.containershort_notm}} API エンドポイント。  この値はオプションです。 [API エンドポイントの選択可能な値を表示します。](cs_regions.html#container_regions)</dd>
   </dl>

**例**:


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

IBMid ユーザーに対する現在のメッセージを表示します。

**例**:

```
bx cs messages
```
{: pre}


<br />


## クラスター・コマンド: 管理
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

ログインした後に、クラスターに接続して `kubectl` コマンドを実行するための Kubernetes 構成データと証明書をダウンロードします。 それらのファイルは、`user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>` にダウンロードされます。

**コマンド・オプション**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--admin</code></dt>
   <dd>スーパーユーザー役割の TLS 証明書と許可ファイルをダウンロードします。 それらの証明書を使用すると、再認証しなくてもクラスターでのタスクを自動化することができます。 それらのファイルは、`<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin` にダウンロードされます。 この値はオプションです。</dd>

   <dt><code>--export</code></dt>
   <dd>export コマンド以外のメッセージなしで、Kubernetes の構成データと証明書をダウンロードします。 メッセージが表示されないため、自動化スクリプトを作成するときにこのフラグを使用することができます。 この値はオプションです。</dd>
   </dl>

**例**:

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt]
{: #cs_cluster_create}

組織内にクラスターを作成します。

<strong>コマンド・オプション</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>標準クラスターを作成する YAML ファイルのパス。 このコマンドに用意されているオプションを使用してクラスターの特性を定義する代わりに、YAML ファイルを使用することができます。  この値は、標準クラスターではオプションで、フリー・クラスターでは使用できません。

<p><strong>注:</strong> YAML ファイル内のパラメーターと同じオプションをコマンドで指定した場合は、コマンドの値が YAML 内の値よりも優先されます。 例えば、YAML ファイル内で場所を定義し、コマンドで <code>--location</code> オプションを使用した場合は、コマンド・オプションに入力した値が YAML ファイル内の値をオーバーライドします。

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
    <caption>表 1. YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td><code><em>&lt;cluster_name&gt;</em></code> をクラスターの名前に置き換えます。</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td><code><em>&lt;location&gt;</em></code> を、クラスターを作成する場所に置き換えます。 使用可能な場所は、ログインしている地域によって異なります。 使用可能な場所をリストするには、<code>bx cs locations</code> を実行します。 </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>デフォルトでは、パブリックとプライベートの両方のポータブル・サブネットが、クラスターに関連付けられた VLAN 上に作成されます。 クラスターを使用してサブネットを作成することを回避するには、<code><em>&lt;no-subnet&gt;</em></code> を <code><em>true</em></code> に置き換えます。 後でサブネットを[作成](#cs_cluster_subnet_create)したり、クラスターに[追加](#cs_cluster_subnet_add)したりすることができます。</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td><code><em>&lt;machine_type&gt;</em></code> を、ワーカー・ノードに使用するマシン・タイプに置き換えます。 現在の場所で使用可能なマシン・タイプをリストするには、<code>bx cs machine-types <em>&lt;location&gt;</em></code> を実行します。</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td><code><em>&lt;private_vlan&gt;</em></code> を、ワーカー・ノードに使用するプライベート VLAN の ID に置き換えます。 使用可能な VLAN をリストするには、<code>bx cs vlans <em>&lt;location&gt;</em></code> を実行して、<code>bcr</code> で始まる VLAN ルーター (バックエンド・ルーター) を探します。</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td><code><em>&lt;public_vlan&gt;</em></code> を、ワーカー・ノードに使用するパブリック VLAN の ID に置き換えます。 使用可能な VLAN をリストするには、<code>bx cs vlans <em>&lt;location&gt;</em></code> を実行して、<code>fcr</code> で始まる VLAN ルーター (フロントエンド・ルーター) を探します。</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>ワーカー・ノードのハードウェア分離のレベル。 使用可能な物理リソースを自分専用にする場合には dedicated を使用し、他の IBM の顧客と物理リソースを共有することを許可する場合には shared を使用してください。 デフォルトは <code>shared</code> です。</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td><code><em>&lt;number_workers&gt;</em></code> を、デプロイするワーカー・ノードの数に置き換えます。</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>クラスター・マスター・ノードの Kubernetes のバージョン。 この値はオプションです。 これを指定しなかった場合、クラスターは、サポートされている Kubernetes のバージョンのデフォルトを使用して作成されます。 使用可能なバージョンを確認するには、<code>bx cs kube-versions</code> を実行します。</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>ワーカー・ノードには、デフォルトでディスク暗号化の機能があります。[詳しくはこちらを参照してください](cs_secure.html#worker)。 暗号化を無効にするには、このオプションを組み込んで値を <code>false</code> に設定します。</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>ワーカー・ノードのハードウェア分離のレベル。 使用可能な物理リソースを自分専用にする場合は dedicated を使用し、IBM の他のお客様と物理リソースを共有することを許可する場合は shared を使用します。 デフォルトは shared です。  この値は、標準クラスターではオプションで、フリー・クラスターでは使用できません。</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>クラスターを作成する場所。 使用可能な場所は、ログインしている {{site.data.keyword.Bluemix_notm}} 地域によって異なります。 最高のパフォーマンスを得るために、物理的に最も近い地域を選択してください。  この値は、標準クラスターでは必須で、フリー・クラスターではオプションです。

<p>[使用可能なロケーション](cs_regions.html#locations)を参照してください。
</p>

<p><strong>注:</strong> 国外の場所を選択する場合は、その国にデータを物理的に保管する前に法的な許可を得なければならないことがあります。</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>選択したマシン・タイプによって、ワーカー・ノードにデプロイされたコンテナーで使用できるメモリーとディスク・スペースの量が左右されます。 使用可能なマシン・タイプをリストするには、[bx cs machine-types <em>LOCATION</em>](#cs_machine_types) を参照してください。  この値は、標準クラスターでは必須で、フリー・クラスターでは使用できません。</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>クラスターの名前。  この値は必須です。</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>クラスター・マスター・ノードの Kubernetes のバージョン。 この値はオプションです。 これを指定しなかった場合、クラスターは、サポートされている Kubernetes のバージョンのデフォルトを使用して作成されます。 使用可能なバージョンを確認するには、<code>bx cs kube-versions</code> を実行します。</dd>

<dt><code>--no-subnet</code></dt>
<dd>デフォルトでは、パブリックとプライベートの両方のポータブル・サブネットが、クラスターに関連付けられた VLAN 上に作成されます。 クラスターを使用してサブネットを作成することを回避するには、<code>--no-subnet</code> フラグを指定します。 後でサブネットを[作成](#cs_cluster_subnet_create)したり、クラスターに[追加](#cs_cluster_subnet_add)したりすることができます。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>このパラメーターは、フリー・クラスターでは使用できません。</li>
<li>この標準クラスターが、この場所に作成する最初の標準クラスターである場合は、このフラグを含めないでください。 プライベート VLAN は、クラスターが作成されるときに作成されます。</li>
<li>前に標準クラスターをこのロケーションに作成している場合、または前にプライベート VLAN を IBM Cloud インフラストラクチャー (SoftLayer) に作成している場合は、そのプライベート VLAN を指定する必要があります。

<p><strong>注:</strong> create コマンドで指定するパブリック VLAN とプライベート VLAN は、一致していなければなりません。 必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成するときにこれらの VLAN を使用するには、それらの接頭部の後に続く数値と文字の組み合わせが一致していなければなりません。 クラスターを作成するときに、一致していないパブリック VLAN とプライベート VLAN を使用しないでください。</p></li>
</ul>

<p>特定の場所にプライベート VLAN が既に存在するかどうかや、既存のプライベート VLAN の名前を確認するには、<code>bx cs vlans <em>&lt;location&gt;</em></code> を実行します。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>このパラメーターは、フリー・クラスターでは使用できません。</li>
<li>この標準クラスターが、この場所に作成する最初の標準クラスターである場合は、このフラグを使用しないでください。 パブリック VLAN は、クラスターが作成されるときに作成されます。</li>
<li>前に標準クラスターをこのロケーションに作成している場合、または前にパブリック VLAN を IBM Cloud インフラストラクチャー (SoftLayer) に作成している場合は、そのパブリック VLAN を指定する必要があります。

<p><strong>注:</strong> create コマンドで指定するパブリック VLAN とプライベート VLAN は、一致していなければなりません。 必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成するときにこれらの VLAN を使用するには、それらの接頭部の後に続く数値と文字の組み合わせが一致していなければなりません。 クラスターを作成するときに、一致していないパブリック VLAN とプライベート VLAN を使用しないでください。</p></li>
</ul>

<p>特定の場所にパブリック VLAN が既に存在するかどうかや、既存のパブリック VLAN の名前を確認するには、<code>bx cs vlans <em>&lt;location&gt;</em></code> を実行します。</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>クラスターにデプロイするワーカー・ノードの数。 このオプションを指定しない場合、1 つのワーカー・ノードを持つクラスターが作成されます。 この値は、標準クラスターではオプションで、フリー・クラスターでは使用できません。

<p><strong>注:</strong> ワーカー・ノードごとに、固有のワーカー・ノード ID とドメイン名が割り当てられます。クラスターが作成された後にこれらを手動で変更してはいけません。 ID またはドメイン名を変更すると、Kubernetes マスターがクラスターを管理できなくなります。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>ワーカー・ノードには、デフォルトでディスク暗号化の機能があります。[詳しくはこちらを参照してください](cs_secure.html#worker)。 暗号化を無効にするには、このオプションを組み込みます。</dd>
</dl>

**例**:

  

  標準クラスターの例:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  フリー・クラスターの例:

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} 環境の場合の例:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}


### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

組織内のクラスターに関する情報を表示します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>クラスターの VLAN とサブネットを表示します。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-get my_cluster
  ```
  {: pre}


### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

組織が使用するクラスターを削除します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずに強制的にクラスターを削除するには、このオプションを使用します。 この値はオプションです。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

Kubernetes マスターをデフォルトの API バージョンに更新します。 更新中、クラスターにアクセスすることも変更することもできません。 ユーザーがデプロイしたワーカー・ノード、アプリ、リソースは変更されず、引き続き実行されます。

今後のデプロイメント用に、YAML ファイルを変更する必要がある場合があります。 詳しくは、この[リリース・ノート](cs_versions.html)を確認してください。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>クラスターの Kubernetes のバージョン。 バージョンを指定しない場合、Kubernetes マスターはデフォルトの API バージョンに更新されます。使用可能なバージョンを確認するには、[bx cs kube-versions](#cs_kube_versions) を実行します。 この値はオプションです。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずにマスターを強制的に更新するには、このオプションを使用します。 この値はオプションです。</dd>

   <dt><code>--force-update</code></dt>
   <dd>変更が 2 つのマイナー・バージョンより大規模である場合でも、更新を試行します。 この値はオプションです。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters
{: #cs_clusters}

組織が使用するクラスターのリストを表示します。

<strong>コマンド・オプション</strong>:

  なし

**例**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions
{: #cs_kube_versions}

{{site.data.keyword.containershort_notm}} でサポートされている Kubernetes のバージョンのリストを表示します。 最新の安定した機能を使用するために、[クラスター・マスター](#cs_cluster_update)と[ワーカー・ノード](#cs_worker_update)をデフォルト・バージョンに更新してください。

**コマンド・オプション**:

  なし

**例**:

  ```
  bx cs kube-versions
  ```
  {: pre}


<br />


## クラスター・コマンド: サービスと統合
{: #cluster_services_commands}

### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_bind}

{{site.data.keyword.Bluemix_notm}} サービスをクラスターに追加します。 {{site.data.keyword.Bluemix_notm}} カタログにある {{site.data.keyword.Bluemix_notm}} サービスを表示するには、`bx service offerings` を実行します。{{site.data.keyword.Bluemix_notm}} サービス・インスタンスを IBM Cloud スペースに既にプロビジョンしている場合、`bx service list` を実行してそれらをリストできます。**注**: サービス・キーをサポートする {{site.data.keyword.Bluemix_notm}} サービスだけを追加できます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名前空間の名前。 この値は必須です。</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>バインドする {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの ID。 サービス・インスタンスの ID を確認するには、`bx cs cluster-services <cluster_name_or_ID>` を実行します。この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

クラスターから {{site.data.keyword.Bluemix_notm}} サービスを削除します。

**注:** {{site.data.keyword.Bluemix_notm}} サービスを削除すると、サービス資格情報がクラスターから削除されます。 そのサービスをまだ使用しているポッドがあるなら、サービス資格情報を検出できなくなるためそのポッドは失敗します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名前空間の名前。 この値は必須です。</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>削除する {{site.data.keyword.Bluemix_notm}} サービス・インスタンスの ID。 サービス・インスタンスの ID を確認するには、`bx cs cluster-services <cluster_name_or_ID>` を実行します。この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

クラスター内の 1 つまたはすべての Kubernetes 名前空間にバインドされたサービスをリストします。 指定されたオプションがない場合、デフォルトの名前空間のサービスが表示されます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>クラスター内の特定の名前空間にバインドされたサービスを含めます。 この値はオプションです。</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>クラスター内のすべての名前空間にバインドされたサービスを含めます。 この値はオプションです。</dd>
    </dl>

**例**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}


### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --URL URL
{: #cs_webhook_create}

Web フックを登録します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd><code>Normal</code> や <code>Warning</code> などの通知レベル。 <code>警告</code> はデフォルト値です。 この値はオプションです。</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Web フックのタイプ。現在は、slack がサポートされています。この値は必須です。</dd>

   <dt><code>--URL <em>URL</em></code></dt>
   <dd>Web フックの URL。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --URL http://github.com/<mywebhook>
  ```
  {: pre}


<br />


## クラスター・コマンド: サブネット
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

指定されたクラスターで、IBM Cloud インフラストラクチャー (SoftLayer) アカウント内のサブネットを使用できるようにします。

**注:** クラスターでサブネットを使用できるようにすると、このサブネットの IP アドレスは、クラスターのネットワーキングの目的で使用されるようになります。 IP アドレスの競合を回避するため、1 つのサブネットは必ず 1 つのクラスターでのみ使用してください。 あるサブネットを複数のクラスターで使用したり、同時に他の目的で {{site.data.keyword.containershort_notm}}の外部で使用したりしないでください。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>サブネットの ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-subnet-add my_cluster subnet
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

IBM Cloud インフラストラクチャー (SoftLayer) アカウントでサブネットを作成し、{{site.data.keyword.containershort_notm}} において指定されたクラスターでそのサブネットを使用できるようにします。

**注:** クラスターでサブネットを使用できるようにすると、このサブネットの IP アドレスは、クラスターのネットワーキングの目的で使用されるようになります。 IP アドレスの競合を回避するため、1 つのサブネットは必ず 1 つのクラスターでのみ使用してください。 あるサブネットを複数のクラスターで使用したり、同時に他の目的で {{site.data.keyword.containershort_notm}}の外部で使用したりしないでください。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。 クラスターをリストするには、`bx cs clusters` [コマンド](#cs_clusters)を使用します。</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>サブネット IP アドレスの数。 この値は必須です。 指定可能な値は 8、16、32、64 です。</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>サブネットを作成する VLAN。 この値は必須です。 使用可能な VLAN をリストするには、`bx cs vlans<location>` [コマンド](#cs_vlans)を使用します。 </dd>
   </dl>

**例**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

{{site.data.keyword.containershort_notm}} のクラスターに独自のプライベート・サブネットを追加します。

このプライベート・サブネットは、IBM Cloud インフラストラクチャー (SoftLayer) から提供されたプライベート・サブネットではありません。 このため、そのサブネットに対してインバウンドおよびアウトバウンドのネットワーク・トラフィックのルーティングをすべて構成する必要があります。 IBM Cloud インフラストラクチャー (SoftLayer) のサブネットを追加する場合は、`bx cs cluster-subnet-add` [コマンド](#cs_cluster_subnet_add)を使用します。

**注**: ユーザーのプライベート・サブネットをクラスターに追加する場合、そのクラスター内のプライベートのロード・バランサーでこのサブネットの IP アドレスが使用されます。 IP アドレスの競合を回避するため、1 つのサブネットは必ず 1 つのクラスターでのみ使用してください。 あるサブネットを複数のクラスターで使用したり、同時に他の目的で {{site.data.keyword.containershort_notm}}の外部で使用したりしないでください。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>サブネットのクラスレス・ドメイン間ルーティング (CIDR)。 この値は必須です。IBM Cloud インフラストラクチャー (SoftLayer) によって使用されるどのサブネットとも競合しない値にする必要があります。

   サポートされる接頭部の範囲は、`/30` (1 個の IP アドレス) から `/24` (253 個の IP アドレス) です。 CIDR の接頭部の長さを設定した後でその長さを変更することが必要になった場合は、まず新しい CIDR を追加し、その後[古い CIDR を削除](#cs_cluster_user_subnet_rm)します。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>プライベート VLAN の ID。 この値は必須です。 この値は、クラスター内にある 1 つ以上のワーカー・ノードのプライベート VLAN ID と一致していなければなりません。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-user-subnet-add my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

指定されたクラスターから独自のプライベート・サブネットを削除します。

**注:** 独自のプライベート・サブネットに属する IP アドレスにデプロイされたサービスは、そのサブネットが削除された後にもすべてアクティブのままになります。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>サブネットのクラスレス・ドメイン間ルーティング (CIDR)。 この値は必須です。`bx cs cluster-user-subnet-add` [コマンド](#cs_cluster_user_subnet_add)で設定された CIDR と一致する値にする必要があります。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>プライベート VLAN の ID。 この値は必須です。`bx cs cluster-user-subnet-add` [コマンド](#cs_cluster_user_subnet_add)で設定された VLAN ID と一致する値にする必要があります。</dd>
   </dl>

**例**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}

### bx cs subnets
{: #cs_subnets}

IBM Cloud インフラストラクチャー (SoftLayer) アカウントで使用できるサブネットのリストを表示します。

<strong>コマンド・オプション</strong>:

   なし

**例**:

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## インフラストラクチャー・コマンド
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

{{site.data.keyword.Bluemix_notm}} アカウントに IBM Cloud インフラストラクチャー (SoftLayer) アカウントの資格情報を設定します。 これらの資格情報が設定されると、{{site.data.keyword.Bluemix_notm}} アカウントを使用して IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスできます。

**注:** 1 つの {{site.data.keyword.Bluemix_notm}} アカウントに複数の資格情報を設定しないでください。 それぞれの {{site.data.keyword.Bluemix_notm}} アカウントは、1 つの IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオだけにリンクされます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud インフラストラクチャー (SoftLayer) アカウントのユーザー名。 この値は必須です。</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud インフラストラクチャー (SoftLayer) アカウントの API キー。 この値は必須です。

 <p>
API キーを生成するには、以下のようにします。

  <ol>
  <li>[IBM Cloud インフラストラクチャー (SoftLayer) ポータル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) にログインします。</li>
  <li><strong>「アカウント」</strong>を選択し、次に<strong>「ユーザー」</strong>を選択します。</li>
  <li><strong>「生成」</strong>をクリックして、アカウント用の IBM Cloud インフラストラクチャー (SoftLayer) の API キーを生成します。</li>
  <li>このコマンドで使用するために API キーをコピーします。</li>
  </ol>

  既存の API キーを表示するには、以下のようにします。
  <ol>
  <li>[IBM Cloud インフラストラクチャー (SoftLayer) ポータル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) にログインします。</li>
  <li><strong>「アカウント」</strong>を選択し、次に<strong>「ユーザー」</strong>を選択します。</li>
  <li><strong>「表示」</strong>をクリックして既存の API キーを表示します。</li>
  <li>このコマンドで使用するために API キーをコピーします。</li>
  </ol>
  </p></dd>
  </dl>

**例**:

  ```
  bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

{{site.data.keyword.Bluemix_notm}} アカウントから IBM Cloud インフラストラクチャー (SoftLayer) アカウントの資格情報を削除します。 資格情報を削除すると、{{site.data.keyword.Bluemix_notm}} アカウントを使用して IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスすることはできなくなります。

<strong>コマンド・オプション</strong>:

   なし

**例**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION
{: #cs_machine_types}

ワーカー・ノードのために使用できるマシン・タイプのリストを表示します。 各マシン・タイプには、クラスター内の各ワーカー・ノード用の仮想 CPU、メモリー、ディスク・スペースの量が含まれます。
- デフォルトでは、ホストの Docker データはマシン・タイプで暗号化されます。 すべてのコンテナー・データが格納される `/var/lib/docker` ディレクトリーは、LUKS 暗号化で暗号化されます。 クラスターの作成時に `disable-disk-encrypt` オプションが指定された場合、ホストの Docker データは暗号化されません。 [暗号化について詳しくは、こちらをご覧ください。](cs_secure.html#encrypted_disks)
- 名前に `u2c` または `b2c` が含まれるマシン・タイプは、信頼性を確保するためにストレージ・エリア・ネットワーク (SAN) ではなくローカル・ディスクを使用します。信頼性が高いと、ローカル・ディスクへのバイトのシリアライズ時のスループットが向上し、ネットワーク障害が原因のファイル・システムのパフォーマンス低下が軽減されます。 これらのマシン・タイプには、ユーザーがアクセスできない OS ファイル・システム用の 1 次ローカル・ディスク・ストレージ 25 GB と、すべてのコンテナー・データが書き込まれる `/var/lib/docker` ディレクトリー用の 2 次ローカル・ディスク・ストレージ 100 GB があります。
- 名前に `u1c` または `b1c` が含まれているマシン・タイプ (`u1c.2x4` など) は推奨されません。 マシン・タイプ `u2c` と `b2c` を使用するには、`bx cs worker-add` コマンドを使用して、その更新されたマシン・タイプを使用しているワーカー・ノードを追加します。 その後、`bx cs worker-rm` コマンドを使用して、推奨されないマシン・タイプを使用しているワーカー・ノードを削除します。
</p>


<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>使用可能なマシン・タイプをリストする対象の場所を入力します。 この値は必須です。 [使用可能なロケーション](cs_regions.html#locations)を参照してください。</dd></dl>

**例**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

### bx cs vlans LOCATION 
{: #cs_vlans}

あるロケーションにおいて、IBM Cloud インフラストラクチャー (SoftLayer) アカウントで使用可能なパブリック VLAN とプライベート VLAN をリストします。 使用可能な VLAN をリストするには、有料アカウントが必要です。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>プライベート VLAN とパブリック VLAN をリストする対象の場所を入力します。 この値は必須です。 [使用可能なロケーション](cs_regions.html#locations)を参照してください。</dd>
   
   </dl>

**例**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## ロギング・コマンド
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_create}

ロギング構成を作成します。 このコマンドを使用して、コンテナー、アプリケーション、ワーカー・ノード、Kubernetes クラスター、Ingress アプリケーション・ロード・バランサーのログを {{site.data.keyword.loganalysisshort_notm}} または外部 syslog サーバーに転送できます。

<strong>コマンド・オプション</strong>:

<dl>
<dt><code><em>CLUSTER</em></code></dt>
<dd>クラスターの名前または ID。</dd>
<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>ログ転送を有効にする対象のログ・ソース。 指定可能な値は <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code> です。 この値は必須です。</dd>
<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>ログの転送元になる Kubernetes 名前空間。 ログ転送は、Kubernetes 名前空間 <code>ibm-system</code> と <code>kube-system</code> ではサポートされていません。 この値はコンテナー・ログ・ソースについてのみ有効で、オプションです。 名前空間を指定しないと、クラスター内のすべての名前空間でこの構成が使用されます。</dd>
<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>ロギング・タイプが <code>syslog</code> であるとき、ログ・コレクター・サーバーのホスト名または IP アドレス。 この値は <code>syslog</code> の場合に必須です。 ロギング・タイプが <code>ibm</code> であるとき、{{site.data.keyword.loganalysislong_notm}} 取り込み URL。 選択可能な取り込み URL のリストは、[ここを参照してください](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)。 取り込み URL を指定しない場合、クラスターが作成された地域のエンドポイントが使用されます。</dd>
<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>ログ・コレクター・サーバーのポート。 この値はオプションです。 ポートを指定しないと、標準ポート <code>514</code> が <code>syslog</code> で使用され、標準ポート <code>9091</code> が <code>ibm</code> で使用されます。</dd>
<dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
<dd>ログの送信先となる Cloud Foundry スペースの名前。 この値はログ・タイプ <code>ibm</code> についてのみ有効で、オプションです。 スペースを指定しない場合、ログはアカウント・レベルに送信されます。</dd>
<dt><code>--org <em>CLUSTER_ORG</em></code></dt>
<dd>このスペースが属する Cloud Foundry 組織の名前。 この値はログ・タイプ <code>ibm</code> についてのみ有効で、スペースを指定した場合には必須です。</dd>
<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>使用するログ転送プロトコル。 現在、<code>syslog</code> と <code>ibm</code> がサポートされています。 この値は必須です。</dd>
<dt><code>--json</code></dt>
<dd>オプションで、コマンド出力を JSON フォーマットで出力します。</dd>
</dl>

**例**:

デフォルト・ポート 9091 で `container` ログ・ソースから転送されるログ・タイプ `ibm` の例:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

デフォルト・ポート 514 で `container` ログ・ソースから転送されるログ・タイプ `syslog` の例:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname my_hostname-or-IP --type syslog
  ```
  {: pre}

デフォルトではないポートで `ingress` ソースからログを転送するログ・タイプ `syslog` の例:

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname my_hostname-or-IP --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

クラスターのログ転送構成をすべて表示するか、またはログ・ソースを基準にロギング構成をフィルターに掛けます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
   <dd>フィルター操作で取得するログ・ソースの種類。 クラスター内のこのログ・ソースのロギング構成のみが返されます。 指定可能な値は <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code> です。 この値はオプションです。</dd>
   <dt><code>--json</code></dt>
   <dd>オプションで、コマンド出力を JSON フォーマットで出力します。</dd>
   </dl>

**例**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

クラスターのロギング構成をリフレッシュします。 クラスター内のスペース・レベルに転送を行うすべてのロギング構成のロギング・トークンをリフレッシュします。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER --id LOG_CONFIG_ID
{: #cs_logging_rm}

ログ転送構成を削除します。 これにより、リモート syslog サーバーまたは {{site.data.keyword.loganalysisshort_notm}} へのログの転送が停止します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>ログ・ソースから削除するロギング構成 ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--hostname LOG_SERVER_HOSTNAME_OR_IP][--port LOG_SERVER_PORT] [--space CLUSTER_SPACE][--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_update}

ログ転送構成の詳細を更新します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>
   <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>更新するロギング構成 ID。 この値は必須です。</dd>
   <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>ロギング・タイプが <code>syslog</code> であるとき、ログ・コレクター・サーバーのホスト名または IP アドレス。 この値は <code>syslog</code> の場合に必須です。 ロギング・タイプが <code>ibm</code> であるとき、{{site.data.keyword.loganalysislong_notm}} 取り込み URL。 選択可能な取り込み URL のリストは、[ここを参照してください](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)。 取り込み URL を指定しない場合、クラスターが作成された地域のエンドポイントが使用されます。</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>ログ・コレクター・サーバーのポート。 この値は、ロギング・タイプが <code>syslog</code> の場合にオプションです。 ポートを指定しないと、標準ポート <code>514</code> が <code>syslog</code> で使用され、<code>9091</code> が <code>ibm</code> で使用されます。</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>ログの送信先となるスペースの名前。 この値はログ・タイプ <code>ibm</code> についてのみ有効で、オプションです。 スペースを指定しない場合、ログはアカウント・レベルに送信されます。</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>このスペースが属する組織の名前。 この値はログ・タイプ <code>ibm</code> についてのみ有効で、スペースを指定した場合には必須です。</dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>使用するログ転送プロトコル。 現在、<code>syslog</code> と <code>ibm</code> がサポートされています。 この値は必須です。</dd>
   <dt><code>--json</code></dt>
   <dd>オプションで、コマンド出力を JSON フォーマットで出力します。</dd>
   </dl>

**ログ・タイプ `ibm`** の例

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**ログ・タイプ `syslog` の例**

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


<br />


## 地域コマンド
{: #region_commands}

### bx cs locations
{: #cs_datacenters}

クラスターを作成するために使用できる場所のリストを表示します。

<strong>コマンド・オプション</strong>:

   なし

**例**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

現在自分が属している {{site.data.keyword.containershort_notm}} 地域を見つけます。 その地域に固有のクラスターを作成して管理します。 地域を変更するには、`bx cs region-set` コマンドを使用します。

**例**:

```
bx cs region
```
{: pre}

**出力**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

{{site.data.keyword.containershort_notm}} の地域を設定します。 その地域に固有のクラスターを作成して管理します。高可用性を確保するために、複数の地域にクラスターを作成することもできます。

例えば、米国南部地域の {{site.data.keyword.Bluemix_notm}} にログインしてクラスターを作成できます。 次に `bx cs region-set eu-central` を使用して中欧地域をターゲットにし、別のクラスターを作成できます。 最後に、`bx cs region-set us-south` を使用して米国南部地域に戻り、その地域のクラスターを管理できます。

**コマンド・オプション**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>ターゲットにする地域を入力します。 この値はオプションです。 地域を指定しない場合、出力に含まれるリストからそれを選択できます。

選択可能な地域のリストを参照するには、[地域とロケーション](cs_regions.html)を確認するか、`bx cs regions` [コマンド](#cs_regions)を使用します。</dd></dl>

**例**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**出力**:
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

選択可能な地域をリストします。 `Region Name` は {{site.data.keyword.containershort_notm}} 名、`Region Alias` はその地域の一般的な {{site.data.keyword.Bluemix_notm}} 名です。

**例**:

```
bx cs regions
```
{: pre}

**出力**:
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


<br />


## ワーカー・ノード・コマンド
{: worker_node_commands}

### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

ワーカー・ノードを標準クラスターに追加します。

<strong>コマンド・オプション</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>クラスターの名前または ID。 この値は必須です。</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>ワーカー・ノードをクラスターに追加する YAML ファイルのパス。 このコマンドに用意されているオプションを使用して追加のワーカー・ノードを定義する代わりに、YAML ファイルを使用することができます。 この値はオプションです。

<p><strong>注:</strong> YAML ファイル内のパラメーターと同じオプションをコマンドで指定した場合は、コマンドの値が YAML 内の値よりも優先されます。 例えば、YAML ファイル内でマシン・タイプを定義し、コマンドで --machine-type オプションを使用した場合は、コマンド・オプションに入力した値が YAML ファイル内の値をオーバーライドします。

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
<caption>表 2. YAML ファイルの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td><code><em>&lt;cluster_name_or_id&gt;</em></code> を、ワーカー・ノードの追加先のクラスターの名前または ID に置き換えます。</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td><code><em>&lt;location&gt;</em></code> を、ワーカー・ノードをデプロイする場所に置き換えます。 使用可能な場所は、ログインしている地域によって異なります。 使用可能な場所をリストするには、<code>bx cs locations</code> を実行します。</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td><code><em>&lt;machine_type&gt;</em></code> を、ワーカー・ノードに使用するマシン・タイプに置き換えます。 現在の場所で使用可能なマシン・タイプをリストするには、<code>bx cs machine-types <em>&lt;location&gt;</em></code> を実行します。</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td><code><em>&lt;private_vlan&gt;</em></code> を、ワーカー・ノードに使用するプライベート VLAN の ID に置き換えます。 使用可能な VLAN をリストするには、<code>bx cs vlans <em>&lt;location&gt;</em></code> を実行して、<code>bcr</code> で始まる VLAN ルーター (バックエンド・ルーター) を探します。</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td><code>&lt;public_vlan&gt;</code> を、ワーカー・ノードに使用するパブリック VLAN の ID に置き換えます。 使用可能な VLAN をリストするには、<code>bx cs vlans &lt;location&gt;</code> を実行して、<code>fcr</code> で始まる VLAN ルーター (フロントエンド・ルーター) を探します。 <br><strong>注</strong>: ワーカー・ノードをプライベート VLAN だけに接続するために、パブリック VLAN を選択しないことにした場合は、代替ソリューションを構成する必要があります。詳しくは、[ワーカー・ノードの VLAN 接続](cs_clusters.html#worker_vlan_connection)を参照してください。</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>ワーカー・ノードのハードウェア分離のレベル。 使用可能な物理リソースを自分専用にする場合には dedicated を使用し、他の IBM の顧客と物理リソースを共有することを許可する場合には shared を使用してください。 デフォルトは shared です。</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td><code><em>&lt;number_workers&gt;</em></code> を、デプロイするワーカー・ノードの数に置き換えます。</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>ワーカー・ノードには、デフォルトでディスク暗号化の機能があります。[詳しくはこちらを参照してください](cs_secure.html#worker)。 暗号化を無効にするには、このオプションを組み込んで値を <code>false</code> に設定します。</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>ワーカー・ノードのハードウェア分離のレベル。 使用可能な物理リソースを自分専用にする場合には dedicated を使用し、他の IBM の顧客と物理リソースを共有することを許可する場合には shared を使用してください。 デフォルトは shared です。 この値はオプションです。</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>選択したマシン・タイプによって、ワーカー・ノードにデプロイされたコンテナーで使用できるメモリーとディスク・スペースの量が左右されます。 この値は必須です。 使用可能なマシン・タイプをリストするには、[bx cs machine-types LOCATION](#cs_machine_types) を参照してください。</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>クラスター内に作成するワーカー・ノードの数を表す整数。 デフォルト値は 1 です。この値はオプションです。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>クラスターの作成時に指定されたプライベート VLAN。 この値は必須です。

<p><strong>注:</strong> 指定するパブリック VLAN とプライベート VLAN は、一致していなければなりません。 必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成するときにこれらの VLAN を使用するには、それらの接頭部の後に続く数値と文字の組み合わせが一致していなければなりません。 クラスターを作成するときに、一致していないパブリック VLAN とプライベート VLAN を使用しないでください。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>クラスターの作成時に指定されたパブリック VLAN。 この値はオプションです。 ワーカー・ノードがプライベート VLAN だけに存在するようにするには、パブリック VLAN ID を指定しないでください。<strong>注</strong>: パブリック VLAN を選択しないことにした場合、代替ソリューションを構成する必要があります。 詳しくは、[ワーカー・ノードの VLAN 接続](cs_clusters.html#worker_vlan_connection)を参照してください。

<p><strong>注:</strong> 指定するパブリック VLAN とプライベート VLAN は、一致していなければなりません。 必ず、プライベート VLAN ルーターの先頭は <code>bcr</code> (バックエンド・ルーター)、パブリック VLAN ルーターの先頭は <code>fcr</code> (フロントエンド・ルーター) になります。 クラスターを作成するときにこれらの VLAN を使用するには、それらの接頭部の後に続く数値と文字の組み合わせが一致していなければなりません。 クラスターを作成するときに、一致していないパブリック VLAN とプライベート VLAN を使用しないでください。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>ワーカー・ノードには、デフォルトでディスク暗号化の機能があります。[詳しくはこちらを参照してください](cs_secure.html#worker)。 暗号化を無効にするには、このオプションを組み込みます。</dd>
</dl>

**例**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} の場合の例:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}


### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

ワーカー・ノードの詳細を表示します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>ワーカー・ノードのクラスターの名前または ID。 この値はオプションです。</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>ワーカー・ノードの ID。 クラスター内のワーカー・ノードの ID を表示するには、<code>bx cs workers <em>CLUSTER</em></code> を実行します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
  ```
  {: pre}


### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

クラスター内のワーカー・ノードをリブートします。 ワーカー・ノードに問題がある場合は、まずワーカー・ノードをリブートして再始動してみてください。 リブートしても問題が解決されない場合、`worker-reload` コマンドを試してください。 リブートの際に、ワーカー・ノードの状態は変更されません。 状態は `deployed` のままですが、状況は更新されます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずに強制的にワーカー・ノードを再始動するには、このオプションを使用します。 この値はオプションです。</dd>

   <dt><code>--hard</code></dt>
   <dd>ワーカー・ノードの電源を遮断することでワーカー・ノードのハード再始動を強制するには、このオプションを使用します。 このオプションは、ワーカー・ノードが応答しない場合、またはワーカー・ノードで Docker ハングが生じた場合に使用します。 この値はオプションです。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>1 つ以上のワーカー・ノードの名前または ID。 複数のワーカー・ノードをリストするには、スペースを使用します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs worker-reboot my_cluster my_node1 my_node2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

クラスター内のワーカー・ノードを再ロードします。 ワーカー・ノードに問題がある場合は、まずワーカー・ノードをリブートしてみてください。 リブートしても問題が解決されない場合、`worker-reload` コマンドを試してください。
ワーカー・ノードに必要なすべての構成が再ロードされます。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずに強制的にワーカー・ノードを再ロードするには、このオプションを使用します。 この値はオプションです。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>1 つ以上のワーカー・ノードの名前または ID。 複数のワーカー・ノードをリストするには、スペースを使用します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs worker-reload my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

1 つ以上のワーカー・ノードをクラスターから削除します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>クラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずに強制的にワーカー・ノードを削除するには、このオプションを使用します。 この値はオプションです。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>1 つ以上のワーカー・ノードの名前または ID。 複数のワーカー・ノードをリストするには、スペースを使用します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs worker-rm my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

ワーカー・ノードを最新の Kubernetes バージョンに更新します。 `bx cs worker-update` を実行すると、アプリとサービスにダウン時間が発生する可能性があります。 更新中、すべてのポッドが他のワーカー・ノードにスケジュール変更されるので、ポッドの外部に保管されていないデータは削除されます。 ダウン時間を回避するには、[選択したワーカー・ノードの更新中に、ワークロードを処理するために十分なワーカー・ノード数を確保するようにしてください。](cs_cluster_update.html#worker_node)

更新の前に、デプロイメント用に YAML ファイルを変更する必要がある場合があります。 詳しくは、この[リリース・ノート](cs_versions.html)を確認してください。

<strong>コマンド・オプション</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>使用可能なワーカー・ノードをリストする対象のクラスターの名前または ID。 この値は必須です。</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>クラスターの Kubernetes のバージョン。 このフラグを指定しなかった場合、ワーカー・ノードはデフォルトのバージョンに更新されます。 使用可能なバージョンを確認するには、[bx cs kube-versions](#cs_kube_versions) を実行します。 この値はオプションです。</dd>

   <dt><code>-f</code></dt>
   <dd>ユーザー・プロンプトを出さずにマスターを強制的に更新するには、このオプションを使用します。 この値はオプションです。</dd>

   <dt><code>--force-update</code></dt>
   <dd>変更が 2 つのマイナー・バージョンより大規模である場合でも、更新を試行します。 この値はオプションです。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>1 つ以上のワーカー・ノードの ID。 複数のワーカー・ノードをリストするには、スペースを使用します。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs worker-update my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs workers CLUSTER
{: #cs_workers}

ワーカー・ノードのリストと、それぞれのクラスター内での状況を表示します。

<strong>コマンド・オプション</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>使用可能なワーカー・ノードをリストする対象のクラスターの名前または ID。 この値は必須です。</dd>
   </dl>

**例**:

  ```
  bx cs workers mycluster
  ```
  {: pre}
