# Glossário Técnico: Segurança em LLM e Engenharia de Prompts

Este documento estabelece o referencial técnico e estratégico para a governança de sistemas baseados em Modelos de Linguagem de Larga Escala (LLM), integrando frameworks de vulnerabilidades, táticas adversárias, gestão de risco organizacional e técnicas de engenharia de precisão.

1. Vulnerabilidades e Riscos: OWASP Top 10 for LLM Applications (2026)

O framework OWASP Top 10 para Aplicações de LLM (Versão 2026) consolidou-se como o padrão de ouro para a segurança de sistemas inteligentes, marcando a transição crítica da visão de "modelos como componentes" para "modelos como agentes autônomos". Diferente de versões anteriores baseadas apenas no julgamento de especialistas, a edição de 2026 fundamenta-se em uma análise rigorosa de 7.714 incidentes reais. Como Arquitetos de Governança, devemos observar o "gap" estratégico revelado pelos dados: enquanto profissionais de segurança ainda priorizam a Injeção de Prompt como o maior risco percebido, as evidências de incidentes mostram que a Desinformação (Misinformation) possui um volume de ocorrências significativamente maior no mundo real. Esta discrepância exige uma defesa arquitetural que não apenas barre ataques, mas que trate o sistema como inerentemente falível.

#### Injeção de Prompt (Prompt Injection) — LLM01:2026

* Termo Original: Prompt Injection
* Definição: Vulnerabilidade que ocorre quando entradas enviadas ao modelo — sejam diretas do usuário, conteúdos recuperados (RAG), saídas de ferramentas, imagens ou áudio — alteram o comportamento do LLM de formas não pretendidas pelo desenvolvedor.
* Análise de Impacto ("So What?"): A falha reside na ausência de distinção arquitetural entre instruções e dados; ambos são tratados como um fluxo único de tokens. Isso permite que um atacante manipule o modelo para exfiltrar dados, executar comandos não autorizados ou comprometer a lógica da aplicação através de canais multimodais ou esteganográficos.
* Estratégias de Mitigação:
  * Implementação da "Rule of Two" (Regra de Dois): Garantir que nenhum agente tenha acesso simultâneo a dados sensíveis, ingestão de conteúdo não confiável e capacidade de comunicação externa.
  * Uso de declarações declarativas de permissão (allow/deny) estritas no prompt de sistema.
  * Mediação por mecanismos de controle determinísticos para validar intenções antes da execução de ações irreversíveis.

#### Agrupamento da Janela de Contexto (Context-Window Pooling)

* Termo Original: Context-Window Pooling
* Definição: Propriedade de implantação onde o modelo processa o prompt de sistema, entradas de usuário, documentos recuperados e histórico como um único fluxo de tokens, sem barreiras de confiança.
* Análise de Impacto ("So What?"): A fusão de fluxos de tokens de diferentes origens e níveis de confiança facilita ataques onde instruções maliciosas escondidas em dados externos (como um e-mail ou documento via RAG) sobrepõem as diretrizes de segurança do sistema, explorando a falta de segmentação estrutural.
* Estratégias de Mitigação:
  * Adoção de XML Structuring (padrão Claude): Uso de tags como <context> e <instructions> para segmentar dados e mitigar riscos de evasão.
  * Passagem de conteúdo externo através de canais estruturalmente separados e rotulados por proveniência (provenance-labeled channels).

#### Divulgação de Informações Sensíveis — LLM02:2026

* Termo Original: Sensitive Information Disclosure
* Definição: Exposição de dados confidenciais, PII, PHI ou segredos comerciais através de outputs, argumentos de ferramentas, logs, telemetria ou canais secundários de inferência (como tempo e comprimento de tokens).
* Análise de Impacto ("So What?"): O impacto é massivo em termos regulatórios (GDPR, LGPD, EU AI Act), pois dados sensíveis podem ser extraídos através de "divergência de prompt" ou inferência de membros em modelos fine-tuned, expondo a organização a sanções civis e perda de propriedade intelectual.
* Estratégias de Mitigação (Estrutura em Tiers):
  * Tier 1 (Foundational): Governança de corpora com scrubbing de PII no ingest; higienização de prompts de sistema (não armazenar segredos neles).
  * Tier 2 (Hardening): Implementação de DP-SGD (Privacidade Diferencial) em fine-tuning; classificação e redação de reasoning traces antes do log.
  * Tier 3 (Advanced): Uso de Computação Confidencial (Enclaves AWS Nitro/Intel TDX) e frameworks de unlearning verificáveis para remoção de dados conforme o "direito ao esquecimento".

#### Agência Excessiva — LLM03:2026

* Termo Original: Excessive Agency
* Definição: Vulnerabilidade onde o sistema concede funcionalidades, permissões ou autonomia exageradas a um agente, permitindo ações prejudiciais em resposta a outputs manipulados ou alucinações.
* Análise de Impacto ("So What?"): Cria a "Lethal Trifecta" (Tríade Letal): acesso a dados sensíveis + ingestão de conteúdo não confiável + capacidade de impacto externo. Isso amplia o Blast Radius (Raio de Explosão), transformando uma simples falha de saída em um incidente hostil, como deleção de bancos de dados ou exfiltração em massa via API.
* Estratégias de Mitigação:
  * Aplicação rigorosa do Princípio do Menor Privilégio para ferramentas (ex: ferramentas de leitura não devem ter permissão de escrita).
  * Adoção da "Rule of Two" como piso de segurança arquitetural.
  * Exigir aprovação humana explícita para ações de alto impacto e irreversíveis.

#### Envenenamento de Dados e Modelos — LLM05:2026

* Termo Original: Data and Model Poisoning
* Definição: Manipulação da integridade de conjuntos de dados ou artefatos de modelo (pesos, adaptadores LoRA) para introduzir comportamentos maliciosos ocultos.
* Análise de Impacto ("So What?"): Permite a criação de "sleeper agents" ou backdoors na cadeia de suprimentos de IA. O modelo pode funcionar perfeitamente até encontrar um gatilho específico que ative um comportamento de exfiltração ou viés decisório.
* Estratégias de Mitigação:
  * Vetting rigoroso de fornecedores e uso de inventários assinados (AIBOM/SBOM).
  * Monitoramento de overfitting em fine-tuning como proxy para detecção de memorização maliciosa.

#### Exposição de Contexto Oculto — LLM08:2026

* Termo Original: Hidden Context Exposure
* Definição: Termo que expande e substitui o antigo "System Prompt Leakage", referindo-se à extração de instruções de sistema, regras de ferramentas e metadados operacionais.
* Análise de Impacto ("So What?"): A revelação do contexto oculto permite que adversários realizem engenharia reversa das defesas da aplicação, facilitando a criação de payloads de injeção mais precisos e evasivos.
* Estratégias de Mitigação:
  * Não armazenar chaves de API ou segredos nos prompts.
  * Monitoramento de consultas que buscam enumerar capacidades ou extrair o "extended thinking" do modelo.

#### Consumo Ilimitado — LLM06:2026

* Termo Original: Unbounded Consumption
* Definição: Falha em estabelecer limites econômicos e computacionais para o uso do LLM.
* Análise de Impacto ("So What?"): Resulta em incidentes de "Denial-of-Wallet" (exaustão financeira) ou negação de serviço (DoS) devido a queries recursivas ou agenticas descontroladas.
* Estratégias de Mitigação:
  * Implementação de circuit breakers para execuções agenticas.
  * Rate limiting granular por usuário e por custo de token.

#### Tratamento Inadequado de Saídas — LLM10:2026

* Termo Original: Improper Output Handling
* Definição: Aceitação cega de saídas do modelo sem sanitização, permitindo que estas alcancem componentes críticos downstream.
* Análise de Impacto ("So What?"): Pode comprometer intérpretes de shell, navegadores ou bancos de dados (SQL Injection via LLM) se o código ou comando gerado for executado sem isolamento.
* Estratégias de Mitigação:
  * Validação estrutural rigorosa via schema (JSON/XML).
  * Execução de saídas em ambientes isolados (sandboxing).

A compreensão dessas falhas internas é o alicerce para mapear como adversários externos exploram tais sistemas, exigindo uma transição para a taxonomia de ataque externa do framework MITRE ATLAS™.

2. Paisagem de Ameaças Adversárias: MITRE ATLAS™

O MITRE ATLAS™ (Adversarial Threat Landscape for Artificial-Intelligence Systems) funciona como uma base de conhecimento global de táticas e técnicas adversárias. Ele permite que equipes de segurança mapeiem ataques reais e demonstrações de red teaming contra sistemas de IA, permitindo a transposição da teoria para o monitoramento operacional no SOC.

* ATLAS (AML.TAXXXX): Repositório vivo que organiza como os adversários operam, permitindo a modelagem de ameaças baseada em evidências.
* Tactic (Tática - AML.TAXXXX): O objetivo estratégico do adversário (ex: Exfiltration vs. Impact). É fundamental para entender o "porquê" do ataque.
* Technique (Técnica - AML.TXXXX): Ações técnicas específicas que materializam as táticas. Exemplos incluem LLM Jailbreak (AML.T0054), RAG Poisoning (AML.T0070) e LLM Prompt Injection (AML.T0051). Incluir esses identificadores em ferramentas de SIEM é obrigatório para conformidade técnica.
* Mitigation (Mitigação): Defesa operacional para interromper a kill chain. O ATLAS lista 37 mitigações focadas em robustez e resiliência.

Entender o ataque é o primeiro passo para a governança estruturada, servindo de ponte para os processos de gestão de risco organizacionais do NIST.

3. Governança e Gestão de Risco: NIST AI RMF 1.0

O NIST AI Risk Management Framework 1.0 estabelece a abordagem organizacional para cultivar confiança e gerenciar riscos sistêmicos. Ele conecta a conformidade técnica (como o EU AI Act e GDPR) à responsabilidade corporativa.

* AI Actor (Ator de IA): Indivíduos ou grupos com responsabilidades no ciclo de vida da IA. O arquiteto deve garantir que desenvolvedores, auditores e usuários finais compreendam suas obrigações de conformidade.
* Trustworthiness (Confiabilidade): Conjunto de dimensões críticas (resiliência, explicabilidade, privacidade, segurança e justiça) tratadas como requisitos não funcionais.
* Govern, Map, Measure, Manage: Funções que operacionalizam o risco:
  * Govern: Estabelece a cultura de risco e políticas.
  * Map: Identifica contextos e riscos (conexão com OWASP).
  * Measure: Analisa e quantifica (conexão com ATLAS para medir eficácia de controles).
  * Manage: Aloca recursos para mitigar riscos prioritários, garantindo conformidade regulatória.
* Robustness (Robustez): Capacidade de manter o desempenho sob condições adversas ou entradas inesperadas.
* Socio-technical Approach (Abordagem Sociotécnica): Visão de que o risco de IA não é apenas um bug no código, mas uma interação complexa com dados, pessoas e valores sociais.

Um sistema robusto e governado depende da precisão das instruções fornecidas, o que introduz a engenharia de prompts como ferramenta técnica de controle.

4. Otimização e Controle: Claude Platform Prompt Engineering

A engenharia de prompts na Plataforma Claude é tratada como uma disciplina técnica de otimização, fundamental para reduzir a estocasticidade e garantir que a segurança pretendida se manifeste na operação.

* Prompt Engineering: Ciclo de refinamento contínuo baseado em critérios de sucesso e segurança, visando a previsibilidade do comportamento do agente.
* Metaprompt: Uso de geradores de prompt para criar estruturas robustas prontas para produção. Funciona como um controle arquitetural que garante que salvaguardas (como a "Rule of Two") sejam aplicadas programaticamente, reduzindo o erro humano.
* XML Structuring (Tags XML): Padrão recomendado pela Anthropic para o Claude, utilizando tags como <context>, <instructions> e <data>. Esta é a defesa técnica primordial contra o Context-Window Pooling, forçando o modelo a reconhecer as fronteiras entre fontes de dados confiáveis e não confiáveis.
* Prompt Chaining (Encadeamento): Decomposição de tarefas complexas em subtarefas. Reduz o Blast Radius ao isolar permissões específicas em cada etapa, melhorando a latência, a consistência e o controle de custos (circuit breaker financeiro).

A combinação de governança (NIST), mapeamento de ameaças (ATLAS), controle de vulnerabilidades (OWASP) e engenharia de precisão (Claude) forma a estratégia de defesa em profundidade indispensável para a resiliência de sistemas de IA modernos.
