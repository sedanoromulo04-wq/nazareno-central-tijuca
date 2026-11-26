# Visão

## 1. Propósito

Transformar o que a igreja já registra (cultos, louvores, pregações, fotos) em conteúdo publicável, com o menor esforço humano possível **sem abrir mão da decisão humana no final**.

A central funciona como uma pequena agência de marketing automatizada: um fluxo em cadeia de etapas, algumas feitas por código, outras por agentes de IA, e as decisões de valor feitas por pessoas.

## 2. O problema

- Há muito material bruto no Google Drive e pouca vazão para transformá-lo em peças.
- A produção manual consome o tempo de poucos voluntários.
- Um projeto anterior (a "Central" do repositório `minha-empresa`) falhou em sustentar-se por depender de peças frágeis: espelho em Notion, artifact do claude.ai, tarefa agendada no Windows. Também misturava regras de vários clientes no mesmo contexto.

## 3. O que a central faz

| Entrada | Processo | Saída |
| --- | --- | --- |
| Material do culto (vídeo, áudio, foto) indexado do Drive | Seleção de trechos, transcrição, proposta de corte, legenda, tratamento | Peça pronta para revisão |
| Pedido de conteúdo motivacional | Pauta, texto, composição visual | Peça pronta para revisão |
| Edições humanas (texto, imagem, corte) | Re-render a partir dos parâmetros | Peça aprovada |
| Peça aprovada | Pacote de publicação (arquivo, legenda, horário sugerido, hipótese) | Publicação feita por uma pessoa |

Três linhas de conteúdo na igreja: **cortes de pregação**, **cortes de louvor** e **conteúdos motivacionais**.

## 4. O que a central NÃO é

- Não publica sozinha. Publicar é ato humano.
- Não é editor de vídeo em linha do tempo. Edita-se por parâmetros.
- Não armazena vídeos no git. O Drive guarda o peso.
- Não substitui o pastor nem a liderança na decisão do que é adequado.
- Não é ferramenta de tráfego pago. Estratégia é orgânica; pago é exceção decidida por humano.
- Não gera pessoas reais por IA nem simula a voz de ninguém.

## 5. Princípios

1. **Organização antes de ação.** Documenta-se, valida-se, só então se constrói.
2. **Humano no final.** Toda peça passa por aprovação humana antes de sair.
3. **Verificável antes de opinável.** O que código confere, código confere.
4. **Peso fora do git.** Só índice, metadado e decisão no repositório.
5. **Ferramentas locais e simples.** Nada que dependa de serviço agendado frágil.
6. **Aprendizado registrado.** Toda reprovação vira regra com data.
7. **Adaptável por construção.** O motor não conhece a igreja; a igreja é configuração (ver `CAMADAS.md`).

## 6. Critérios de sucesso (a validar com o usuário)

| Critério | Medida proposta |
| --- | --- |
| Vazão | Número de peças aprovadas por semana (meta a definir na Fase 3) |
| Esforço humano | Minutos de revisão por peça aprovada |
| Qualidade | Taxa de aprovação na primeira revisão |
| Aprendizado | Reprovações que viraram regra / total de reprovações |
| Custo | Gasto com geração de imagem por peça, registrado |

Meta numérica fica em aberto até haver base real (`PENDENCIAS.md`).

## 7. Estratégia de conteúdo (resumo, a testar)

O objetivo é alcançar pessoas novas sem anúncio pago. As fontes e níveis de confiança estão em `canais/instagram/README.md`. Resumo:

- O Instagram declara usar sinais como tempo de exibição, curtidas e **envios** (compartilhamentos por mensagem). A ordem exata dos pesos varia entre as fontes; trata-se como **hipótese a testar**, não como fato.
- Conteúdo original tem prioridade; agregadores perderam elegibilidade para recomendações (informação de abril de 2026 em fonte secundária, a reconfirmar).
- Cada peça carrega uma **hipótese** e uma **métrica** desde o nascimento, para que a central aprenda com o resultado.

## 8. Esqueleto adaptável

A igreja é o primeiro projeto, não o centro do desenho. O motor (`nucleo/`) e a plataforma (`canais/`) não citam a igreja. Um novo projeto de marketing reaproveita o motor trocando apenas `projeto/`. Detalhe em `CAMADAS.md` e `GUIA-NOVO-PROJETO.md`.
