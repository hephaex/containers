---

copyright:
  years: 2014, 2018
lastupdated: "2017-01-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Tutorial: criando clusters
{: #cs_cluster_tutorial}

Implemente e gerencie seu próprio cluster do Kubernetes no {{site.data.keyword.Bluemix_short}}. É possível automatizar a implementação, a operação, o ajuste de escala e o monitoramento de apps conteinerizados em um cluster de hosts de cálculo independentes denominados de nós do trabalhador.
{:shortdesc}

Nesta série de tutoriais, é possível ver como uma firma fictícia de relações públicas usa os recursos do Kubernetes para implementar um app conteinerizado no {{site.data.keyword.Bluemix_notm}}. Usando o {{site.data.keyword.toneanalyzerfull}}, o escritório de RP analisa seus press releases e recebe feedback.


## Objetivos

Neste primeiro tutorial, você atua como administrador de networking do escritório de RP. Você configura um cluster customizado do Kubernetes que é usado para implementar e testar uma versão Hello World do app.

Para configurar a infraestrutura:

-   Criar um cluster do Kubernetes com um nó do trabalhador
-   Instale as CLIs para executar comandos do Kubernetes e gerenciar imagens do Docker
-   Crie um repositório de imagem privada no {{site.data.keyword.registrylong_notm}} para armazenar suas imagens
-   Inclua o serviço {{site.data.keyword.toneanalyzershort}} no cluster para que qualquer app no cluster possa usar esse serviço


## Tempo Necessário

40
minutos


## Público

Este tutorial é destinado a desenvolvedores de software e administradores de rede que nunca criaram um cluster do Kubernetes antes.


## Pré-requisitos

-  Uma conta Pay-As-You-Go ou de Assinatura [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/registration/)


## Lição 1: criando um cluster e configurando a CLI
{: #cs_cluster_tutorial_lesson1}

Crie seu cluster na GUI e instalar as CLIs necessárias.
{: shortdesc}


Para criar seu cluster:

1. Pode levar alguns minutos para provisionar seu cluster. Para aproveitar ao máximo seu tempo, [crie seu cluster na GUI ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/containers-kubernetes/launch?env_id=ibm:yp:united-kingdom) antes de instalar as CLIs. Para este tutorial, crie seu cluster na região leste dos EUA.


As CLIs a seguir e seus pré-requisitos são usados para gerenciar clusters por meio da CLI:
-   CLI do {{site.data.keyword.Bluemix_notm}}
-   Plug-in do {{site.data.keyword.containershort_notm}}
-   Kubernetes CLI
-   Plug-in do {{site.data.keyword.registryshort_notm}}
-   Docker CLI

</br>
Para instalar as CLIs e seus pré-requisitos:

1.  Como um pré-requisito para o plug-in do {{site.data.keyword.containershort_notm}}, instale a CLI do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://clis.ng.bluemix.net/ui/home.html). Para executar comandos da CLI do {{site.data.keyword.Bluemix_notm}}, use o prefixo `bx`.
2.  Siga os prompts para selecionar uma conta e uma organização do {{site.data.keyword.Bluemix_notm}}. Os clusters são específicos para uma conta, mas são independentes de uma organização ou espaço do {{site.data.keyword.Bluemix_notm}}.

4.  Instale o plug-in do {{site.data.keyword.containershort_notm}} para criar clusters do Kubernetes e gerenciar nós do trabalhador. Para executar comandos de plug-in do {{site.data.keyword.containershort_notm}}, use o prefixo `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

5.  Para visualizar uma versão local do painel do Kubernetes e implementar apps em seus clusters, [instale a CLI do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Para executar comandos usando a CLI do Kubernetes, use o prefixo `kubectl`.
    1.  Para obter compatibilidade funcional completa, faça download da versão de CLI do Kubernetes que corresponda à versão do cluster do Kubernetes que você planeja usar. A versão atual do Kubernetes padrão do {{site.data.keyword.containershort_notm}} é 1.8.6.

        OS X: [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl)

        Linux: [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl)

        Windows: [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe)

          **Dica:** se estiver usando o Windows, instale a CLI do Kubernetes no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas
mudanças de caminho de arquivo ao executar comandos posteriormente.

    2.  Se você estiver usando o S.O. X ou Linux, conclua as etapas a seguir.
        1.  Mova o arquivo executável para o diretório `/usr/local/bin`.

            ```
            mv /<path_to_file>/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Assegure-se de que /usr/local/bin esteja listado em sua variável do sistema `PATH`. A variável `PATH` contém todos os diretórios nos quais o sistema operacional pode localizar arquivos executáveis. Os diretórios que estão listados na variável `PATH` servem propósitos diferentes. `/usr/local/bin` é usado para armazenar arquivos executáveis para o software que não faz parte do sistema operacional e que foi instalado manualmente pelo administrador do sistema.

            ```
            echo $PATH
            ```
            {: pre}

            Sua saída de CLI é semelhante à seguinte.

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Torne o arquivo executável.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. Para configurar e gerenciar um repositório de imagem privada no {{site.data.keyword.registryshort_notm}}, instale o plug-in do {{site.data.keyword.registryshort_notm}}. Para executar comandos de registro, use o prefixo `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    Para verificar se o serviço de contêiner e os plug-ins de registro de contêiner estão instalados corretamente, execute o comando a seguir:

    ```
    bx plugin list
    ```
    {: pre}

7. Para construir imagens localmente e enviá-las por push para o repositório de imagem privada, [instale a CLI do Docker CE ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.docker.com/community-edition#/download). Se você estiver usando o Windows 8 ou anterior, será possível instalar o [Docker Toolbox ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) como alternativa.

Parabéns! Você instalou com êxito as CLIs para as lições e os tutoriais a seguir. Em seguida, configure seu ambiente em cluster e inclua o serviço {{site.data.keyword.toneanalyzershort}}.

## Lição 2: configurando seu registro privado
{: #cs_cluster_tutorial_lesson2}

Configure um repositório de imagem privada no {{site.data.keyword.registryshort_notm}} e inclua segredos em seu cluster para que o app possa acessar o serviço {{site.data.keyword.toneanalyzershort}}.

1.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}} usando suas credenciais {{site.data.keyword.Bluemix_notm}}, quando solicitado.

    ```
    bx login [--sso]
    ```
    {: pre}

    **Nota:** se você tiver um ID federado, use a sinalização `--sso` para efetuar login. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável.

2.  Configure o seu próprio repositório de imagem privada no {{site.data.keyword.registryshort_notm}} para armazenar com segurança e compartilhar imagens do Docker com todos os usuários do cluster. Um repositório de imagem privada no {{site.data.keyword.Bluemix_notm}} é identificado por um namespace. O namespace é usado para criar uma URL exclusiva para o seu repositório de imagem que
os desenvolvedores podem usar para acessar imagens privadas do Docker.

    Neste exemplo, a firma PR deseja criar somente um repositório de imagem no {{site.data.keyword.registryshort_notm}}; portanto, eles escolhem _pr_firm_ como o seu namespace para agrupar todas as imagens em sua conta. Substitua _&lt;your_namespace&gt;_ por um namespace de sua escolha que não esteja relacionado ao tutorial.

    ```
    bx cr namespace-add <your_namespace>
    ```
    {: pre}

3.  Antes de continuar com a próxima etapa, verifique se a implementação de seu nó do trabalhador está concluída.

    ```
    bx cs workers <cluster_name>
    ```
     {: pre}

    Quando seu nó do trabalhador conclui o fornecimento, o status muda para **Pronto** e é possível iniciar a ligação de serviços do {{site.data.keyword.Bluemix_notm}}.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status
    kube-par02-pafe24f557f070463caf9e31ecf2d96625-w1   169.48.131.37   10.177.161.132   free           normal    Ready
    ```
    {: screen}

## Lição 3: configurando seu ambiente em cluster
{: #cs_cluster_tutorial_lesson3}

Configure o contexto para seu cluster na CLI. Toda vez que você efetua login na CLI do contêiner para trabalhar com clusters, deve-se executar esses comandos para configurar o caminho para o arquivo de configuração do cluster como uma variável de sessão. O Kubernetes CLI usa essa variável para localizar um arquivo de configuração local e certificados que são necessárias para se conectar ao cluster no {{site.data.keyword.Bluemix_notm}}.

1.  Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração do Kubernetes.

    ```
    bx cs cluster-config <cluster_name>
    ```
    {: pre}

    Quando o download dos arquivos de configuração estiver concluído, será exibido um comando que poderá ser usado para configurar o caminho para o seu arquivo de configuração local do Kubernetes como uma variável de ambiente.

    Exemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  Copie e cole o comando que é exibido em seu terminal para configurar a variável de ambiente `KUBECONFIG`.

3.  Verifique se a variável de ambiente `KUBECONFIG` está configurada corretamente.

    Exemplo para OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Saída:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Verifique se os comandos `kubectl` são executados adequadamente com seu cluster verificando a versão do servidor de CLI do Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Saída de exemplo:

    ```
    Versão do cliente: v1.8.6 Versão do servidor: v1.8.6
    ```
    {: screen}

## Lição 4: incluindo um serviço em seu cluster
{: #cs_cluster_tutorial_lesson4}

Com serviços {{site.data.keyword.Bluemix_notm}}, é possível aproveitar a funcionalidade já desenvolvida nos apps. Qualquer serviço do {{site.data.keyword.Bluemix_notm}} que estiver ligado ao cluster poderá ser usado por qualquer app que for implementado nesse cluster. Repita as etapas a seguir para cada serviço do {{site.data.keyword.Bluemix_notm}} que você deseja usar com seus apps.

1.  Inclua o serviço {{site.data.keyword.toneanalyzershort}} em sua conta do {{site.data.keyword.Bluemix_notm}}.

    **Nota:** quando você incluir o serviço {{site.data.keyword.toneanalyzershort}} em sua conta, será exibida uma mensagem de que o serviço não é grátis. Se você limitar sua chamada API, este tutorial não incorrerá em encargos do serviço {{site.data.keyword.watson}}. [Revise as informações de precificação para o serviço {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block).

    ```
    bx service create tone_analyzer standard <mytoneanalyzer>
    ```
    {: pre}

2.  Ligue a instância do {{site.data.keyword.toneanalyzershort}} ao namespace `padrão` do Kubernetes para o cluster. Posteriormente, será possível criar seus próprios namespaces para gerenciar o acesso de usuário aos recursos do Kubernetes, mas por enquanto, use o namespace `padrão`. Os namespaces do Kubernetes são diferentes do namespace de registro que você criou anteriormente.

    ```
    bx cs cluster-service-bind <cluster_name> default <mytoneanalyzer>
    ```
    {: pre}

    Saída:

    ```
    bx cs cluster-service-bind <cluster_name> default <mytoneanalyzer>
        Binding service instance to namespace...
        OK
        Namespace:	default
        Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Verifique se o segredo do Kubernetes foi criado em seu namespace do cluster. Cada serviço do {{site.data.keyword.Bluemix_notm}} é definido por um arquivo JSON que inclui informação confidencial sobre o serviço, como o nome do usuário, senha e URL que o contêiner usa para acessar o serviço. Para armazenar essas informações com segurança, segredos do Kubernetes são usados. Neste exemplo, o segredo inclui as credenciais para acessar a instância do {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} que é provisionada em sua conta.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Saída:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
Bom trabalho! Você já configurou seu cluster e seu ambiente local está pronto para você começar a implementar apps no cluster.

## O que Vem a Seguir?
{: #next}

* [Teste seus conhecimentos e faça um teste! ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)

* Tente o [Tutorial: implementando apps nos clusters do Kubernetes no {{site.data.keyword.containershort_notm}}](cs_tutorials_apps.html#cs_apps_tutorial) para implementar o app da firma PR no cluster que você criou.
