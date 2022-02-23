# Trabalho Prático/PBL (30%)

# *Project Briefing* - Trabalho Prático em Project Base Learning (PBL) para as Unidades Curriculares de Administração e Gestão de Informação, Programação Web, Cibersegurança e Redes e Comunicação de Dados

# Sistema de Autenticação de Dois Factores 

## Implementação de todas as componentes, redundantes e e de forma segura, de um Sistema de Autenticação de Dois Factores

### Descrição genérica do trabalho:

Pretende-se a implementação de um sistema de autenticação de dois fatores suportado num ambiente multi-máquina com comunicações seguras, executado em máquinas virtuais ou ambiente de Cloud.
O objetivo é aumentar a segurança da autenticação de aplicações, para tal deve associar o sistema a uma "aplicação exemplo" já existente ou a desenvolver. Essa aplicação deve ser identificada na proposta de projeto.
O sistema de dois fatores a utilizar deve ser proposto pelo grupo (app mobile, endereço de email registado, etc) e sempre baseado em TOTP (Time-based One-Time Password). 

Aplicação exemplo, especificar e implementar uma solução com um modelo de negócio análogo ao da UBER, ou seja, uma solução onde o utilizador autenticado (i.e., cliente registrado, com um perfil atribuído) pode solicitar a reserva de um serviço. No caso do exemplo da UBER o serviço solicitado pelo cliente  tem a ver com o serviço de transporte. Usando como referencial o modelo de negócio da UBER, o sistema irá atribuir prioridade ao serviço solicitado com base no perfil e ranking do cliente. Quando o sistema regista um pedido de um novo serviço, o mesmo é encaminhado para os condutores com um perfil & veículo compatível com o perfil e ranking do cliente que solicita o serviço. 
  
- Perfil Cliente (consumidor do serviço): ocasional (O), frequente (F), executivo (E)
  - O perfil do cliente é definido de uma de duas formas possíveis:
    - Ao registar-se, o cliente selecciona a opção de pagar uma fee para ficar com:
      - Classificação de F, ou 
      - Paga 2x o valor da fee F para ficar com a classificação  de E
      - Valor pago trimestralmente
      - Um cliente registrado pode em qualquer momento alterar o seu perfil e optar por pagar a fee trimestral. Neste cenário, o seu perfil é automaticamente atualizado em conformidade com a opção escolhida (i.e., F ou E). Se deixar de pagar a fee trimestral, então no final do trimestre aplicam-se as regras definidas para um utilizador padrão.
    - Ao registar-se, o cliente não  selecciona a opção de pagar uma fee (i.e., utilizador padrão); neste caso a classificação é determinada pelas seguintes regras:
      - Cliente quando se regista fica automaticamente com a classificação base (O)
      - Se num período de 90 dias tiver realizado > 45 viagens é feito um upgrade do perfil para (F)
      - Se num período de 90 dias tiver realizado > 75 viagens é feito um upgrade do perfil para (E)
      - 90 dias = 1 Trim, ou seja a classificação do cliente é revista trimestralmente, podendo haver um downgrade do perfil E -> F ou de F -> O caso o nº de viagens não seja atingido
      - O cliente deve ter uma classificação (Ranking) decorrente da avaliação realizada pelos condutores que prestam o serviço (classificação 0 - 5)
    - Este ranking é definido pela média (avg) das avaliações efetuadas pelos condutores
  - Cliente tem de ter atributos encriptados (e.g., password de acesso, email, etc.)
- Perfil Veículos, também dispõem de três perfis: padrão (vP), executivo (vE), luxo (vL)
  - Esta classificação é atribuída com base em critérios definidos com 
  - O veículo deve ter um conjunto de atributos que o caracterizam de forma a haver critérios de seleção/filtros face aos requisitos emanados do serviço solicitado.
- Perfil Condutor
  - O Condutor deve ter uma classificação (Ranking) decorrente da avaliação realizada pelos clientes que usufruíram do serviço de transporte prestado pelo condutor (classificação 0 - 5)
    - Este ranking é definido pela média (avg) das avaliações efetuadas pelos clientes

**Observações**: modelos de negócio análogos ao exemplo da UBER apresentado como referencial:
- Airbnb: https://www.airbnb.com/ 
- eDreams: https://www.edreams.pt/ 
- Booking: https://www.booking.com/ 

Tendo por base os requisitos elencados acima, ou seja, especificação de uma solução web-based que atua como intermediária entre a procura e a oferta de serviços, existe flexibilidade para criatividade na apresentação de propostas de modelos de negócio análogos. Todavia, as propostas apresentadas têm de cumprir com as regras elencadas no exemplo de referência.



### Objetivos a concretizar:

- No final da unidade curricular, o aluno deverá ser capaz de analisar, planear, e implementar um projeto multidisciplinar
- Trabalhar em grupo para atingir o objetivo final a que o grupo se propõe
- Compreender as diferentes tecnologias e etapas envolvidas no desenvolvimento de um projeto
- Utilizar uma metodologia de desenvolvimento ágil, com várias apresentações de funcionalidades e possibilidade de acomodar alterações durante a execução do projeto;

### Requisitos

Os alunos devem:
- Utilizar o GitHub para gestão de versões e documentação em que a página de descrição do projeto contém a descrição e links para demais documentação do projeto;
- Base de dados cifrada
- Comunicação segura entre todas as componentes
- Utilizar o Discord do Projeto para comunicação com Docentes e membros do grupo.

### Contributos das Unidades Curriculares

#### Administração e Gestão de Informação 
*(a preencher pelo Docente da UC, incluindo necessidade de BDs cifradas)*

#### Programação Web
*(a preencher pelo Docente da UC, incluindo implementação prática do sistema e app exemplo)*

#### Cibersegurança
- Comunicação segura
- Cibersegurança de aplicações web

#### Redes e Comunicação de Dados
- Configuração da Arquitetura de disponibilização do serviço
- Disponibilização de um sistema multi-máquina com correta interligação e filtragem de pacotes entre servidores
- Ajuda na implementação da comunicação segura (em conjunto com CiberSegurança)

### Fases do Projeto
- Análise e Planeamento;
- Prototipagem;
- Implementação;
- Testes;
- Apresentação.

**Entregáveis:**
- Constituição do Grupo: **25 de fevereiro**
- Entrega da proposta de trabalho: **4 de março** - **ATENÇÃO: ver [Documentação](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/TrabsP/Documentacao.md) para o formato da proposta.**
- Entrega de vídeo com Pitch de 3 min com apresentação da proposta **4 de março**
- Apresentação da proposta de trabalho: **semana de 7 a 11 de março**
- Apresentação intermédia que deve incluir apresentação do protótipo **1 de abril**
- Relatório final do projecto até **13 de maio**, de acordo **a [Documentação](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/TrabsP/Documentacao.md)**
- Apresentação Final: **ultima semana de aulas**


