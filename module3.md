### 1. **Introdução às Migrações para a Oracle Cloud**

*   **Personagens**: Jason (Proprietário de Negócio) e Emily (Arquiteta de Nuvem) são apresentados em um contexto de migração para a Oracle Cloud Infrastructure (OCI).
*   **Desafio**: O data center atual de Jason está com a lease prestes a expirar, levando à necessidade de migração para a nuvem.

![image](https://github.com/user-attachments/assets/e9fa409c-b517-4ae6-a2ad-f194cc76d3ca)

### 2. **Opções de Migração**

*   **Opção 1**: Mover para OCVS (Oracle Cloud VMware Solution), criando um centro de dados definido por software.
*   **Opção 2**: Migrar para OCI Compute, utilizando máquinas virtuais.

![image](https://github.com/user-attachments/assets/8ad0769f-c283-4b00-bf6c-21443d7f71f3)

### 3. **Benefícios da Migração**

*   **Automação**: Migração automatizada a um custo menor.
*   **Testes**: Possibilidade de testar a migração antes da implementação completa.
*   **Integrações**: Expansão de integrações nativas com OCI.
*   **Redução de Sobrecarga Operacional**: Diminuição da complexidade e dos custos operacionais.

![image](https://github.com/user-attachments/assets/d54de614-ed7a-4071-ab07-8ed8d1fc5997)

### 4. **O que é Oracle Cloud Migrations?**

*   **Serviço Centralizado**: Um serviço que permite descobrir VMs externas, agrupar e organizar VMs, calcular custos de projeto, replicar dados de VMs e lançá-los como OCI Compute.
*   **Automatização**: Oferece fluxos de trabalho automatizados para facilitar a migração de aplicações para a infraestrutura da Oracle Cloud.

![image](https://github.com/user-attachments/assets/6ed63224-6c28-4145-ac64-db7e50319263)

### **Conclusão**

Esta é uma solução prática e eficiente para a migração de serviços e aplicações existentes para a Oracle Cloud, com ênfase em automação, custo e integração com novas tecnologias.

---------

### **Key Concepts**

### 1. **Terminologias Importantes**

*   **OVA Appliance**: Imagem pré-empacotada que inclui sistema operacional e software, pronta para ser importada ou implantada nas instalações.
*   **Plugin**: Recursos que melhoram a funcionalidade existente de um produto.
*   **VDDK**: Virtual Disk Development Kit, um kit de desenvolvimento para manipulação de discos virtuais.
*   **Snapshot**: Cópia do arquivo do disco da Máquina Virtual em um dado momento.
*   **Replication**: Processo de copiar e manter objetos de um sistema de origem para um ambiente alvo.
*   **Hydration Agent**: Instância de computação temporária, provisionada e desativada automaticamente.

![image](https://github.com/user-attachments/assets/f053c674-9137-4bfb-a4cc-ee4e497e0d57)

### 2. **Fases das Migrações para a Oracle Cloud**

*   **Connect and Discover**:
    *   Utiliza VMware vSphere e um agente remoto para descobrir e conectar ativos (Máquinas Virtuais).
*   **Build Inventory**:
    *   Anotar e agrupar os ativos descobertos para melhor gerenciamento e planejamento.
*   **Assess and Plan**:
    *   Avaliação de métricas, compatibilidade dos componentes e estimativa de custos para planejamento da migração.
*   **Replicate**:
    *   Realização de um snapshot VMDK, seguido pelo armazenamento no OCI Object Storage e volume block no OCI.
*   **Launch**:
    *   Utilização do OCI Resource Manager (stacks do Terraform) para lançar a nova instância de computação nativa OCI.

![image](https://github.com/user-attachments/assets/a89c5f46-cbf2-4a7c-b7ef-46e0fb16d7c7)

### **Conclusão**

Esses conceitos e fases fornecem um quadro abrangente sobre como realizar migrações para a Oracle Cloud, abordando desde a descoberta dos ativos até a implantação final.

---------

Guia de Integração de Migrações do Oracle Cloud
===============================================

Este guia apresenta os pré-requisitos necessários para o funcionamento do serviço de migrações da Oracle Cloud (OCM) para migrações de VMware. O processo é também conhecido como "onboarding" do serviço OCM.

1. **Prérequisitos para o Serviço OCM**
---------------------------------------

### a. Verificando o Compartimento de Migração

1.  **Acesse o Compartimento**: Primeiro, acesse o compartimento onde deseja ativar o serviço OCM.
2.  **Verificação de Ativação**: Se o compartimento não estiver habilitado para o OCM, a seguinte mensagem aparecerá:
    
    > "Você não pode usar este serviço ainda neste compartimento."
    

### b. Criando Pré-requisitos

1.  **Criar Pré-requisitos**: Clique em **Criar Pré-requisitos** para iniciar a configuração.
2.  **Implantação**: Os pré-requisitos são implantados por meio de uma pilha anônima e um pacote é enviado para o bucket de armazenamento de objetos.
3.  **Acesso ao Pacote**: A interface do usuário fornecerá a URL do pacote, que pode ser usada para baixar arquivos de pré-requisitos do repositório GitHub, disponível em uma pasta chamada "prerequisites".

### c. Usando um Provedor Terraform Personalizado

1.  **Download do ZIP**: Você pode baixar um arquivo zip com scripts Terraform e o arquivo YAML contidos.
2.  **Upload**: Se optar por usar um provedor Terraform personalizado, faça o upload do arquivo para um bucket de armazenamento de objetos no compartimento selecionado. Para esta configuração, usaremos o provedor padrão.

2. **Aceitando os Termos e Criando Pilha**
------------------------------------------

1.  **Revisão dos Termos**: Revise e aceite os termos de uso da Oracle.
2.  **Nome e Descrição**: Opcionalmente, forneça um nome e uma descrição para a pilha.
3.  **Selecionar Compartimento**: Escolha o compartimento (por exemplo, OCI_Dev) para a criação do stack do Gerenciador de Recursos.
4.  **Versão do Terraform**: Selecione a versão do Terraform (ex.: 1.2.x).
5.  **Marcação**: As tags são opcionais.

3. **Configuração de Compartimentos**
-------------------------------------

1.  **Selecionar Compartimento Raiz**: Escolha o compartimento raiz onde os compartimentos de migração e segredos de migração serão criados.
2.  **Recursos Globais**: Você pode desmarcar a opção de pilha de pré-requisitos primária se já tiver criado os compartimentos necessários.

4. **Habilitando Migrações**
----------------------------

*   **Tipos de Migrações**: O OCM suporta duas migrações: VMware VMs e AWS EC2. Para este guia, habilitaremos a migração de VMs VMware.
*   **Local de Replicação**: Defina um bucket de replicação, por exemplo, "OCM replication", e selecione a opção para criar um novo bucket de replicação.

5. **Configurações Opcionais**
------------------------------

*   **Políticas IAM**: Você pode especificar a criação de políticas IAM para grupos de usuários.
*   **Log do Agente Remoto**: Habilite opcionalmente o log do agente remoto.
*   **Log do Agente de Hidratação**: Preservar as configurações padrão.

6. **Revisão e Aplicação**
--------------------------

1.  **Revisão Final**: Revise a página de configuração e, em seguida, execute a aplicação na pilha criada.
2.  **A aplicação da pilha** ocorrerá imediatamente após a criação.

7. **Finalização do Processo de Onboarding**
--------------------------------------------

*   Após a configuração dos pré-requisitos, você pode verificar as **Namespaces de Tags** e as **Políticas de Serviço** criadas durante a configuração.
*   **Hierarquia de Compartimentos**: A pilha de pré-requisitos criará um compartimento de migração, onde serão criados todos os recursos temporários necessários para executar migrações.

8. **Considerações Finais**
---------------------------

Este guia fornece uma visão geral de como habilitar o serviço OCM para migrações no Oracle Cloud. Para mais detalhes específicos, consulte a documentação da Oracle Cloud ou entre em contato com o suporte da Oracle.

---------

### **Discovery and Inventory**

Guia de Descoberta e Inventário na Migração para a Oracle Cloud
===============================================================

Neste guia, abordaremos as fases de descoberta e inventário no processo de migração para a Oracle Cloud (OCI), destacando a integração com VMware usando o Oracle Cloud Bridge (OCB).

1. **Fase de Descoberta**
-------------------------

### a. O que é o Oracle Cloud Bridge?

*   O **Oracle Cloud Bridge** é um appliance de agente remoto, disponível como uma imagem OVA, que contém as ferramentas necessárias para descoberta e replicação.
*   Esse appliance deve ser implantado na rede de origem e registrado no serviço OCM (Oracle Cloud Migration).

![image](https://github.com/user-attachments/assets/c264df20-64cd-433e-9fd0-4d84b183f32f)

### b. Implantação do Appliance de Agente Remoto

1.  **Conectividade**: O appliance estabelece a conectividade entre o ambiente on-premises (VMware) e a OCI.
2.  **Início da Descoberta**: Uma vez implantado e registrado, o processo de descoberta é iniciado automaticamente a partir do appliance.

### c. Pré-requisitos para Implantação do OCB

*   **Ambientes VMware**: É necessário utilizar ambientes VMware vSphere gerenciados pelo vCenter.
*   **Conectividade**: O appliance OCB deve ter conectividade externa com a OCI e interna com o servidor vCenter e hosts ESXi (hipervisores).
*   **DNS**: Um servidor DNS deve ser capaz de resolver os nomes de domínio totalmente qualificados dos componentes VMware e endpoints da OCI.
*   **Configurações de Interface de Rede**: As configurações de rede podem ser definidas via DHCP ou estaticamente.

![image](https://github.com/user-attachments/assets/99a42a7d-5b0a-48c6-92d0-acd83aea8cc4)

### d. Dependência do Agente

*   O OCB utiliza três plugins:
    *   **Plugin de Descoberta**: Busca máquinas virtuais no ambiente VMware.
    *   **Plugin de Replicação**: Gerencia o processo de replicação dos ativos.
    *   **Plugin de Monitoramento de Saúde do Agente**: Monitora o estado do agente.

![image](https://github.com/user-attachments/assets/9c7718eb-232c-4343-926e-c7feb30e8470)

### e. Upload do VDDK

*   **VDDK (Virtual Disk Development Kit)**: É necessário fazer upload do VDDK em um bucket de armazenamento de objetos para permitir o acesso às volumes de armazenamento das máquinas virtuais.

2. **Configuração da Fonte de Ativos**
--------------------------------------

*   O **OC Vault** é utilizado para gerenciar credenciais de descoberta e replicação, garantindo acesso seguro.
*   É importante estipular um cronograma de descoberta, que pode ser diariamente, semanalmente, mensalmente ou anualmente.
*   Você pode coletar métricas em tempo real e históricas das VMs, o que ajudará a otimizar o processo de migração.

![image](https://github.com/user-attachments/assets/ba1b6742-5b20-4768-86cf-13df21d4a9da)

### a. Processo de Descoberta

*   **Descoberta Automatizada**: Permite descobrir VMs, discos virtuais associados, uso de CPU e memória, e tags específicas do VMware.
*   Você pode optar por descoberta sob demanda ou programada, dependendo das configurações de fonte de ativos.

3. **Fase de Inventário**
-------------------------

*   O **Inventário** é a representação na OCI dos ativos descobertos, incluindo suas metadatas.

### a. Funções do Inventário

*   Permite a revisão dos detalhes das VMs descobertas, facilitando a decisão sobre a forma de destino de cada VM (quantidade de CPUs, memória, armazenamento, etc.).
*   Os ativos no inventário são organizados e podem ser etiquetados conforme a aplicação ou função.

![image](https://github.com/user-attachments/assets/eed51e40-1fef-4411-9a6c-61d1a4772638)

### b. Importação de Ativos

*   Você pode importar ativos via CSV, o que facilita a organização antes das migrações.

4. **Resumo Final**
-------------------

1.  **Implantação do OCB**: Implante o appliance OCB na rede VMware.
2.  **Registro do Agente**: Registre o agente para criar conexões remotas.
3.  **Plugins**: Configure os três plugins necessários (descoberta, replicação e monitoramento).
4.  **Hole e VDDK**: Complete as dependências do agente com o upload do VDDK.
5.  **Descoberta e Inventário**: Realize a descoberta e use a página de inventário para revisar e organizar ativos para migração.

![image](https://github.com/user-attachments/assets/23e9fdc2-448c-4218-8c98-06d72392446a)

Este guia fornece um panorama claro das fases de descoberta e inventário durante migrações para a Oracle Cloud.

---------

Guia de Onboarding da Fase de Descoberta na Migração para a Oracle Cloud
========================================================================

Neste guia, abordaremos a fase de descoberta utilizando o serviço OCM (Oracle Cloud Migration). Vamos explorar como navegar pelo serviço e configurar o ambiente necessário.

1. **Navegando até o Serviço OCM**
----------------------------------

1.  **Acessar o Painel OCM**: Clique no ícone do menu no canto superior esquerdo (hambúrguer).
2.  **Selecionar Migração e Recuperação de Desastres** e, em seguida, clique em **Migrações em Nuvem**.

2. **Pré-requisitos**
---------------------

Antes de configurar o serviço OCM, são necessários alguns pré-requisitos, incluindo a configuração de políticas de gerenciamento de identidade (IAM) e a criação de compartimentos.

### a. Repositório GitHub

*   Acesse o repositório GitHub através da URL fornecida para facilitar o processo de configuração.
*   No repositório, há recursos que incluem:
    *   **Compartimentos**: Dois compartimentos serão utilizados:
        *   **Migration**: Para configurar o serviço OCM.
        *   **MigrationSecrets**: Para armazenar credenciais do ambiente de origem usando o serviço Vault.

### b. Armazenamento de Objetos

*   Crie um bucket de armazenamento onde os dados de snapshot do vSphere serão transferidos para o ambiente OCI.
*   Crie as políticas IAM e grupos dinâmicos necessários para a comunicação entre os componentes do serviço OCI.
Para implantar esses recursos, baixe o arquivo zip do repositório e aplique como uma pilha (stack) no Gerenciador de Recursos da OCI.

3. **Configuração da Fase de Descoberta**
-----------------------------------------

### a. Criar Ambiente de Origem

1.  **Criar um Novo Ambiente**: No painel do OCM, crie um ambiente de origem e dê-lhe um nome, como "source".
2.  **Selecionar Compartimento**: Escolha o compartimento **Migration**.

### b. Configurar o Agente Remoto

3.  **Baixar OCB**: Clique em **Baixar VM do agente** para obter detalhes sobre a versão do appliance do OCB.
4.  **Implantar o Appliance**:
    *   Clique com o botão direito no cluster e selecione **Implantar Template OVF**.
    *   Siga as instruções (Clique em **Next** repetidamente e revise os detalhes).
5.  **Armazenamento e Rede**:
    *   Selecione o armazenamento necessário para implantar o appliance.
    *   Certifique-se de que o appliance tenha conectividade interna com o vCenter e hosts ESXi, além de conectividade externa com endpoints OCI.
    *   Configure as propriedades de rede, use DHCP para o IP e forneça o DNS necessário.
![Imagem: Implantação do OCB](chrome-extension://difoiogjjojoaoomphldepapgpbgkhkb/link-da-imagem)

### c. Ativando o Appliance

1.  **Ligar a VM**: No console do VMware, encontre a VM do OCB e ative-a.
2.  **Registrar o Agente**: Anote o endereço IP atribuído à VM e volte para a interface do OCM para registrar o agente.
    *   Clique em **Registrar agente** e insira o IP e o nome único para o agente.

4. **Plugins e Dependências**
-----------------------------

*   Após o registro, você verá três plugins: Discovery, Agent Health Monitoring e Replication.
*   **VDDK**: Para o plugin de replicação ser ativado, faça o upload do arquivo VDDK em um bucket de armazenamento de objetos e crie uma dependência de agente.

### a. Criar Dependência do Agente

1.  **Ir para Ambiente de Origem**: Volte para o ambiente de origem e clique em **Criar dependência do agente**.
2.  **Selecionar Bucket**: Escolha o bucket onde o VDDK já está armazenado (baixe do site da VMware).

5. **Criar Fonte de Ativos**
----------------------------

1.  **Criar Fonte de Ativos**: Vá para **Fontes de Ativos** e clique em **Criar fonte de ativos**.
2.  **Configurar Endpoints**: Insira o URL do vCenter e utilize o compartimento **MigrationSecrets** para as credenciais de descoberta.

### a. Agendamento de Descoberta

*   Por agora, não utilize um agendamento, mas há a opção de habilitar a coleta de métricas históricas e em tempo real.

6. **Executar a Descoberta**
----------------------------

1.  **Executar Descoberta Sob Demanda**: Após a configuração da fonte de ativos, você pode optar por executar a descoberta sob demanda. Isso permite que você inicie o processo imediatamente, em vez de esperar por um agendamento.
    
2.  **Confirmação da Execução**: Clique em **Executar Descoberta** e aguarde a conclusão do processo. Este passo permitirá que o sistema descubra as máquinas virtuais e outros ativos no seu ambiente de origem, coletando informações importantes sobre eles.
    
3.  **Finalização do Processo**: Uma vez que a descoberta for finalizada, verifique os resultados. Você terá acesso a uma lista detalhada de ativos descobertos, incluindo máquinas virtuais, discos associados, redes, uso de CPU e memória, além de tags específicas do VMware.

7. **Conclusão da Fase de Descoberta**
--------------------------------------

*   Uma vez que o processo de descoberta esteja completo, você terá uma visão geral dos ativos disponíveis em seu ambiente. Isso inclui as máquinas virtuais, discos associados, rede, utilização de CPU, consumo de memória e quaisquer tags específicas do VMware que possam ter sido atribuídas.

### a. Próximos Passos

*   A fase de descoberta é apenas o primeiro passo na migração para a Oracle Cloud. Agora que você teve sucesso em configurar e executar a descoberta, o próximo estágio será a fase de inventário, onde você poderá revisar e organizar os ativos descobertos.

### b. Acesso ao Inventário

*   No painel do OCM, navegando até a seção de **Inventário**, você poderá visualizar todos os ativos que foram detectados. Você terá a capacidade de agrupar, taguear e preparar esses ativos para a migração.

### c. Finalização

*   Com a fase de descoberta finalizada, você agora pode se preparar para as etapas de configuração e execução da migração. As informações coletadas durante a descoberta são essenciais para planejar a migração de forma eficaz, garantindo que todas as dependências e configurações necessárias sejam consideradas.

Resumo
------

Neste guia, cobrimos as etapas fundamentais para completar a fase de descoberta no serviço OCM para migrações na Oracle Cloud. Desde a navegação pelo serviço até a criação de fontes de ativos, todas as etapas foram detalhadas para garantir que você possa preparar seu ambiente para uma migração bem-sucedida.

---------

### **Migration Projects**

---------

### **Replicate & Launch**

---------

### **Migration Flow and Architecture**

---------

