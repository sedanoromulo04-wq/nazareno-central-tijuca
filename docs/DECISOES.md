# Decisões

Registro datado. Uma decisão por bloco; não se edita o passado, acrescenta-se uma nova que o revoga.

Formato: **D-NNN · data · decisão** → motivo → alternativa descartada → consequência.

---

### D-001 · 2026-11-26 · Projeto da igreja em repositório próprio
Motivo: o `minha-empresa` mistura projetos, skills e ferramentas, o que contamina o contexto de cada um.
Descartado: continuar a igreja dentro do `minha-empresa`.
Consequência: o `minha-empresa` segue ativo para os demais projetos; a igreja não depende dele.

### D-002 · 2026-11-26 · Forma de app local
Motivo: o usuário tem computador próprio; ffmpeg nativo; elimina dependências frágeis da Central anterior (Notion, artifact do claude.ai, tarefa agendada no Windows).
Descartado: PWA em nuvem e ffmpeg no navegador.
Consequência: acesso por celular fica em aberto (`PENDENCIAS.md`, P-07).

### D-003 · 2026-11-26 · Peso no Drive, só índice e decisão no git
Motivo: repositório leve; respeita direito de imagem; evita vazamento.
Consequência: `.gitignore` bloqueia mídia; índice em `dados/indice/`.

### D-004 · 2026-11-26 · Publicação é ato humano
Motivo: segurança editorial e de imagem; confiança da liderança.
Consequência: nenhum componente publica sozinho.

### D-005 · 2026-11-26 · Edição por parâmetros, não por linha do tempo
Motivo: cobre o requisito de edição humana com custo muito menor.
Consequência: a peça é uma spec JSON; cada edição é uma nova versão.

### D-006 · 2026-11-26 · Estados enxutos
Estados: `pauta`, `produzida`, `aprovada`, `publicada`, mais `reprovada`, `travada` e `arquivada`.
Motivo: etapa sem decisão não merece estado.
Descartado: estado `roteiro` separado.

### D-007 · 2026-11-26 · Esqueleto em três camadas
Núcleo, Canal, Projeto; a camada de baixo nunca cita a de cima.
Motivo: impedir que regra de um projeto vire regra de todos.
Consequência: `CAMADAS.md` é lei de fronteira.

### D-008 · 2026-11-26 · Critério anti-framework
Só vira configuração o que já variou entre dois projetos reais (igreja e Rio Sol).
Consequência: a lista de pontos de variação é curta de propósito.

### D-009 · 2026-11-26 · Esqueleto distribuído como repositório-modelo
Motivo: isola contexto por projeto.
Custo: melhorias do núcleo não propagam sozinhas.
Consequência: `nucleo/VERSAO`; script de atualização só após o segundo projeto. Local do repositório-modelo em aberto (P-01).

### D-010 · 2026-11-26 · Id do modelo de imagem é configuração
Motivo: "gpt 2.6" não foi confirmado na API da OpenAI; ids mudam e há relato de aposentadoria.
Consequência: `capacidades.json` guarda o id; confirmar antes da Fase 5.

### D-011 · 2026-11-26 · Fase 0 só com documentos
Motivo: lema "organização antes de ação".
Consequência: nenhum código antes de o dono validar os documentos (`ROTEIRO.md`).
