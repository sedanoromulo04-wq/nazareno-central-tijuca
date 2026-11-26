# Central de Gerenciamento — constituição

Este arquivo é lido em toda sessão. Mantenha-o curto. A profundidade mora em `docs/`.

## O que é

Uma central local que organiza a produção de conteúdo de um projeto de marketing: do material bruto até a peça aprovada por um humano. Este repositório é a **instância da igreja** (Nazareno Tijuca) de um **esqueleto adaptável**: o motor serve a outros projetos; o que é da igreja mora só em `projeto/`.

## Princípio-mãe

**A organização dos processos vem antes de qualquer ação.** Nada se constrói sem estar documentado em `docs/`. Mudou o comportamento, muda o documento primeiro.

## Regras de método

1. **Três camadas, a de baixo nunca cita a de cima:** `nucleo/` (motor) → `canais/` (plataforma) → `projeto/` (contexto). Regra de igreja, de Instagram ou de cliente nunca entra no núcleo. Detalhe: `docs/CAMADAS.md`.
2. **Só vira configuração o que já variou entre projetos reais.** O resto fica fixo. Sem framework especulativo.
3. **Quem gera não aprova.** O portão não sabe quem escreveu.
4. **Critério de portão é verificável.** Se dá para checar por código, não passa pelo modelo.
5. **Publicação é sempre ato humano.** A central prepara; uma pessoa decide e publica.
6. **Peso fica no Drive; no git só índice, metadado e decisão.** Vídeo e foto nunca entram no repositório.
7. **Afirmação técnica anda com fonte.** Sem fonte, é hipótese e entra em `docs/PENDENCIAS.md`.
8. **Limite de 2 tentativas** no ciclo de reprovação; depois a peça fica `travada` e só sai por decisão humana.
9. **Edição por parâmetros:** a peça é um JSON (cortes, legenda, imagens, áudio). Edita-se o campo e re-renderiza.
10. **Segredos em variável de ambiente.** Nunca em arquivo versionado.

## Onde está o resto

| Preciso de | Está em |
| --- | --- |
| Por que isto existe, o que não é | `docs/VISAO.md` |
| Fronteira núcleo / canal / projeto | `docs/CAMADAS.md` |
| Contrato de dados (Culto, Material, Peça, Aprovação) | `docs/ENTIDADES.md` |
| Estados, transições, ledger | `docs/ESTADOS.md` |
| Componentes e fluxo técnico | `docs/ARQUITETURA.md` |
| Papéis humanos, agentes e a mesa de especialistas | `docs/PAPEIS.md` |
| O que integrar, o que não | `docs/INTEGRACOES.md` |
| Fases de construção | `docs/ROTEIRO.md` |
| Criar um projeto novo a partir deste | `docs/GUIA-NOVO-PROJETO.md` |
| Termos fixos | `docs/GLOSSARIO.md` |
| Decisões datadas | `docs/DECISOES.md` |
| Perguntas abertas | `docs/PENDENCIAS.md` |
| Voz, restrições, estilo desta igreja | `projeto/` |
| Regras do Instagram | `canais/instagram/` |
