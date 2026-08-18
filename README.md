# 🛡️ Miniguia de Estudos: Segurança em LLMs (Large Language Models)

> Projeto do desafio **"Treinando uma IA de Aprendizagem"** — Curso Dados, Cybersec e IA Generativa | DIO + Bradesco
> Caderno Temático construído com apoio do **NotebookLM** como ferramenta de aprendizagem ativa.

---

## 🎯 Contexto e Objetivos

### Por que esse tema?
Escolhi estudar **segurança em aplicações com LLMs (Large Language Models)** — os riscos de Prompt Injection, vazamento de dados sensíveis, alucinação usada como vetor de ataque e as boas práticas de mitigação recomendadas por frameworks como OWASP e MITRE ATLAS.

O tema conecta diretamente as três frentes do curso:
- **Dados** → como dados sensíveis podem vazar através de respostas de modelos de IA;
- **Cybersecurity** → novos vetores de ataque que não existiam na segurança de aplicações tradicionais;
- **IA Generativa** → o próprio objeto de estudo é a tecnologia que está sendo protegida (ou atacada).

Além disso, é um assunto muito atual (2025/2026), pouco explorado por quem está começando na área, e extremamente relevante para o setor financeiro — instituições como o Bradesco já usam LLMs em atendimento, análise de dados e automação interna, o que torna a segurança desses sistemas uma prioridade real de negócio.

### Objetivos de estudo
- [x] Entender os principais tipos de ataque contra aplicações com LLMs (Prompt Injection direto e indireto, vazamento de dados sensíveis, jailbreaking, etc.);
- [x] Compreender como frameworks como o **OWASP Top 10 for LLM Applications** e o **MITRE ATLAS** classificam esses riscos;
- [x] Aprender práticas de mitigação aplicáveis no design de sistemas com IA generativa;
- [x] Construir um vocabulário técnico sólido sobre o tema para usar em entrevistas e projetos futuros;
- [x] Praticar engenharia de prompts para extrair conhecimento estruturado de uma IA (NotebookLM) a partir de fontes técnicas densas.

---

## 📚 Curadoria de Fontes

Fontes abertas selecionadas e carregadas no NotebookLM (texto/PDF):

| # | Fonte | Tipo | Link | Acesso |
|---|-------|------|------|--------|
| 1 | OWASP Top 10 for LLM Applications (2026) | PDF/Relatório técnico | https://genai.owasp.org/llm-top-10/ | ago/2026 |
| 2 | MITRE ATLAS — Adversarial Threat Landscape for AI Systems | Base de conhecimento | https://atlas.mitre.org/ | ago/2026 |
| 3 | NIST AI Risk Management Framework (AI RMF 1.0) | PDF/Relatório governamental | https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf | ago/2026 |
| 4 | Anthropic — Documentação sobre segurança e boas práticas de prompting | Documentação técnica | https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview | ago/2026 |

> 💡 Como esses frameworks são atualizados com frequência (o OWASP, por exemplo, mudou de versão 2025→2026 durante o desenvolvimento deste projeto), registrar a data de acesso ajuda a rastrear qual versão embasou cada resposta da IA.

---

## 🧪 Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Aqui documento o processo real de tentativa e erro ao interrogar o NotebookLM sobre as fontes.

### Prompt 1 — Pergunta muito aberta
**Prompt testado:**
> "O que é segurança em LLMs?"

**Resultado:** resposta correta, porém ampla e superficial — misturou conceitos gerais de segurança de dados com riscos específicos de LLM, sem ancorar a resposta em uma fonte específica (falou de arquitetura, do NIST AI RMF e do OWASP Top 10 tudo junto, sem estrutura clara).

**Ajuste feito:**
> "Com base nas fontes carregadas, liste e explique os 3 principais riscos de segurança específicos de aplicações com LLM, citando a fonte de cada um."

**Resultado:** resposta muito mais estruturada e útil. A IA identificou os 3 riscos que mais subiram no ranking do OWASP Top 10 2026 — **Prompt Injection (LLM01)**, **Sensitive Information Disclosure (LLM02)** e **Excessive Agency (LLM03)** — e, para cada um, cruzou automaticamente com a tática correspondente do MITRE ATLAS (ex: Prompt Injection → *Initial Access*, AML.TA0004) e com a categoria de risco do NIST AI 600-1. Esse cruzamento entre as três fontes é reaproveitado no resumo estruturado abaixo.

**Lição aprendida:** perguntas amplas geram respostas genéricas mesmo com boas fontes; é preciso *ancorar* o prompt nas fontes explicitamente e pedir uma quantidade limitada de itens (3, não "todos").

---

### Prompt 2 — Comparação entre frameworks
**Prompt testado:**
> "Compare o OWASP Top 10 for LLM com o MITRE ATLAS."

**Resultado:** a comparação estava correta (identificou bem que um é uma lista de riscos de aplicação e o outro é uma matriz de táticas de ataque), mas veio como um texto corrido muito longo, com 4 seções e parágrafos densos — difícil de revisar rapidamente depois.

**Ajuste feito:**
> "Compare o OWASP Top 10 for LLM e o MITRE ATLAS em uma tabela com as colunas: Objetivo, Público-alvo, Formato do conteúdo e Quando usar cada um."

**Resultado:** tabela objetiva, cruzando as quatro dimensões pedidas. Ficou claro que o OWASP é voltado a **desenvolvedores/AppSec** com foco defensivo-arquitetural, enquanto o MITRE ATLAS é voltado a **red teams e SOC** com foco ofensivo-tático (16 táticas, 178 técnicas, 37 mitigações, catalogadas a partir de mais de 68 estudos de caso reais).

**Lição aprendida:** pedir formato de saída (tabela, lista, colunas específicas) reduz ambiguidade e força a IA a estruturar melhor a comparação — e economiza MUITO tempo de leitura em relação a um texto corrido.

---

### Prompt 3 — Extração de glossário
**Prompt testado:**
> "Monte um glossário dos termos técnicos das fontes."

**Resultado:** glossário bom, mas com termos redundantes e alguns fora de escopo (ex: termos genéricos de IA não relacionados especificamente a segurança).

**Ajuste feito:**
> "Monte um glossário APENAS com termos relacionados a ataques ou defesas de segurança em LLMs mencionados nas fontes, no formato: Termo — Definição em 1 frase — Fonte."

**Resultado:** glossário enxuto, focado e rastreável — cada termo veio com a fonte de origem específica (ex: *Prompt Injection* → OWASP; *XML Structuring* → docs da Anthropic; *Robustness* → NIST AI RMF). O resultado completo, já organizado em tabela, está na seção **Miniguia de Estudo → Glossário** abaixo.

**Lição aprendida:** restringir o escopo ("APENAS") e definir o formato de cada item evita respostas "infladas" com informação irrelevante — e pedir a fonte junto ao termo cria rastreabilidade automática, sem trabalho manual extra de checagem depois.

---

### Dificuldade geral encontrada
- Perguntas muito longas (com várias subperguntas) tendiam a fazer o modelo responder só a última parte — resolvido quebrando em prompts sequenciais, um pedido por vez.

---

## 📖 Miniguia de Estudo (Entrega Final)

### 1. Resumo estruturado

**O que é Segurança em LLMs?**
É o campo da cybersecurity dedicado a identificar e mitigar riscos específicos de aplicações que usam modelos de linguagem (LLMs) — riscos que vão além das vulnerabilidades tradicionais de software, porque o próprio modelo pode ser manipulado através da linguagem natural.

**O desafio arquitetural de fundo:**
Nos LLMs atuais, não existe separação estrutural entre "instrução" (comando do sistema) e "dado" (entrada do usuário ou conteúdo externo) — tudo é processado como um único fluxo de tokens. É essa característica que torna o Prompt Injection possível: dados de fora podem se comportar como comandos.

**Os 3 riscos mais críticos (OWASP Top 10 for LLM Applications, 2026):**

| Risco | O que é | Mapeamento no MITRE ATLAS |
|---|---|---|
| **LLM01 — Prompt Injection** | Entrada (direta ou indireta, via RAG, ferramentas, arquivos) que altera o comportamento pretendido do modelo. | *Initial Access* (AML.TA0004) — o "ponto de apoio" do atacante. |
| **LLM02 — Sensitive Information Disclosure** | Exposição de dados protegidos (PII, credenciais, segredos) via respostas, logs ou dados de treinamento. | *Exfiltration* (AML.TA0010) e *Credential Access* (AML.TA0013). |
| **LLM03 — Excessive Agency** | Agentes de LLM com permissões, ferramentas ou autonomia além do necessário, executando ações destrutivas. | *Execution* (AML.TA0005) e *Impact* (AML.TA0011). |

**Como o MITRE ATLAS complementa o OWASP:**
O OWASP lista *o quê* pode dar errado na aplicação (vulnerabilidades). O MITRE ATLAS descreve *como* o atacante explora isso na prática — táticas, técnicas e estudos de caso reais, no mesmo espírito do MITRE ATT&CK usado em segurança tradicional. Um mapeia riscos de arquitetura; o outro mapeia o comportamento do adversário.

**Boas práticas de mitigação:**
- Nunca confiar cegamente na saída do modelo (validação e sanitização de output);
- Aplicar o princípio do menor privilégio a agentes de IA (evitar Excessive Agency);
- Separar claramente instruções do sistema de conteúdo gerado pelo usuário (ex: delimitadores XML);
- Monitorar e registrar interações para detectar tentativas de abuso;
- Usar frameworks de governança como o NIST AI RMF para gerenciar risco de forma estruturada, olhando para Segurança, Resiliência e Safety (segurança física/humana).

---

### 2. Glossário

*Extraído do NotebookLM a partir das fontes carregadas, com a fonte de origem de cada termo (ver Prompt 3 no troubleshooting).*

| Termo | Definição | Fonte |
|---|---|---|
| **Prompt Injection** | Modificação indesejada do comportamento de um modelo por meio de entradas arbitrárias que agem como instruções de controle. | OWASP Top 10 for LLM 2026 |
| **Context-Window Pooling** | Fusão das instruções do sistema, entrada do usuário e dados externos em um único fluxo de tokens, sem barreiras de confiança rígidas. | OWASP Top 10 for LLM 2026 |
| **Sensitive Information Disclosure** | Revelação não autorizada de dados confidenciais ou segredos de negócio via saídas ou logs do modelo. | OWASP Top 10 for LLM 2026 |
| **Excessive Agency** | Agentes de LLM executando ações indesejadas em sistemas externos por excesso de permissões, ferramentas ou autonomia. | OWASP Top 10 for LLM 2026 |
| **Data and Model Poisoning** | Injeção de dados maliciosos no treinamento/fine-tuning para implantar falhas de comportamento ocultas. | OWASP Top 10 for LLM 2026 |
| **Hidden Context Exposure** | Extração indevida de instruções internas do sistema e regras invisíveis presentes na janela de contexto. | OWASP Top 10 for LLM 2026 |
| **Unbounded Consumption** | Requisições abusivas ou loops de agentes que esgotam tokens/recursos, gerando indisponibilidade ou custo excessivo. | OWASP Top 10 for LLM 2026 |
| **Improper Output Handling** | Falha em validar/sanitizar a saída do modelo antes de repassá-la a sistemas executáveis (bancos, scripts, etc.). | OWASP Top 10 for LLM 2026 |
| **Mitigation (Mitigação)** | Prática ou controle de segurança aplicado para detectar, impedir ou limitar o impacto de ações adversárias. | MITRE ATLAS™ |
| **XML Structuring** | Uso de tags delimitadoras (ex: `<instructions>`) para separar diretivas de dados, reduzindo risco de injeção. | Anthropic — Prompt Engineering Docs |
| **Prompt Chaining** | Divisão de tarefas complexas em etapas sequenciais menores, isolando o processamento de dados. | Anthropic — Prompt Engineering Docs |
| **Robustness (Robustez)** | Capacidade do sistema de IA manter desempenho e integridade mesmo sob circunstâncias inesperadas ou ataques. | NIST AI RMF 1.0 |

---

### 3. Prompts Reutilizáveis (para futuras revisões)

```
1. "Com base nas fontes carregadas, liste os 3 principais riscos de segurança 
   em LLMs e cite a fonte de cada um."

2. "Crie um quiz de 5 perguntas de múltipla escolha sobre [tema] baseado 
   apenas nas fontes carregadas, com gabarito no final."

3. "Explique [conceito técnico] como se estivesse ensinando para alguém 
   que já entende de cybersecurity tradicional, mas é novo em segurança de IA."

4. "Monte uma tabela comparativa entre [Framework A] e [Framework B] com 
   as colunas: Objetivo, Escopo, Formato e Quando usar."

5. "Monte um glossário APENAS com termos relacionados a [tema específico], 
   no formato: Termo — Definição em 1 frase — Fonte."

6. "Quais dessas informações já estão desatualizadas ou podem ter mudado 
   desde a publicação da fonte? Aponte trechos que merecem verificação."
```

---

## 🚀 Como usar este repositório
Este README concentra toda a documentação do desafio, do contexto inicial à entrega final. Sinta-se à vontade para usar essa mesma estrutura como ponto de partida para seus próprios cadernos temáticos no NotebookLM.

**Autor(a):** João Gabriel
**Curso:** Dados, Cybersecurity e IA Generativa — DIO + Bradesco
