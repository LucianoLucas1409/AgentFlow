# Decisões técnicas

## ADR-001 — Clean Architecture

**Contexto:** a aplicação integra UI, persistência, IA, diretório e sistema de chamados.

**Decisão:** separar Domain, Application, Infrastructure e Web, mantendo contratos na Application e implementações técnicas na Infrastructure.

**Consequência:** integrações podem evoluir sem espalhar detalhes técnicos pelos casos de uso. O custo é uma quantidade maior de contratos e composição no container de DI.

## ADR-002 — Planejamento estruturado com catálogo fechado

**Contexto:** permitir que o modelo escolha qualquer ferramenta ou gere parâmetros arbitrários aumenta o risco de uso incorreto.

**Decisão:** fornecer um catálogo de capacidades e exigir saída estruturada. O backend valida capability, argumentos, origem dos identificadores e limites.

**Consequência:** o modelo continua flexível para interpretar linguagem natural, mas não possui autoridade para ampliar suas próprias permissões.

## ADR-003 — Plano completo antes da execução

**Contexto:** loops sucessivos de “planejar e executar” podem reler a mesma fonte, perder dependências e aumentar latência.

**Decisão:** decompor a pergunta em objetivos e gerar o plano completo no início do turno.

**Consequência:** dependências ficam visíveis, limites podem ser aplicados antecipadamente e operações equivalentes são deduplicadas.

## ADR-004 — Fatos tipados entre etapas

**Contexto:** reutilizar texto livre produzido pelo modelo pode introduzir identificadores incorretos.

**Decisão:** capabilities publicam fatos nomeados e tipados, que são usados em bindings validados.

**Consequência:** etapas dependentes trabalham com valores rastreáveis e não com uma reinterpretação livre da conversa.

## ADR-005 — Busca híbrida

**Contexto:** pesquisa textual é precisa para códigos e erros; busca vetorial é melhor para variações de linguagem.

**Decisão:** combinar keywords, texto e similaridade por embeddings, adicionando reranking apenas quando o conjunto permanece ambíguo.

**Consequência:** maior cobertura sem abandonar correspondências exatas. O pipeline precisa de métricas e thresholds calibráveis.

## ADR-006 — Embeddings com fallback

**Contexto:** o serviço de embeddings pode ficar lento ou indisponível.

**Decisão:** aplicar timeout próprio e continuar com o ranking textual quando o vetor da consulta não puder ser obtido.

**Consequência:** perda gradual de qualidade sem indisponibilizar toda a busca.

## ADR-007 — Evidência separada de evento operacional

**Contexto:** “o AD não respondeu” não significa “o usuário não pertence ao grupo”.

**Decisão:** resultados admitidos recebem identificadores de evidência; indisponibilidade, timeout e rejeição de fonte viram eventos operacionais separados.

**Consequência:** a resposta pode explicar uma limitação sem transformá-la em fato do domínio.

## ADR-008 — Afirmações estruturadas

**Contexto:** instruir o modelo a citar fontes não garante que toda afirmação interna esteja realmente apoiada.

**Decisão:** exigir claims tipadas com referências e validar cada uma antes da renderização.

**Consequência:** afirmações não suportadas podem ser removidas deterministicamente pelo servidor.

## ADR-009 — Confirmação humana para efeitos externos

**Contexto:** uma interpretação incorreta não pode abrir, iniciar ou encerrar uma operação sem conhecimento do usuário.

**Decisão:** separar preparação, seleção, preview, confirmação e execução.

**Consequência:** o fluxo possui mais etapas, mas o efeito externo permanece explícito e auditável.

## ADR-010 — Hangfire e workers para tarefas recorrentes

**Contexto:** indexação, importação e monitoramento não devem depender de uma requisição aberta no navegador.

**Decisão:** usar jobs persistidos para agendas e workers dedicados para filas internas.

**Consequência:** tarefas podem ser retomadas e observadas, desde que idempotência e concorrência sejam tratadas em cada serviço.

## ADR-011 — MVC e JavaScript incremental

**Contexto:** o projeto nasceu e evoluiu no ecossistema ASP.NET Core sem um pipeline frontend separado.

**Decisão:** usar Razor Views, Bootstrap, JavaScript e jQuery, modularizados por feature.

**Consequência:** menor complexidade de build e entrega, com a responsabilidade de evitar arquivos monolíticos e estado global descontrolado.

## ADR-012 — Tendência como sinal, não como diagnóstico

**Contexto:** aumento de chamados correlacionados pode indicar um problema, mas não comprova sua causa.

**Decisão:** gerar alertas com baseline, amostras e termos recorrentes, mantendo reconhecimento e investigação humana.

**Consequência:** o sistema ajuda a priorizar atenção sem automatizar conclusões de causa raiz.

