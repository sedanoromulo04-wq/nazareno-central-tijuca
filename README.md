# Nazareno Central Tijuca

Central de gerenciamento da produção de conteúdo da igreja: cortes de pregação, cortes de louvor e conteúdos motivacionais, com revisão e edição humana antes de qualquer publicação.

O repositório é, ao mesmo tempo:

- a **instância da igreja** (tudo em `projeto/`), e
- o **esqueleto adaptável** para outros projetos de marketing (`nucleo/` e `canais/`).

## Estado atual: fase de fundação (documentação)

Nenhum código de motor existe ainda. Por princípio, o processo é documentado e validado antes de ser construído. Ordem de leitura:

1. [`docs/VISAO.md`](docs/VISAO.md): propósito, escopo, o que não é.
2. [`docs/CAMADAS.md`](docs/CAMADAS.md): como o esqueleto se adapta.
3. [`docs/ENTIDADES.md`](docs/ENTIDADES.md) e [`docs/ESTADOS.md`](docs/ESTADOS.md): o contrato.
4. [`docs/ARQUITETURA.md`](docs/ARQUITETURA.md): como será construído.
5. [`docs/PAPEIS.md`](docs/PAPEIS.md): quem faz o quê.
6. [`docs/INTEGRACOES.md`](docs/INTEGRACOES.md): o que entra e o que fica de fora.
7. [`docs/ROTEIRO.md`](docs/ROTEIRO.md): fases.
8. [`docs/GUIA-NOVO-PROJETO.md`](docs/GUIA-NOVO-PROJETO.md): reaproveitar para outro projeto.
9. [`docs/PENDENCIAS.md`](docs/PENDENCIAS.md): o que ainda precisa de resposta.

## Estrutura

```
CLAUDE.md          constituição curta
nucleo/            motor genérico (ainda vazio: fase 1+)
canais/instagram/  regras da plataforma
projeto/           contexto desta igreja (o que muda a cada projeto)
dados/             índices, fila e ledger locais
docs/              a fundação
```
