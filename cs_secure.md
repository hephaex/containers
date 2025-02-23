---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Security for {{site.data.keyword.containerlong_notm}}
{: #security}

You can use built-in security features in {{site.data.keyword.containerlong}} for risk analysis and security protection. These features help you to protect your Kubernetes cluster infrastructure and network communication, isolate your compute resources, and ensure security compliance across your infrastructure components and container deployments.
{: shortdesc}

## Security by cluster component
{: #cluster}

Each {{site.data.keyword.containerlong_notm}} cluster has security features that are built in to its [master](#master) and [worker](#worker) nodes.
{: shortdesc}

If you have a firewall, need to access load balancing from outside the cluster, or want to run `kubectl` commands from your local system when corporate network policies prevent access to public internet endpoints, [open ports in your firewall](cs_firewall.html#firewall). If you want to connect apps in your cluster to an on-premises network or to other apps external to your cluster, [set up VPN connectivity](cs_vpn.html#vpn).

In the following diagram, you can see security features that are grouped by Kubernetes master, worker nodes, and container images.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} cluster security" style="width:400px; border-style: none"/>


  <table summary="The first row in the table spans both columns. The remaining rows are to be read left to right, with the server location in column one and IP addresses to match in column two.">
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Built-in cluster security settings in {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes master</td>
      <td>The Kubernetes master in each cluster is managed by IBM and is highly available. It includes {{site.data.keyword.containershort_notm}} security settings that ensure security compliance and secure communication to and from the worker nodes. Security updates are performed by IBM as required. The dedicated Kubernetes master centrally controls and monitors all Kubernetes resources in the cluster. Based on the deployment requirements and capacity in the cluster, the Kubernetes master automatically schedules your containerized apps to deploy across available worker nodes. For more information, see [Kubernetes master security](#master).</td>
    </tr>
    <tr>
      <td>Worker node</td>
      <td>Containers are deployed on worker nodes that are dedicated to a cluster and that ensure compute, network, and storage isolation for IBM customers. {{site.data.keyword.containershort_notm}} provides built-in security features to keep your worker nodes secure on the private and public network and to ensure worker node security compliance. For more information, see [Worker node security](#worker). In addition, you can add [Calico network policies](cs_network_policy.html#network_policies) to further specify the network traffic that you want to allow or block to and from a pod on a worker node.</td>
     </tr>
     <tr>
      <td>Images</td>
      <td>As the cluster admin, you can set up your own secure Docker image repository in {{site.data.keyword.registryshort_notm}} where you can store and share Docker images between your cluster users. To ensure safe container deployments, every image in your private registry is scanned by Vulnerability Advisor. Vulnerability Advisor is a component of {{site.data.keyword.registryshort_notm}} that scans for potential vulnerabilities, makes security recommendations, and provides instructions to resolve vulnerabilities. For more information, see [Image security in {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Kubernetes master
{: #master}

Review the built-in Kubernetes master security features to protect the Kubernetes master and to secure the cluster network communication.
{: shortdesc}

<dl>
  <dt>Fully managed and dedicated Kubernetes master</dt>
    <dd>Every Kubernetes cluster in {{site.data.keyword.containershort_notm}} is controlled by a dedicated Kubernetes master that is managed by IBM in an IBM-owned IBM Cloud infrastructure (SoftLayer) account. The Kubernetes master is set up with the following dedicated components that are not shared with other IBM customers.
    <ul><li>etcd data store: Stores all Kubernetes resources of a cluster, such as Services, Deployments, and Pods. Kubernetes ConfigMaps and Secrets are app data that are stored as key value pairs so that they can be used by an app that runs in a pod. Data in etcd is stored on an encrypted disk that is managed by IBM and backed up daily. When sent to a pod, data is encrypted via TLS to assure data protection and integrity. </li>
    <li>kube-apiserver: Serves as the main entry point for all requests from the worker node to the Kubernetes master. The kube-apiserver validates and processes requests and can read from and write to the etcd data store.</li>
    <li>kube-scheduler: Decides where to deploy pods, taking into account capacity and performance needs, hardware and software policy constraints, anti-affinity specifications, and workload requirements. If no worker node can be found that matches the requirements, the pod is not deployed in the cluster.</li>
    <li>kube-controller-manager: Responsible for monitoring replica sets, and creating corresponding pods to achieve the desired state.</li>
    <li>OpenVPN: {{site.data.keyword.containershort_notm}}-specific component to provide secured network connectivity for all Kubernetes master to worker node communication.</li></ul></dd>
  <dt>TLS secured network connectivity for all worker node to Kubernetes master communication</dt>
    <dd>To secure the network communication to the Kubernetes master, {{site.data.keyword.containershort_notm}} generates TLS certificates that encrypt the communication to and from the kube-apiserver and etcd data store components for every cluster. These certificates are never shared across clusters or across Kubernetes master components.</dd>
  <dt>OpenVPN secured network connectivity for all Kubernetes master to worker node communication</dt>
    <dd>Although Kubernetes secures the communication between the Kubernetes master and worker nodes by using the `https` protocol, no authentication is provided on the worker node by default. To secure this communication, {{site.data.keyword.containershort_notm}} automatically sets up an OpenVPN connection between the Kubernetes master and the worker node when the cluster is created.</dd>
  <dt>Continuous Kubernetes master network monitoring</dt>
    <dd>Every Kubernetes master is continuously monitore
      d by IBM to control and remediate process level Denial-Of-Service (DOS) attacks.</dd>
  <dt>Kubernetes master node security compliance</dt>
    <dd>{{site.data.keyword.containershort_notm}} automatically scans every node where the Kubernetes master is deployed for vulnerabilities found in Kubernetes and OS-specific security fixes that need to be applied to assure master node protection. If vulnerabilities are found, {{site.data.keyword.containershort_notm}} automatically applies fixes and resolves vulnerabilities on behalf of the user.</dd>
</dl>

<br />


## Worker nodes
{: #worker}

Review the built-in worker node security features to protect the worker node environment and to assure resource, network, and storage isolation.
{: shortdesc}

<dl>
  <dt>Worker node ownership</dt>
    <dd>The ownership of worker nodes depends on the type of cluster that you create. <p> Worker nodes in free clusters are provisioned in to the IBM Cloud infrastructure (SoftLayer) account that is owned by IBM. Users can deploy apps to the worker nodes but cannot change settings or install additional software on the worker node.</p> <p>Worker nodes in standard clusters are provisioned in to the IBM Cloud infrastructure (SoftLayer) account that is associated with the customer's public or dedicated IBM Cloud account. The worker nodes are owned by the customer. Customers can choose to change security settings or install additional software on the worker nodes as provided by IBM Cloud Container Service.</p> </dd>
  <dt>Compute, network and storage infrastructure isolation</dt>
    <dd>When you create a cluster, worker nodes are provisioned as virtual machines by IBM using the IBM Cloud infrastructure (SoftLayer) portfolio. Worker nodes are dedicated to a cluster and do not host workloads of other clusters.<p> Every {{site.data.keyword.Bluemix_notm}} account is set up with IBM Cloud infrastructure (SoftLayer) VLANs to assure quality network performance and isolation on the worker nodes. You can also designate worker nodes as private by connecting them to a private VLAN only.</p> <p>To persist data in your cluster, you can provision dedicated NFS based file storage from IBM Cloud infrastructure (SoftLayer) and leverage the built-in data security features of that platform. </p></dd>
  <dt>Secured worker node set up</dt>
    <dd>Every worker node is set up with an Ubuntu operating system that cannot be changed by the worker node owner. Because the operating system of the worker node is Ubuntu, all containers deployed to the worker node must use a Linux distribution that uses the Ubuntu kernel. Linux distributions that must talk to the kernel in a different way cannot be used. To protect the operating system of the worker nodes from potential attacks, every worker node is configured with expert firewall settings that are enforced by Linux iptable rules.<p> All containers that run on Kubernetes are protected by predefined Calico network policy settings that are configured on every worker node during cluster creation. This set up ensures secure network communication between worker nodes and pods. </p><p> SSH access is disabled on the worker node. If you have a standard cluster and you want to install additional features on your worker node, you can use [Kubernetes daemon sets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) for everything that you want to run on every worker node, or [Kubernetes jobs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) for any one-time action you must execute.</p></dd>
  <dt>Kubernetes worker node security compliance</dt>
    <dd>IBM works with internal and external security advisory teams to address potential security compliance vulnerabilities. <b>Important</b>: Use the `bx cs worker-update` [command](cs_cli_reference.html#cs_worker_update) regularly (such as monthly) to deploy updates and security patches to the operating system and to update the Kubernetes version. When updates are available, you are notified when you view information about the worker nodes, such as with the `bx cs workers <cluster_name>` or `bx cs worker-get <cluster_name> <worker_ID>` commands.</dd>
  <dt>Option to deploy nodes on physical (bare metal) servers</dt>
  <dd>If you choose to provision your worker nodes on bare metal physical servers (instead of virtual server instances), you have additional control over the compute host, such as the memory or CPU. This setup eliminates the virtual machine hypervisor that allocates physical resources to virtual machines that run on the host. Instead, all of a bare metal machine's resources are dedicated exclusively to the worker, so you don't need to worry about "noisy neighbors" sharing resources or slowing down performance. Bare metal servers are dedicated to you, with all its resources available for cluster usage.</dd>
  <dt id="trusted_compute">{{site.data.keyword.containershort_notm}} with Trusted Compute</dt>
  <dd><p>When you [deploy your cluster on bare metal](cs_clusters.html#clusters_ui) that supports Trusted Compute, you can enable trust. The Trusted Platform Module (TPM) chip is enabled on each bare metal worker node in the cluster that supports Trusted Compute (including future nodes that you add to the cluster). Therefore, after you enable trust, you cannot later disable it for the cluster. A trust server is deployed on the master node, and a trust agent is deployed as a pod on the worker node. When your worker node starts up, the trust agent pod monitors each stage of the process.</p>
  <p>The hardware is at the root of trust, which sends measurements by using the TPM. TPM generates encryption keys that are used for securing the transmission of the measurement data throughout the process. The trust agent passes along to the trust server the measurement of each component in the startup process: from the BIOS firmware that interacts with the TPM hardware to the bootloader and OS kernel. Then, the trusted agent compares these measurements with the expected values in the trusted server to attest whether the startup was valid. The trusted compute process does not monitor other pods in your worker nodes, such as applications.</p>
  <p>For example, if an unauthorized user gains access to your system and modifies the OS kernel with additional logic to collect data, the trust agent detects this and changes the node's trusted status so that you know that the worker is no longer trusted. With trusted compute, you can verify your worker nodes against tampering.</p>
  <p><img src="images/trusted_compute.png" alt="Trusted Compute for bare metal clusters" width="480" style="width:480px; border-style: none"/></p></dd>
  <dt id="encrypted_disks">Encrypted disk</dt>
  <dd>By default, {{site.data.keyword.containershort_notm}} provides two local SSD encrypted data partitions for all worker nodes when the worker nodes are provisioned. The first partition is not encrypted, and the second partition mounted to _/var/lib/docker_ is unlocked by using LUKS encryption keys. Each worker in each Kubernetes cluster has its own unique LUKS encryption key, managed by {{site.data.keyword.containershort_notm}}. When you create a cluster or add a worker node to an existing cluster, the keys are pulled securely and then discarded after the encrypted disk is unlocked.
  <p><b>Note</b>: Encryption can impact disk I/O performance. For workloads that require high-performance disk I/O, test a cluster with encryption both enabled and disabled to help you decide whether to turn off encryption.</p>
  </dd>
  <dt>Support for IBM Cloud infrastructure (SoftLayer) network firewalls</dt>
    <dd>{{site.data.keyword.containershort_notm}} is compatible with all [IBM Cloud infrastructure (SoftLayer) firewall offerings ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/network-security). On {{site.data.keyword.Bluemix_notm}} Public, you can set up a firewall with custom network policies to provide dedicated network security for your standard cluster and to detect and remediate network intrusion. For example, you might choose to set up a [Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/vyatta-1) to act as your firewall and block unwanted traffic. When you set up a firewall, [you must also open up the required ports and IP addresses](cs_firewall.html#firewall) for each region so that the master and the worker nodes can communicate.</dd>
  <dt>Keep services private or selectively expose services and apps to the public internet</dt>
    <dd>You can choose to keep your services and apps private and leverage the built-in security features described in this topic to assure secured communication between worker nodes and pods. To expose services and apps to the public internet, you can leverage the Ingress and load balancer support to securely make your services publicly available.</dd>
  <dt>Securely connect your worker nodes and apps to an on-premises data center</dt>
  <dd>To connect your worker nodes and apps to an on-premises data center, you can configure a VPN IPSec endpoint with a strongSwan service or with a Vyatta Gateway Appliance or a Fortigate Appliance.<br><ul><li><b>strongSwan IPSec VPN Service</b>: You can set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/) that securely connects your Kubernetes cluster with an on-premises network. The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPsec) protocol suite. To set up a secure connection between your cluster and an on-premises network, you must [install an IPsec VPN gateway in your on-premises data center](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection). Then you can [configure and deploy the strongSwan IPSec VPN service](cs_vpn.html#vpn-setup) in a Kubernetes pod.</li><li><b>Vyatta Gateway Appliance or Fortigate Appliance</b>: If you have a larger cluster, want to access non-Kubernetes resources over the VPN, or want to access multiple clusters over a single VPN, you might choose to set up a Vyatta Gateway Appliance or Fortigate Appliance to configure an IPSec VPN endpoint. To configure a Vyatta, see [Setting up VPN connectivity with Vyatta](cs_vpn.html#vyatta).</li></ul></dd>
  <dt>Continuous monitoring and logging of cluster activity</dt>
    <dd>For standard clusters, all cluster-related events, such as adding a worker node, rolling update progress, or capacity usage information can be logged and monitored by {{site.data.keyword.containershort_notm}} and sent to {{site.data.keyword.loganalysislong_notm}} and {{site.data.keyword.monitoringlong_notm}}. For information about setting up logging and monitoring, see [Configuring cluster logging](/docs/containers/cs_health.html#logging) and [Configuring cluster monitoring](/docs/containers/cs_health.html#monitoring).</dd>
</dl>

<br />


## Images
{: #images}

Manage the security and integrity of your images with built-in security features.
{: shortdesc}

<dl>
<dt>Secured Docker private image repository in {{site.data.keyword.registryshort_notm}}</dt>
<dd>You can set up your own Docker image repository in a multi-tenant, highly available, and scalable private image registry that is hosted and managed by IBM to build, securely store, and share Docker images across cluster users.</dd>

<dt>Image security compliance</dt>
<dd>When you use {{site.data.keyword.registryshort_notm}}, you can leverage the built-in security scanning that is provided by Vulnerability Advisor. Every image that is pushed to your namespace is automatically scanned for vulnerabilities against a database of known CentOS, Debian, Red Hat, and Ubuntu issues. If vulnerabilities are found, Vulnerability Advisor provides instructions for how to resolve them to assure image integrity and security.</dd>
</dl>

To view the vulnerability assessment for your images, [review the Vulnerability Advisor documentation](/docs/services/va/va_index.html#va_registry_cli).

<br />


## In-cluster networking
{: #in_cluster_network}

Secured, in-cluster network communication between worker nodes and pods is realized with private virtual local area networks (VLANs). A VLAN configures a group of worker nodes and pods as if they were attached to the same physical wire.
{:shortdesc}

When you create a cluster, every cluster is automatically connected to a private VLAN. The private VLAN determines the private IP address that is assigned to a worker node during cluster creation.

|Cluster type|Manager of the private VLAN for the cluster|
|------------|-------------------------------------------|
|Free clusters in {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Standard clusters in {{site.data.keyword.Bluemix_notm}}|You in your IBM Cloud infrastructure (SoftLayer) account <p>**Tip:** To have access to all VLANs in your account, turn on [VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).</p>|

All pods that are deployed to a worker node are also assigned a private IP address. Pods are assigned an IP in the 172.30.0.0/16 private address range and are routed between worker nodes only. To avoid conflicts, do not use this IP range on any nodes that will communicate with your worker nodes. Worker nodes and pods can securely communicate on the private network by using the private IP addresses. However, when a pod crashes or a worker node needs to be re-created, a new private IP address is assigned.

Because it is difficult to track changing private IP addresses for apps that must be highly available, you can use the built-in Kubernetes service discovery features and expose apps as cluster IP services on the private network in the cluster. A Kubernetes service groups a set of pods and provides a network connection to these pods for other services in the cluster without exposing the actual private IP address of each pod. When you create a cluster IP service, a private IP address is assigned to that service from the 10.10.10.0/24 private address range. As with the pod private address range, do not use this IP range on any nodes that will communicate with your worker nodes. This IP address is accessible inside the cluster only. You cannot access this IP address from the internet. At the same time, a DNS lookup entry is created for the service and stored in the kube-dns component of the cluster. The DNS entry contains the name of the service, the namespace where the service was created, and the link to the assigned private cluster IP address.

If an app in the cluster needs to access a pod that lies behind a cluster IP service, it can either use the private cluster IP address that was assigned to the service or send a request by using the name of the service. When you use the name of the service, the name is looked up in the kube-dns component and routed to the private cluster IP address of the service. When a request reaches the service, the service ensures that all requests are equally forwarded to the pods, independent of their private IP addresses and the worker node they are deployed to.

For more information about how to create a service of type cluster IP, see [Kubernetes services ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

For information about securely connecting apps in a Kubernetes cluster to an on-premises network, see [Setting up VPN connectivity](cs_vpn.html#vpn). For information about exposing your apps for external network communication, see [Allowing public access to apps](cs_network_planning.html#public_access).




