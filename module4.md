Guia de Estudos: Migrações com o Oracle Cloud Migration Service
===============================================================

Este guia fornece uma visão geral sobre a migração de workloads usando o Oracle Cloud Migration Service (OCM), focando especialmente nas migrações de ambientes AWS. As informações são extraídas de uma apresentação e explicações detalhadas sobre o fluxo de migração.

1. Introdução ao Oracle Cloud Migration Service
-----------------------------------------------

O Oracle Cloud Migration Service (OCM) é uma ferramenta que facilita a migração de workloads, especialmente aqueles baseados em VMware, para a Oracle Cloud Infrastructure (OCI) e também permite migrar workloads do ambiente AWS.

### Imagem 1: Migrações na Nuvem Oracle

![image](https://github.com/user-attachments/assets/bdb83f1c-2f54-4401-a143-c4c9011f313e)

*   **VMware**: Suporte para migrações de workloads VMware para OCI.
*   **AWS (EC2)**: Capacidade de migrar instâncias de EC2 da AWS para OCI.

2. Fluxo de Trabalho de Migração do OCM
---------------------------------------

O fluxo de trabalho de migração do OCM é dividido em três principais estágios:

### 2.1. Gerenciar Ativos

1.  **Conectar à Fonte**: Conectar-se ao ambiente AWS de origem.
2.  **Descobrir Ativos**: Identificar todos os ativos (instâncias EC2 e volumes EBS).
3.  **Inventariar Ativos**: Registro e classificação dos ativos descobertos.

### Imagem 2: Fluxo de Trabalho de Migrações do OCM

![image](https://github.com/user-attachments/assets/b622684e-e6d3-4723-bb7f-2beb8dd283f9)

### 2.2. Planejar e Replicar

4.  **Criar Projeto de Migração**: Definir um projeto específico para a migração.
5.  **Criar Plano de Migração**: Detalhar como os ativos serão migrados.
6.  **Replicar Ativos**: Transferir os ativos selecionados para o OCM.

### 2.3. Lançar e Validar

7.  **Implantar Ativos**: Realizar a implantação dos ativos replicados na OCI.
8.  **Validar Ativos**: Executar validações necessárias para garantir que a migração foi bem-sucedida.
9.  **Completar Migração**: Finalizar o projeto de migração após validação.

3. Considerações Finais
-----------------------

O fluxo de migração do OCM pode ser visualizado como um processo eficiente dividido em etapas que garantem uma transição suave dos ambientes AWS para OCI. Para uma compreensão mais profunda, siga os próximos módulos que detalham cada uma dessas etapas.

---------

Guia de Estudos: Preparação para Migração com O Oracle Cloud Migration Service
==============================================================================

Este guia aborda a preparação necessária para a migração de instâncias EC2 do AWS para a Oracle Cloud Infrastructure (OCI). Aqui estão os requisitos e as políticas do AWS para garantir uma migração bem-sucedida.

1. Ambiente de Origem AWS
-------------------------

Para efetuar migrações do AWS, alguns requisitos são essenciais:

### Requisitos no Ambiente AWS:

*   **ID da Conta AWS**: Identificador único da conta AWS.
*   **Conta de Usuário AWS com Chave de Acesso**: Necessária para autenticação e uso no OCI Vault Service.
*   **Políticas IAM do AWS**: Para autorizar ações de descoberta e replicação de instâncias.
*   **AWS Cost Explorer e CloudWatch**: Para estimativas de custo e recomendações de dimensionamento.
*   **Agente AWS CloudWatch**: Para coletar métricas de desempenho avançadas (opcional).

### Imagem 1: Ambiente de Origem AWS

![image](https://github.com/user-attachments/assets/36aea453-d9e0-48dd-8ad6-16c6128d463d)

2. Políticas IAM do AWS
-----------------------

Duas políticas IAM são requeridas:

### 2.1. Política IAM para Descoberta

Permissões necessárias para identificar instâncias EC2 e volumes:
*   **Amazon EC2**
    *   Listar instâncias e volumes.
*   **AWS Cost Explorer**
    *   Ler dados de custo/uso.
*   **Amazon CloudWatch**
    *   Ler métricas e estatísticas.

### 2.2. Política IAM para Replicação

Permissões para replicar instâncias e criar snapshots:
*   **Amazon EC2**
    *   Listar instâncias, volumes, criar snapshots, criar tags.
*   **EBS (Elastic Block Store)**
    *   Ler dados de blocos de snapshots.

### Imagem 2: Políticas IAM do AWS

![image](https://github.com/user-attachments/assets/d4cad9fd-e7c3-4956-9df6-2db0544b69d7)

3. Pré-requisitos no Oracle Cloud Migration
-------------------------------------------

### Configurações Necessárias:

1.  **Namespace de Tag "CloudMigrations"**: Para identificação e organização.
2.  **Três Compartimentos Recomendados**:
    *   **Migrations**: Recursos temporários para a migração.
    *   **MigrationSecrets**: Armazenar segredos e credenciais para o AWS.
    *   **Destination**: Compartimento final para os ativos migrados.

### Credenciais

As credenciais para acessar o ambiente AWS devem estar prontas e armazenadas no compartimento de segredos de migração.

### Imagem 3: Pré-requisitos no OCM

![image](https://github.com/user-attachments/assets/f2da6217-dd5d-4eaa-a5cd-0a829b3398d9)


4. Conclusão
------------

A preparação para migrações utilizando o Oracle Cloud Migration Service envolve requisitos em ambos os ambientes AWS e OCI. Certifique-se de configurar corretamente as políticas IAM, o ambiente AWS e os compartimentos no OCI.
Para um melhor entendimento, siga os próximos módulos e informações detalhadas sobre cada etapa do processo de migração.

---------

Guia de Estudos: Prontidão para Migração - Parte 1
==================================================

Neste guia, vamos explorar os pré-requisitos necessários para realizar uma migração de instâncias EC2 do AWS para a Oracle Cloud Infrastructure (OCI). O foco será na configuração do ambiente AWS.

1. Acessando o Console AWS
--------------------------

1.  **Faça login na AWS** com seu ID de Conta AWS, que é um número exclusivo para identificar sua conta, semelhante ao tenancy do OCI.
2.  **Selecione a região**: Certifique-se de estar na **Região US North Virginia**.
3.  **Acesse a Página EC2**: Na barra de pesquisa, digite "EC2" e clique para abrir.

2. Verificação das Instâncias EC2
---------------------------------

Na seção de **Instâncias**, verifique as instâncias disponíveis. Você verá uma instância **em execução** e outra **parada**. Escolha uma instância a ser migrada.

3. Criação das Políticas IAM
----------------------------

### 3.1. Navegando no IAM

1.  Acesse o serviço **IAM** (Identity and Access Management).
2.  Clique em **Policies** na seção **Access Management**.

### 3.2. Criação da Política para Descoberta

1.  Clique em **Create policy**.
2.  Acesse o repositório do GitHub da Oracle para obter exemplos de políticas.
3.  Copie a política de **descoberta** (em formato JSON).

### 3.3. Inserindo a Política

1.  Cole o JSON no editor de política da AWS.
2.  Nomeie a política como **ocm-discovery**.
3.  Clique em **Next** e revise as permissões.
4.  Crie a política.

### 3.4. Criação da Política para Replicação

1.  Clique novamente em **Create policy** e selecione **JSON**.
2.  Acesse o repositório do GitHub e copie a política de **replicação**.
3.  Faça as necessárias atualizações no campo **Resource**, substituindo **ACCOUNTID** pelo ID da sua conta.
4.  Nomeie a política como **ocm-replication** e crie-a.

4. Atribuição de Políticas ao Usuário
-------------------------------------

1.  No menu **Users**, selecione o usuário (no exemplo, **iamadmin**).
2.  Clique em **Add permissions** e escolha **Attach policies directly**.
3.  Selecione as políticas **ocm-discovery** e **ocm-replication** e clique em **Next** para adicionar.

5. Criação da Chave de Acesso
-----------------------------

1.  Sob a aba **Security credentials**, localize **Access keys**.
2.  Clique em **Create access key**, forneça uma descrição e crie a chave.
3.  **Copie a chave de acesso e a chave secreta**. É crucial armazenar estas informações com segurança.

6. Verificação da Instância EC2
-------------------------------

1.  Retorne à página **EC2** e selecione a instância que será migrada.
2.  Verifique as configurações de **Storage** e assegure-se de que um volume EBS está anexado.
3.  Acesse a aba de **Monitoring** para verificar se o **CloudWatch** está coletando métricas.

7. Configuração do Cost Explorer
--------------------------------

1.  Na barra de pesquisa, digite **Cost Explorer** e clique em **Preferences**.
2.  Habilite a opção de **Resource-level data at daily granularity** para EC2.
3.  Ative também a **granularidade horária** para dados passados. Clique em **Save preferences**.

8. Conclusão
------------

Nesta primeira parte do guia, abordamos os pré-requisitos essenciais a serem configurados no ambiente AWS para suportar a migração para OCI. Na próxima parte, exploraremos os próximos passos na configuração e execução da migração.

---------

Guia de Estudos: Prontidão para Migração - Parte 2
==================================================

Nesta parte do guia, exploraremos os pré-requisitos necessários para usar o Oracle Cloud Migration Service (OCM), incluindo a configuração de compartments e políticas no OCI.

1. Acesso ao OCM
----------------

1.  **Abra o Console OCI**: Clique no ícone de menu (burger icon) no canto superior esquerdo.
2.  **Navegue até "Cloud Migrations"**: Em **Migration and Disaster Recovery**, selecione **Cloud Migrations**.
3.  **Região**: Certifique-se de que você está na região **Australia Southeast (Melbourne)**.
4.  **Compartment**: Você deve estar em um compartment chamado **Migration**.

2. Compartimentos Necessários
-----------------------------

### Tipos de Compartimentos:

*   **Migration Compartment**: Armazena os recursos temporários usados pelo OCM.
*   **Migration Secrets Compartment**: Armazena as credenciais do AWS, recomendado para ter acesso restrito.
*   **Destination Compartment**: Local onde as instâncias migradas serão lançadas.

3. Criando Pré-requisitos
-------------------------

1.  **Selecionar um Compartment**: Caso queira visualizar um compartment não configurado, altere para **MyDemo**.
2.  **Criar Pré-requisitos**: Clique em **Create prerequisites**. Os pré-requisitos são automaticamente gerados através de uma pilha do Oracle Resource Manager.
3.  **Usando Scripts Terraform**: Os arquivos de pré-requisitos, como scripts Terraform, estão disponíveis no repositório do GitHub.

### Passos na Criação:

1.  **Aceitar Termos**: Aceite os Termos de Uso da Oracle.
2.  **Selecionar Compartimento**: Escolha **MyDemo** para criar a pilha.
3.  **Escolher Compartimento Raiz**: Selecione o compartimento raiz onde serão criados os compartimentos de migração e segredos.

4. Habilitação de Migrações
---------------------------

### Tipos de Migrações:

*   **AWS EC2**: Selecione a opção para habilitar migrações de instâncias EC2.
*   **Configuração de Bucket de Replicação**: Um bucket de replicação deve ser criado como parte do fluxo, mesmo que não seja usado.

### Recursos Opcionais:

*   Criação de políticas base para grupos de usuários.
*   Ativação de logs para o agente de hidratação (não aplicável para migrações AWS).

5. Revisão da Configuração
--------------------------

1.  **Revisão dos Detalhes**: Verifique as configurações selecionadas antes de aplicar a pilha.
2.  **Criação da Pilha**: Clique em **Create** para iniciar a criação da pilha.

6. Recursos Criados
-------------------

Após a criação da pilha, verifique os seguintes recursos:

### 6.1. Namespace de Tags

1.  Acesse **Tag Namespaces**: No console, busque por namespaces de tags no compartimento raiz.
2.  Confirme a criação do namespace **CloudMigrations**.

### 6.2. Políticas de Serviço

1.  Navegue até **Identity and Security** > **Policies** para verificar as políticas criadas para suporte à migração.
2.  Política específica para migrações AWS será listada.

### 6.3. Compartimentos Criados

Os compartimentos **Migration** e **Migration Secrets** também devem ter sido criados.

7. Conclusão
------------

Nesta segunda parte do guia, discutimos como preparar o Oracle Cloud para migrações utilizando o OCM. A compreensão dos compartimentos, das políticas IAM e da criação de pré-requisitos é essencial para garantir uma migração bem-sucedida.

---------

### Manage Assets

Guia de Estudos: Gerenciamento de Ativos - Descoberta e Inventário
==================================================================

Neste guia, abordaremos como funciona o processo de descoberta e a criação de inventários para migrações do AWS utilizando o Oracle Cloud Migration Service (OCM).

1. Fonte de Ativos
------------------

### Informações Necessárias:

Para configurar a fonte de ativos, você precisará fornecer as seguintes informações:
*   **AWS Account ID**: O identificador único da sua conta AWS.
*   **AWS Region**: A região do AWS onde estão localizados seus recursos (diferente das regiões OCI).

### Credenciais:

*   **Vault**: As credenciais (ID da chave de acesso e chave de acesso secreta) necessárias para realizar a descoberta e replicações devem ser armazenadas no OCI Vault Service.

### Cronograma de Descoberta:

*   Você pode definir um cronograma para a descoberta (diário, semanal, mensal, etc.).

### Coleta de Métricas:

*   Coleta de métricas históricas, em tempo real e de estimativas de custo para recomendações de dimensionamento.

### Imagem 1: Fonte de Ativos

![image](https://github.com/user-attachments/assets/5e933551-e89e-46ae-84f3-7c57ce52114f)

2. Processo de Descoberta
-------------------------

### Conexões com AWS:

A descoberta se conecta à infraestrutura do AWS para coletar dados dos recursos. A OCM utiliza as APIs EC2 e EBS para autenticação e coleta de informações sem a necessidade de um agente remoto.

### Como Funciona:

1.  **Autenticação**: As credenciais de descoberta são usadas para autenticar-se no ambiente AWS.
2.  **Coleta de Dados**: As instâncias EC2 e volumes EBS são acessados através das APIs, coletando dados como:
    *   Tipo de instância (AWS Instance type)
    *   Endereço IP
    *   Nome do host
    *   Sistema operacional
    *   Alocações de CPU, memória e armazenamento
    *   Configuração do EBS (IOPS, tamanho, etc.)

### Imagem 2: Discovery & Inventory

![image](https://github.com/user-attachments/assets/8c72ff9e-a868-4305-aca5-196a5bcec2bb)

### Montagem do Inventário:

O propósito principal da descoberta é construir uma página de inventário que mostre todos os detalhes das instâncias e volumes associados. Isso inclui:
*   Configurações de EC2
*   Utilização de recursos (tempo real e histórico)

3. Conclusão
------------

Nesta lição, exploramos o processo de descoberta dentro da etapa de gerenciamento de ativos. O OCM coleta informações relevantes do ambiente AWS e constrói um inventário detalhado que será utilizado nas etapas de migração.

---------

Guia de Estudos: Fase de Gerenciamento de Ativos - Processo de Descoberta
=========================================================================

Neste guia, iremos explorar a primeira fase na migração do AWS, conhecida como "Gerenciamento de Ativos". Focaremos no processo de descoberta das instâncias EC2 e volumes EBS, seguido pela construção da página de inventário.

1. Introdução ao OCM
--------------------

Primeiramente, lembre-se que já configuramos os pré-requisitos para o Oracle Cloud Migration Service (OCM) em demonstrações anteriores. Agora, estamos prontos para iniciar o processo de descoberta.

### Acessando a Compartment de Migração

*   **Navegue até a Compartment de Migração**: A partir do OCM, clique em **Discovery** à esquerda.

2. Criando a Fonte de Ativos
----------------------------

### Passos para Criar a Fonte de Ativos:

1.  **Clique em "Create Asset Source"**.
2.  **Tipo de Fonte de Ativos**: Escolha "AWS" e nomeie a fonte como **asset source demo**.
3.  **ID da Conta AWS**: Cole o ID da conta AWS que você usou para login.
4.  **Selecionar Região**: Essa deve ser a região AWS (por exemplo, **US East 1** para Virginia do Norte).
5.  **Selecionar Compartimentos**: Use a compartment de **migração** para ambas as configurações (fonte e destino).

### Conectar ao Ambiente de Fonte

*   **Criar Novo Ambiente de Fonte**: Nomeie como **environment AWS demo**.
*   **Configurar Credenciais de Descoberta**: Insira as chaves de acesso AWS (Access Key ID e Secret Access Key) no Vault.

### Usando o Vault

1.  **Escolha o Compartimento de Segredos de Migração**: Selecione o vault previamente criado.
2.  **Selecionar Chave de Criptografia**: Selecionar a chave criptográfica do vault.

3. Configuração de Credenciais
------------------------------

### Inserir Chaves

*   **Copie as chaves do arquivo CSV** (baixado anteriormente) para o campo adequado.

### Programação da Descoberta

*   **Escolha o Cronograma de Descoberta**: Opte por "sem cronograma" para executar a descoberta sob demanda.

### Habilitar Coleta de Métricas

*   Opções incluem:
    *   Coleta de métricas históricas.
    *   Coleta de métricas em tempo real.
    *   Estimativa de custos através do **AWS Cost Explorer**.

4. Criar a Fonte de Ativos
--------------------------

1.  **Clique em "Create Asset Source"** e aguarde a confirmação de que está feito.
2.  **Execute a Descoberta**: Clique em **Run Discovery** para iniciar o processo.

### Monitoramento do Processo de Descoberta

*   **Verifique o Status das Solicitações**: Acompanhe o progresso e verifique se 100% foi concluído.

5. Verificando os Ativos Descobertos
------------------------------------

1.  **Navegue para "Assets"** sob **Resources**: Aqui, veja os recursos descobertos, incluindo a instance EC2 e volumes EBS atrelados.
    
2.  **Ver Inventário**:
    *   Acesse **Cloud Migrations** > **Inventory** para visualizar um dashboard com detalhes do inventário.
    *   Avalie o total de memória, armazenamento e CPU.
3.  **Selecionar Recursos para Migração**: Prepare-se para escolher os ativos que você deseja migrar nas próximas etapas.
    

6. Conclusão
------------

Neste guia, cobrimos o processo de descoberta como parte da fase de gerenciamento de ativos para migração AWS. Você agora tem um inventário com as instâncias e volumes EBS, pronto para avançar nas próximas etapas de migração.

---------
### Planning & Replication

Guia de Estudos: Planejamento e Replicação de Recursos AWS para OCI
===================================================================

Nesta lição, focaremos em como planejar e replicar ou migrar recursos da AWS para a Oracle Cloud Infrastructure (OCI).

1. O que são Projetos de Migração?
----------------------------------

![image](https://github.com/user-attachments/assets/380c145e-c1b8-410b-8912-7728203650cb)

Um projeto de migração é um contêiner lógico onde são criados os **ativos de migração** e os **planos de migração**. Através deste projeto, você executará as tarefas de migração e, ao final, marcará a conclusão dessas tarefas.

### Componentes do Projeto de Migração:

![image](https://github.com/user-attachments/assets/b6773a5e-8cb7-4588-a7b9-3ed36307ac2f)

*   **Ativos de Migração**: São os recursos selecionados para migração, por exemplo, se você descobrir 10 instâncias EC2, mas decidir migrar apenas 5, essas 5 se tornam seus ativos de migração.
*   **Local de Replicação**: Embora parte do projeto, o local de replicação não é utilizado nas migrações da AWS, pois os volumes EBS são replicados diretamente para os volumes de boot ou de bloco da OCI.
*   **Cronograma de Replicação**: Define com que frequência a replicação deve ocorrer (diariamente, semanalmente, mensalmente ou anualmente).

2. Planos de Migração
---------------------

![image](https://github.com/user-attachments/assets/fc1cf0fb-f770-4cc2-92ad-cd42859af798)

### Definição de um Plano de Migração:

O plano de migração detalha como os ativos de migração serão mapeados para os ativos de destino na OCI.

#### Componentes do Plano de Migração:

*   **Estratégia**: Define o tipo de recomendação de migração com base no uso de CPU, memória ou ambos.
*   **Ativos de Destino**: Mapeamento dos ativos de migração para uma forma de computação recomendada.
    *   Exemplo: Uma instância EC2 com tipo T2 micro pode ser mapeada para um tipo de computação compatível na OCI, como VM standard E4 flex.
*   **Compatibilidade de Ativos**: O plano verifica a compatibilidade dos ativos com as formas de computação da OCI, considerando a arquitetura do CPU.
*   **Estimativa de Custo**: Compara os custos entre AWS e OCI, fornecendo uma estimativa dos custos individuais de cada tipo de recurso.

3. Fluxo de Replicação
----------------------

![image](https://github.com/user-attachments/assets/b69eb24d-26cf-44a3-ba48-61d8c2f4f10b)

### Processo de Replicação:

1.  **Transferência de Dados**: Realizada de forma segura via HTTPS.
2.  **Snapshot EBS**: Durante a replicação, um snapshot EBS é criado na infraestrutura da AWS.
3.  **Agente de Hidratação**: Um agente temporário é lançado para utilizar a API Direta de EBS e transferir dados do snapshot para o volume de armazenamento na OCI.
    *   As transferências podem incluir dados de snapshots completos ou blocos alterados.

### Importante:

*   O bucket de replicação **não** é utilizado durante as migrações da AWS, independentemente da configuração do projeto de migração.

4. Conclusão
------------

Nesta lição, revisamos a estrutura dos projetos de migração, os ativos de migração, os planos de migração e o fluxo de replicação de dados. Esta compreensão é crucial para a implementação bem-sucedida da migração de recursos da AWS para a OCI.

---------

Demonstração de Planejamento e Replicação de Migração AWS para OCI
==================================================================

Nesta demonstração, iremos percorrer o processo de planejamento e replicação de um recurso da AWS (EC2) para a Oracle Cloud Infrastructure (OCI).

1. Preparação na AWS
--------------------

### Acessando o Console AWS

*   **Navegar até a Página de Instâncias**: No AWS console, localize as instâncias.
*   **Selecionar Instância**: Identifique a instância chamada **workload to migrate**. Esta está em estado de execução e usa o tipo `t2.micro`.

### Verificando Armazenamento

*   **Tipo de Armazenamento**: O armazenamento é fornecido pelo **Elastic Block Store (EBS)**.
*   **Tamanho da Volume**: A dimensão do volume é de **8 GB**.

2. Planejamento da Migração na OCI
----------------------------------

### Acessando o Console OCI

1.  **Navegar até OCM**: No console OCI, clique em **Migrations**.
2.  **Criar Projeto de Migração**: Escolha criar um novo projeto de migração com um plano de migração inicial.
    *   **Nome do Projeto**: Chame-o de **migration demo**.
    *   **Selecionar Compartimento**: Use a compartment de **migração**.
    *   **Programação de Replicação**: Para este demo, vamos selecionar "sem programação" e proceder.

### Adicionando Ativos à Migração

*   **Adicionar Ativos da Inventário**: Clique em **Add from OCM inventory** e selecione a instância **workload to migrate**.

### Configurando Local de Replicação

*   **Escolher a Localização de Replicação**: Escolha **AD 1** como a disponibilidade dos volumes e a compartment de migração como a compartment de replicação.

3. Criando um Plano de Migração
-------------------------------

### Definições do Plano

1.  **Nome do Plano**: Nomeie como **migration plan demo**.
2.  **Selecionar Compartimento**: Armazene na compartment de migração.
3.  **Compartimento de Destino**: Indique onde os recursos migrados serão lançados (por exemplo, **my demo compartment**).
4.  **Forma de Computação**: Escolha **sistema de recomendações** para as formas de computação que serão utilizadas.

### Revisão e Submissão

*   **Revisão de Detalhes**: Revise as informações e clique em **Submit**. Aguarde a confirmação de que o estado do plano está agora **ativo**.

4. Iniciando a Replicação
-------------------------

### Replicação dos Recursos

*   **Iniciar Replicação**: Clique em **Replicate** e confirme a replicação.
*   **Monitorar Progresso**: Acompanhe a barra de progresso no console OCI.

### Verificando o Console AWS

*   **Navegar até Snapshots**: Sob a seção **Elastic Block Store**, observe que um snapshot foi criado.

5. Hidratação e Monitoramento na OCI
------------------------------------

### Hidratação do Volume

*   **Hidratação Agent**: Um agente temporário é executado, que lê o snapshot do volume EBS e escreve diretamente no serviço de volumes de bloco da OCI.
*   **Verificando Volumes**: Clique em **Block Volumes > Boot Volumes** na OCI para ver os volumes replicados.

### Características do Volume

*   **Detalhes do Volume**: O volume replicado apresentará um tamanho de **47 GB** por padrão no OCI para instâncias Linux, mesmo que o EBS original fosse de **8 GB**.

6. Finalizando a Migração
-------------------------

### Verificando Volume Replicado

1.  **Acessar Ativos de Migração**: Retorne ao projeto de migração na OCI, selecione **Migration assets** e verifique a instância replicada.
2.  **Conferir Progresso da Replicação**: Assegure-se de que a replicação foi concluída com sucesso através do painel de **Work requests**.

### Conclusão do Processo

*   **Marcar Migração Completa**: Esta etapa de finalização será realizada após a validação das etapas pós-migração.

* * *

Esse guia resume a demonstração de planejamento e replicação de ativos AWS para OCI, cobrindo desde a configuração no AWS até a finalização na OCI.

---------

Guia de Estudos: Fase Final da Migração AWS para OCI
====================================================

Nesta seção, vamos examinar a fase final da migração de recursos da AWS para a Oracle Cloud Infrastructure (OCI), que envolve o lançamento e a validação dos ativos migrados.

1. Revisão do Fluxo de Migração
-------------------------------

As etapas de migração até agora são:
1.  **Fonte de Ativos (Asset-Source)**:
    *   Descoberta e inventário dos recursos.
2.  **Projeto de Migração (Migration Project)**:
    *   Criação de um plano de migração e seleção dos ativos a serem migrados.
3.  **Revisão**:
    *   Avaliação das recomendações e estimativas de custo.
4.  **Replicações**:
    *   Replicação dos volumes em volumes de boot e de bloco da OCI.

![image](https://github.com/user-attachments/assets/2bf4ea16-2835-4f82-b141-56bfa7f81778)

2. Etapas Finais da Migração
----------------------------

### Geração da Stack do Resource Manager

*   **Stack do Resource Manager**: Uma stack será gerada para lançar as instâncias-alvo a partir dos volumes replicados.
    *   Esta stack contém scripts do Terraform com configurações individuais para cada ativo.

### Vantagem de Múltiplos Planos de Migração

*   **Stacks Únicas para Cada Plano**: A criação de planos de migração permite que stacks únicas possam ser criadas com base nos ciclos de validação, como testes de integração ou carga, cada uma com suas próprias configurações de ativos-alvo.

### Modificação dos Volumes de Boot

*   Para instâncias EC2 baseadas em Linux, os volumes de boot são automaticamente modificados pelo serviço OCI para garantir que a inicialização ocorra corretamente.
    *   Isso inclui a instalação de drivers necessários, atualização de anexos de armazenamento e ajuste de parâmetros do kernel.

3. Validação Pós-Migração
-------------------------

### Importância da Validação

*   **Validação Operacional**: Certifique-se de que a carga de trabalho migrada está operacional.
*   **Integração com Serviços OCI**: Após os testes, pode-se integrar com outros serviços OCI, como serviços de observação e gerenciamento, para monitorar a aplicação.

### Marcação do Projeto de Migração como Completo

*   **Finalização do Projeto**: Marcar um projeto de migração como completo impede que ele seja modificado, ajudando a evitar alterações nos recursos implantados que foram verificados e estão em produção.

![image](https://github.com/user-attachments/assets/44839950-e5ca-4248-bef8-edaac1d34bcf)

4. Conclusão
------------

Nesta lição, examinamos as fases de lançamento e validação após a etapa de replicação. Agora você tem uma compreensão clara de como completar a migração de recursos da AWS para a OCI.

* * *

Guia de Estudos: Demonstração de Lançamento e Validação
=======================================================

Nesta demonstração, iremos explorar a fase final da migração de instâncias AWS para a Oracle Cloud Infrastructure (OCI), focando no lançamento da instância de computação usando o volume replicado.

1. Acesso ao Projeto de Migração
--------------------------------

### Navegando no Console OCI

1.  **Acessar o Projeto de Migração**: Clique no projeto de migração criado anteriormente.
2.  **Selecionar o Plano de Migração**: Clique em **Migration plan** para ver as opções disponíveis.

### Geração da Stack do Resource Manager

*   **Gerar RMS Stack**: Há uma opção para gerar uma stack do Resource Manager (RMS) que irá provisionar uma instância de computação usando o volume replicado.

2. Revisão da Compatibilidade e Custos
--------------------------------------

### Compatibilidade do Ativo

*   **Resumo de Compatibilidade**: A compatibilidade do ativo para rodar na OCI é alta, sem erros ou avisos.
*   **Estimativa de Custos**: Compare o custo do recurso na AWS e na OCI, com detalhes sobre armazenamento, computação e custos totais mensais.

### Estratégia e Assets Alvo

*   **Estratégia Usada**: A estratégia selecionada é baseada no tipo de recurso de CPU com as configurações padrão.
*   **Selecionar Ativos Alvo**: O ativo de migração foi replicado como um volume de boot na OCI.

### Opções de Configuração

*   **Reconfigurar Ativo Alvo**: Aqui, você pode mudar o tipo de capacidade, selecionar uma VCN e uma subnet, e atribuir um endereço IP público. Para esta demonstração, não faremos alterações.

3. Processo de Geração e Deploy da Stack
----------------------------------------

### Geração da Stack

1.  **Gerar a Stack**: Navegue de volta ao plano de migração e clique em **Generate RMS stack**.
2.  **Acompanhar o Progresso**: Verifique o progresso da geração da stack na seção **Work request**.

### Implementação da Stack

*   **Deploy da Stack**: Após a geração ser concluída, clique em **Deploy RMS stack** e confirme para iniciar a implantação.

4. Verificando Instância de Computação
--------------------------------------

### Acessando a Instância

1.  **Mudar Compartimento**: Mude para o compartimento onde a instância foi provisionada (por exemplo, **my demo**).
2.  **Verificar Instância**: Confirme que a instância de computação foi lançada e está em execução, utilizando o tipo de forma **VM standard E4 flex**.

5. Validações Pós-Migração
--------------------------

*   **Validações Necessárias**: Realize as validações pós-migração específicas para cada aplicação, assegurando que tudo funciona conforme esperado. Este guia não cobrirá as validações aqui.

6. Finalizando o Processo de Migração
-------------------------------------

1.  **Retornar ao OCM**: Mude de volta para o compartimento de migração, selecione o projeto de migração.
2.  **Marcar Migração como Completa**: Após a validação, o último passo é marcar a migração como completa. Uma vez marcada como completa, não poderá ser modificada, garantindo a integridade dos recursos implantados.

* * *

Esse guia abrange a demonstração de como migrar instâncias EC2 da AWS para a OCI, focando nas etapas de lançamento e validação.

---------

![image](https://github.com/user-attachments/assets/09bb6dc5-4097-4ebc-a50e-f2aeac5e6eb1)
**1. Qual dos seguintes recursos da AWS é migrado para o OCI usando Oracle Cloud Migrations (OCM)?**
*   ✅ Volume EBS
*   ❌ Sub-redes VPC
*   ❌ Bancos de Dados RDS
*   ❌ Grupos de Usuários
   
**Nota:** Correto. A Oracle Cloud Migrations inicia um snapshot do volume EBS anexado à instância EC2, que é replicado no armazenamento em bloco do OCI.

![image](https://github.com/user-attachments/assets/20610d62-d1f1-44df-aca0-8918cc09b781)
**2. Quais dos seguintes serviços da AWS devem ser ativados para permitir a estimativa de custos e recomendações de dimensionamento no Plano de Migração da Oracle Cloud Migrations (OCM)?**
*   ✅ Amazon CloudWatch
*   ❌ AWS Snowball
*   ✅ AWS Cost Explorer
*   ❌ AWS Shield
   
**Nota:** Correto. O AWS Cost Explorer e o Amazon CloudWatch são necessários se estimativas de custo e recomendações de dimensionamento forem parte de um plano de migração. Um agente do Amazon CloudWatch precisa ser configurado em cada instância para coletar métricas de desempenho mais precisas e avançadas.

![image](https://github.com/user-attachments/assets/803e09a4-33d2-4796-a4d5-fcfb7145d756)
**3. O que a Oracle Cloud Migrations utiliza para realizar a descoberta de recursos da AWS no OCI?**
*   ❌ Resource Manager Stack
*   ❌ Service Connector Hub
*   ✅ EC2 & EBS APIs
*   ❌ Remote Agent Appliance
   
**Nota:** Correto. A Oracle Cloud Migrations (OCM) está integrada com as APIs do AWS EC2 e EBS para reunir detalhes das instâncias EC2 e dos volumes EBS necessários para a avaliação e o planejamento da migração. A OCM se conecta à AWS através de um papel IAM para extrair metadados sobre as instâncias EC2 e seus volumes EBS associados.

Obtenha uma resposta mais inteligente de GPT-4o

![image](https://github.com/user-attachments/assets/291a768d-df2d-4e58-a1cf-4d17be752cc0)
**4. O que é criado como parte da execução de pré-requisitos na Oracle Cloud Migrations?**
*   ❌ Migration Plan
*   ❌ Remote Agent appliance
*   ✅ Tag namespace
*   ❌ Migration Asset

**Nota:** Correto. Um namespace de tag "CloudMigrations" é criado no nível do inquilino com algumas definições de chave de tag. Essas tags são usadas pelo serviço de Oracle Cloud Migration para rastrear recursos migrados.

![image](https://github.com/user-attachments/assets/573b6b2e-f6df-4929-8c04-4a8663834c1a)
**5. Quais das seguintes afirmações são válidas para o serviço Oracle Cloud Migrations?**
*   ❌ Hydration Agent representa a instância EC2 migrada
*   ✅ Os volumes de inicialização do Linux são modificados para um lançamento bem-sucedido
*   ❌ Os volumes EBS são replicados em um bucket de armazenamento de objetos OCI
*   ✅ A transferência de dados é realizada usando HTTPS

**Nota:** Correto. Quando se migra um sistema Linux para a Oracle Cloud Infrastructure (OCI) usando o serviço Oracle Cloud Migrations, o volume de inicialização da máquina virtual Linux será automaticamente modificado para garantir que inicialize corretamente no hypervisor OCI em segundo plano. O método de migração usado para transferir dados para o OCI varia com base no tipo de ambiente de origem, mas sempre é realizado de forma segura usando HTTPS.












