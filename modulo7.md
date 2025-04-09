![image](https://github.com/user-attachments/assets/8bec52a2-5264-40c2-b98e-f92fbeb78b7c)

1.  Na Oracle Cloud Infrastructure (OCI) Full Stack Disaster Recovery service, como você chamaria uma coleção de diferentes recursos OCI que compõem uma aplicação e que é tratada como um grupo combinado ao realizar operações de recuperação de desastre?

*   Grupo de Proteção DR ✅
*   Grupo de Plano DR
*   Plano DR
*   Grupo de Segurança DR

**Explicação:** Um Grupo de Proteção DR representa um agrupamento de consistência definido para os propósitos de recuperação de desastres. Por exemplo, um Grupo de Proteção DR pode consistir em servidores de aplicação, armazenamento em bloco associado e bancos de dados.

---------

![image](https://github.com/user-attachments/assets/44fef29e-bced-4a6f-957a-4f9323e41c88)

2.  O que acontece com o bucket de destino após uma política de replicação ser criada em um bucket de Armazenamento de Objetos da Oracle Cloud Infrastructure (OCI)?
*   Ele congela completamente
*   Todos os objetos existentes são deletados
*   Ele se torna gravável e você pode enviar objetos diretamente para esse bucket
*   Ele se torna somente leitura e atualizado apenas por replicação do bucket de origem ✅

**Explicação:** Depois que a política de replicação é criada, o bucket de destino é somente leitura e atualizado apenas por replicação a partir do bucket de origem. Você não pode enviar objetos diretamente para esse bucket.

---------

![image](https://github.com/user-attachments/assets/ed9d57af-8c3b-4db3-b022-3e49e41b783d)

3.  Que tipo de replicação você pode configurar em um serviço de Volume de Bloco da Oracle Cloud Infrastructure (OCI)?
*   Apenas domínio de disponibilidade Cross
*   Apenas região Cross
*   Ambos, região Cross e domínio de disponibilidade Cross ✅
*   Você não pode replicar um Volume de Bloco para uma região diferente

**Explicação:** Existem dois tipos de replicação no serviço de Volume de Bloco: replicação entre regiões (Cross region) para replicação entre regiões e replicação entre domínios de disponibilidade (Cross availability domain) para replicação entre domínios de disponibilidade dentro da mesma região.

---------

![image](https://github.com/user-attachments/assets/c1d16ce0-7e26-42a0-a485-a7d4a3770c91)

4.  Qual recurso na Oracle Cloud Infrastructure (OCI) Armazenamento de Objetos você usaria para copiar objetos para outros buckets na mesma região e para buckets em outras regiões?
*   Duplicar Objeto
*   Copiar Objeto ✅
*   Replicar Objeto
*   Mover Objeto

**Explicação:** Você pode copiar objetos para outros buckets na mesma região e para buckets em outras regiões usando a funcionalidade Copy Object. Para isso, você deve autorizar o serviço de Armazenamento de Objetos para copiar objetos em seu nome.

---------

![image](https://github.com/user-attachments/assets/d199052e-99b2-497e-af4b-e002d314dda2)

5.  Na Oracle Cloud Infrastructure (OCI) Full Stack Disaster Recovery service, onde podemos criar uma execução de Plano DR?
*   Grupo de Proteção DR em espera ✅
*   Grupo de Proteção DR primário
*   Ambos, Grupo de Proteção DR primário e em espera
*   Nem Grupo de Proteção DR primário nem em espera

**Explicação:** Uma Execução de Plano DR representa uma instância em execução de um Plano DR. Ela pode ser criada ou iniciada apenas em um Grupo de Proteção DR em espera.

---------
