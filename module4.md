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














