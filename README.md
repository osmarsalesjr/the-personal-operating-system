# 🧠 Meu POS — Sistema Operacional Pessoal

Sistema de organização pessoal baseado em **Trello + Make + Google Calendar + Gmail + IA**.

O Meu POS centraliza tarefas, projetos, rotina e compromissos e utiliza uma automação para consolidar essas informações, gerar uma análise com IA e enviá-la por e-mail.

> **Template fixo. Configuração individual. Análise adaptativa.**

---

## 1. Visão geral

O sistema utiliza cada ferramenta para uma responsabilidade específica:

| Componente | Responsabilidade |
|---|---|
| **Trello** | Tarefas, projetos, rotina e configuração do usuário |
| **Google Calendar** | Compromissos e eventos que ocupam horários |
| **Make** | Orquestração e execução da automação |
| **Gemini** | Análise contextual e recomendações |
| **Gmail** | Entrega da análise por e-mail |

Princípio central:

> **Automação executa regras; IA analisa e recomenda; o usuário decide.**

A IA não cria compromissos, não escolhe horários e não executa ações no Trello ou Calendar.

---

# 2. Trello

## Quadro

Para começar, acesse o quadro-base do Meu POS:

**🔗 Link do quadro:**  
`https://trello.com/invite/b/6ab03307cd6728a1c56a18d1/ATTI0c75fbd6e6ff5b4e38e63780e04045c4733EFC4E/copypos`

O link deve apontar para a cópia do quadro preparada para distribuição.

O Trello permite compartilhar um link de um quadro e também copiar quadros para reutilização. Ao copiar um quadro, listas, cards e descrições são preservados, enquanto histórico de atividade, comentários, membros dos cards e cards arquivados não são copiados. Devido a limitação de criar templates no plano gratuito, a opção viavel foi criar link de compartilhamento para usuario acessarem como membro e criarem uma copia.

### Como utilizar

1. Acesse o link acima.
2. Faça uma cópia do quadro para sua própria utilização.
3. Utilize a cópia como seu POS pessoal.
4. Adapte os cards de configuração à sua realidade.
5. Crie, altere e organize suas tarefas conforme sua necessidade.

### ⚠️ Não altere as listas do quadro

A estrutura das listas é utilizada pelo cenário do Make.

Mantenha estas listas:

1. `⚙️ Configuração`
2. `📥 Inbox`
3. `📚 Backlog`
4. `📅 Esta Semana`
5. `🔨 Em Andamento`
6. `🏁 Concluído`

**Não renomeie, exclua ou substitua essas listas.**

A automação utiliza os identificadores das listas para separar configuração e tarefas e para determinar quais estados entram na análise.

A organização interna dos cards pode ser adaptada, mas a estrutura operacional do board deve ser preservada.

---

# 3. Configuração do POS

Após copiar o quadro, personalize principalmente os cards da lista `⚙️ Configuração`:

- `👤 Meu Perfil`
- `🕐 Minha Rotina`
- `🗂️ Minhas Áreas`
- `⚙️ Minhas Preferências`

Essas informações são utilizadas como contexto para a análise.

A configuração deve representar a rotina real de cada usuário. Não é necessário reproduzir a rotina utilizada durante o desenvolvimento do projeto.

## Cards de modelo

Os cards:

- `📌 [MODELO] Tarefa`
- `📌 [MODELO] Projeto`
- `📌 [MODELO] Rotina`

servem como referência para criação de novos cards.

Cards cujo nome contenha `[MODELO]` são ignorados pela análise automática.

---

# 4. Estrutura das tarefas

As tarefas utilizam informações nos campos do card e nas labels.

A estrutura atualmente utilizada contempla:

- **Tipo:** Tarefa, Projeto ou Rotina
- **Área:** Estudos, Trabalho, Pessoal, Saúde ou Financeiro
- **Prioridade:** Alta, Média ou Baixa
- **Estimativa:** duração aproximada quando conhecida
- **Prazo:** campo de data do próprio Trello

Exemplo de descrição:

```text
Tipo: Tarefa
Área: Estudos
Estimativa: 30min
```

As labels também representam área e prioridade.

### Prazo

O campo de data do Trello representa o **deadline**, não um horário de execução.

A automação considera a data do prazo e não interpreta o horário do campo como horário reservado para execução.

---

# 5. Google Calendar

O Google Calendar representa os compromissos que devem ser considerados na análise.

O cenário consulta os eventos do calendário principal em uma janela de **hoje até os próximos 7 dias**.

São considerados dados como:

- início;
- fim;
- título;
- status;
- localização;
- descrição;
- link do evento.

A consulta ao Calendar serve principalmente para fornecer contexto objetivo sobre compromissos e possíveis conflitos.

## Conexão e permissões

Cada usuário deve conectar **sua própria conta Google** ao cenário.

A conexão utilizada durante o desenvolvimento não deve ser reutilizada por outros usuários.

Se o Google exigir configuração adicional de OAuth, a conexão precisa possuir as permissões necessárias para o serviço utilizado. O Make documenta que conexões Google podem exigir APIs habilitadas, configuração OAuth e os escopos correspondentes ao serviço utilizado. citeturn0search4

Durante o desenvolvimento, a conexão precisou ser autorizada novamente quando as permissões foram ampliadas. Esse processo foi necessário para permitir que o cenário acessasse corretamente os dados do Google.

> **Importante:** nunca compartilhe credenciais, tokens, Client ID ou Client Secret no repositório.

---

# 6. Make

O cenário principal é:

**`MyPosRoutineAnalysis`**

### 🔗 Link do cenário/documentação no Make

`https://us2.make.com/public/shared-scenario/qhuqTIYEWtv/my-pos-routine-analysis`

O arquivo JSON do cenário está [aqui](./make/MyPosRoutineAnalysis.blueprint.json)

O cenário é responsável por:

1. Ler a estrutura do quadro Trello.
2. Identificar as listas.
3. Coletar os cards.
4. Separar configuração e tarefas.
5. Ignorar cards `[MODELO]`.
6. Coletar eventos do Google Calendar.
7. Consolidar os dados em `POS_CONTEXT_v2`.
8. Enviar o contexto para o Gemini.
9. Interpretar o JSON retornado pela IA.
10. Preparar a mensagem.
11. Obter o endereço de e-mail da conta conectada.
12. Enviar a análise pelo Gmail.

Fluxo conceitual:

```text
Trello
  │
  ├── Listas
  ├── Configuração
  └── Tarefas
       │
       ▼
  POS_CONTEXT_v2
       │
       ├───────────────┐
       │               │
       ▼               ▼
   Gemini 2.5       Calendar
       │               │
       └───────┬───────┘
               ▼
        Análise estruturada
               │
               ▼
        Montagem da mensagem
               │
               ▼
             Gmail
               │
               ▼
        E-mail do usuário
```

---

# 7. Configuração do cenário

O cenário foi construído para trabalhar com os identificadores das listas do quadro-base.

Ao distribuir o cenário para outro usuário, é necessário configurar a referência do **board do usuário** e suas respectivas conexões.

### Não utilizar as conexões do projeto original

O cenário deve utilizar:

- conexão própria do Trello;
- conexão própria do Google Calendar;
- conexão própria do Gmail;
- conexão própria do Gemini, quando aplicável.

As credenciais originais do desenvolvimento não fazem parte da distribuição.

---

# 8. Estrutura interna do cenário

## Trello

A coleta do Trello obtém:

```text
id
name
desc
due
idList
labels
```

As listas são carregadas primeiro para que o cenário possa identificar seus IDs.

Depois os cards são coletados e filtrados de acordo com as listas operacionais.

A configuração é agregada separadamente das tarefas.

---

## POS_CONTEXT_v2

Os dados são consolidados em uma estrutura JSON denominada:

```text
POS_CONTEXT_v2
```

Ela possui três grupos principais:

```text
config
tasks
calendar
```

O contexto é então enviado ao Gemini.

Embora o schema contenha campos semânticos como `type`, `area`, `priority` e `estimate`, esses valores são interpretados principalmente a partir das descrições e labels durante a análise.

---

# 9. Gemini

O módulo de IA utiliza:

**Gemini 2.5 Flash**

O prompt utilizado pelo cenário está versionado separadamente no repositório.

### 🔗 Prompt Gemini

Caso o prompt gemini não carregue ao importar o cenário, [aqui está o contéudo](/prompts/gemini.md). Ajuste as variáveis conforme a necessidade.

O prompt determina que a IA:

- analise exclusivamente o contexto recebido;
- diferencie deadline de horário de execução;
- considere o Calendar apenas para conflitos objetivos;
- não invente informações;
- não escolha horários;
- não crie deadlines;
- não invente duração;
- não invente progresso;
- não invente dependências;
- não diagnostique comportamento;
- não execute ações;
- retorne somente JSON válido.

A resposta possui:

```text
summary
facts
risks
recommendations
planning_notes
```

As recomendações possuem:

```text
task
action
```

As ações permitidas atualmente são:

```text
Priorizar.
Resolver.
Revisar.
Acompanhar.
Antecipar.
```

---

# 10. Processamento da resposta da IA

O JSON retornado pelo Gemini é processado pelo módulo de parsing.

A estrutura é validada antes da montagem da mensagem.

Depois disso, o cenário prepara os dados para envio ao Gmail.

O objetivo dessa separação é evitar que o módulo de e-mail precise interpretar diretamente a resposta bruta da IA.

---


# 10. Data Structures do Make

O cenário utiliza duas **Data Structures** importantes no Make para manter o fluxo de dados estruturado:

- `POS_CONTEXT_v2`
- `GeminiAnalysisResponse`

Essas estruturas devem ser criadas no próprio Make antes de configurar os módulos que dependem delas.

---

## 10.1 Data Structure `POS_CONTEXT_v2`

A `POS_CONTEXT_v2` representa o contexto consolidado que será enviado ao Gemini.

Ela possui três grupos:

```text
config
tasks
calendar
```

### Como criar

No módulo **JSON > Create JSON**:

1. No campo **Data structure**, selecione **Add**.
2. Informe o nome:

```text
POS_CONTEXT_v2
```

3. Crie a estrutura com os campos abaixo.

### `config`

Crie um **Array** de Collections contendo:

```text
id       → Text
name     → Text
desc     → Text
due      → Text
idList   → Text
labels   → Array
```

### `tasks`

Crie um **Array** de Collections contendo:

```text
id              → Text
name            → Text
desc            → Text
due             → Text
idList          → Text
labels          → Array
type            → Text
area            → Text
priority        → Text
estimate        → Text
```

### `calendar`

Crie um **Array** de Collections contendo:

```text
id          → Text
end         → Text
start       → Text
status      → Text
summary     → Text
htmlLink    → Text
location    → Text
eventType   → Text
description → Text
```

A estrutura final deve ser equivalente a:

```text
POS_CONTEXT_v2
│
├── config[]
│   ├── id
│   ├── name
│   ├── desc
│   ├── due
│   ├── idList
│   └── labels[]
│
├── tasks[]
│   ├── id
│   ├── name
│   ├── desc
│   ├── due
│   ├── idList
│   ├── labels[]
│   ├── type
│   ├── area
│   ├── priority
│   └── estimate
│
└── calendar[]
    ├── id
    ├── end
    ├── start
    ├── status
    ├── summary
    ├── htmlLink
    ├── location
    ├── eventType
    └── description
```

Depois de criar a estrutura, o módulo **Create JSON** recebe:

```text
tasks    → dados agregados das tarefas
config   → dados agregados da configuração
calendar → dados agregados do Google Calendar
```

> **Observação:** os campos `type`, `area`, `priority` e `estimate` fazem parte do schema de `tasks`, mas a normalização atual não os preenche semanticamente. A interpretação dessas informações é feita pelo Gemini a partir das descrições e labels disponíveis no contexto.

---

## 10.2 Data Structure `GeminiAnalysisResponse`

A `GeminiAnalysisResponse` representa o JSON retornado pelo Gemini.

Ela é utilizada pelo módulo:

**JSON > Parse JSON**

### Como criar

No módulo **JSON > Parse JSON**:

1. No campo **Data structure**, selecione **Add**.
2. Informe:

```text
GeminiAnalysisResponse
```

3. Crie a estrutura abaixo.

### `summary`

Tipo:

```text
Text
```

### `facts`

Tipo:

```text
Array
```

Cada item do array é uma **Collection** com:

```text
task            → Text
area            → Text
priority        → Text
type            → Text
estimate        → Text
temporal_status → Text
reason          → Text
```

### `risks`

Tipo:

```text
Array
```

Cada item é uma Collection:

```text
task   → Text
level  → Text
reason → Text
```

### `recommendations`

Tipo:

```text
Array
```

Cada item é uma Collection:

```text
task   → Text
action → Text
```

### `planning_notes`

Tipo:

```text
Array
```

Itens:

```text
Text
```

A estrutura final:

```text
GeminiAnalysisResponse
│
├── summary
│
├── facts[]
│   ├── task
│   ├── area
│   ├── priority
│   ├── type
│   ├── estimate
│   ├── temporal_status
│   └── reason
│
├── risks[]
│   ├── task
│   ├── level
│   └── reason
│
├── recommendations[]
│   ├── task
│   └── action
│
└── planning_notes[]
```

O módulo **Parse JSON** recebe como entrada o resultado textual do Gemini e transforma a resposta em campos estruturados que podem ser utilizados pelos módulos seguintes.

---

## 10.3 JSON esperado pelo Gemini

O Gemini deve retornar somente um objeto JSON compatível com `GeminiAnalysisResponse`.

Exemplo estrutural:

```json
{
  "summary": "Resumo da análise.",
  "facts": [
    {
      "task": "Nome da tarefa",
      "area": "Estudos",
      "priority": "Alta",
      "type": "Tarefa",
      "estimate": "30min",
      "temporal_status": "deadline",
      "reason": "Motivo factual."
    }
  ],
  "risks": [
    {
      "task": "Nome da tarefa",
      "level": "Médio",
      "reason": "Risco identificado a partir dos dados."
    }
  ],
  "recommendations": [
    {
      "task": "Nome da tarefa",
      "action": "Priorizar."
    }
  ],
  "planning_notes": []
}
```

O JSON deve ser válido e não deve conter Markdown, blocos de código ou texto adicional.

---

## 10.4 Relação entre as duas estruturas

As duas Data Structures possuem funções diferentes:

```text
Trello + Calendar
       │
       ▼
POS_CONTEXT_v2
       │
       ▼
Gemini
       │
       ▼
JSON retornado
       │
       ▼
GeminiAnalysisResponse
       │
       ▼
Preparação da análise
       │
       ▼
Mensagem HTML
       │
       ▼
Gmail
```

`POS_CONTEXT_v2` é, portanto, o **contrato de entrada da IA**, enquanto `GeminiAnalysisResponse` é o **contrato de saída da IA**.

Manter essas estruturas estáveis facilita a manutenção do cenário e reduz a dependência entre os módulos.

---

# 11. Gmail

O Gmail é utilizado exclusivamente para entregar a análise ao usuário.

O cenário obtém o endereço da conta Google autenticada por meio da API do Gmail e utiliza esse endereço como destinatário.

Durante a implementação, a tentativa de obter o e-mail por meio do Trello não foi utilizada porque a API retornou o campo de e-mail indisponível. A solução adotada foi utilizar a API do Gmail:

```text
GET /v1/users/me/profile
```

O endereço utilizado pelo cenário é obtido do campo:

```text
body.emailAddress
```

## Permissões Gmail

A conexão Google utilizada para o Gmail precisou ser reautorizada após a inclusão dos escopos necessários.

Foram utilizados, entre outros, escopos para:

```text
userinfo.email
userinfo.profile
openid
gmail.readonly
```

A necessidade de reautorizar a conexão após alteração de permissões é esperada no fluxo OAuth. O Make também documenta que erros como `403 insufficient permission` podem ocorrer quando os escopos necessários não estão configurados e que a conexão deve ser reautorizada após a correção. citeturn0search4

> **Importante:** cada usuário deve autorizar sua própria conta Gmail.

---

# 12. Formato do e-mail

O e-mail enviado pelo sistema utiliza **Raw HTML**.

A estrutura da mensagem é:

```html
<h2>🧠 Análise do seu POS</h2>

<p>[Resumo]</p>

<h3>⚠️ Atenção</h3>
<ul>
  <li>[Risco 1]</li>
  <li>[Risco 2]</li>
</ul>

<h3>💡 Recomendações</h3>
<ul>
  <li>[Tarefa 1]</li>
  <li>[Tarefa 2]</li>
</ul>

<h3>📌 Pontos observados</h3>
<ul>
  <li>[Fato 1]</li>
  <li>[Fato 2]</li>
</ul>

<h3>📅 Observações de planejamento</h3>
<ul>
  <li>[Observação 1]</li>
  <li>[Observação 2]</li>
</ul>
```

As seções são condicionais. Quando não existem itens para uma determinada seção, ela não é exibida.

Ordem final:

1. Resumo
2. Atenção
3. Recomendações
4. Pontos observados
5. Observações de planejamento

---

# 13. Checklist de instalação

### Trello

- [ ] Acessei o quadro compartilhado.
- [ ] Fiz uma cópia para meu uso.
- [ ] Mantive as seis listas originais.
- [ ] Configurei `Meu Perfil`.
- [ ] Configurei `Minha Rotina`.
- [ ] Configurei `Minhas Áreas`.
- [ ] Configurei `Minhas Preferências`.
- [ ] Adaptei os cards de acordo com minha rotina.
- [ ] Mantive os cards `[MODELO]`.

### Make

- [ ] Importe/copie o cenário.
- [ ] Configurei o meu Trello.
- [ ] Configurei minha conexão Google Calendar.
- [ ] Configurei minha conexão Gmail.
- [ ] Configurei a conexão de IA.
- [ ] Verifiquei os IDs/referências do quadro e listas.
- [ ] Salvei o cenário.
- [ ] Executei um teste manual.

### Google

- [ ] Autorizei o acesso ao Google Calendar.
- [ ] Autorizei o acesso ao Gmail.
- [ ] Concedi as permissões solicitadas.
- [ ] Reautorizei a conexão caso novas permissões tenham sido adicionadas.
- [ ] Confirmei que o Gmail consegue identificar o e-mail da conta autenticada.

---

# 14. Teste de funcionamento

Após configurar o cenário, execute uma operação manual.

Verifique se:

1. O Trello retorna as listas.
2. Os cards são encontrados.
3. Os cards `[MODELO]` são ignorados.
4. A configuração é carregada.
5. Os eventos do Calendar são encontrados.
6. `POS_CONTEXT_v2` é gerado.
7. O Gemini retorna JSON válido.
8. O JSON é interpretado corretamente.
9. A mensagem é montada.
10. O Gmail identifica o endereço da conta.
11. O e-mail é enviado.
12. O e-mail apresenta corretamente as seções HTML.

---

# 15. Regras importantes

### Não altere as listas

Os nomes e a estrutura das seis listas são parte do contrato utilizado pela automação.

### Não compartilhe credenciais

Conexões, tokens, Client Secrets e informações de autenticação devem permanecer fora do repositório.

### Não coloque dados pessoais no template

O quadro distribuído deve conter apenas estrutura e exemplos genéricos.

### Não trate a IA como executor

A IA produz análise e recomendações. A decisão e execução continuam sendo responsabilidade do usuário.

### Não hardcode rotinas

Horários, áreas, preferências e compromissos pertencem à configuração de cada usuário.

---

# 16. Arquitetura resumida

```text
                 ┌──────────────────┐
                 │      Trello      │
                 │                  │
                 │ Tarefas          │
                 │ Projetos         │
                 │ Configuração     │
                 └────────┬─────────┘
                          │
                          │
                 ┌────────▼─────────┐
                 │       Make       │
                 │                  │
                 │ Coleta           │
                 │ Filtragem        │
                 │ Consolidação     │
                 └───────┬──────────┘
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
      Google Calendar             Gemini
             │                       │
             │                 Análise IA
             │                       │
             └───────────┬───────────┘
                         ▼
                 Montagem da mensagem
                         │
                         ▼
                       Gmail
                         │
                         ▼
                 Análise do usuário
```

---

## 17. Princípio do projeto

> **O usuário configura os dados; o sistema fornece a lógica.**

O Meu POS foi desenvolvido para que a mesma estrutura possa ser utilizada por pessoas com rotinas diferentes.

A estrutura do sistema permanece estável; o conteúdo do POS é individual; e a análise é produzida dinamicamente a partir do contexto fornecido.
