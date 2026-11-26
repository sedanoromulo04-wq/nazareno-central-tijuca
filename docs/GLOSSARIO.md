# Glossário

Um nome por conceito. Se um termo novo aparece em outro documento, entra aqui primeiro.

| Termo | Definição |
| --- | --- |
| **Central** | Este sistema: servidor local, interface, motor e dados |
| **Esqueleto** | A parte reaproveitável: `nucleo/` e `canais/`, mais os modelos de `projeto/` |
| **Núcleo** | Motor genérico, sem conhecimento de plataforma ou cliente |
| **Canal** | Plataforma de destino e suas regras (ex.: Instagram) |
| **Projeto** | Contexto específico: voz, restrições, público, aprovadores, estilo |
| **Evento** | Ocasião de onde sai material. Na igreja, um **Culto** |
| **Material** | Arquivo bruto indexado (vídeo, áudio, foto). Vive no Drive |
| **Trecho** | Intervalo de um Material com valor de conteúdo |
| **Peça** | Unidade de produção e aprovação; uma publicação |
| **Linha** | Categoria de peça no projeto. Na igreja: corte de pregação, corte de louvor, motivacional |
| **Formato** | Forma na plataforma: reel, carrossel, post, story |
| **Versão** | Estado editável de uma peça, descrito por uma spec JSON |
| **Spec** | O JSON de parâmetros que descreve a peça e permite re-render |
| **Render** | Gerar o arquivo de mídia a partir da spec |
| **Prévia** | Render leve usado só para revisão |
| **Portão** | Verificação que uma peça precisa passar. Automático, editorial ou final |
| **Verificador** | Código que confere um critério de portão |
| **Aprovação** | Registro de veredito sobre uma versão |
| **Reprovação** | Veredito negativo com critério nomeado; consome tentativa |
| **Tentativa** | Reprovação consumida; máximo 2 |
| **Travada** | Estado terminal após duas reprovações; só sai por decisão humana |
| **Ledger** | Registro imutável de transições, uma linha por fato |
| **Regra aprendida** | Regra nascida de reprovação, com id e data |
| **Capacidade** | Motor que o projeto liga ou desliga |
| **Hipótese** | Afirmação de efeito a testar, "se X então Y"; sem fonte, nada é fato |
| **Métrica** | O que será observado para julgar a hipótese |
| **Destilado** | Resumo devolvido por um agente, no lugar do material lido |
| **Mesa** | Método de debate com três especialistas para decisões de desenho |
| **Pendência** | Pergunta em aberto que bloqueia ou orienta uma decisão |
