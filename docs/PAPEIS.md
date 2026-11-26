# Papéis

## 1. Pessoas

| Papel | Quem | O que decide |
| --- | --- | --- |
| **Dono do projeto** | Romulo Sedano | Escopo, prioridades, aprovação final de publicação, liberação de peça `travada` |
| **Aprovador editorial** | A nomear (hoje vazio em `projeto/aprovadores.json`) | Fidelidade ao que foi pregado, tom, adequação |
| **Liderança da igreja** | Pastor e liderança | Referências de design e edição; o que é inadequado; autorização de uso de imagem e nome |
| **Voluntário de mídia** | A definir | Opera a central, revisa e edita |

Nomes e funções em `projeto/aprovadores.json`; este documento descreve os papéis, não as pessoas. Nenhuma pessoa é cadastrada sem o consentimento dela.

## 2. Agentes e skills

Princípios herdados: **chamada de ferramenta fica nas bordas** (entrada e saída); o miolo criativo é texto; todo agente devolve **destilado**, nunca o material que leu; quem gera não aprova.

| Agente (função) | Entra com | Devolve | Pode escrever? |
| --- | --- | --- | --- |
| **Indexador** | Pasta do Drive | Eventos e Materiais indexados | Só `dados/indice/` |
| **Selecionador de trechos** | Transcrição + `projeto/` | Lista de Trechos com resumo | Só propõe |
| **Redator** | Trecho + `voz.md` + `restricoes.md` | Legenda e texto de publicação | Só dentro da Versão |
| **Produtor** | Spec aprovada para render | Arquivos de render | Só pasta de render |
| **Qualidade (portão editorial)** | Peça sem autoria | Veredito com critério nomeado | **Somente leitura** |
| **Curador de mídia** | Pauta | Imagens do acervo e candidatas à geração | Só propõe |
| **Pesquisador** (mesa) | Pergunta | Fontes e nível de confiança | Só leitura e web |

Cada agente do núcleo é genérico e lê `projeto/` em execução (`CAMADAS.md`, seção 7).

Regra de independência: o agente de qualidade **não recebe** o nome do autor, a tentativa nem o histórico.

## 3. A mesa de especialistas

Formato de trabalho para decisões de desenho, com três vozes que debatem e se refutam, com acesso a pesquisa externa. O usuário entra com questionamentos e decide.

| Voz | Lente | Pergunta-guia |
| --- | --- | --- |
| **E1: Engenheiro** | Engenharia com Claude Code. Lema: a organização dos processos vem antes de qualquer ação | "Está documentado e é verificável?" |
| **E2: Automação** | Skills e MCPs para automatizar conteúdo, com aprovação humana no fim | "Isso vira skill? Qual o caminho mais automatizado com humano no final?" |
| **E3: Algoritmo** | Documentação da Meta e do Instagram; crescimento orgânico, mínimo de tráfego pago | "O que a plataforma realmente premia, e com qual fonte?" |

Como a mesa opera:

1. O dono apresenta a questão.
2. Cada voz argumenta e refuta as outras, com cadeia de raciocínio explícita.
3. Pesquisa externa entra com fonte e nível de confiança; sem fonte, é hipótese.
4. A mesa devolve **recomendação** e **divergências abertas**, nunca uma lista de opções sem escolha.
5. O dono decide; a decisão vai para `DECISOES.md`, a dúvida para `PENDENCIAS.md`.

A mesa é um **método de trabalho** do dono do projeto com o Claude, não um componente do software.

## 4. Matriz de responsabilidade (RACI simplificado)

| Etapa | Executa | Aprova | Informado |
| --- | --- | --- | --- |
| Indexar material | Indexador (código) | n/a | Dono |
| Propor trechos | Selecionador | Humano escolhe | n/a |
| Montar spec | Sistema | n/a | Revisor |
| Gate automático | Código | n/a | Revisor |
| Editar e re-renderizar | Humano | n/a | n/a |
| Gate editorial | Aprovador editorial | Aprovador editorial | Dono |
| Gate final e publicação | Dono ou delegado | Dono | Liderança, se sensível |
| Liberar `travada` | Dono | Dono | n/a |
| Nova regra aprendida | Sistema propõe | Dono | n/a |

## 5. Limites de autoridade

- Nenhum agente publica.
- Nenhum agente gera imagem sem aprovação humana registrada.
- Nenhum agente altera `projeto/` sem proposta revisada pelo dono (regras aprendidas passam por revisão).
- Nenhum agente altera `nucleo/` a pedido de um projeto: mudança de núcleo é decisão registrada.
