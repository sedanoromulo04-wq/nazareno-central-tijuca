# Integrações: o que entra, o que fica de fora

Pergunta do dono: **o que realmente integrar e o que seria sobrecarga?** Critério: integra-se o que remove trabalho humano **recorrente** ou garante a segurança e a verificação; o resto espera por necessidade comprovada.

## 1. Tabela de decisão

| Ferramenta | Decisão | Quando | Motivo |
| --- | --- | --- | --- |
| **ffmpeg nativo** | Integrar | Fase 3 | Corte, legenda queimada, áudio e imagem; local, rápido, sem custo |
| **Whisper (local)** | Integrar | Fase 3 | Transcrição com tempo por palavra: base da legenda e da seleção de trechos |
| **Spec JSON + validador** | Integrar | Fase 1 | Habilita edição e re-render; verificável por código |
| **Servidor local + interface web** | Integrar | Fase 4 | Poder de edição do dono; elimina dependências frágeis |
| **API de imagem (OpenAI)** | Integrar, com trava | Fase 5 | Geração e edição de imagem; chave em ambiente, custo registrado, aprovação por geração |
| **Índice do Drive** | Integrar | Fase 2 | Material pesado fica lá; a central só indexa |
| **Recorte de pessoa por máscara** | Depois | Se necessário | Para legenda acima da cabeça; custo alto de engenharia |
| **Remotion** | Depois, se necessário | Verificar licença | Licença gratuita só para pessoa física, até 3 pessoas ou sem fins lucrativos; alguns tipos de organização exigem permissão escrita |
| **Editor de linha do tempo completo** | Não | n/a | Sobrecarga; edição por parâmetros cobre o requisito |
| **ffmpeg.wasm (no navegador)** | Não | n/a | ~31 MB, cabeçalhos especiais, mais lento; há computador local |
| **HeyGen e avatar de IA** | Não | n/a | Pessoa real e confiança importam; risco de imagem e voz |
| **Espelho em Notion** | Não | n/a | Fonte de fragilidade na Central anterior |
| **Artifact do claude.ai como painel** | Não | n/a | Idem |
| **Operário agendado no Windows** | Não | n/a | Idem; execução sob demanda |
| **Radar de tendências automático** | Não | n/a | Sem decisão que dele dependa agora |
| **Lentes de copy (F1 a F7)** | Não | n/a | Excessivo para o volume; voz e restrições bastam |
| **Coordenação de frentes** | Não | n/a | Específica de múltiplas sessões paralelas |

## 2. API de imagem

Fatos verificados e não verificados (pesquisa da mesa):

| Afirmação | Situação |
| --- | --- |
| Existe o modelo `gpt-image-2` na Images API, para geração e edição | Confirmado |
| O id "gpt 2.6" citado pelo dono | **Não confirmado** na API da OpenAI. O nome aparece em ferramentas de terceiros (Higgsfield expõe `gpt_image_2_6`) |
| Aposentadoria do `gpt-image-2` em 10/02/2027 | Informação de agregador, **não confirmada** na fonte primária |
| Preço por imagem | Fontes de terceiros divergem (US$ 0,008 a US$ 0,053); consultar preço oficial antes de orçar |

Consequência de desenho: **o id do modelo é configuração** (`projeto/capacidades.json`), nunca valor fixo no código. Antes da Fase 5: confirmar o id real, o preço, e se há chave e crédito.

Usos previstos (a confirmar com o dono): capa de peça, fundo, ilustração de parábola. Fotografia de pessoa real por IA fica **proibida** pelas restrições do projeto.

Travas:

1. Chave só em variável de ambiente.
2. Aprovação humana antes de cada geração.
3. Custo estimado e real registrados por peça.
4. Prompt sem dados pessoais.
5. Imagem gerada recebe marca de origem na spec (`origem: gerada`) para o portão de divulgação de IA.

## 3. Conectores disponíveis na sessão

O ambiente do Claude Code já expõe conectores (Google Drive, Canva, Higgsfield, Notion, Miro). Regra: **disponível não significa integrado**. Cada um só entra se resolver uma dor registrada. Em particular, a edição de vídeo da Higgsfield (editing room) é alternativa a avaliar contra o ffmpeg local, mas depende de serviço externo e créditos; não entra na base.

## 4. Publicação

A API oficial de publicação do Instagram existe, mas as páginas da Meta divergem no limite diário (de 50 a 100 posts por 24 horas). Para o volume da igreja isso não restringe. Decisão: **publicação é manual** na Fase 5; automatizar via API só depois de o fluxo estar estável e com aprovação explícita do dono.

## 5. Como uma integração nova é aceita

1. Descrever a dor recorrente que resolve.
2. Estimar custo (dinheiro, manutenção, risco de licença e privacidade).
3. Verificar licença e termos com fonte.
4. Prova pequena em uma peça real.
5. Registrar em `DECISOES.md`.
Sem os passos 1 a 3, a integração não entra.
