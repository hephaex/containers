---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Ingress のアノテーション
{: #ingress_annotation}

アプリケーション・ロード・バランサーに機能を追加するため、Ingress リソースにメタデータとしてアノテーションを指定できます。
{: shortdesc}

Ingress サービスとその使用開始方法に関する一般情報については、[Ingress を使用してアプリへのパブリック・アクセスを構成する方法](cs_ingress.html#config)を参照してください。

<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>一般的なアノテーション</th>
 <th>名前</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">外部サービス</a></td>
 <td><code>proxy-external-service</code></td>
 <td>{{site.data.keyword.Bluemix_notm}} でホストされるサービスなどの外部サービスへのパス定義を追加します。</td>
 </tr>
 <tr>
 <td><a href="#alb-id">プライベート・アプリケーションのロード・バランサーのルーティング</a></td>
 <td><code>ALB-ID</code></td>
 <td>プライベート・アプリケーションのロード・バランサーによって着信要求をアプリにルーティングします。</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">再書き込みパス</a></td>
 <td><code>rewrite-path</code></td>
 <td>着信ネットワーク・トラフィックを、バックエンド・アプリが listen する別のパスにルーティングします。</td>
 </tr>
 <tr>
 <td><a href="#sticky-cookie-services">Cookie によるセッション・アフィニティー</a></td>
 <td><code>sticky-cookie-services</code></td>
 <td>スティッキー Cookie を使用して、着信ネットワーク・トラフィックを常に同じアップストリーム・サーバーにルーティングします。</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP ポート</a></td>
 <td><code>tcp-ports</code></td>
 <td>標準以外の TCP ポートを介してアプリにアクセスします。</td>
 </tr>
 </tbody></table>


<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>接続アノテーション</th>
 <th>名前</th>
 <th>説明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">接続タイムアウトおよび読み取りタイムアウトのカスタマイズ</a></td>
  <td><code>proxy-connect-timeout</code></td>
  <td>アプリケーション・ロード・バランサーがバックエンド・アプリへの接続とそこからの読み取りを待機する時間を調整します。この時間を超えると、そのバックエンド・アプリは使用不可と見なされます。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">キープアライブ要求数</a></td>
  <td><code>keepalive-requests</code></td>
  <td>1 つのキープアライブ接続で処理できる要求の最大数を構成します。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">キープアライブ・タイムアウト</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>キープアライブ接続がサーバー上で開いた状態を保つ時間を構成します。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">アップストリーム・キープアライブ</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>アップストリーム・サーバーのアイドル・キープアライブ接続の最大数を構成します。</td>
  </tr>
  </tbody></table>


<table>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>プロキシー・バッファー・アノテーション</th>
 <th>名前</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">クライアント応答データのバッファリング</a></td>
 <td><code>proxy-buffering</code></td>
 <td>クライアントに応答を送信する間、アプリケーション・ロード・バランサーでのクライアント応答バッファリングを無効にします。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">プロキシー・バッファー</a></td>
 <td><code>proxy-buffers</code></td>
 <td>プロキシー・サーバーからの単一の接続に対して、応答を読み取るバッファーの数とサイズを設定します。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">プロキシー・バッファー・サイズ</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>プロキシー・サーバーから受け取る応答の、最初の部分を読み取るバッファーのサイズを設定します。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">プロキシー・ビジー・バッファー・サイズ</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>ビジー状態にすることができるプロキシー・バッファーのサイズを設定します。</td>
 </tr>
 </tbody></table>


<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>要求/応答アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">クライアント要求またはクライアント応答の追加ヘッダー</a></td>
<td><code>proxy-add-headers</code></td>
<td>クライアント要求をバックエンド・アプリに転送する前に要求にヘッダー情報を追加します。または、クライアント応答をクライアントに送信する前に応答にヘッダー情報を追加します。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">クライアント応答ヘッダーの削除</a></td>
<td><code>response-remove-headers</code></td>
<td>クライアント応答をクライアントに転送する前に、応答からヘッダー情報を削除します。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">クライアント要求本体の最大サイズのカスタマイズ</a></td>
<td><code>client-max-body-size</code></td>
<td>アプリケーション・ロード・バランサーに送信可能なクライアント要求本体のサイズを調整します。</td>
</tr>
</tbody></table>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>サービス制限アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">グローバルな速度制限</a></td>
<td><code>global-rate-limit</code></td>
<td>すべてのサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">サービスの速度制限</a></td>
<td><code>service-rate-limit</code></td>
<td>特定のサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。</td>
</tr>
</tbody></table>

<table>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>HTTPS および TLS/SSL 認証アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-port">カスタムの HTTP および HTTPS ポート</a></td>
<td><code>custom-port</code></td>
<td>HTTP (ポート 80) および HTTPS (ポート 443) ネットワーク・トラフィックのデフォルト・ポートを変更します。</td>
</tr>
<tr>
<td><a href="#redirect-to-https">HTTPS への HTTP リダイレクト</a></td>
<td><code>redirect-to-https</code></td>
<td>ドメイン上の非セキュアな HTTP 要求を HTTPS にリダイレクトします。</td>
</tr>
<tr>
<td><a href="#mutual-auth">相互認証</a></td>
<td><code>mutual-auth</code></td>
<td>アプリケーション・ロード・バランサーの相互認証を構成します。</td>
</tr>
<tr>
<td><a href="#ssl-services">SSL サービス・サポート</a></td>
<td><code>ssl-services</code></td>
<td>ロード・バランシングに対する SSL サービス・サポートを許可します。</td>
</tr>
</tbody></table>



## 一般的なアノテーション
{: #general}

### 外部サービス (proxy-external-service)
{: #proxy-external-service}

{{site.data.keyword.Bluemix_notm}} でホストされるサービスなどの外部サービスへのパス定義を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>外部サービスへのパス定義を追加します。 このアノテーションは、バックエンド・サービスの代わりに外部サービスでアプリを実行する場合にのみ使用します。 このアノテーションを使用して外部サービス経路を作成する場合、併用できるのは、`client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout`、`proxy-buffering` の各アノテーションのみです。 その他のアノテーションは、`proxy-external-service` と併用できません。
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cafe-ingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=&lt;mypath&gt; external-svc=https:&lt;external_service&gt; host=&lt;mydomain&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td><code>&lt;<em>mypath</em>&gt;</code> を、外部サービスが listen するパスに置き換えます。</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td><code>&lt;<em>external_service</em>&gt;</code> を、呼び出される外部サービスに置き換えます。 例えば、<code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code> を入力します。</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td><code>&lt;<em>mydomain</em>&gt;</code> を、外部サービスのホスト・ドメインに置き換えます。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### プライベート・アプリケーションのロード・バランサーのルーティング (ALB-ID)
{: #alb-id}

プライベート・アプリケーションのロード・バランサーによって着信要求をアプリにルーティングします。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
パブリック・アプリケーションのロード・バランサーの代わりに着信要求をルーティングするプライベート・アプリケーションのロード・バランサーを選択します。</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/ALB-ID: "&lt;private_ALB_ID&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>プライベート・アプリケーションのロード・バランサーの ID。 プライベート・アプリケーションのロード・バランサーの ID を見つけるには、<code>bx cs albs --cluster <my_cluster></code> を実行します。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />



### 再書き込みパス (rewrite-path)
{: #rewrite-path}

アプリケーション・ロード・バランサー・ドメイン・パスへの着信ネットワーク・トラフィックを、バックエンド・アプリケーションが listen する別のパスにルーティングできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress アプリケーション・ロード・バランサー・ドメインは、<code>mykubecluster.us-south.containers.mybluemix.net/beans</code> への着信ネットワーク・トラフィックをアプリにルーティングします。 アプリは、<code>/beans</code> ではなく <code>/coffee</code> で listen します。 着信ネットワーク・トラフィックをアプリに転送するには、Ingress リソース構成ファイルに再書き込みアノテーションを追加します。 再書き込みアノテーションにより、<code>/beans</code> 上の着信ネットワーク・トラフィックは <code>/coffee</code> パスを使用してアプリに転送されるようになります。 複数のサービスを含める場合は、セミコロン (;) のみを使用して区切ってください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=&lt;myservice1&gt; rewrite=&lt;target_path1&gt;;serviceName=&lt;myservice2&gt; rewrite=&lt;target_path2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td><code>&lt;<em>target_path</em>&gt;</code> を、アプリが listen するパスに置き換えます。 アプリケーション・ロード・バランサー・ドメイン上の着信ネットワーク・トラフィックは、このパスを使用して Kubernetes サービスに転送されます。 大多数のアプリは特定のパスで listen するのではなく、ルート・パスと特定のポートを使用します。 上記の例では、再書き込みパスは <code>/coffee</code> として定義されました。</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### Cookie によるセッション・アフィニティー (sticky-cookie-services)
{: #sticky-cookie-services}

スティッキー Cookie のアノテーションを使用すると、セッション・アフィニティーをアプリケーション・ロード・バランサーに追加し、常に着信ネットワーク・トラフィックを同じアップストリーム・サーバーにルーティングできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>高可用性にするためには、アプリのセットアップによっては、着信クライアント要求を処理する複数のアップストリーム・サーバーをデプロイする必要があります。 クライアントがバックエンド・アプリに接続したら、セッション期間中またはタスクが完了するまでの間、1 つのクライアントに同じアップストリーム・サーバーがサービスを提供するように、セッション・アフィニティーを使用することができます。 着信ネットワーク・トラフィックを常に同じアップストリーム・サーバーにルーティングしてセッション・アフィニティーを保つように、アプリケーション・ロード・バランサーを構成することができます。

</br></br>
バックエンド・アプリに接続した各クライアントは、アプリケーション・ロード・バランサーによって、使用可能なアップストリーム・サーバーのいずれかに割り当てられます。 アプリケーション・ロード・バランサーは、クライアントのアプリに保管されるセッション Cookie を作成します。そのセッション Cookie は、アプリケーション・ロード・バランサーとクライアントの間でやり取りされるすべての要求のヘッダー情報に含められます。 この Cookie の情報により、同一セッションのすべての要求を同じアップストリーム・サーバーで処理することができます。

</br></br>
複数のサービスを含める場合は、セミコロン (;) を使用して区切ってください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=&lt;myservice1&gt; name=&lt;cookie_name1&gt; expires=&lt;expiration_time1&gt; path=&lt;cookie_path1&gt; hash=&lt;hash_algorithm1&gt;;serviceName=&lt;myservice2&gt; name=&lt;cookie_name2&gt; expires=&lt;expiration_time2&gt; path=&lt;cookie_path2&gt; hash=&lt;hash_algorithm2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /myapp
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

  <table>
  <caption>YAML ファイルの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td><code>&lt;<em>cookie_name</em>&gt;</code> を、セッション中に作成されたスティッキー Cookie の名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td><code>&lt;<em>expiration_time</em>&gt;</code> を、スティッキー Cookie が期限切れになるまでの時間 (単位は秒 (s)、分 (m)、または時間 (h)) に置き換えます。 この時間は、ユーザー・アクティビティーとは無関係です。 期限が切れた Cookie は、クライアント Web ブラウザーによって削除され、アプリケーション・ロード・バランサーに送信されなくなります。 例えば、有効期間を 1 秒、1 分、または 1 時間に設定するには、<code>1s</code>、<code>1m</code>、または <code>1h</code> と入力します。</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td><code>&lt;<em>cookie_path</em>&gt;</code> を、Ingress サブドメインに付加されるパスに置き換えます。このパスは、Cookie をアプリケーション・ロード・バランサーに送信する対象となるドメインとサブドメインを示すものです。 例えば、Ingress ドメインが <code>www.myingress.com</code> である場合に、すべてのクライアント要求で Cookie を送信するには、<code>path=/</code> を設定する必要があります。 <code>www.myingress.com/myapp</code> とそのすべてのサブドメインについてにのみ Cookie を送信するには、<code>path=/myapp</code> を設定する必要があります。</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td><code>&lt;<em>hash_algorithm</em>&gt;</code> を、Cookie の情報を保護するハッシュ・アルゴリズムに置き換えます。 <code>sha1</code> のみサポートされます。 SHA1 は、Cookie の情報に基づいてハッシュ合計を生成し、そのハッシュ合計を Cookie に付加します。 サーバーは Cookie の情報を暗号化解除し、データ保全性を検証します。</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />



### アプリケーション・ロード・バランサー用 TCP ポート (tcp-ports)
{: #tcp-ports}

標準以外の TCP ポートを介してアプリにアクセスします。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
このアノテーションは、TCP ストリーム・ワークロードを実行するアプリに使用します。

<p>**注:** アプリケーション・ロード・バランサーはパススルー・モードで動作し、トラフィックをバックエンド・アプリに転送します。 この場合、SSL 終端はサポートされません。</p>
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/tcp-ports: "serviceName=&lt;myservice&gt; ingressPort=&lt;ingress_port&gt; [servicePort=&lt;service_port&gt;]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、標準以外の TCP ポートでアクセスする Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td><code>&lt;<em>ingress_port</em>&gt;</code> を、アプリにアクセスするための TCP ポートに置き換えます。</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>このパラメーターはオプションです。 指定した場合、トラフィックがバックエンド・アプリに送信される前に、ポートはこの値に置き換えられます。 指定しない場合、ポートは Ingress ポートと同じままです。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />



## 接続アノテーション
{: #connection}

### 接続タイムアウトおよび読み取りタイムアウトのカスタマイズ (proxy-connect-timeout、proxy-read-timeout)
{: #proxy-connect-timeout}

アプリケーション・ロード・バランサーの接続タイムアウトおよび読み取りタイムアウトをカスタマイズします。 アプリケーション・ロード・バランサーがバックエンド・アプリへの接続とそこからの読み取りを待機する時間を調整します。この時間を超えると、そのバックエンド・アプリは使用不可と見なされます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>クライアント要求が Ingress アプリケーション・ロード・バランサーに送信されると、アプリケーション・ロード・バランサーはバックエンド・アプリへの接続を開きます。 デフォルトでは、アプリケーション・ロード・バランサーはバックエンド・アプリからの応答を受信するまで 60 秒待機します。 バックエンド・アプリが 60 秒以内に応答しない場合は、接続要求が中止され、バックエンド・アプリは使用不可と見なされます。

</br></br>
アプリケーション・ロード・バランサーがバックエンド・アプリに接続されると、応答データはアプリケーション・ロード・バランサーによってバックエンド・アプリから読み取られます。 この読み取り操作では、アプリケーション・ロード・バランサーはバックエンド・アプリからのデータを受け取るために、2 回の読み取り操作の間に最大 60 秒間待機します。 バックエンド・アプリが 60 秒以内にデータを送信しない場合、バックエンド・アプリへの接続が閉じられ、アプリは使用不可と見なされます。
</br></br>
60 秒の接続タイムアウトと読み取りタイムアウトは、プロキシー上のデフォルトのタイムアウトであり、通常は変更すべきではありません。
</br></br>
アプリの可用性が安定していない場合や、ワークロードが多いためにアプリの応答が遅い場合は、接続タイムアウトまたは読み取りタイムアウトを引き上げることもできます。 タイムアウトを引き上げると、タイムアウトになるまでバックエンド・アプリへの接続を開いている必要があるため、アプリケーション・ロード・バランサーのパフォーマンスに影響が及ぶことに注意してください。
</br></br>
一方、タイムアウトを下げると、アプリケーション・ロード・バランサーのパフォーマンスは向上します。 ワークロードが多いときでも、指定したタイムアウト内でバックエンド・アプリが要求を処理できるようにしてください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "&lt;connect_timeout&gt;s"
   ingress.bluemix.net/proxy-read-timeout: "&lt;read_timeout&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>バックエンド・アプリへの接続で待機する秒数 (例: <code>65s</code>)。 <strong>注:</strong> 接続タイムアウトは、75 秒より長くできません。</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>バックエンド・アプリから読み取る前に待機する秒数 (例: <code>65s</code>)。 <strong>注:</strong> 読み取りタイムアウトは、120 秒より長くできません。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### キープアライブ要求数 (keepalive-requests)
{: #keepalive-requests}

1 つのキープアライブ接続で処理できる要求の最大数を構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
1 つのキープアライブ接続で処理できる要求の最大数を設定します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/keepalive-requests: "serviceName=&lt;myservice&gt; requests=&lt;max_requests&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 このパラメーターはオプションです。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 このパラメーターを指定した場合は、所定のサービスに対してキープアライブ要求が設定されます。 パラメーターを指定しない場合は、キープアライブ要求が構成されていないすべてのサービスに対して、サーバー・レベルの <code>nginx.conf</code> でキープアライブ要求が設定されます。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td><code>&lt;<em>max_requests</em>&gt;</code> を、1 つのキープアライブ接続で処理できる要求の最大数に置き換えます。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### キープアライブ・タイムアウト (keepalive-timeout)
{: #keepalive-timeout}

キープアライブ接続がサーバー・サイドで開いた状態を保つ時間を構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
キープアライブ接続がサーバー上で開いた状態を保つ時間を設定します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;time&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 このパラメーターはオプションです。 このパラメーターを指定した場合は、所定のサービスに対してキープアライブ・タイムアウトが設定されます。 パラメーターを指定しない場合は、キープアライブ・タイムアウトが構成されていないすべてのサービスに対して、サーバー・レベルの <code>nginx.conf</code> でキープアライブ・タイムアウトが設定されます。</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td><code>&lt;<em>time</em>&gt;</code> を時間の長さ (秒単位) に置き換えます。 例: <code>timeout=20s</code>。 値をゼロに設定すると、キープアライブ・クライアント接続が無効になります。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### アップストリーム・キープアライブ (upstream-keepalive)
{: #upstream-keepalive}

アップストリーム・サーバーのアイドル・キープアライブ接続の最大数を構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
所定のサービスのアップストリーム・サーバーへのアイドル・キープアライブ接続の最大数を変更します。 デフォルトでは、64 個のアイドル・キープアライブ接続がアップストリーム・サーバーに設定されています。
</dd>


 <dt>サンプル Ingress リソース YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=&lt;myservice&gt; keepalive=&lt;max_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td><code>&lt;<em>max_connections</em>&gt;</code> を、アップストリーム・サーバーへのアイドル・キープアライブ接続の最大数に置き換えます。 デフォルトは <code>64</code> です。 値を <code>0</code> に設定すると、所定のサービスのアップストリーム・キープアライブ接続が無効になります。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />



## プロキシー・バッファー・アノテーション
{: #proxy-buffer}


### クライアント応答データのバッファリング (proxy-buffering)
{: #proxy-buffering}

バッファー・アノテーションを使用すると、クライアントにデータを送信する間、アプリケーション・ロード・バランサーでの応答データの保存を無効にできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress アプリケーション・ロード・バランサーは、バックエンド・アプリとクライアント Web ブラウザーの間のプロキシーとして機能します。 応答がバックエンド・アプリからクライアントに送信されると、デフォルトでは、応答データがアプリケーション・ロード・バランサーのバッファーに入れられます。 アプリケーション・ロード・バランサーは、クライアント応答をプロキシー処理し、クライアントのペースでクライアントに応答を送信し始めます。 バックエンド・アプリからのすべてのデータをアプリケーション・ロード・バランサーが受信すると、バックエンド・アプリへの接続は閉じられます。 アプリケーション・ロード・バランサーからクライアントへの接続は、クライアントがすべてのデータを受信するまで開いたままです。

</br></br>
アプリケーション・ロード・バランサーでの応答データのバッファリングを無効にすると、データは即時にアプリケーション・ロード・バランサーからクライアントに送信されます。 クライアントが、アプリケーション・ロード・バランサーのペースで着信データを処理できなければなりません。 クライアントの処理速度が遅すぎる場合は、データが失われる可能性があります。
</br></br>
アプリケーション・ロード・バランサーでの応答データのバッファリングは、デフォルトでは有効です。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "False"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>
</dd></dl>

<br />



### プロキシー・バッファー (proxy-buffers)
{: #proxy-buffers}

アプリケーション・ロード・バランサー用のプロキシー・バッファーの数とサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
プロキシー・サーバーからの単一の接続に対して、応答を読み取るバッファーの数とサイズを設定します。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE number=2 size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>number=2 size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=&lt;myservice&gt; number=&lt;number_of_buffers&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、プロキシー・バッファーを適用するサービスの名前に置き換えます。</td>
 </tr>
 <tr>
 <td><code>number_of_buffers</code></td>
 <td><code>&lt;<em>number_of_buffers</em>&gt;</code> を、<em>2</em> などの数値に置き換えます。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### プロキシー・バッファー・サイズ (proxy-buffer-size)
{: #proxy-buffer-size}

応答の最初の部分を読み取るプロキシー・バッファーのサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
プロキシー・サーバーから受け取る応答の、最初の部分を読み取るバッファーのサイズを設定します。 応答のこの部分には通常、短い応答ヘッダーが含まれています。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、proxy-buffers-size を適用するサービスの名前に置き換えます。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### プロキシー・ビジー・バッファー・サイズ (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

ビジー状態にすることができるプロキシー・バッファーのサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
クライアントへの応答がまだ全部読み取られていない間、その応答を送信中のバッファー群のサイズを制限します。 その間、残りのバッファーでは応答を読み取ることができます。さらに必要に応じて、応答の一部を一時ファイルにバッファリングできます。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=&lt;serviceName&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、proxy-busy-buffers-size を適用するサービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## 要求/応答アノテーション
{: #request-response}


### クライアント要求またはクライアント応答の追加ヘッダー (proxy-add-headers)
{: #proxy-add-headers}

クライアント要求をバックエンド・アプリに送信する前に要求にさらにヘッダー情報を追加します。または、クライアント応答をクライアントに送信する前に応答にさらにヘッダー情報を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress アプリケーション・ロード・バランサーは、クライアント・アプリとバックエンド・アプリの間のプロキシーとして機能します。 アプリケーション・ロード・バランサーに送信されたクライアント要求は、処理 (プロキシー処理) され、新しい要求に入れられた後に、アプリケーション・ロード・バランサーからバックエンド・アプリに送信されます。 クライアントから最初に送信された HTTP ヘッダー情報 (ユーザー名など) は、要求のプロキシー処理で削除されます。 そのような情報がバックエンド・アプリに必要な場合は、<strong>ingress.bluemix.net/proxy-add-headers</strong> アノテーションを使用して、アプリケーション・ロード・バランサーからバックエンド・アプリにクライアント要求を転送する前に、ヘッダー情報を要求に追加することができます。

</br></br>
バックエンド・アプリが応答をクライアントに送信する場合は、応答がアプリケーション・ロード・バランサーによってプロキシー処理され、HTTP ヘッダーが応答から削除されます。 クライアント Web アプリで応答を正常に処理するために、このヘッダー情報が必要になる場合があります。 <strong>ingress.bluemix.net/response-add-headers</strong> アノテーションを使用すると、アプリケーション・ロード・バランサーからクライアント Web アプリにクライアント応答を転送する前に、ヘッダー情報を応答に追加することができます。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt; &lt;value1&gt;;
      &lt;header2&gt; &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt; &lt;value3&gt;;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=&lt;myservice1&gt; {
      "&lt;header1&gt;: &lt;value1&gt;";
      "&lt;header2&gt;: &lt;value2&gt;";
      }
      serviceName=&lt;myservice2&gt; {
      "&lt;header3&gt;: &lt;value3&gt;";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /myapp
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>クライアント要求またはクライアント応答に追加するヘッダー情報のキー。</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>クライアント要求またはクライアント応答に追加するヘッダー情報の値。</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### クライアント応答ヘッダーの削除 (response-remove-headers)
{: #response-remove-headers}

バックエンド・アプリからのクライアント応答に含まれているヘッダー情報を削除してから、クライアントに応答を送信します。
 {:shortdesc}

 <dl>
 <dt>説明</dt>
 <dd>Ingress アプリケーション・ロード・バランサーは、バックエンド・アプリとクライアント Web ブラウザーの間のプロキシーとして機能します。 アプリケーション・ロード・バランサーに送信されたバックエンド・アプリからのクライアント応答は、処理 (プロキシー処理) され、新しい応答に入れられた後に、アプリケーション・ロード・バランサーからクライアント Web ブラウザーに送信されます。 応答のプロキシー処理によって、バックエンド・アプリから最初に送信された HTTP ヘッダー情報は削除されますが、バックエンド・アプリに固有のヘッダーが削除されずに残る場合があります。 アプリケーション・ロード・バランサーからクライアント Web ブラウザーにクライアント応答を転送する前に、応答からヘッダー情報を削除します。</dd>
 <dt>サンプル Ingress リソース YAML</dt>
 <dd>
 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
   name: myingress
   annotations:
     ingress.bluemix.net/response-remove-headers: |
       serviceName=&lt;myservice1&gt; {
       "&lt;header1&gt;";
       "&lt;header2&gt;";
       }
       serviceName=&lt;myservice2&gt; {
       "&lt;header3&gt;";
       }
 spec:
   tls:
   - hosts:
     - mydomain
    secretName: mytlssecret
  rules:
   - host: mydomain
    http:
      paths:
       - path: /
         backend:
           serviceName: &lt;myservice1&gt;
           servicePort: 8080
       - path: /myapp
         backend:
           serviceName: &lt;myservice2&gt;
           servicePort: 80</code></pre>

  <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>クライアント応答から削除するヘッダーのキー。</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### クライアント要求本体の最大サイズのカスタマイズ (client-max-body-size)
{: #client-max-body-size}

クライアントが要求の一部として送信できる本体の最大サイズを調整します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>必要なパフォーマンスを維持するため、クライアント要求本体の最大サイズは 1 M バイトに設定されています。 本体サイズがこの制限を超えるクライアント要求が Ingress アプリケーション・ロード・バランサーに送信された場合、データを分割することをクライアントが許可していなければ、アプリケーション・ロード・バランサーは HTTP 応答 413 (要求エンティティーが大き過ぎる) をクライアントに返します。 要求本体のサイズが小さくなるまで、クライアントとアプリケーション・ロード・バランサーの間の接続は確立されません。 データを複数のチャンクに分割することをクライアントが許可した場合は、データが 1 M バイトの複数のパッケージに分割されて、アプリケーション・ロード・バランサーに送信されます。

</br></br>
本体サイズが 1 M バイトを超えるクライアント要求が想定されるために、最大本体サイズを引き上げたい場合があります。 例えば、クライアントが大きなファイルをアップロードできるようにしたい場合があります。 要求本体の最大サイズを引き上げると、要求を受信するまでクライアントへの接続を開いておかなければならないため、アプリケーション・ロード・バランサーのパフォーマンスに影響を及ぼす可能性があります。
</br></br>
<strong>注:</strong> 一部のクライアント Web ブラウザーは、HTTP 応答 413 のメッセージを正しく表示できません。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>クライアント応答本体の最大サイズ。 例えば、200 M バイトに設定するには、<code>200m</code> と定義します。  <strong>注:</strong> サイズを 0 に設定すると、クライアント要求の本体サイズの検査を無効にすることができます。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />



## サービス制限アノテーション
{: #service-limit}


### グローバルな速度制限 (global-rate-limit)
{: #global-rate-limit}

すべてのサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
すべてのサービスに対して、選択したバックエンドのすべてのパスに使用される単一の IP アドレスから渡される、定義済みキーあたりの要求処理速度と接続数を制限します。
</dd>


 <dt>サンプル Ingress リソース YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>ロケーションまたはサービスに基づいて着信要求のグローバル制限を設定するには、`key=location` を使用します。 ヘッダーに基づいて着信要求のグローバル制限を設定するには、`X-USER-ID key==$http_x_user_id` を使用します。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td><code>&lt;<em>rate</em>&gt;</code> を処理速度に置き換えます。 毎秒 (r/s) または毎分 (r/m) の速度として値を入力します。 例: <code>50r/m</code>。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td><code>&lt;<em>conn</em>&gt;</code> を接続数に置き換えます。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### サービスの速度制限 (service-rate-limit)
{: #service-rate-limit}

特定のサービスに対して、要求処理速度と接続数を制限します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>特定のサービスに対して、選択したバックエンドのすべてのパスに使用される単一の IP アドレスから渡される定義済みキーあたりの要求処理速度と接続数を制限します。
</dd>


 <dt>サンプル Ingress リソース YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=&lt;myservice&gt; key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、処理速度を制限するサービスの名前に置き換えます。</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>ロケーションまたはサービスに基づいて着信要求のグローバル制限を設定するには、`key=location` を使用します。 ヘッダーに基づいて着信要求のグローバル制限を設定するには、`X-USER-ID key==$http_x_user_id` を使用します。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td><code>&lt;<em>rate</em>&gt;</code> を処理速度に置き換えます。 毎秒の速度を定義するには、r/s を使用して <code>10r/s</code> のようにします。 毎分の速度を定義するには、r/m を使用して <code>50r/m</code> のようにします。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td><code>&lt;<em>conn</em>&gt;</code> を接続数に置き換えます。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



## HTTPS および TLS/SSL 認証アノテーション
{: #https-auth}


### カスタムの HTTP ポートおよび HTTPS ポート (custom-port)
{: #custom-port}

HTTP (ポート 80) および HTTPS (ポート 443) ネットワーク・トラフィックのデフォルト・ポートを変更します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>デフォルトで、Ingress アプリケーション・ロード・バランサーは、ポート 80 上の着信 HTTP ネットワーク・トラフィックとポート 443 上の着信 HTTPS ネットワーク・トラフィックを listen するように構成されています。 アプリケーション・ロード・バランサー・ドメインのセキュリティーを強化するため、または HTTPS ポートだけを有効にするために、デフォルト・ポートを変更できます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=&lt;protocol1&gt; port=&lt;port1&gt;;protocol=&lt;protocol2&gt; port=&lt;port2&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>着信 HTTP または HTTPS ネットワーク・トラフィックのデフォルト・ポートを変更するには、<strong>http</strong> または <strong>https</strong> を入力します。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>着信 HTTP または HTTPS ネットワーク・トラフィック用に使用するポート番号を入力します。  <p><strong>注:</strong> HTTP または HTTPS 用にカスタム・ポートを指定した場合、デフォルト・ポートは、HTTP と HTTPS のどちらに対しても有効ではなくなります。 例えば、HTTPS のデフォルト・ポートを 8443 に変更し、HTTP ではデフォルト・ポートを使用する場合、それらの両方に対して次のようにカスタム・ポートを設定する必要があります。<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code></p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>使用法</dt>
 <dd><ol><li>アプリケーション・ロード・バランサー用の開かれたポートを確認します。 
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   80:30776/TCP,443:30412/TCP   8d</code></pre></li>
<li>Ingress コントローラーの構成マップを開きます。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>非デフォルトの HTTP および HTTPS ポートを構成マップに追加します。 &lt;port&gt; を、開く HTTP または HTTPS のポートに置き換えます。
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>Ingress コントローラーが非デフォルトのポートによって再構成されたことを確認します。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                     CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
public-ingress-ctl-svc   10.10.10.149   169.60.16.246   &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   8d</code></pre></li>
<li>着信ネットワーク・トラフィックをサービスにルーティングする際に、非デフォルトのポートを使用するように Ingress を構成します。 この解説ではサンプル YAML ファイルを使用します。 </li>
<li>Ingress コントローラー構成を更新します。
<pre class="pre">
<code>kubectl apply -f &lt;yaml_file&gt;</code></pre>
</li>
<li>任意の Web ブラウザーを開いてアプリにアクセスします。 例: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTPS への HTTP リダイレクト (redirect-to-https)
{: #redirect-to-https}

非セキュア HTTP クライアント要求を HTTPS に変換します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress アプリケーション・ロード・バランサーは、IBM 提供の TLS 証明書またはカスタム TLS 証明書を使用してドメインを保護するようにセットアップされます。 一部のユーザーが、アプリケーション・ロード・バランサー・ドメインへの非セキュアな HTTP 要求、例えば、<code>https</code> ではなく <code>http://www.myingress.com</code> を使用してアプリにアクセスしようとする可能性があります。 リダイレクト・アノテーションを使用すると、非セキュアな HTTP 要求を常に HTTPS に変換できます。 このアノテーションを使用しない場合、デフォルトでは非セキュアな HTTP 要求は HTTPS 要求に変換されないので、暗号化されていない機密情報が公開されてしまうおそれがあります。

</br></br>
HTTP 要求の HTTPS へのリダイレクトはデフォルトで無効です。</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>
</dd></dl>

<br />



### 相互認証 (mutual-auth)
{: #mutual-auth}

アプリケーション・ロード・バランサーの相互認証を構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
Ingress アプリケーション・ロード・バランサーの相互認証を構成します。 クライアントはサーバーを認証し、サーバーもクライアントを認証します。どちらも証明書が使用されます。 相互認証は、証明書ベース認証とも双方向認証とも呼ばれます。
</dd>

<dt>前提条件</dt>
<dd>
<ul>
<li>[必要な認証局 (CA) を含んだ有効なシークレットが必要です](cs_app.html#secrets)。 相互認証で認証するためには、<code>client.key</code> と <code>client.crt</code> も必要です。</li>
<li>443 以外のポートでの相互認証を有効にするには、[ロード・バランサーを構成して有効なポートを開きます](cs_ingress.html#opening_ingress_ports)。</li>
</ul>
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/mutual-auth: "secretName=&lt;mysecret&gt; port=&lt;port&gt; serviceName=&lt;servicename1&gt;,&lt;servicename2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td><code>&lt;<em>mysecret</em>&gt;</code> をシークレット・リソースの名前に置き換えます。</td>
</tr>
<tr>
<td><code>&lt;port&gt;</code></td>
<td>アプリケーション・ロード・バランサーのポート番号。</td>
</tr>
<tr>
<td><code>&lt;serviceName&gt;</code></td>
<td>1 つ以上の Ingress リソースの名前。 このパラメーターはオプションです。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### SSL サービス・サポート (ssl-services)
{: #ssl-services}

HTTPS 要求を許可し、アップストリーム・アプリへのトラフィックを暗号化します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
アプリケーション・ロード・バランサーとの間に HTTPS を必要とするアップストリーム・アプリへのトラフィックを暗号化します。

**オプション**: このアノテーションに[片方向認証または相互認証](#ssl-services-auth)を追加できます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: "ssl-service=&lt;myservice1&gt; [ssl-secret=&lt;service1-ssl-secret&gt;];ssl-service=&lt;myservice2&gt; [ssl-secret=&lt;service2-ssl-secret&gt;]
spec:
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /
        backend:
          serviceName: myservice2
          servicePort: 8444</code></pre>

<table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリを表すサービスの名前に置き換えます。 アプリケーション・ロード・バランサーからこのアプリへのトラフィックは暗号化されます。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td><code>&lt;<em>service-ssl-secret</em>&gt;</code> をサービスのシークレットに置き換えます。 このパラメーターはオプションです。 このパラメーターを指定した場合は、アプリが必要とするクライアントの鍵と証明書が値に含まれていなければなりません。</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### 認証を使用する SSL サービス・サポート
{: #ssl-services-auth}

HTTPS 要求を許可し、セキュリティーを強化するために片方向認証または相互認証を使用してアップストリーム・アプリへのトラフィックを暗号化します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
Ingress コントローラーとの間に HTTPS を必要とするロード・バランシング・アプリの相互認証を構成します。

**注**: 始めに、[証明書と鍵を base-64 に変換してください ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.base64encode.org/)。

</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: |
      ssl-service=&lt;myservice1&gt; ssl-secret=&lt;service1-ssl-secret&gt;;
      ssl-service=&lt;myservice2&gt; ssl-secret=&lt;service2-ssl-secret&gt;
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリを表すサービスの名前に置き換えます。 アプリケーション・ロード・バランサーからこのアプリへのトラフィックは暗号化されます。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td><code>&lt;<em>service-ssl-secret</em>&gt;</code> をサービスのシークレットに置き換えます。 このパラメーターはオプションです。 このパラメーターを指定した場合は、アプリが必要とするクライアントの鍵と証明書が値に含まれていなければなりません。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />



