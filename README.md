# Projeto: GitOps na Prática com Kubernetes e ArgoCD

[cite\_start]Este projeto demonstra a implementação de um fluxo de GitOps para implantar, de forma automatizada, o conjunto de microserviços **Online Boutique** [cite: 17] num cluster Kubernetes local (`kind`). [cite\_start]A gestão do estado desejado da aplicação é feita pelo **ArgoCD** [cite: 13][cite\_start], que utiliza este repositório Git como única fonte da verdade [cite: 11][cite\_start], permitindo que deploys sejam feitos de forma segura e auditável através de um simples `git push`[cite: 13].

[cite\_start]Este trabalho foi desenvolvido como parte do Programa de Bolsas - DevSecOps da Compass UOL[cite: 4].

## Tecnologias Utilizadas

  * [cite\_start]**Kubernetes (`kind`)**: Orquestrador de contentores [cite: 10] [cite\_start]para executar a aplicação em ambiente distribuído[cite: 12].
  * **Docker**: Motor de contentorização utilizado como base para o `kind`.
  * [cite\_start]**Git & GitHub**: Sistema de controlo de versão e repositório remoto, servindo como fonte da verdade para a infraestrutura[cite: 11].
  * [cite\_start]**ArgoCD**: Ferramenta de GitOps para entrega contínua, responsável por sincronizar o estado do cluster com o repositório Git[cite: 13].
  * **kubectl**: Ferramenta de linha de comando para interagir com o cluster Kubernetes.

## Arquitetura da Solução

O fluxo de trabalho implementado segue o princípio fundamental do GitOps:

`Usuário/Dev` -\> `git push` -\> `Repositório GitHub` -\> `ArgoCD (sincroniza)` -\> `Cluster Kubernetes (kind)`

Qualquer alteração no manifesto da aplicação que for enviada para a branch `main` deste repositório será automaticamente detetada e aplicada pelo ArgoCD ao cluster local.

## Passos Executados no Projeto

1.  **Preparação do Ambiente**: Foi configurado um ambiente de desenvolvimento local com `kind` e `kubectl`.
2.  [cite\_start]**Configuração do Repositório GitOps**: Foi criado este repositório público no GitHub para servir como fonte da verdade[cite: 69]. O manifesto original da aplicação foi otimizado para rodar em ambientes com recursos limitados.
3.  [cite\_start]**Instalação e Configuração do ArgoCD**: O ArgoCD foi instalado no cluster Kubernetes [cite: 70] e a sua CLI foi utilizada para redefinir e configurar o acesso administrativo.
4.  [cite\_start]**Criação da Aplicação no ArgoCD**: Foi criada uma aplicação no ArgoCD, estabelecendo o vínculo entre este repositório Git e o namespace `default` do cluster Kubernetes[cite: 71].
5.  [cite\_start]**Sincronização e Validação**: A aplicação foi sincronizada, e os seus recursos (pods, serviços, etc.) foram criados automaticamente pelo ArgoCD[cite: 73]. [cite\_start]O acesso ao frontend foi validado via `kubectl port-forward`[cite: 73].

## Como Executar este Projeto

Para replicar este ambiente, siga os passos abaixo:

### Pré-requisitos

  * Git
  * Docker
  * `kind`
  * `kubectl`

### Instruções

1.  **Clone este repositório:**

    ```bash
    git clone https://github.com/oig04/projeto_gitops.git
    cd projeto_gitops
    ```

2.  **Crie o cluster Kubernetes local com `kind`:**

    ```bash
    kind create cluster
    ```

3.  **Instale o ArgoCD no cluster:**

    ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

4.  **Aceda ao ArgoCD e crie a aplicação**:

      * Aceda à interface do ArgoCD conforme as instruções do projeto (usando `port-forward` e obtendo a palavra-passe).
      * Crie uma **nova aplicação** apontando para este repositório (`https://github.com/oig04/projeto_gitops.git`) e para o `path: k8s`.

5.  **Aceda à aplicação Online Boutique**:

      * Aguarde a aplicação ser sincronizada e ficar com o estado `Healthy` no ArgoCD.
      * Execute o port-forward para aceder ao frontend:
        ```bash
        kubectl port-forward svc/frontend-external 8080:80
        ```
      * Abra o seu navegador em `http://localhost:8080`.

## Autor(a)

  * **Giovanna** - [GitHub Profile](https://www.google.com/search?q=https://github.com/oig04)