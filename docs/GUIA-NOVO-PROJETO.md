# Guia: criar um projeto novo a partir deste esqueleto

Status: **rascunho**. Será validado quando houver um segundo projeto real (Fase 7 do roteiro).

## 1. Antes de começar

Responda por escrito, em `projeto/perfil.md`:

1. Qual é o objetivo de negócio e como se mede?
2. Quem é o público (e quem **não** é)?
3. Quais canais serão usados? (cada canal precisa existir em `canais/`)
4. De onde vem o material bruto e quem pode autorizar o uso?
5. Quem aprova e quem publica?
6. Quais limites legais e éticos existem?

Sem estas respostas, não se copia nada.

## 2. Passo a passo

1. **Copiar o esqueleto** (repositório-modelo, assim que existir) para um repositório novo e privado.
2. **Limpar `projeto/`**: substituir o conteúdo da igreja pelos modelos vazios.
3. **Preencher os arquivos de projeto**, nesta ordem:
   `perfil.md` → `restricoes.md` → `voz.md` → `aprovadores.json` → `estilo.json` → `fontes.md` → `capacidades.json`.
4. **Escolher os canais**: copiar e adaptar `canais/instagram/` ou criar outro canal com a mesma estrutura.
5. **Definir enums do projeto** (tipos de evento, categorias de material, linhas de conteúdo) em `projeto/perfil.md`, seção "Vocabulário".
6. **Ajustar `CLAUDE.md`** somente na linha de identidade ("Este repositório é a instância de…"). As regras de método não mudam.
7. **Rodar a validação de projeto** (Fase 1 em diante): o núcleo confere se os arquivos de `projeto/` estão completos e consistentes.
8. **Produzir uma peça de teste** ponta a ponta antes de usar em produção.

## 3. O que NÃO fazer

| Erro | Por que quebra |
| --- | --- |
| Editar `nucleo/` para atender um projeto | Cria divergência; o núcleo deixa de ser compartilhável |
| Colocar regra do cliente em `CLAUDE.md` | Contamina o contexto (problema que originou este esqueleto) |
| Copiar a voz de outro projeto | Voz é do projeto; é a primeira causa de peça com tom errado |
| Ligar todas as capacidades "por precaução" | Carrega peso e superfície de erro sem uso |
| Pular `restricoes.md` | Os portões dependem dele |

## 4. Quando o esqueleto precisa mudar

Se algo no novo projeto não cabe em `projeto/` ou `canais/`:

1. Registrar a necessidade em `PENDENCIAS.md` do **esqueleto**.
2. Verificar o critério anti-framework: o que já variou entre dois projetos reais?
3. Se passar, decidir em `DECISOES.md` e alterar o núcleo **uma vez**, para todos.

## 5. Checklist de prontidão de um projeto

- [ ] `perfil.md` responde as seis perguntas
- [ ] `restricoes.md` cobre direito de imagem, menores, música e claims
- [ ] `voz.md` tem exemplos aprovados e anti-exemplos
- [ ] `aprovadores.json` tem editorial e final distintos de quem produz
- [ ] `estilo.json` valida contra o esquema do núcleo
- [ ] `fontes.md` aponta pasta e responsável por autorizações
- [ ] `capacidades.json` liga só o que será usado
- [ ] Peça de teste aprovada e registrada no ledger
