# Meu Sistema Operacional Pessoal --- Análise e Discussão da Solução

## 1. Diagnóstico da rotina atual

A rotina combina trabalho remoto, estudos, atividade física, obrigações
financeiras, compromissos de calendário e demandas pessoais que podem
surgir de forma não planejada.

### Trabalho

O trabalho ocorre de segunda a sexta-feira, das 09:00 às 12:00 e das
14:00 às 18:00. As tarefas profissionais são administradas pelo sistema
utilizado pela equipe e, por isso, não são duplicadas no POS. Esses
períodos são considerados como parte ocupada da rotina.

### Estudos e curso

As aulas ao vivo ocorrem às terças e quintas-feiras, das 19:00 às 21:00.
Há também períodos de estudo às segundas, quartas e sextas-feiras,
normalmente das 19:00 às 21:00, podendo chegar às 22:00.

Além das atividades recorrentes, existem projetos acadêmicos com prazos
definidos. O projeto mensal da disciplina é um exemplo de atividade que
exige várias horas de dedicação e precisa ser acompanhado ao longo do
ciclo.

### Atividade física

A academia ocorre de segunda a sexta-feira, das 12:00 às 14:00, e aos
sábados a partir das 10:30. Esse período é considerado parte da rotina e
não simplesmente tempo disponível para outras tarefas.

### Obrigações financeiras

Existem obrigações recorrentes distribuídas ao longo do mês, incluindo
pagamentos de cartões, água, curso, telefone e internet. Essas
obrigações são tratadas como tarefas de rotina dentro do sistema.

### Compromissos e demandas variáveis

O Google Calendar concentra compromissos, viagens e outros eventos. Ao
mesmo tempo, demandas pessoais podem surgir em horários não previstos.

Por isso, o sistema separa três conceitos:

-   **Tarefas:** o que precisa ser feito.
-   **Calendário:** o que está marcado.
-   **Rotina:** como o tempo normalmente é estruturado.

Essa separação permite analisar a rotina sem transformar o planejamento
em uma agenda rígida.

------------------------------------------------------------------------

## 2. Principais desafios de produtividade identificados

### Multiplicidade de responsabilidades

A rotina reúne trabalho, curso, estudos, projetos acadêmicos, atividade
física, finanças, tarefas pessoais e compromissos. Essas demandas
possuem diferentes prioridades, prazos e esforços, tornando inadequada
uma lista única sem contexto.

### Diferenciar tarefa de compromisso

Uma aula possui horário definido, enquanto uma tarefa como "Revisar
anotações da disciplina" pode ter deadline sem possuir horário de
execução.

Por isso, o campo de data do Trello é tratado como **deadline**, não
como horário de execução.

### Procrastinação em projetos

Existe tendência de iniciar projetos acadêmicos mais próximos do prazo.
O projeto mensal da disciplina é um exemplo relevante.

O sistema não diagnostica esse comportamento. Ele utiliza somente sinais
objetivos, como prazo, prioridade, estado, tipo, estimativa e checklist,
para permitir que a IA sinalize situações que merecem atenção.

### Ausência de estimativas

Muitas tarefas inicialmente não possuíam estimativa de duração. Isso
dificulta avaliar a carga de trabalho de uma semana, pois uma tarefa de
20 minutos é diferente de um projeto de 8 horas.

A estimativa passou a fazer parte do modelo quando conhecida, mas a IA
não pode inventar uma duração ausente.

### Inconsistência de informações

Durante a construção foram identificados problemas como prioridade
registrada em mais de um lugar, datas inseridas no título, tarefas
importantes permanecendo na Inbox e tarefas em andamento sem metadados
suficientes.

A solução passou a considerar como informações importantes:

-   área;
-   prioridade;
-   tipo;
-   estado;
-   deadline;
-   estimativa, quando conhecida.

### Excesso de decisões manuais

Sem integração, seria necessário reunir mentalmente tarefas,
compromissos e rotina para realizar uma revisão. O POS reduz esse
esforço ao coletar automaticamente Trello e Google Calendar e apresentar
o contexto à IA.

------------------------------------------------------------------------

## 3. Métodos utilizados

### GTD --- captura e organização

A **Inbox** funciona como ponto de captura:

``` text
Nova demanda
     ↓
Inbox
     ↓
Organização
     ↓
Backlog / Esta Semana / Em Andamento
     ↓
Concluído
```

A captura é separada da organização, o que é útil para demandas pessoais
que surgem de forma imprevisível.

### Eisenhower --- prioridade

As tarefas utilizam três níveis:

-   🔴 Alta;
-   🟡 Média;
-   🟢 Baixa.

A prioridade é definida pelo usuário. A IA utiliza essa informação na
análise, mas não deve alterá-la automaticamente.

### Planejamento semanal

A lista **Esta Semana** representa o horizonte semanal. Ela não é uma
lista "Hoje".

Essa decisão mantém a separação entre planejamento e agenda e reduz a
rigidez do sistema.

### Pomodoro

Pomodoro pode ser utilizado durante a execução de tarefas que exigem
concentração, especialmente estudos e projetos. O POS não precisa
controlar os ciclos automaticamente: ele organiza a tarefa e o usuário
aplica a técnica quando necessário.

### Checklists

Projetos maiores utilizam checklists para transformar uma demanda ampla
em etapas executáveis. O projeto mensal da disciplina, por exemplo,
possui checklist e estimativa total de 8 horas.

------------------------------------------------------------------------

## 4. Ferramentas escolhidas e justificativa

### Trello

O Trello é o núcleo operacional das tarefas e da configuração.

Foi escolhido pela organização visual, simplicidade, facilidade de
movimentação entre estados, labels, campo nativo de prazo e
possibilidade de criar um template reutilizável.

A estrutura utilizada é:

``` text
⚙️ Configuração
📥 Inbox
📚 Backlog
📅 Esta Semana
🔨 Em Andamento
🏁 Concluído
```

O Trello responde:

> **O que preciso fazer?**

Como o plano utilizado possui limitações em recursos avançados, como
campos personalizados, parte dos metadados é mantida em descrições e
labels.

### Google Calendar

O Google Calendar é a fonte dos compromissos. O sistema consulta eventos
e utiliza informações como título, início, fim, status, local e
descrição.

O Calendar responde:

> **O que tenho marcado?**

As tarefas não são duplicadas como eventos.

### Make.com

O Make.com é a camada de integração e automação.

Ele busca dados, filtra cards, separa configuração e tarefas, ignora
modelos, agrega informações, consulta o Calendar, cria o contexto para a
IA, interpreta o retorno e envia o relatório.

A automação executa regras determinísticas e não toma decisões pessoais.

### Gemini 2.5 Flash

O Gemini é a camada de interpretação. Recebe um contexto estruturado
denominado `POS_CONTEXT`, contendo:

``` text
config
tasks
calendar
```

A IA produz:

``` text
summary
facts
risks
recommendations
planning_notes
```

### Gmail

O Gmail é utilizado para entregar a análise ao usuário. A mensagem é
montada em HTML, com resumo, atenção, recomendações, pontos observados e
observações de planejamento.

------------------------------------------------------------------------

## 5. Como a IA foi utilizada para apoiar a organização

A IA foi colocada depois da coleta e organização dos dados:

``` text
Trello + Calendar + Configuração
                ↓
           POS_CONTEXT
                ↓
             Gemini
                ↓
       Análise estruturada
                ↓
             Relatório
                ↓
             Usuário
```

### Análise de fatos

A IA identifica informações objetivas, sempre mencionando a tarefa.

Exemplo:

> "Enviar documentação importante possui prioridade Alta e deadline em
> 2026-09-04."

### Identificação de riscos

Os riscos representam situações que merecem atenção com base nos dados.
Não devem simplesmente repetir um fato sem acrescentar informação
relevante.

### Recomendações

Cada recomendação contém:

``` text
task
action
```

As ações permitidas são:

-   `Priorizar.`
-   `Resolver.`
-   `Revisar.`
-   `Acompanhar.`
-   `Antecipar.`

As recomendações não definem horários nem executam tarefas.

### Observações de planejamento

A IA pode destacar informações relevantes para o planejamento, como
deadlines e compromissos, desde que sustentadas pelos dados disponíveis.

### Regra de não invenção

A IA não deve:

-   inventar prazos;
-   inventar estimativas;
-   inventar prioridades;
-   inventar compromissos;
-   escolher horários;
-   criar eventos;
-   executar tarefas;
-   inventar progresso;
-   inventar dependências;
-   inventar etapas de projetos;
-   diagnosticar procrastinação;
-   afirmar consequências não sustentadas pelos dados.

A regra é:

> **Se o dado não existe, a IA não deve criá-lo.**

### Usuário como decisor

A divisão de responsabilidades é:

``` text
Automação → executa regras e coleta dados
IA         → analisa e recomenda
Usuário    → decide e executa
```

------------------------------------------------------------------------

## 6. Estratégias para comunicação, procrastinação e saúde mental

### Comunicação

Demandas de comunicação são tratadas como tarefas concretas.

Exemplo:

``` text
Responder email do cliente - orçamento Q3
```

Essa demanda pode possuir área, prioridade, estimativa, deadline e
estado. Isso evita depender somente da memória para acompanhar respostas
pendentes e permite que comunicação seja considerada no planejamento
junto às demais atividades.

### Redução da procrastinação

A estratégia adotada é estrutural, não baseada em pressão.

São utilizados:

1.  captura rápida pela Inbox;
2.  deadlines explícitos;
3.  prioridades;
4.  estimativas;
5.  planejamento semanal;
6.  checklists;
7.  análise da IA como sinalizador.

Projetos grandes podem ser acompanhados por etapas, enquanto a IA pode
destacar situações objetivas que merecem atenção.

### Evitar planejamento excessivamente rígido

O sistema não tenta preencher todo período livre com tarefas.

A rotina possui trabalho, estudos, academia, compromissos, vida pessoal
e demandas inesperadas. Portanto, tempo sem compromisso não é
automaticamente considerado tempo disponível para trabalho.

Além disso, uma deadline não representa um horário de execução.

### Preservação do tempo pessoal

Sexta-feira após 21:00, sábado e domingo possuem espaço para descanso,
vida social e atividades pessoais.

O sistema considera esses períodos como parte da rotina, evitando uma
visão de produtividade baseada apenas na quantidade de tarefas
concluídas.

### Bem-estar

O objetivo não é maximizar ocupação, mas melhorar a organização de uma
rotina que precisa acomodar diferentes responsabilidades.

A solução considera conjuntamente:

-   trabalho;
-   estudos;
-   atividade física;
-   compromissos;
-   descanso;
-   vida pessoal.

A IA fornece informação e recomendações; a decisão sobre quanto fazer e
quando fazer permanece humana.

------------------------------------------------------------------------

## 7. Síntese da solução

A solução pode ser resumida em quatro camadas:

``` text
Trello + Google Calendar
          ↓
       Make.com
          ↓
     POS_CONTEXT
          ↓
   Gemini / Inteligência Artificial
          ↓
     Análise + recomendações
          ↓
        Usuário
```

O Trello representa o que precisa ser feito. O Google Calendar
representa o que está marcado. O Make.com integra as informações. O
Gemini interpreta o contexto. O usuário decide e executa.

O princípio central é:

> **Automação executa regras; IA recomenda; usuário decide.**

E o princípio de reutilização é:

> **O template é fixo. A configuração é individual. A análise é
> adaptativa.**

A solução, portanto, utiliza tecnologia e Inteligência Artificial para
reduzir o esforço de organização sem transformar a rotina em um processo
totalmente automatizado ou rígido.
