# Estados, transições e aprendizado

## 1. Máquina de estados

Enxuta de propósito. Herda a lógica da empresa anterior (pauta, roteiro, produzida, aprovada, publicada), mas sem etapas que não geram decisão.

```
        ┌──────────────────────────────────────────┐
        ▼                                          │
     pauta ──▶ produzida ──▶ aprovada ──▶ publicada │
                  │  ▲                              │
                  ▼  │ (nova versão, ≤ 2 vezes)     │
               reprovada ──────────────▶ travada ───┘ (só por decisão humana)
```

| Estado | Significado | Quem entra | Quem sai |
| --- | --- | --- | --- |
| `pauta` | Ideia ou trecho escolhido; sem peça montada | Humano ou agente propõe | Produção monta a peça |
| `produzida` | Há uma Versão renderizada, aguardando portão | Produção | Portão decide |
| `aprovada` | Passou nos portões; pronta para pacote | Portão final (humano) | Humano publica |
| `reprovada` | Falhou em um portão; aguarda nova versão | Portão | Produção refaz ou trava |
| `travada` | Duas reprovações consumidas | Sistema | **Só por decisão humana** (liberar com nova pauta, ou arquivar) |
| `publicada` | Uma pessoa registrou a publicação | Humano | Terminal (recebe métricas) |

Um estado extra, `arquivada`, existe como saída terminal para pautas descartadas por decisão humana.

## 2. Tabela de transições permitidas

| De | Para | Condição | Registrado por |
| --- | --- | --- | --- |
| (nova) | `pauta` | Trecho ou tema escolhido | Quem propõe |
| `pauta` | `produzida` | Versão 1 renderizada e verificadores automáticos passaram | Produção |
| `pauta` | `arquivada` | Decisão humana | Humano |
| `produzida` | `aprovada` | Portão editorial e final aprovam | Aprovador |
| `produzida` | `reprovada` | Qualquer portão reprova, com critério nomeado | Aprovador ou verificador |
| `reprovada` | `produzida` | Nova versão gerada; `tentativas` < 2 | Produção |
| `reprovada` | `travada` | `tentativas` chega a 2 | Sistema |
| `travada` | `pauta` ou `arquivada` | Decisão humana, com motivo | Humano |
| `aprovada` | `publicada` | Humano registra link ou data da publicação | Humano |
| `aprovada` | `reprovada` | Humano reabre antes de publicar | Humano |

Qualquer transição fora desta tabela é rejeitada pelo código.

## 3. Edição humana não conta como reprovação

Quando o aprovador **edita** a peça (troca legenda, ajusta corte, troca imagem), isso gera uma nova **Versão** dentro de `produzida`, sem consumir tentativa. Só **reprovar** consome tentativa. Isso preserva o objetivo de a pessoa ter poder de edição sem estourar o limite.

## 4. Os portões

Três portões em sequência. Cada um tem critérios verificáveis.

| Portão | Quem executa | Exemplos de critério |
| --- | --- | --- |
| **Automático** | Código | Duração dentro do limite do formato; proporção correta; legenda sincronizada; loudness dentro da faixa; material com autorização confirmada; limites de hashtags do canal; campos obrigatórios (hipótese, métrica) |
| **Editorial** | Aprovador humano ou agente de qualidade independente | Fidelidade ao que foi dito; ausência de claims proibidos (`restricoes.md`); aderência à voz (`voz.md`) |
| **Final** | Humano | Decisão de publicar; revisão visual; confirmação de que não há menores ou pessoas sem autorização |

Regras:

1. **Quem gera não aprova.** O portão não sabe quem escreveu, nem por quê, nem quantas tentativas houve.
2. **O que o código verifica, não passa pelo modelo.** Agentes só avaliam o que não dá para medir.
3. **Reprovação sempre cita critério nomeado.** "Não gostei" não é reprovação válida; vira nota na versão, não veredito.

## 5. Ledger

Toda transição escreve uma linha em `dados/ledger.jsonl`. Campos: ver `ENTIDADES.md`, seção 9. O ledger:

- é só de acréscimo (append-only);
- permite reconstruir o estado de qualquer peça;
- alimenta as métricas de `VISAO.md` (taxa de aprovação, retrabalho, tempo no estado).

## 6. Ciclo de aprendizado

Reprovação ensina. Mecanismo no núcleo, conteúdo no projeto.

1. Veredito `reprovada` com critério nomeado.
2. O sistema pergunta: **o critério já existia?**
   - Sim: o verificador ou o texto da regra falhou. Reforça-se a regra existente (sem criar duplicata).
   - Não: nasce **regra nova** em `projeto/regras-aprendidas.md`, com `id`, data, origem (peça e versão) e critério.
3. Regra nova que pode ser conferida por código vira **verificador automático** (propostas ficam em fila de revisão humana).
4. O ledger registra que a reprovação gerou ou reforçou regra, para a métrica "reprovações que viraram regra".

Formato de regra:

```
### R-0007 · 2026-12-02
Critério: legenda cobre o rosto do pregador
Origem: peca-20261129-001 v2
Regra: legenda nunca fica abaixo de 15% da altura nem sobre o rosto
Verificável por código: sim (posição da legenda × zona segura)
```

## 7. Hipótese e métrica por peça

Toda peça passa a `produzida` com `hipotese` e `metrica`. Ao `publicada`, o humano (ou, no futuro, um coletor) registra o resultado. Sem hipótese, a peça não sai de `pauta`. O objetivo é que o desempenho vire dado de decisão, e não impressão.

## 8. O que está fora desta máquina

Etapas que a empresa anterior tinha e **não** entram aqui: roteiro como estado separado, `radar` automático de tendências, lentes de copy em fases F1 a F7, coordenação de frentes entre sessões. Justificativa em `INTEGRACOES.md`.
