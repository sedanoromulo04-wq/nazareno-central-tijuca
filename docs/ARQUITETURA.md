# Arquitetura

Status: **proposta**, a validar antes de qualquer código. Itens marcados *(hipótese)* dependem de respostas em `PENDENCIAS.md`.

## 1. Forma geral

Um **app local**: um servidor que roda no computador do usuário e uma interface web aberta no navegador. Claude Code constrói e mantém o sistema; skills e scripts fazem o trabalho em cadeia.

Por que local: o usuário tem computador próprio; ffmpeg nativo é mais rápido e simples que no navegador; os vídeos estão no Drive e podem ter proxy local; elimina serviços agendados frágeis (lição da Central anterior).

```
 Google Drive ──índice──▶ ┌────────────────── Servidor local ──────────────────┐
 (material pesado)        │  API  │ Estado/ledger │ Portões │ Fila de trabalho  │
                          └───┬──────────┬───────────┬──────────────┬───────────┘
                              │          │           │              │
                    Interface web   Renderizador   Skills/agentes   Wrapper de imagem
                    (revisão e      (ffmpeg,       (propor trecho,  (API de imagem,
                     edição)        Whisper)       legenda, texto)   custo registrado)
```

## 2. Componentes

| Componente | Pasta | Responsabilidade | Fase |
| --- | --- | --- | --- |
| **Índice de mídia** | `nucleo/indice/` | Lê o Drive (leitura), grava Evento/Material em `dados/indice/`. Nunca baixa tudo | 2 |
| **Armazém de estado** | `nucleo/estado/` | Peça, Versão, Aprovação; ledger append-only; valida transições | 1 |
| **Validador de spec** | `nucleo/spec/` | Esquema JSON da Versão; recusa spec inválida | 1 |
| **Portões** | `nucleo/portoes/` | Verificadores automáticos; interface para verificadores do canal e do projeto | 1 e 3 |
| **Renderizador** | `nucleo/render/` | Spec → vídeo: cortes, legenda queimada, tratamento, via ffmpeg | 3 |
| **Transcrição** | `nucleo/render/` | Whisper local, tempo por palavra | 3 |
| **Servidor local** | `nucleo/servidor/` | API HTTP local, fila, entrega de prévias | 4 |
| **Interface de revisão** | `nucleo/servidor/web/` | Telas de fila, revisão, edição por campos, aprovação | 4 |
| **Wrapper de imagem** | `nucleo/imagem/` | Chama a API de imagem; id do modelo por configuração; custo; aprovação por geração | 5 |
| **Pacote de publicação** | `nucleo/publicacao/` | Reúne arquivo, legenda, horário sugerido, hipótese | 5 |
| **Skills do núcleo** | `nucleo/skills/` | Skills genéricas que leem `projeto/` | 3 em diante |

## 3. Fluxo de uma peça (corte de pregação)

1. **Indexar.** Culto no Drive é indexado: lista de vídeos, durações, categoria.
2. **Transcrever.** Whisper local gera texto com tempo por palavra (derivado, fora do git).
3. **Propor trechos.** Agente lê a transcrição e propõe N trechos com resumo, respeitando `voz.md` e `restricoes.md`. Devolve **destilado**, não a transcrição inteira.
4. **Escolher.** Humano escolhe trechos na interface (ou aceita em lote).
5. **Montar a spec.** Sistema cria a Peça em `pauta`, com Versão 1 (cortes, legenda palavra a palavra, tratamento padrão do projeto), hipótese e métrica.
6. **Portão automático.** Verificadores conferem a spec antes de gastar render.
7. **Renderizar.** ffmpeg gera a prévia a partir da spec. Estado `produzida`.
8. **Revisar e editar.** Humano vê a prévia, edita campos (legenda, cortes, imagens), re-renderiza. Cada edição é nova Versão.
9. **Portões editorial e final.** Humano aprova ou reprova com critério.
10. **Pacote.** Peça `aprovada` gera pacote de publicação em `dados/pacotes/`.
11. **Publicar.** Humano publica fora da central e registra (data, link).

Cortes de louvor seguem o mesmo fluxo com outras regras (sem transcrição de fala; seleção por música; restrição de direito musical). Motivacionais começam por texto e imagem (sem corte de vídeo) e entram pelo passo 5.

## 4. Edição por parâmetros

A decisão central do desenho. A peça é um JSON; a interface edita campos e pede re-render.

| Ação do usuário | Campo alterado | Custo de render |
| --- | --- | --- |
| Corrigir uma palavra da legenda | `legenda.palavras[i].texto` | Baixo (só a camada de legenda) |
| Inserir ou excluir texto | `texto_publicacao`, camadas de texto | Baixo |
| Trocar imagem | `imagens[i].ref` | Médio |
| Ajustar início ou fim de corte | `cortes[i]` | Médio a alto |
| Pedir imagem gerada | `imagens[i]` com `origem: gerada` | API (custo e aprovação) |
| Ligar tratamento de áudio ou imagem | `audio`, `tratamento_imagem` | Médio |

Prévia: *(hipótese)* renderização de prévia em baixa resolução, com espera de segundos a minutos. Prévia instantânea no navegador só se for necessária, e custaria complexidade; decisão em `PENDENCIAS.md`.

## 5. Fronteira Drive e repositório

| Vai para o Drive | Vai para o git |
| --- | --- |
| Vídeos, fotos, áudio brutos | Índice (ids, nomes, durações, categorias) |
| Renders finais e prévias | Specs (JSON) das peças |
| Imagens geradas | Ledger |
| Transcrições brutas grandes | Regras aprendidas, decisões, docs |

Regra de segurança: `.gitignore` bloqueia extensões de mídia e a pasta `dados/local/`.

Acesso ao Drive: *(hipótese)* leitura via conector ou pasta sincronizada no computador. O link anterior da igreja retornou 401 no repositório antigo; resolver acesso é pré-requisito da Fase 2.

## 6. Segredos e configuração

| Item | Onde |
| --- | --- |
| Chave da API de imagem | Variável de ambiente (`.env` local, ignorado pelo git) |
| Id do modelo de imagem | `projeto/capacidades.json`; **não confirmado** como "gpt 2.6"; ver `INTEGRACOES.md` |
| Caminho do Drive/proxy local | Configuração local, não versionada |

## 7. Segurança e privacidade

- Aprovação humana obrigatória antes de gerar imagem e antes de publicar.
- Material com pessoas identificáveis e sem autorização confirmada não entra em peça.
- Menores de idade: regra restritiva em `projeto/restricoes.md`, com portão em código quando possível.
- Nenhum dado pessoal em chamadas a serviços externos além do estritamente necessário (prompt da imagem não carrega nome de pessoa).
- O servidor escuta apenas em `127.0.0.1`.

## 8. Tecnologias propostas

| Necessidade | Escolha proposta | Alternativa descartada | Motivo |
| --- | --- | --- | --- |
| Vídeo | ffmpeg nativo | ffmpeg.wasm | ~31 MB, exige cabeçalhos especiais, mais lento |
| Transcrição | Whisper local | Serviço pago | Tempo por palavra, sem custo por uso, privacidade |
| Legenda | libass (via ffmpeg) | Remotion | Licença exige verificação para organização; só se necessário |
| Servidor | A definir na Fase 4 (Node ou Python) | n/d | Escolher pelo que o usuário já mantém: ver `PENDENCIAS.md` |
| Interface | Web simples, sem framework pesado | SPA complexa | Manutenibilidade |

## 9. Observabilidade

- Ledger de transições.
- Registro de custo de geração por peça.
- Relatório semanal: peças por estado, taxa de aprovação, retrabalho, custo.

## 10. Como o núcleo continua genérico

Nenhum componente acima conhece "igreja". Entradas específicas chegam por `projeto/` (categorias de material, linhas de conteúdo, tipos de evento) e por `canais/` (formatos, limites). O núcleo valida o formato dessas configurações, não o conteúdo.
