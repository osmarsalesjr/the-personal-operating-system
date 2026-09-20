# Meu POS — Sistema Operacional Pessoal

## 1. Visão geral

O **Meu POS (Sistema Operacional Pessoal)** é um sistema de organização pessoal desenvolvido para centralizar tarefas, projetos, rotinas e informações de contexto e, a partir desses dados, gerar uma análise assistida por Inteligência Artificial.

A solução atualmente utiliza:

- **Trello** — organização das tarefas, projetos, estados e configuração do usuário;
- **Google Calendar** — consulta de compromissos dentro do horizonte de planejamento;
- **Make.com** — integração, filtragem, agregação e orquestração dos dados;
- **Gemini 2.5 Flash** — análise do contexto e geração de fatos, riscos, recomendações e observações;
- **Gmail** — entrega da análise ao usuário por e-mail.

A arquitetura foi inicialmente pensada como um template reutilizável. A rotina utilizada no desenvolvimento representa uma **configuração de referência**, e não uma regra fixa da automação.

### Princípio central

> **O template é fixo. A configuração é individual. A análise é adaptativa.**

E, na divisão de responsabilidades:

> **Automação executa regras; IA analisa e recomenda; usuário decide.**

A solução atual já implementa a coleta de dados do Trello e do Google Calendar, a montagem de um contexto estruturado, a análise com Gemini e o envio do resultado por Gmail. Funcionalidades adicionais previstas na concepção inicial permanecem como evolução futura e não devem ser tratadas como funcionalidades já implementadas.

---

# 2. Objetivos e princípios da solução

O POS foi concebido para reduzir o esforço necessário para compreender a própria rotina e identificar o que merece atenção.

A solução procura:

- centralizar tarefas pessoais em um fluxo simples;
- preservar a separação entre tarefas e compromissos;
- utilizar a rotina do usuário como configuração;
- evitar regras dependentes da rotina utilizada no desenvolvimento;
- reduzir decisões manuais repetitivas;
- utilizar IA para interpretação, e não como agente autônomo;
- manter o usuário como responsável pelas decisões;
- permitir evolução futura sem reescrever a lógica principal.

### Princípio de parametrização

As informações específicas do usuário devem estar nos dados de configuração, e não codificadas diretamente no cenário.

A automação não deve depender, por exemplo, de uma regra como:

```text
Segunda-feira às 19h = estudar
```

como regra fixa do sistema.

O horário da rotina de referência pode existir nos dados do usuário, mas a lógica do cenário deve permanecer genérica.

---

# 3. Rotina de referência utilizada no desenvolvimento

A rotina real utilizada durante o desenvolvimento serviu como caso de teste para validar o modelo.

## 3.1 Trabalho

O trabalho ocorre remotamente de segunda a sexta-feira:

```text
09:00–12:00
14:00–18:00
```

As tarefas profissionais são gerenciadas pelo sistema utilizado pela equipe de trabalho e, por isso, não são duplicadas no POS.

No contexto do sistema, esses períodos fazem parte da realidade do usuário, mas o POS não administra as tarefas internas do trabalho.

## 3.2 Estudos

O curso possui aulas ao vivo:

```text
Terça-feira: 19:00–21:00
Quinta-feira: 19:00–21:00
```

Também existem períodos destinados a estudos às:

```text
Segunda-feira: 19:00–22:00
Quarta-feira: 19:00–22:00
Sexta-feira: 19:00–22:00
```

Além das aulas e estudos recorrentes, existem projetos acadêmicos com prazo de entrega.

O projeto mensal da disciplina possui, como exemplo real utilizado no sistema, estimativa total de **8 horas** e deadline definido.

## 3.3 Academia

A academia ocorre:

```text
Segunda a sexta-feira: 12:00–14:00
Sábado: a partir das 10:30
```

Esse período é considerado parte da rotina e não deve ser automaticamente interpretado como disponibilidade para tarefas.

## 3.4 Financeiro

Existem obrigações recorrentes ao longo do mês, como:

- faturas de cartão;
- água;
- telefone;
- internet;
- mensalidade do curso.

No caso utilizado durante o desenvolvimento, os vencimentos recorrentes incluem dias 1, 4, 5, 7, 10 e 15, conforme a obrigação.

Essas atividades são representadas como cartões de rotina no Trello.

## 3.5 Vida pessoal

A configuração de referência também considera:

```text
Sexta-feira após 21:00
Sábado
Domingo
```

como períodos destinados a descanso, vida social e atividades pessoais.

A existência desses períodos reforça que o objetivo do sistema não é preencher todo o tempo disponível com tarefas.

---

# 4. Modelo conceitual

O POS diferencia principalmente quatro conceitos.

## 4.1 Tarefa

É uma atividade que precisa ser realizada.

Exemplo:

```text
Revisar anotações da disciplina
```

A tarefa pertence ao Trello.

## 4.2 Projeto

É uma demanda maior que pode envolver múltiplas etapas.

Exemplo:

```text
Projeto de disciplina — Setembro
```

Projetos podem utilizar checklist, estimativa, prioridade e deadline.

## 4.3 Rotina

Representa uma atividade ou obrigação recorrente.

Exemplo:

```text
Pagar telefonia
```

A recorrência é representada atualmente por cartões e respectivas datas no Trello. A geração automática de ocorrências recorrentes ainda não faz parte do cenário implementado.

## 4.4 Compromisso

É um evento que ocupa um horário específico.

Exemplo:

```text
Aula ao vivo
```

Os compromissos são obtidos do Google Calendar.

### Separação fundamental

```text
Trello
"O que preciso fazer?"

Calendar
"O que tenho marcado?"

Configuração
"Como minha rotina normalmente funciona?"

IA
"O que os dados indicam e o que merece atenção?"
```

Essa separação evita transformar todo compromisso em tarefa ou toda tarefa em evento de calendário.

---

# 5. Estrutura atual do Trello

O board utilizado é:

> **🧠 Meu POS — Sistema Operacional Pessoal**

As listas atuais são:

```text
⚙️ Configuração
📥 Inbox
📚 Backlog
📅 Esta Semana
🔨 Em Andamento
🏁 Concluído
```

As listas representam principalmente o **estado operacional** dos cartões.

Datas, labels e descrições representam informações adicionais para organização e análise.

---

## 5.1 Configuração

A lista de configuração contém os cartões que descrevem a utilização do sistema:

- 👤 Meu Perfil;
- 🕐 Minha Rotina;
- 🗂️ Minhas Áreas;
- ⚙️ Minhas Preferências;
- 📖 Como Usar o POS.

Os quatro primeiros são utilizados atualmente pelo cenário como configuração operacional.

O cartão **📖 Como Usar o POS** funciona principalmente como documentação e não está agregado ao contexto operacional atual.

---

## 5.2 Inbox

A Inbox é o ponto de entrada para novas demandas.

Exemplos de demandas que podem chegar à Inbox:

- Agendar revisão médica anual;
- Revisar anotações da aula;
- Comprar material;
- Responder e-mail;
- Pesquisar um livro.

A Inbox existe como parte da estrutura do POS, mas o cenário atual **não realiza ainda um processamento automático da Inbox**.

A automação futura poderá utilizar IA para auxiliar na classificação dessas demandas, mas isso não deve ser confundido com o fluxo atualmente implementado.

---

## 5.3 Backlog

O Backlog contém demandas que ainda não estão no fluxo de execução da semana.

Também existem no Backlog cartões utilizados como modelos:

```text
📌 [MODELO] Tarefa
📌 [MODELO] Projeto
📌 [MODELO] Rotina
```

Esses cartões são explicitamente ignorados pelo cenário de análise.

---

## 5.4 Esta Semana

A lista representa o conjunto de tarefas selecionadas para o planejamento semanal.

Ela não representa necessariamente o dia atual.

A arquitetura mantém a distinção:

```text
Lista = estado
Data = deadline
```

A automação atual coleta os cartões dessa lista para análise, mas ainda não movimenta cartões automaticamente para ela com base em recomendações da IA.

---

## 5.5 Em Andamento

Representa tarefas ou projetos em execução.

Um exemplo utilizado no desenvolvimento é:

```text
Projeto de disciplina — Setembro
```

Outro exemplo é:

```text
Atualizar documentação técnica do projeto X
```

A análise pode considerar esses cartões, seus metadados e seus deadlines.

---

## 5.6 Concluído

Representa tarefas finalizadas.

Os cartões concluídos podem futuramente ser utilizados para histórico e métricas. Entretanto, o cenário atual de análise coleta somente os cartões das listas:

```text
Inbox
Backlog
Esta Semana
Em Andamento
```

Portanto, o histórico da lista Concluído ainda não participa do `POS_CONTEXT` atual.

---

# 6. Metadados atuais dos cartões

Como o plano utilizado do Trello não oferece os recursos avançados inicialmente previstos para custom fields, a solução utiliza principalmente:

- labels;
- descrição;
- nome;
- data nativa do cartão;
- lista.

As informações conceitualmente relevantes são:

| Informação | Utilização atual |
|---|---|
| Área | Labels e/ou descrição |
| Prioridade | Labels |
| Tipo | Descrição |
| Estimativa | Descrição |
| Deadline | Data nativa do Trello |
| Estado | Lista |
| Observações | Descrição |

### Tipos utilizados nos cartões

A estrutura atual utiliza principalmente:

- Tarefa;
- Projeto;
- Rotina.

O tipo **Aguardando**, previsto na concepção inicial, ainda não constitui um fluxo operacional implementado no cenário atual.

### Deadline

A data do cartão é tratada como **deadline**, e não como horário de execução.

O cenário de análise considera somente a parte de data:

```text
YYYY-MM-DD
```

O timestamp original e seu fuso horário não são utilizados para decidir se uma tarefa está vencida ou possui deadline futuro.

A regra atual é:

```text
due < data atual
→ atrasada

due >= data atual
→ deadline

due = null
→ sem prazo informado
```

---

# 7. Labels e áreas

As labels atualmente utilizadas representam principalmente:

### Áreas

- 💼 Trabalho;
- 🎓 Estudos;
- 🏠 Pessoal;
- 💰 Financeiro;
- 🏃 Saúde.

### Prioridades

- 🔴 Alta;
- 🟡 Média;
- 🟢 Baixa.

Essas categorias fazem parte da configuração utilizada atualmente.

A arquitetura, porém, foi concebida para evitar que a lógica de análise dependa semanticamente de uma quantidade fixa de áreas.

---

# 8. Cartões-modelo

O board possui cartões-modelo para orientar a criação manual de novos cartões:

```text
📌 [MODELO] Tarefa
📌 [MODELO] Projeto
📌 [MODELO] Rotina
```

O cenário de análise possui filtros que ignoram cartões cujo nome contém o marcador de modelo.

Isso evita que os exemplos utilizados para documentação sejam interpretados como tarefas reais.

---

# 9. Arquitetura atualmente implementada

O fluxo efetivamente implementado no Make é:

```text
Trello
  ↓
Coleta de listas
  ↓
Coleta de cartões
  ↓
Separação entre configuração e tarefas
  ↓
Agregação
  ↓
Google Calendar
  ↓
POS_CONTEXT
  ↓
Gemini 2.5 Flash
  ↓
Parse do JSON
  ↓
Preparação da análise
  ↓
Montagem da mensagem
  ↓
Gmail
```

O cenário atualmente implementado é denominado:

> **MyPosRoutineAnalysis**

---

# 10. Cenário MyPosRoutineAnalysis

A estrutura atual utiliza os seguintes módulos principais.

## 10.1 Coleta das listas do Trello

O módulo inicial consulta as listas do board.

São utilizadas as listas:

```text
⚙️ Configuração
📥 Inbox
📚 Backlog
📅 Esta Semana
🔨 Em Andamento
🏁 Concluído
```

Os IDs são utilizados pelo cenário para identificar em qual estado cada cartão se encontra.

## 10.2 Agregação das listas

O cenário agrega:

- ID da lista;
- nome da lista.

Isso permite utilizar posteriormente os IDs para filtrar os cartões.

## 10.3 Coleta dos cartões

O cenário consulta os cartões do board utilizando:

```text
id
name
desc
due
idList
labels
```

Essas informações são suficientes para a análise atual.

## 10.4 Separação da configuração

Os cartões:

- 👤 Meu Perfil;
- 🕐 Minha Rotina;
- 🗂️ Minhas Áreas;
- ⚙️ Minhas Preferências;

são separados dos demais cartões.

O resultado é armazenado como `config`.

## 10.5 Separação das tarefas

O cenário seleciona cartões pertencentes a:

- Inbox;
- Backlog;
- Esta Semana;
- Em Andamento.

Os cartões-modelo são excluídos.

O resultado é armazenado como `tasks`.

## 10.6 Agregação e normalização

Os dados dos cartões são agregados antes da construção do contexto.

Essa etapa organiza os atributos:

```text
id
name
desc
due
idList
labels
```

A normalização atual é estrutural.

O módulo de agregação não realiza parsing semântico da descrição ou das labels.

Isso é importante porque informações como área, tipo, prioridade e estimativa ainda são interpretadas pela IA a partir do contexto disponível.

---

# 11. Google Calendar

O cenário consulta o Google Calendar para complementar a visão das tarefas.

A consulta atual utiliza:

```text
Time Min: data atual
Time Max: data atual + 7 dias
Single Events: true
Order By: startTime
Limit: 100
```

O calendário utilizado durante o desenvolvimento é o calendário principal da conta conectada.

Os eventos retornados são agregados com informações como:

- ID;
- início;
- fim;
- status;
- título;
- link;
- localização;
- tipo;
- descrição.

### Função do Calendar

O Calendar não é utilizado para criar tarefas.

Ele fornece contexto sobre compromissos já existentes.

Exemplo:

```text
Aula Ao vivo
```

é um compromisso de calendário e não precisa ser duplicado como cartão do Trello.

---

# 12. POS_CONTEXT

Depois da coleta e agregação, o Make constrói um objeto JSON denominado:

> `POS_CONTEXT_v2`

Sua estrutura conceitual é:

```text
POS_CONTEXT
├── config
├── tasks
└── calendar
```

### Config

Contém informações dos cartões de configuração:

```text
id
name
desc
due
idList
labels
```

### Tasks

O contexto possui campos destinados a representar:

```text
id
name
desc
due
idList
labels
type
area
priority
estimate
```

Entretanto, na implementação atual, os campos semânticos:

```text
type
area
priority
estimate
```

não são preenchidos por uma etapa de transformação determinística no Make.

O cenário mantém os dados originais de `desc` e `labels`, e o Gemini interpreta essas informações.

Essa distinção é importante para não atribuir ao módulo de normalização uma capacidade que ele atualmente não possui.

### Calendar

Contém:

```text
id
end
start
status
summary
htmlLink
location
eventType
description
```

---

# 13. Uso da Inteligência Artificial

A IA é utilizada depois que os dados foram coletados e organizados.

O fluxo é:

```text
Trello
   +
Google Calendar
   +
Configuração
        ↓
   POS_CONTEXT
        ↓
Gemini 2.5 Flash
        ↓
Análise estruturada
```

O modelo utilizado atualmente é:

> **Gemini 2.5 Flash**

A IA recebe exclusivamente o contexto produzido pelo cenário.

---

# 14. Regras atuais da análise da IA

O prompt do Gemini estabelece que a IA deve analisar somente o `POS_CONTEXT`.

A análise considera:

- tarefas;
- prioridades;
- áreas;
- tipos;
- estimativas disponíveis;
- deadlines;
- estado dos cartões;
- configuração;
- compromissos do Calendar.

## 14.1 Data atual

A data atual é fornecida pelo Make utilizando o fuso:

```text
America/Sao_Paulo
```

A análise de deadlines utiliza a data, e não o timestamp completo do Trello.

## 14.2 Fatos

Os fatos devem ser objetivos e identificar explicitamente a tarefa.

Exemplo:

```text
Enviar documentação importante possui prioridade Alta e deadline em 2026-09-04.
```

A estrutura limita a quantidade de fatos a no máximo 10.

## 14.3 Riscos

Os riscos identificam situações que merecem atenção.

Cada risco deve:

- identificar claramente a tarefa;
- possuir uma justificativa;
- evitar simplesmente repetir um fato;
- não inventar consequências.

A quantidade é limitada a no máximo 5 riscos.

## 14.4 Recomendações

As recomendações possuem a estrutura:

```text
task
action
```

A tarefa deve utilizar exatamente o nome existente nos dados.

As ações permitidas são:

```text
Priorizar.
Resolver.
Revisar.
Acompanhar.
Antecipar.
```

A recomendação não define horário, duração ou agenda.

## 14.5 Planning notes

As `planning_notes` são observações objetivas relacionadas ao planejamento.

Elas não representam necessariamente uma ação.

---

# 15. Limites da IA

Uma decisão importante do projeto é impedir que a IA complete lacunas com informações inventadas.

A IA não deve:

- inventar deadlines;
- inventar estimativas;
- inventar prioridades;
- inventar compromissos;
- escolher horários de execução;
- criar eventos;
- executar tarefas;
- inventar progresso;
- inventar dependências;
- inventar etapas de projetos;
- diagnosticar procrastinação;
- afirmar consequências que não estejam sustentadas pelos dados.

O princípio é:

> **Se o dado não existe, a IA não deve criá-lo.**

Isso mantém a análise rastreável aos dados disponíveis.

---

# 16. Parse e preparação do resultado

O retorno do Gemini é solicitado em JSON.

O módulo de parsing transforma o resultado em uma estrutura com:

```text
summary
facts
risks
recommendations
planning_notes
```

O módulo seguinte prepara essas estruturas para utilização na mensagem final.

Atualmente, essa etapa está funcionando como esperado e recebe os arrays e textos retornados pelo parser.

---

# 17. Montagem da mensagem

O módulo **Assemble Analysis Message** reúne o resultado da análise.

A estrutura conceitual da mensagem é:

```text
🧠 Análise do seu POS

Resumo

⚠️ Atenção
- riscos

💡 Recomendações
- recomendações

📌 Pontos observados
- fatos

📅 Observações de planejamento
- planning notes
```

As seções são condicionais: somente aparecem quando existe conteúdo correspondente.

O módulo atualmente utiliza uma variável denominada:

```text
message
```

e funciona em um ciclo (`roundtrip`).

### Formato para e-mail

O Gmail está configurado para receber:

> **Raw HTML**

Por isso, a mensagem final deve ser construída em HTML real, utilizando elementos como:

```html
<h2>
<h3>
<p>
<ul>
<li>
```

A estrutura lógica da mensagem já está definida; a adequação final do conteúdo do módulo para HTML é uma etapa de formatação, e não uma mudança na arquitetura da análise.

---

# 18. Gmail

O Gmail é utilizado como canal de entrega da análise.

O cenário utiliza uma chamada à API do Gmail para obter o endereço da conta autenticada:

```text
GET /v1/users/me/profile
```

O retorno fornece:

```text
emailAddress
```

Esse endereço é utilizado como destinatário do relatório.

O envio é realizado pelo módulo:

> **Gmail — Send an Email**

Configuração atual:

```text
To:
emailAddress da conta autenticada

Subject:
🧠 Análise do seu POS

Body:
mensagem produzida pelo módulo Assemble Analysis Message

Body Type:
Raw HTML
```

Não são utilizados atualmente:

- CC;
- BCC;
- anexos;
- outros módulos de entrega.

---

# 19. Estratégia de planejamento

O modelo conceitual continua baseado na separação:

> **Listas representam estado; datas representam deadline; Calendar representa compromissos.**

Por isso, não existe uma lista física chamada:

```text
Hoje
```

A lista:

```text
📅 Esta Semana
```

representa planejamento semanal.

A identificação de tarefas prioritárias para um determinado período pode ser feita futuramente por análise dos deadlines, prioridades, estado e calendário.

O cenário atual ainda não movimenta cartões automaticamente com base nessas análises.

---

# 20. Métodos de produtividade utilizados

A solução incorpora princípios de diferentes métodos, mas não transforma nenhum deles em uma regra rígida do cenário.

## GTD

A Inbox funciona como ponto de captura de demandas.

```text
Capturar
   ↓
Organizar
   ↓
Planejar
   ↓
Executar
   ↓
Concluir
```

O processamento automático da Inbox é uma evolução prevista, não uma funcionalidade já implementada.

## Eisenhower

A prioridade é representada por:

```text
Alta
Média
Baixa
```

A prioridade é um dado do cartão e pode ser utilizada pela IA durante a análise.

## Pomodoro

Pomodoro é tratado como técnica de execução, principalmente para estudos e projetos.

O POS não controla atualmente os ciclos Pomodoro.

## Checklists

Projetos podem utilizar checklist para representar suas etapas.

O projeto mensal da disciplina é o principal exemplo utilizado na configuração real.

---

# 21. Tratamento da procrastinação

A solução procura tratar procrastinação de maneira operacional, sem realizar diagnóstico psicológico.

Os sinais que podem ser observados nos dados incluem:

- proximidade do deadline;
- estado do projeto;
- existência ou ausência de estimativa;
- existência de checklist;
- quantidade de tarefas acumuladas;
- distribuição temporal das atividades, quando houver histórico suficiente.

No cenário atualmente implementado, a IA analisa principalmente os dados presentes no contexto atual.

A análise histórica e a identificação automática de padrões de procrastinação ainda são possibilidades de evolução e não devem ser descritas como funcionalidades atuais.

A estratégia atual consiste em tornar as demandas mais visíveis e estruturadas por:

- Inbox;
- prioridade;
- deadline;
- estimativa;
- estado;
- checklist;
- análise assistida.

---

# 22. Comunicação e bem-estar

Demandas de comunicação podem ser registradas como tarefas concretas.

Exemplo:

```text
Responder email do cliente - orçamento Q3
```

Isso permite acompanhar uma atividade de comunicação dentro do mesmo fluxo das demais tarefas.

A solução também preserva períodos pessoais na configuração de referência.

O objetivo não é maximizar a ocupação do usuário.

A arquitetura procura considerar conjuntamente:

- trabalho;
- estudos;
- atividade física;
- compromissos;
- tarefas pessoais;
- descanso;
- vida social.

A IA não decide automaticamente quanto o usuário deve trabalhar ou estudar.

---

# 23. O que está implementado atualmente

A situação atual do projeto pode ser resumida da seguinte forma.

### Implementado

- Board Trello;
- listas operacionais;
- cartões de configuração;
- cartões-modelo;
- tarefas reais de teste;
- labels de áreas e prioridades;
- descrições padronizadas;
- consulta de listas do Trello;
- consulta de cartões do Trello;
- filtragem dos cartões-modelo;
- separação entre configuração e tarefas;
- consulta do Google Calendar;
- agregação dos eventos;
- construção do `POS_CONTEXT_v2`;
- análise com Gemini 2.5 Flash;
- retorno estruturado em JSON;
- parsing da resposta;
- preparação da análise;
- montagem da mensagem;
- obtenção do e-mail pela API do Gmail;
- envio da análise por Gmail.

### Parcial / em ajuste

- Formatação final da mensagem como HTML para o Gmail.

A arquitetura de envio já está configurada como `Raw HTML`, mas a variável montada no módulo de mensagem precisa utilizar HTML real para que títulos, parágrafos e listas sejam renderizados corretamente.

### Ainda não implementado como automação

As seguintes ideias fazem parte da concepção/evolução do POS, mas não estão no cenário atual:

- processamento automático da Inbox;
- classificação automática de novos cartões;
- decomposição automática de projetos em checklists;
- planejamento semanal automático;
- escolha automática de horários;
- criação automática de eventos no Calendar;
- geração automática de ocorrências de rotinas;
- movimentação automática de cartões com base em recomendações;
- análise histórica de procrastinação;
- revisão semanal baseada no histórico completo;
- métricas consolidadas;
- dashboard;
- banco de dados externo;
- aplicação própria;
- SaaS multiusuário.

Essa distinção mantém a documentação coerente com o estado real do projeto.

---

# 24. Arquitetura de responsabilidades

A divisão de responsabilidades atual é:

```text
┌───────────────────────────────┐
│            TRELLO             │
│                               │
│ Tarefas                       │
│ Projetos                      │
│ Estados                       │
│ Configuração                  │
│ Prioridades                   │
│ Deadlines                     │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│             MAKE              │
│                               │
│ Coleta                        │
│ Filtros                       │
│ Agregação                     │
│ Integração                    │
│ Montagem do contexto          │
│ Orquestração                  │
└───────┬─────────────────┬─────┘
        │                 │
        ▼                 ▼
┌───────────────┐   ┌────────────────┐
│ Google        │   │ Gemini         │
│ Calendar      │   │ 2.5 Flash      │
│               │   │                │
│ Compromissos  │   │ Análise        │
│ Eventos       │   │ Riscos         │
│ Horários      │   │ Recomendações  │
└───────────────┘   └───────┬────────┘
                            │
                            ▼
                    ┌────────────────┐
                    │     Gmail      │
                    │                │
                    │ Entrega        │
                    └────────────────┘
                            │
                            ▼
                         USUÁRIO
```

O usuário permanece fora da automação como responsável pela decisão final.

---

# 25. Princípios arquiteturais

## Simplicidade

A solução deve utilizar somente a quantidade de estrutura necessária para resolver o problema.

## Parametrização

Informações pessoais devem ser configuráveis.

## Reutilização

A mesma estrutura deve poder ser utilizada com diferentes configurações.

## Separação de responsabilidades

Cada ferramenta possui uma função clara:

```text
Trello → tarefas e configuração
Calendar → compromissos
Make → integração e regras
Gemini → análise
Gmail → comunicação
Usuário → decisão
```

## Automação consciente

Regras determinísticas devem ser implementadas como automação tradicional.

A IA deve ser utilizada quando existe necessidade de interpretação.

## Controle humano

A IA não deve executar decisões pessoais de maneira autônoma.

## Rastreabilidade

As conclusões da IA devem ser fundamentadas nos dados recebidos.

## Independência da rotina de referência

A rotina utilizada no desenvolvimento é uma configuração real de teste, não uma regra de negócio.

---

# 26. Evoluções previstas

A arquitetura inicial prevê uma evolução gradual.

Entre as possibilidades futuras estão:

1. processamento assistido da Inbox;
2. classificação de tarefas;
3. decomposição de projetos;
4. análise de capacidade semanal;
5. identificação de conflitos;
6. análise histórica;
7. revisão semanal;
8. métricas;
9. dashboard;
10. geração de recorrências;
11. maior integração com Calendar.

Essas evoluções devem preservar os mesmos princípios:

```text
Dados do usuário
       ↓
Regras genéricas
       ↓
Análise
       ↓
Recomendação
       ↓
Decisão do usuário
```

A evolução não deve transformar dados específicos da rotina atual em regras fixas no código.

---

# 27. Visão final

O Meu POS foi estruturado para funcionar como uma camada de organização e análise sobre ferramentas já utilizadas no cotidiano.

A lógica central é:

```text
                  CONFIGURAÇÃO
                       │
                       ▼
             ┌─────────────────┐
             │     TRELLO      │
             │                 │
             │    Tarefas      │
             │    Projetos     │
             │    Estados      │
             └────────┬────────┘
                      │
                      ▼
                ┌───────────┐
                │   MAKE    │
                │           │
                │ Integração│
                │ Regras    │
                └─────┬─────┘
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
     CALENDAR                  CONTEXTO
          │                       │
          │                       ▼
          │                 ┌───────────┐
          └────────────────►│  GEMINI   │
                            │           │
                            │ Análise   │
                            │ Riscos    │
                            │ Recomenda.│
                            └─────┬─────┘
                                  │
                                  ▼
                               GMAIL
                                  │
                                  ▼
                               USUÁRIO
```

O objetivo não é criar uma rotina universal ou substituir a capacidade de decisão do usuário.

O objetivo é criar uma estrutura na qual:

> **o template seja fixo, a configuração seja individual e a análise seja adaptativa.**

A solução utiliza automação para reduzir trabalho repetitivo, IA para interpretar informações e o usuário como responsável pelas decisões sobre sua própria rotina.
