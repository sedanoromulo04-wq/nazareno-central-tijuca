# Entidades: o contrato de dados

Este é o vocabulário estrutural do núcleo. Tudo o mais (interface, skills, scripts) lê e escreve estas entidades. Nomes de campo são em português, sem acento, em minúsculas com sublinhado.

> No esqueleto, a entidade "Evento" é genérica. Na igreja, um Evento é um **Culto**.

## 1. Visão geral

```
Projeto ──1:N──▶ Evento ──1:N──▶ Material ──1:N──▶ Trecho
                                                     │
                                                     ▼ (N:M)
                                        Peça ──1:N──▶ Versão ──1:N──▶ Aprovação
                                          │
                                          └──1:N──▶ Evento-de-ledger (histórico)
```

## 2. Evento (Culto)

Uma ocasião da qual sai material.

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `id` | texto | sim | `culto-AAAA-MM-DD` (no esqueleto: `evento-AAAA-MM-DD`) |
| `data` | data | sim | Data do evento |
| `tipo` | enum | sim | Valores definidos em `projeto/` (igreja: `quinta`, `domingo`, `especial`) |
| `titulo` | texto | não | Tema, se houver |
| `pregador` | texto | não | Quem ministrou |
| `passagens` | lista | não | Referências bíblicas (igreja) |
| `pasta_drive` | texto | sim | Caminho ou id da pasta no Drive |
| `indexado_em` | data-hora | não | Último índice |

## 3. Material

Um arquivo bruto no Drive. A central guarda **apenas o índice**, nunca o arquivo.

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `id` | texto | sim | Estável: hash do id do Drive |
| `evento_id` | texto | sim | Origem |
| `drive_id` | texto | sim | Id do arquivo no Drive |
| `nome` | texto | sim | Nome original |
| `tipo` | enum | sim | `video`, `audio`, `foto` |
| `duracao_s` | número | vídeo/áudio | Duração |
| `categoria` | enum | não | Definida em `projeto/` (igreja: `louvor`, `ministracao`, `pregacao`, `bastidor`, `foto`) |
| `pessoas_identificaveis` | enum | sim | `sim`, `nao`, `desconhecido`. Alimenta os portões de imagem |
| `autorizacao_imagem` | enum | sim | `confirmada`, `pendente`, `negada`, `nao_se_aplica` |
| `transcricao_ref` | texto | não | Caminho local da transcrição (arquivo derivado, fora do git) |
| `proxy_local` | texto | não | Cópia leve local para prévia (fora do git) |
| `notas` | texto | não | Observação humana |

Regra: material com `autorizacao_imagem` diferente de `confirmada` e `pessoas_identificaveis` diferente de `nao` **não pode ser usado em peça** (portão em código).

## 4. Trecho

Um intervalo de um Material com valor de conteúdo.

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `id` | texto | |
| `material_id` | texto | |
| `inicio_s`, `fim_s` | número | Limites |
| `resumo` | texto | Uma linha do que acontece |
| `origem` | enum | `humano`, `agente` |
| `pontuacao` | número | Opcional. Ordenação das propostas |

Trecho é a unidade que agentes **propõem** e humanos **escolhem**.

## 5. Peça

A unidade de produção e de aprovação. Uma publicação = uma peça.

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `id` | texto | sim | `peca-AAAAMMDD-NNN` |
| `linha` | enum | sim | Definida em `projeto/` (igreja: `corte_pregacao`, `corte_louvor`, `motivacional`) |
| `canal` | texto | sim | `instagram` etc. |
| `formato` | enum | sim | Definido pelo canal (`reel`, `carrossel`, `post`, `story`) |
| `estado` | enum | sim | Ver `ESTADOS.md` |
| `tentativas` | inteiro | sim | Reprovações consumidas (máx. 2) |
| `hipotese` | texto | sim ao produzir | "Se X, então Y, medido por Z" |
| `metrica` | texto | sim ao produzir | O que será observado |
| `versao_atual` | inteiro | sim | Aponta para a Versão corrente |
| `criada_em`, `atualizada_em` | data-hora | sim | |

## 6. Versão (a "spec" da peça)

O **JSON de parâmetros** que descreve a peça. Re-renderiza-se a partir dele. Cada edição humana cria uma nova versão; versões antigas ficam para auditoria.

```json
{
  "peca_id": "peca-20261129-001",
  "versao": 3,
  "formato": "reel",
  "duracao_alvo_s": 45,
  "cortes": [
    { "material_id": "m-abc", "inicio_s": 812.4, "fim_s": 840.1 },
    { "material_id": "m-abc", "inicio_s": 901.0, "fim_s": 918.6 }
  ],
  "legenda": {
    "modo": "palavra_a_palavra",
    "palavras": [
      { "texto": "Deus", "inicio_s": 0.0, "fim_s": 0.4, "destaque": true }
    ]
  },
  "imagens": [
    { "origem": "drive", "ref": "m-foto-12", "inicio_s": 0, "fim_s": 3 },
    { "origem": "gerada", "prompt_ref": "g-77", "inicio_s": 3, "fim_s": 6 }
  ],
  "audio": { "normalizar": true, "reduzir_ruido": true },
  "tratamento_imagem": { "perfil": "padrao_projeto" },
  "texto_publicacao": { "legenda": "…", "hashtags": [] },
  "criada_por": "humano|agente",
  "gerada_em": "2026-11-29T10:00:00-03:00"
}
```

Este exemplo é **ilustrativo**. O esquema definitivo nasce na Fase 1, com validador em código.

Campos editáveis por humanos (requisito do usuário): texto e tempo de cada palavra da legenda; inserir e excluir textos; trocar imagens; pedir imagem gerada; ajustar início e fim de cortes; ligar e desligar tratamento de áudio e imagem.

## 7. Imagem gerada

Registro de cada geração (para custo e auditoria).

| Campo | Descrição |
| --- | --- |
| `id` | |
| `peca_id` | |
| `modelo` | Lido de configuração (id do modelo **não** é fixo no código) |
| `prompt` | Texto enviado |
| `custo_estimado` | Registrado a cada chamada |
| `aprovada_por` | Quem autorizou a geração |
| `arquivo_local` | Fora do git |

Regra: **toda geração exige aprovação humana prévia** e fica registrada.

## 8. Aprovação

| Campo | Descrição |
| --- | --- |
| `id` | |
| `peca_id`, `versao` | O que foi avaliado |
| `portao` | Nome do portão (`automatico`, `editorial`, `final`) |
| `veredito` | `aprovada`, `reprovada` |
| `criterio` | Critério nomeado que decidiu (obrigatório em reprovação) |
| `por` | Pessoa ou verificador (código) |
| `em` | Data-hora |
| `motivo` | Texto curto |

Todo veredito `reprovada` gera uma **proposta de regra** (ver `ESTADOS.md`, seção de aprendizado).

## 9. Evento de ledger

Registro imutável, uma linha por fato, em `dados/ledger.jsonl`.

```json
{"em":"2026-11-29T10:05:00-03:00","peca":"peca-20261129-001","de":"produzida","para":"aprovada","por":"romulo","versao":3,"nota":""}
```

O ledger é a **fonte da verdade do histórico**. O estado atual da peça pode ser reconstruído a partir dele.

## 10. Regras de integridade (verificadas por código)

1. Toda peça aponta para pelo menos um Material existente.
2. Toda Versão referencia só Materiais indexados.
3. Material sem autorização válida bloqueia a peça.
4. `tentativas` nunca passa de 2 fora do estado `travada`.
5. Transições só ocorrem segundo `ESTADOS.md`.
6. Quem registrou a geração de uma versão não pode registrar a sua aprovação.

## 11. Onde cada coisa mora

| Dado | Local | Versionado no git? |
| --- | --- | --- |
| Índice de Eventos, Materiais, Trechos | `dados/indice/` | Sim (leve) |
| Peças e Versões (JSON) | `dados/pecas/` | Sim |
| Ledger | `dados/ledger.jsonl` | Sim |
| Vídeo, áudio, foto, render, proxy, transcrição bruta | Drive e pasta local | **Não** |
| Imagens geradas | Pasta local e Drive | **Não** |
