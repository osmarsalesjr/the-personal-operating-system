# Meu POS — Sistema Operacional Pessoal

## 1. Visão geral

O **Meu POS (Sistema Operacional Pessoal)** é um template de organização pessoal desenvolvido para ajudar o usuário a organizar tarefas, projetos, compromissos e diferentes áreas da vida em um fluxo simples de captura, organização, planejamento, execução e revisão.

A solução utiliza inicialmente:

- **Trello** — gestão de tarefas, projetos e configuração do usuário;
- **Google Calendar** — compromissos e bloqueios de horário;
- **Make** — automação e integração;
- **IA** — análise, planejamento e recomendações, com **Gemini** como candidato inicial.

O projeto é construído a partir de uma rotina de referência, mas **não deve ser dependente dela**. A rotina utilizada durante o desenvolvimento serve apenas como exemplo de configuração.

### Princípio central

> **O usuário configura os dados; o sistema fornece a lógica.**

O template deve ser simples para o uso diário e, ao mesmo tempo, suficientemente genérico para que diferentes usuários possam adaptá-lo às próprias rotinas sem modificar a lógica dos cenários de automação.

---

## 2. Estratégia do projeto

O Meu POS seguirá uma estratégia de **template reutilizável**.

Um novo usuário deverá conseguir:

- Copiar o template;
- Configurar seu perfil;
- Definir sua própria rotina;
- Definir suas áreas;
- Configurar preferências e restrições;
- Criar tarefas e projetos;
- Conectar suas próprias contas;
- Utilizar as mesmas automações;
- Receber recomendações baseadas na própria realidade.

A diferença entre usuários deve estar principalmente nos **dados de configuração**, e não na implementação dos cenários.

### Princípio de parametrização

Nenhuma automação essencial deve depender de:

- Dias específicos da rotina de referência;
- Horários específicos da rotina de referência;
- Áreas específicas;
- Compromissos específicos;
- Quantidades fixas de horas disponíveis;
- Regras pessoais que não estejam configuradas pelo usuário.

Por exemplo, uma automação não deve assumir que o usuário estuda segunda, quarta e sexta à noite. Ela deve consultar a configuração de rotina e identificar os períodos disponíveis daquele usuário.

---

## 3. Arquitetura

A arquitetura conceitual é:

```text
                    ┌─────────────────────┐
                    │       TRELLO        │
                    │                     │
                    │ Tarefas             │
                    │ Projetos            │
                    │ Rotinas             │
                    │ Configuração         │
                    └──────────┬──────────┘
                               │
                               │ dados
                               ▼
                    ┌─────────────────────┐
                    │        MAKE         │
                    │                     │
                    │ Integração           │
                    │ Regras               │
                    │ Automação            │
                    │ Orquestração         │
                    └──────┬─────────┬────┘
                           │         │
                ┌──────────┘         └──────────┐
                ▼                               ▼
     ┌──────────────────┐             ┌──────────────────┐
     │ Google Calendar  │             │        IA        │
     │                  │             │                  │
     │ Compromissos     │             │ Análise          │
     │ Bloqueios        │             │ Planejamento     │
     │ Disponibilidade  │             │ Recomendações    │
     └──────────────────┘             └──────────────────┘
```

### Responsabilidade de cada componente

**Trello**

Fonte principal para:

- Tarefas;
- Projetos;
- Rotinas;
- Estados das demandas;
- Prazos;
- Prioridades;
- Estimativas;
- Configurações do usuário.

**Google Calendar**

Fonte principal para:

- Compromissos com horário;
- Eventos recorrentes;
- Bloqueios de disponibilidade;
- Outros períodos que não devem ser ocupados por tarefas.

**Make**

Camada de:

- Integração;
- Automação;
- Orquestração;
- Aplicação de regras determinísticas;
- Comunicação entre Trello, Calendar e IA.

**IA**

Camada de:

- Interpretação;
- Classificação;
- Planejamento;
- Decomposição;
- Análise de conflitos;
- Identificação de padrões;
- Recomendações de melhoria.

**Usuário**

Responsável por:

- Configurar sua realidade;
- Confirmar recomendações;
- Decidir prioridades;
- Executar as atividades;
- Alterar sua rotina quando necessário.

### Regra arquitetural

> **Automação executa regras; IA recomenda; usuário decide.**

A IA não é a fonte de verdade do sistema e não deve alterar decisões importantes de forma autônoma.

---

# 4. Modelo conceitual

O POS diferencia quatro conceitos principais:

### Compromisso

Algo que ocupa um horário específico.

Exemplo:

> Aula terça-feira, das 19h às 21h.

Normalmente pertence ao Google Calendar.

### Tarefa

Algo que precisa ser realizado.

Exemplo:

> Revisar referências do trabalho.

Pertence ao Trello.

### Rotina

Uma regra recorrente que descreve como o tempo ou as atividades do usuário normalmente funcionam.

Exemplo:

> Academia de segunda a sexta, das 12h às 14h.

A rotina pode ser utilizada para gerar recorrências, identificar bloqueios e orientar a análise.

### Disponibilidade

Períodos em que o usuário pode potencialmente executar tarefas.

A disponibilidade não precisa ser cadastrada manualmente para cada tarefa. Ela pode ser **inferida a partir da rotina, compromissos, bloqueios e preferências do usuário**.

---

# 5. Estrutura do Trello

O template utiliza um único board:

> **🧠 Meu POS — Sistema Operacional Pessoal**

Listas:

```text
⚙️ Configuração
📥 Inbox
📚 Backlog
📅 Esta Semana
🔨 Em Andamento
🏁 Concluído
```

As listas representam o **estado operacional** dos cartões.

Datas, etiquetas, descrições e, quando disponíveis, campos representam informações de planejamento.

---

## 5.1 Configuração

A lista de configuração não participa do fluxo operacional das tarefas.

Ela contém informações que descrevem como o usuário utiliza o sistema e como sua rotina funciona.

Cartões previstos:

- 👤 Meu Perfil;
- 🕐 Minha Rotina;
- 🗂️ Minhas Áreas;
- ⚙️ Minhas Preferências;
- 📖 Como Usar o POS.

Esses cartões são especialmente importantes para as futuras automações, pois representam a configuração individual do usuário.

---

## 5.2 Inbox

A **Inbox** é o ponto de entrada para novas demandas.

Regra:

> Tudo que surgir e ainda não tiver sido processado deve ser colocado na Inbox.

O usuário não precisa decidir imediatamente:

- Área;
- Tipo;
- Prioridade;
- Prazo;
- Estimativa;
- Planejamento.

Exemplos:

- Comprar material;
- Fazer trabalho da disciplina;
- Pagar conta;
- Pesquisar determinado assunto.

Posteriormente, a Inbox poderá ser processada manualmente ou com apoio da IA.

---

## 5.3 Backlog

O Backlog contém demandas que já foram processadas, mas ainda não foram selecionadas para execução no período atual.

O Backlog não representa necessariamente tarefas atrasadas.

Ele representa demandas existentes que ainda não fazem parte do planejamento atual.

---

## 5.4 Esta Semana

A lista **Esta Semana** representa aquilo que o usuário decidiu que pretende executar durante a semana.

Ela é uma decisão de planejamento.

Uma tarefa pode estar nesta lista mesmo que seu prazo seja posterior à semana atual, caso o usuário ou a IA recomende antecipar sua execução.

---

## 5.5 Em Andamento

Contém as tarefas que estão sendo efetivamente executadas.

Recomenda-se manter poucas tarefas simultaneamente em andamento para reduzir troca de contexto e favorecer o foco.

---

## 5.6 Concluído

Contém tarefas e projetos finalizados.

Os cartões concluídos não devem ser removidos imediatamente, pois podem servir posteriormente para:

- Revisão semanal;
- Métricas;
- Análise de produtividade;
- Identificação de padrões;
- Recomendações da IA.

---

# 6. Estratégia de planejamento

O POS utiliza a seguinte estratégia:

> **Listas representam estado; datas e informações do cartão representam planejamento.**

Não haverá uma lista física chamada **Hoje**.

A identificação de tarefas para hoje ou próximos dias deverá utilizar:

- Datas;
- Filtros;
- Visualizações do Trello;
- Automações;
- Análise do calendário.

Exemplos de visões:

- 🎯 Hoje;
- 📅 Próximos 7 dias;
- 🔴 Alta prioridade;
- 🎓 Estudos;
- 📂 Projetos;
- ⚠️ Atrasadas.

Isso evita movimentações desnecessárias de cartões.

---

# 7. Áreas

As áreas representam os diferentes contextos da vida do usuário.

O template pode apresentar inicialmente:

- 💼 Trabalho;
- 🎓 Estudos;
- 🏠 Pessoal;
- 💰 Financeiro;
- 🏃 Saúde.

Essas áreas são apenas uma configuração inicial.

O usuário poderá:

- Adicionar áreas;
- Remover áreas;
- Renomear áreas;
- Criar novas categorias conforme sua realidade.

As automações não devem depender da existência dessas áreas específicas.

---

# 8. Tipos de cartão

O POS utiliza inicialmente quatro tipos:

- 📌 Tarefa;
- 📂 Projeto;
- 🔁 Rotina;
- ⏳ Aguardando.

## Tarefa

Representa uma ação concreta que pode ser iniciada e concluída.

Exemplo:

```text
Pagar conta de internet
```

## Projeto

Representa um objetivo que exige múltiplas ações ou etapas.

Exemplo:

```text
Trabalho da disciplina
```

## Rotina

Representa uma atividade ou obrigação recorrente.

Exemplo:

```text
Compras quinzenais
```

## Aguardando

Representa uma demanda que depende de uma ação, informação ou pessoa externa antes de poder continuar.

---

# 9. Projetos

No MVP, um projeto poderá ser representado por um cartão com checklist de etapas.

Exemplo:

```text
📂 Trabalho da disciplina

☐ Entender requisitos
☐ Pesquisar referências
☐ Criar estrutura
☐ Desenvolver
☐ Revisar
☐ Finalizar
☐ Entregar
```

O projeto pode conter:

- Objetivo;
- Resultado esperado;
- Etapas;
- Observações;
- Prazo;
- Prioridade;
- Estimativa.

Caso uma etapa exija acompanhamento individual, ela poderá posteriormente ser transformada em uma tarefa independente.

### Projetos recorrentes

Quando um projeto se repete periodicamente, cada ciclo deve preferencialmente possuir seu próprio cartão.

Exemplo:

```text
📂 Projeto de disciplina — Setembro
📂 Projeto de disciplina — Outubro
📂 Projeto de disciplina — Novembro
```

Isso permite preservar o histórico, comparar ciclos e analisar padrões de execução.

A geração desses projetos recorrentes poderá ser feita posteriormente pelo Make.

---

# 10. Informações dos cartões

O modelo conceitual dos cartões utiliza:

### Área

Identifica o contexto ao qual a demanda pertence.

### Tipo

Identifica se o cartão representa:

- Tarefa;
- Projeto;
- Rotina;
- Aguardando.

### Prioridade

Utiliza inicialmente:

- P1 — Alta;
- P2 — Média;
- P3 — Baixa.

### Estimativa

Representa o esforço aproximado:

- 15m;
- 30m;
- 1h;
- 2h+.

### Prazo

Utiliza a data nativa do Trello.

O prazo representa **quando algo precisa estar concluído**, e não necessariamente quando deve ser executado.

### Limitação atual do Trello

Na configuração atual do projeto, o plano gratuito do Trello não disponibiliza os recursos de campos personalizados e modelos nativos utilizados originalmente no desenho.

Como alternativa inicial:

- As informações são mantidas nos cartões;
- Os cartões-modelo ficam disponíveis no Backlog;
- Etiquetas podem representar área e prioridade;
- As descrições seguem uma estrutura padronizada.

A estrutura deverá continuar compatível com uma futura evolução para campos personalizados, caso isso seja necessário.

---

# 11. Etiquetas

Como alternativa simples aos campos personalizados, o template poderá utilizar etiquetas.

### Área

- 💼 Trabalho;
- 🎓 Estudos;
- 🏠 Pessoal;
- 💰 Financeiro;
- 🏃 Saúde.

### Prioridade

- 🔴 P1 — Alta;
- 🟡 P2 — Média;
- 🟢 P3 — Baixa.

As etiquetas de área são exemplos iniciais e devem ser adaptáveis.

O sistema não deve assumir que todos os usuários terão as mesmas áreas.

---

# 12. Modelos de cartão

O template possui cartões-modelo no Backlog:

- 📌 [MODELO] Tarefa;
- 📌 [MODELO] Projeto;
- 📌 [MODELO] Rotina.

Esses cartões servem como referência para criação manual de novos cartões enquanto o plano utilizado não disponibilizar modelos nativos.

## Modelo de tarefa

```text
📌 Revisar referências do trabalho

Área: Estudos
Tipo: Tarefa
Prioridade: P2
Estimativa: 1h
Prazo: 10/09
```

Descrição:

```text
OBJETIVO

Revisar as referências selecionadas.

RESULTADO ESPERADO

Referências organizadas para utilização no trabalho.

OBSERVAÇÕES

...
```

## Modelo de projeto

```text
📂 Trabalho — Engenharia de Software

Área: Estudos
Tipo: Projeto
Prioridade: P1
Estimativa: 2h+
Prazo: 20/09
```

Descrição e checklist podem conter:

- Objetivo;
- Resultado esperado;
- Etapas;
- Observações.

---

# 13. Configuração do usuário

A configuração é uma das partes mais importantes da arquitetura porque permite que o mesmo template seja utilizado por diferentes pessoas.

## Meu Perfil

Deve conter informações básicas necessárias para personalizar a utilização do sistema.

## Minha Rotina

Deve descrever como o tempo do usuário normalmente é distribuído.

Exemplo de configuração:

```text
ROTINA FIXA

Segunda a sexta

09:00–12:00
Trabalho

12:00–14:00
Disponibilidade pessoal

14:00–18:00
Trabalho

19:00–22:00
Estudos
```

Esses horários são apenas um exemplo.

Outro usuário poderá configurar:

```text
Segunda a sexta

08:00–17:00
Trabalho

18:00–20:00
Faculdade

Sábado
09:00–12:00
Estudos
```

As automações devem funcionar nos dois casos sem alteração de lógica.

### Minha Rotina como fonte de parâmetros

A rotina deve ser tratada como **dados de configuração**, não como código.

O Make e a IA poderão consultar essas informações para:

- Identificar períodos ocupados;
- Identificar períodos disponíveis;
- Avaliar conflitos;
- Planejar tarefas;
- Avaliar capacidade semanal;
- Sugerir redistribuição de atividades.

## Minhas Áreas

Define os contextos relevantes para o usuário.

## Minhas Preferências

Pode conter preferências como:

- Horários preferidos para determinadas atividades;
- Períodos que devem ser protegidos;
- Limite desejado de tarefas simultâneas;
- Preferência por tarefas curtas ou longas em determinados períodos;
- Regras pessoais de planejamento.

## Como Usar o POS

Contém orientações resumidas sobre o funcionamento do sistema.

---

# 14. Compromissos, tarefas e disponibilidade

O POS deve manter uma separação clara:

```text
TRELLO
"O que preciso fazer?"

CALENDAR
"O que tenho marcado?"

ROTINA
"Como meu tempo normalmente funciona?"

IA
"O que seria melhor fazer considerando
tarefas, compromissos e disponibilidade?"
```

O sistema não deverá duplicar desnecessariamente compromissos do Google Calendar como cartões.

### Exemplo

Uma aula às 19h não precisa gerar uma tarefa chamada:

```text
Assistir aula
```

se o objetivo for apenas representar que aquele período está ocupado.

Já uma tarefa como:

```text
Revisar conteúdo da aula
```

deve existir no Trello se precisar ser realizada.

---

# 15. Papel da IA

A IA será utilizada principalmente quando houver necessidade de interpretação, análise ou recomendação.

## 15.1 Processar Inbox

Fluxo:

```text
Inbox
  ↓
IA
  ↓
Classificação e sugestões
  ↓
Usuário
  ↓
Trello
```

A IA poderá sugerir:

- Área;
- Tipo;
- Prioridade;
- Estimativa;
- Prazo;
- Perguntas necessárias para completar a demanda.

O usuário permanece responsável pela confirmação.

---

## 15.2 Decompor projetos

Fluxo:

```text
Projeto
  ↓
IA
  ↓
Etapas sugeridas
  ↓
Usuário
  ↓
Checklist / Tarefas
```

A IA poderá transformar objetivos maiores em etapas executáveis.

---

## 15.3 Planejar a semana

Fluxo:

```text
Tarefas + Projetos
        +
Rotina do usuário
        +
Google Calendar
        ↓
       IA
        ↓
Plano recomendado
        ↓
     Usuário
```

A IA deve considerar:

- Prazos;
- Prioridades;
- Estimativas;
- Tarefas já planejadas;
- Compromissos;
- Períodos disponíveis;
- Preferências;
- Capacidade disponível.

A recomendação deve ser adaptada à rotina do usuário, e não à rotina utilizada durante o desenvolvimento.

---

## 15.4 Identificar risco de atraso

A IA poderá analisar a relação entre:

```text
Esforço restante
+
Prazo
+
Disponibilidade
+
Compromissos
```

e identificar situações como:

> O projeto exige mais tempo do que a disponibilidade existente antes do prazo.

Nesse caso, poderá recomendar:

- Antecipar o início;
- Dividir o projeto;
- Reduzir tarefas de menor prioridade;
- Utilizar outro período disponível;
- Negociar o prazo, quando aplicável.

---

## 15.5 Analisar procrastinação

O sistema poderá identificar padrões de comportamento a partir do histórico.

Exemplo:

- Projetos recorrentes são iniciados próximos do prazo;
- O usuário possui disponibilidade anterior ao prazo;
- As tarefas são concentradas nos últimos dias.

A IA poderá sugerir antecipação ou distribuição gradual do trabalho.

A intenção não é julgar o usuário, mas transformar o histórico em recomendações práticas.

---

## 15.6 Revisar a semana

Fluxo:

```text
Dados do período
      ↓
     IA
      ↓
Análise
      ↓
Recomendações
```

A revisão poderá analisar:

- O que foi concluído;
- O que ficou atrasado;
- Projetos iniciados;
- Projetos concluídos;
- Distribuição de esforço;
- Conflitos;
- Padrões recorrentes;
- Oportunidades de melhoria.

---

# 16. Automação com Make

O Make será utilizado como camada de integração e execução das automações.

As automações devem priorizar regras:

- Simples;
- Previsíveis;
- Reutilizáveis;
- Parametrizadas;
- Independentes da rotina de referência.

## Princípio de genericidade

Um cenário não deve conter regras como:

```text
Se segunda-feira às 19h, criar tarefa de estudo.
```

Deve conter lógica equivalente a:

```text
Consultar configuração da rotina
        ↓
Identificar regra recorrente
        ↓
Gerar ocorrência correspondente
        ↓
Criar/atualizar cartão
```

Assim, a mesma automação pode funcionar para diferentes usuários.

---

# 17. Arquitetura dos cenários genéricos

Os cenários do Make devem seguir, sempre que possível, esta lógica:

```text
1. Obter configuração
        ↓
2. Obter dados operacionais
        ↓
3. Aplicar regras genéricas
        ↓
4. Consultar Calendar quando necessário
        ↓
5. Utilizar IA quando houver necessidade
        ↓
6. Gerar recomendação ou executar ação
        ↓
7. Atualizar Trello / Calendar
```

### Dados que podem ser utilizados

- Configuração do usuário;
- Rotina;
- Áreas;
- Preferências;
- Tarefas;
- Projetos;
- Prazos;
- Prioridades;
- Estimativas;
- Histórico;
- Compromissos do Calendar.

A rotina completa deve ser considerada sempre que a análise depender de disponibilidade ou capacidade.

---

# 18. Automação versus IA

Nem toda automação precisa de IA.

### Preferir automação determinística para:

- Criar ocorrências recorrentes;
- Identificar prazos vencidos;
- Mover cartões quando uma regra objetiva for satisfeita;
- Sincronizar informações;
- Consultar calendário;
- Executar tarefas repetitivas.

### Preferir IA para:

- Classificar demandas ambíguas;
- Decompor projetos;
- Planejar;
- Priorizar;
- Interpretar rotina;
- Identificar conflitos complexos;
- Detectar padrões;
- Sugerir melhorias.

Isso reduz:

- Custo;
- Complexidade;
- Latência;
- Dependência de IA.

---

# 19. Google Calendar

O Google Calendar é o sistema de referência para compromissos com horário.

Pode conter:

- Trabalho;
- Aulas;
- Academia;
- Consultas;
- Viagens;
- Compromissos pessoais;
- Outros bloqueios.

A automação poderá consultar o calendário para determinar quais períodos já estão ocupados.

### Disponibilidade

A disponibilidade deverá ser obtida a partir da combinação entre:

```text
Rotina configurada
+
Compromissos do Calendar
+
Bloqueios
+
Preferências
+
Tarefas existentes
```

A IA poderá então sugerir onde determinadas tarefas poderiam ser executadas.

---

# 20. Recorrências

Atividades recorrentes devem ser tratadas de forma genérica.

Exemplos:

- Contas mensais;
- Compras quinzenais;
- Atividades semanais;
- Projetos mensais;
- Outras obrigações periódicas.

A regra recorrente deve estar associada à configuração do usuário, e não codificada diretamente no cenário.

Exemplo conceitual:

```text
ROTINA / RECORRÊNCIA

Periodicidade: mensal
Dia: definido pelo usuário
Descrição: pagar determinada obrigação
Área: definida pelo usuário
```

O Make poderá utilizar essas informações para gerar a ocorrência correspondente.

---

# 21. Prioridade

O sistema utiliza três níveis:

| Prioridade | Significado |
|---|---|
| **P1** | Alta |
| **P2** | Média |
| **P3** | Baixa |

A prioridade poderá ser definida pelo usuário ou sugerida pela IA.

A IA não deverá alterar prioridades críticas automaticamente sem decisão ou confirmação do usuário.

O conceito de Eisenhower poderá ser utilizado como referência para análise, mas não será reproduzido integralmente como estrutura do Trello.

---

# 22. Métricas

O MVP utilizará poucas métricas.

Indicadores inicialmente previstos:

- Tarefas concluídas;
- Tarefas atrasadas;
- Taxa de conclusão;
- Projetos em andamento;
- Projetos iniciados antes do prazo.

Também poderão ser avaliados posteriormente:

- Antecedência média de início de projetos;
- Tempo estimado versus tempo disponível;
- Distribuição de tarefas por área;
- Concentração de tarefas próximas aos prazos;
- Quantidade de tarefas replanejadas;
- Cumprimento de rotinas.

A métrica de antecedência é especialmente relevante para avaliar se o sistema está ajudando o usuário a evitar concentração de trabalho próximo aos prazos.

Não será criado inicialmente um banco de dados específico apenas para métricas.

Sempre que possível, os dados serão obtidos a partir do Trello, Calendar e automações.

---

# 23. Dashboard

O dashboard será uma camada separada do Trello.

O Trello deverá permanecer focado em:

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

Enquanto o dashboard e a IA serão responsáveis por:

```text
Analisar
   ↓
Medir
   ↓
Recomendar
```

O dashboard será desenvolvido posteriormente, após a validação do fluxo principal.

---

# 24. Princípios de design

## Simplicidade

Evitar campos, listas e automações sem função clara.

## Reutilização

O template deve poder ser copiado e utilizado por diferentes pessoas.

## Parametrização

Diferenças entre usuários devem ser representadas por dados de configuração, e não por alterações nos cenários.

## Baixa complexidade

Priorizar recursos nativos do Trello e ferramentas com planos gratuitos.

## Automação consciente

Automatizar tarefas repetitivas, mas evitar automação excessiva.

## IA como apoio

A IA apoia decisões e recomenda melhorias, mas não substitui a decisão do usuário.

## Separação de responsabilidades

```text
Trello → Tarefas, projetos e configuração
Calendar → Compromissos e bloqueios
Make → Automação e integração
IA → Análise e recomendação
Usuário → Decisão e execução
```

## Independência da rotina de referência

A rotina utilizada durante o desenvolvimento não deve se transformar em regra de negócio.

---

# 25. Escopo do MVP

## Incluído

- Template Trello;
- Configuração do usuário;
- Inbox;
- Backlog;
- Planejamento semanal;
- Execução;
- Projetos;
- Tarefas;
- Rotinas;
- Prioridades;
- Estimativas;
- Google Calendar;
- Make;
- IA;
- Processamento assistido da Inbox;
- Decomposição de projetos;
- Planejamento assistido;
- Análise de disponibilidade;
- Identificação de risco de atraso;
- Revisão semanal;
- Métricas básicas;
- Cenários genéricos e parametrizados.

## Fora do MVP

- Aplicativo próprio;
- Sistema de autenticação;
- Banco de dados externo;
- SaaS multiusuário;
- Frontend próprio;
- Chatbot completo;
- Automações complexas de hábitos;
- Gerenciamento das tarefas internas do trabalho;
- Notificações avançadas;
- Dashboard avançado;
- Alteração autônoma de prioridades ou rotina.

---

# 26. Critérios para novos cenários do Make

Antes de implementar qualquer cenário, verificar:

### 1. Ele depende de alguma informação específica do usuário?

Se sim, essa informação deve ser transformada em configuração.

### 2. Essa regra poderia funcionar para outro usuário?

Se não, a implementação deve ser revista.

### 3. O cenário precisa realmente de IA?

Se a regra for determinística, preferir automação convencional.

### 4. O cenário consegue obter os dados da configuração?

A lógica não deve depender de valores fixos no cenário.

### 5. O usuário consegue adaptar a rotina sem alterar o cenário?

Esse é um dos principais critérios de aceitação.

### 6. A automação pode executar uma ação incorreta sem confirmação?

Se sim, avaliar se a ação deve se tornar uma recomendação para o usuário em vez de uma alteração automática.

---

# 27. Estratégia de validação

Antes de construir todas as automações, o fluxo deverá ser testado com dados reais e fictícios.

Fluxo principal:

```text
Capturar
   ↓
Processar
   ↓
Backlog
   ↓
Planejar
   ↓
Executar
   ↓
Concluir
   ↓
Revisar
```

A primeira validação deve utilizar a rotina de referência apenas como **caso de teste**.

Depois, o mesmo cenário deverá ser testado com pelo menos uma rotina hipotética diferente para verificar se:

- As automações continuam funcionando;
- Nenhum horário está codificado;
- A disponibilidade é recalculada;
- As recomendações mudam de acordo com o usuário;
- Nenhum cenário precisa ser editado.

---

# 28. Estratégia de evolução

A implementação será incremental:

1. Consolidar estrutura do Trello;
2. Finalizar configuração do usuário;
3. Definir formato das informações de rotina;
4. Validar o fluxo manualmente;
5. Criar dados de teste para diferentes perfis de rotina;
6. Definir contratos de entrada e saída dos cenários;
7. Criar automações determinísticas;
8. Integrar Google Calendar;
9. Integrar Gemini;
10. Criar processamento da Inbox;
11. Criar planejamento assistido;
12. Criar análise de risco de atraso;
13. Criar revisão semanal;
14. Implementar métricas;
15. Criar dashboard;
16. Testar o template com diferentes perfis;
17. Documentar os cenários;
18. Preparar a versão compartilhável do template.

---

# 29. Status atual

**Arquitetura conceitual:** Definida

**Estratégia:** Template reutilizável e parametrizado

**Ferramenta principal:** Trello

**Planejamento:** Listas representam estado; datas e informações representam planejamento

**Calendário:** Google Calendar

**Automação:** Make

**IA candidata:** Gemini

**Estrutura do board:** Criada

**Lista de configuração:** Criada

**Cartões de configuração:** Criados

**Cartões-modelo:** Criados

**Campos personalizados:** Não disponíveis no plano atual do Trello

**Alternativa atual:** Etiquetas e descrições padronizadas

**Princípio de genericidade:** Definido

**Rotina de referência:** Deve ser tratada como configuração de teste, não como regra do sistema

**Próximo passo:** Validar o template com diferentes configurações de rotina antes de construir os cenários do Make

---

# 30. Visão final do sistema

O Meu POS deve evoluir para um sistema no qual:

```text
                 CONFIGURAÇÃO DO USUÁRIO
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
       Rotina         Preferências        Áreas
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                  ┌─────────────────┐
                  │      MAKE       │
                  │                 │
                  │ Regras genéricas│
                  └────────┬────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
           TRELLO       CALENDAR        IA
              │            │            │
              └────────────┼────────────┘
                           ▼
                     RECOMENDAÇÕES
                           │
                           ▼
                        USUÁRIO
```

O objetivo não é criar uma rotina ideal universal.

O objetivo é criar uma **estrutura universal capaz de compreender diferentes rotinas** e ajudar cada usuário a organizar sua própria realidade.

> **O template é fixo. A configuração é individual. A análise é adaptativa.**
