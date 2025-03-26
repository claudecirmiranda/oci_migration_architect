Fundamentos de Redes de Nuvem Virtual (VCN) no OCI
==================================================

O que é uma VCN?
----------------

Uma Rede de Nuvem Virtual (VCN) é como uma rede privada virtual dentro de uma região do Oracle Cloud Infrastructure. Podemos fazer uma analogia simples:
🏢 Imagine uma empresa com um escritório grande:
*   A região do OCI é como o prédio inteiro
*   A VCN é como uma sala de reuniões reservada e isolada dentro desse prédio
*   Cada VCN é um espaço de rede privado e seguro

Características Principais da VCN
---------------------------------

### 1. Localização e Abrangência

*   Sempre reside em uma única região do OCI
*   Pode se estender por múltiplos domínios de disponibilidade (ADs) na mesma região
*   **Não pode** atravessar diferentes regiões

### 2. Endereçamento IP

#### Requisitos de CIDR

*   Mínimo de 1 bloco CIDR (obrigatório)
*   Máximo de 5 blocos CIDR para IPv4
*   Máximo de 5 blocos CIDR para IPv6
*   Tamanho do bloco: de /16 a /30

#### Exemplo Prático

Vamos considerar um bloco CIDR: 192.168.0.0/24
Divisão dos IPs:
*   192.168.0.0 → Endereço de rede (reservado)
*   192.168.0.1 → Gateway padrão da sub-rede (reservado)
*   192.168.0.255 → Endereço de broadcast (reservado)
*   Restantes: Disponíveis para atribuição

### 3. Flexibilidade

*   Possível redimensionar blocos CIDR após criação
*   Blocos CIDR não podem se sobrepor
*   Suporte para IPv4 e IPv6

Opções de Prefixo IPv6
----------------------

Duas possibilidades:
1.  Prefixo alocado pelo Oracle (/56)
2.  Traga seu próprio prefixo (Bring Your Own IP)

Exemplo Didático Completo
-------------------------

### Cenário: Criando uma VCN para uma Startup de Tecnologia

📍 Região: São Paulo (SA-SAOPAULO-1) 🌐 Bloco CIDR Principal: 10.0.0.0/16 🔢 Sub-redes possíveis:
*   Desenvolvimento: 10.0.1.0/24
*   Produção: 10.0.2.0/24
*   Teste: 10.0.3.0/24

Considerações Importantes
-------------------------

✅ Sempre verifique:
*   Blocos CIDR não sobrepostos
*   Reserva dos primeiros e últimos IPs
*   Limites de endereçamento

Benefícios da VCN
-----------------

1.  Isolamento de Rede
2.  Segurança
3.  Flexibilidade de Configuração
4.  Escalabilidade

Conclusão
---------

A VCN é fundamental para arquitetar redes seguras e eficientes no Oracle Cloud, permitindo um controle granular sobre o ambiente de infraestrutura.

Tabelas de Rota em VCN: Guia Completo
=====================================

Conceito Fundamental
--------------------

Uma tabela de rota é como um "GPS de rede" que determina:
*   Como o tráfego vai fluir dentro da VCN
*   Para onde o tráfego será direcionado

Tipos de Tabelas de Rota
------------------------

### 1. Tabela de Rota Padrão (Default)

*   Criada automaticamente ao criar a VCN
*   Contém uma regra local implícita
*   Permite comunicação entre subredes da mesma VCN
*   Aparece com 0 regras, mas tem rota local "oculta"

### 2. Tabela de Rota Personalizada (Custom)

*   Criada manualmente pelo usuário
*   Permite configurações de roteamento específicas
*   Útil para diferenciar roteamento de subredes públicas e privadas

Estrutura de uma Tabela de Rota
-------------------------------

### Colunas Principais:

1.  Bloco CIDR de Destino
2.  Próximo Salto (Target)

Exemplo de Tabela de Rota

Clique para abrir documento

Toque para abrir

Tipos de Targets Suportados
---------------------------

1.  Gateway de Internet (Internet Gateway)
    *   Para subredes públicas
    *   Acesso direto à internet
2.  Gateway NAT (NAT Gateway)
    *   Para subredes privadas
    *   Permite downloads da internet
    *   Comunicação unidirecional
3.  Gateway de Serviço (Service Gateway)
    *   Acesso privado a serviços Oracle
    *   Exemplo: Acessar Object Storage
4.  Gateway de Roteamento Dinâmico (Dynamic Routing Gateway)
    *   Comunicação com redes locais
5.  Gateway de Peering Local (Local Peering Gateway)
    *   Comunicação entre VCNs na mesma região
6.  IP Privado
    *   Roteamento para instâncias específicas na VCN

| Destino CIDR | Próximo Salto (Target) |
| --- | --- |
| 0.0.0.0/0 | Internet Gateway |
| 10.0.0.0/16 | Local Peering Gateway |
| 100.0.0.0/16 | NAT Gateway |

Regra de Especificidade
-----------------------

🔍 **Regra de Ouro**: A regra mais específica sempre vence!

### Exemplo Prático

Considere dois cenários:
*   Rota para 0.0.0.0/0 (Internet Gateway)
*   Rota para 200.100.0.0/16 (Service Gateway)
Se um pacote corresponder a 200.100.0.0/16, ele seguirá pela rota do Service Gateway, mesmo sendo parte do 0.0.0.0/0.

Restrições Importantes
----------------------

*   Uma subrede pode ter APENAS UMA tabela de rota
*   Uma tabela de rota pode ser associada a MÚLTIPLAS subredes
*   Se não houver correspondência de rota, o tráfego é descartado

Diagrama Conceitual
-------------------
```
VCN
│
├── Subrede Pública
│   └── Rota para Internet Gateway (0.0.0.0/0)
│
├── Subrede Privada
│   ├── Rota para NAT Gateway (para downloads)
│   └── Rota para Service Gateway (serviços Oracle)
│
└── Tabelas de Rota
    ├── Padrão (local)
    └── Personalizada
```

Boas Práticas
-------------

✅ Planeje cuidadosamente suas rotas 

✅ Use gateways específicos para cada necessidade 

✅ Entenda a regra de especificidade 

✅ Separe subredes públicas e privadas

Exemplo Real
------------

### Cenário: Aplicação Web em OCI

1.  Subrede Pública (Frontend)
    *   Target: Internet Gateway
    *   CIDR: 10.0.1.0/24
2.  Subrede Privada (Banco de Dados)
    *   Target: NAT Gateway (downloads)
    *   Target: Service Gateway (Object Storage)
    *   CIDR: 10.0.2.0/24

![image](https://github.com/user-attachments/assets/3d726eed-9319-4dc4-8752-46f84cde365f)

O diagrama ilustra a arquitetura de rede de um centro de dados da Oracle Cloud, destacando a estrutura de sub-redes e suas interações com a Internet e outros serviços.

### Componentes Principais:

1.  **Sub-redes**:
    *   **Subnet Público**: Contém uma Máquina Virtual (IP: 10.0.1.0/24) e se conecta à Internet através de um **Gateway da Internet**.
    *   **Subnet Privada**: Contém um **Sistema de Banco de Dados** (IP: 10.0.0.0/24) e permite a comunicação com os serviços de armazenamento.
2.  **Gateways**:
    *   **Gateway da Internet**: Facilita a comunicação com a Internet.
    *   **NAT Gateway**: Permite que instâncias na sub-rede privada acessem a Internet sem expor seus endereços IP.
    *   **Service Gateway**: Usado para acessar serviços da Oracle Cloud (como armazenamento de objetos) de maneira segura.
3.  **Objetos de Armazenamento**: Integração com o **Object Storage** para armazenamento de dados.
    
4.  **Tabela de Roteamento**:
    *   Destinos e tipos de alvo, indicando que o tráfego destinado a 0.0.0.0/0 é direcionado ao **NAT Gateway**, e o tráfego para o armazenamento de objetos é gerido pelo **Service Gateway**.
Esta arquitetura facilita a comunicação segura e eficiente entre os componentes na nuvem, oferecendo uma maneira estruturada de gerenciar dados e serviços.

Segurança em VCN: Grupos de Segurança de Rede e Listas de Segurança
===================================================================

Introdução
----------

A segurança de rede é fundamental na infraestrutura em nuvem. No Oracle Cloud Infrastructure (OCI), temos duas ferramentas principais para controlar o tráfego de rede:
1.  Grupos de Segurança de Rede (Network Security Groups - NSG)
2.  Listas de Segurança (Security Lists)

1. Grupos de Segurança de Rede (NSG)
------------------------------------

### Conceito Básico

*   Um firewall virtual para definir regras de entrada (ingress) e saída (egress)
*   Associado a Interfaces de Rede Virtuais (VNICs)

### Recursos Suportados

*   Instâncias de Computação
*   Balanceadores de Carga
*   Destinos de Montagem
*   Gateway de API

### Componentes de um NSG

1.  Conjunto de VNICs
2.  Conjunto de Regras de Segurança

### Tipos de Regras

#### Regras de Entrada (Ingress)

*   Definem tráfego permitido para dentro da instância
*   Tipos de Fonte:
    1.  Bloco CIDR
    2.  Serviço
    3.  Outro Grupo de Segurança de Rede

#### Regras de Saída (Egress)

*   Definem tráfego permitido para fora da instância
*   Tipos de Destino:
    1.  Bloco CIDR
    2.  Serviço
    3.  Outro Grupo de Segurança de Rede

### Regras Stateful vs Stateless

#### Stateful

*   Respostas de tráfego são automaticamente rastreadas e permitidas
*   Usa rastreamento de conexão
*   Exemplo: Permite resposta automática para tráfego de entrada na porta 80

#### Stateless

*   Requer definição explícita de regras de saída para permitir respostas
*   Sem rastreamento de conexão
*   Recomendado para sites de alto volume (HTTP/HTTPS)

2. Listas de Segurança
----------------------

### Conceito Básico

*   Coleção de regras de firewall
*   Associada a subredes, não a VNICs individuais

### Características

*   Aplica-se a todas as instâncias em uma subrede
*   Pode conter regras de entrada e saída
*   Aplicada no nível de VNIC
*   Pode ser associada a múltiplas subredes

### Diferenças entre NSG e Lista de Segurança

| Característica | Grupo de Segurança de Rede | Lista de Segurança |
| --- | --- | --- |
| Associação | VNIC | Subrede |
| Tipos de Fonte | CIDR, Serviço, NSG | CIDR, Serviço |

### Princípio de União de Regras

*   Quando NSG e Lista de Segurança são aplicados:
    *   União de todas as regras
    *   Tráfego permitido se houver regra de permissão em qualquer construto

Exemplo Prático
---------------

### Cenário: Aplicação Web Multicamadas

Copiar

```
VCN
│
├── Subrede Web
│   ├── NSG-Web (permite tráfego HTTP/HTTPS)
│   └── Lista de Segurança (regras padrão)
│
└── Subrede Aplicação
    ├── NSG-App (permite comunicação com Web)
    └── Lista de Segurança (restringe acesso externo)
```

Boas Práticas
-------------

1.  Use NSGs para segurança granular de instâncias
2.  Use Listas de Segurança para políticas de subrede
3.  Combine NSGs e Listas de Segurança para defesa em profundidade
4.  Minimize regras, conceda apenas o acesso necessário
5.  Prefira regras stateful quando possível

Conclusão
---------

A segurança de rede no OCI é flexível e poderosa. Grupos de Segurança de Rede e Listas de Segurança oferecem múltiplas camadas de proteção para suas aplicações em nuvem.

![image](https://github.com/user-attachments/assets/6ae881e1-f73a-421e-a254-4275fe66a23b)

![image](https://github.com/user-attachments/assets/18243579-a9ac-4da0-be6a-69509992dd97)

![image](https://github.com/user-attachments/assets/1277b56e-7222-4026-a1e9-18fbecaabf3b)

![image](https://github.com/user-attachments/assets/f71e5943-8dd7-44d9-85df-1de8ed086809)

![image](https://github.com/user-attachments/assets/c54c689a-641d-4d52-9099-787c01df0364)

![image](https://github.com/user-attachments/assets/0f101416-a9c2-4875-bc2c-4ae145f95a22)

![image](https://github.com/user-attachments/assets/5c7a40dd-74e8-4e2e-90a7-846a90d644de)







