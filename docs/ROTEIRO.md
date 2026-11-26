# Roteiro de construção

Regra: **nenhuma fase começa sem os documentos da fase anterior validados pelo dono, linha por linha.** Cada fase termina com uma demonstração em peça real.

## Fase 0: Fundação (em curso)

Entregáveis: `CLAUDE.md`, `README.md`, `docs/*`, `projeto/*` preliminar, `canais/instagram/README.md`.
Saída: dono valida VISAO, CAMADAS, ENTIDADES, ESTADOS e responde as pendências bloqueantes (`PENDENCIAS.md`).

## Fase 1: Contrato executável

| Item | Descrição |
| --- | --- |
| Esquemas JSON | Evento, Material, Peça, Versão, Aprovação |
| Máquina de estados em código | `nucleo/estados.json` + validador de transições |
| Ledger | Escrita append-only e reconstrução de estado |
| Validador de spec | Rejeita spec inválida |
| Testes | Transições permitidas e proibidas |
Saída: dá para criar uma Peça de teste e movê-la por todos os estados via linha de comando.

## Fase 2: Índice do Drive

| Item | Descrição |
| --- | --- |
| Acesso | Resolver leitura da pasta da igreja (pendência: acesso) |
| Indexador | Gera Eventos e Materiais em `dados/indice/` |
| Campos humanos | Marcar autorização de imagem e pessoas identificáveis |
Saída: um culto real indexado, com autorização marcada.

## Fase 3: Produção mínima de vídeo

| Item | Descrição |
| --- | --- |
| Transcrição | Whisper local com tempo por palavra |
| Renderizador | Spec para vídeo: cortes, legenda, tratamento de áudio e imagem |
| Selecionador de trechos | Skill que propõe trechos |
| Portão automático | Verificadores de duração, proporção, legenda, loudness, autorização |
Saída: um corte de pregação real, de ponta a ponta, em linha de comando.

## Fase 4: Servidor e interface de revisão

| Item | Descrição |
| --- | --- |
| Servidor local | API em `127.0.0.1` |
| Telas | Fila por estado; revisão com prévia; edição por campos; aprovar e reprovar com critério |
| Versões | Histórico e comparação |
Saída: o dono edita legenda, texto e cortes e aprova pela interface.

## Fase 5: Imagem, pacote e publicação

| Item | Descrição |
| --- | --- |
| Wrapper de imagem | Modelo por configuração, custo, aprovação por geração |
| Pacote de publicação | Arquivo, legenda, horário sugerido, hipótese |
| Registro de resultado | Métrica manual por peça |
Saída: primeiro lote publicado por humano com hipótese registrada.

## Fase 6: Segunda linha de conteúdo e aprendizado

Cortes de louvor; motivacionais; ciclo de regras aprendidas operando; relatório semanal.

## Fase 7: Extração do esqueleto

Só depois de a igreja funcionar. Passos:

1. Separar o que é núcleo e canal do que é projeto (já separado por pasta).
2. Criar repositório-modelo (local definido em `PENDENCIAS.md`).
3. Escrever `nucleo/VERSAO` e o procedimento de atualização.
4. Aplicar a um **segundo projeto real**; o que quebrar mostra onde a fronteira estava errada.
5. Só então decidir script de atualização do núcleo.

## Ordem de prioridade e cortes

Se o tempo apertar, a ordem de corte é: Fase 6, depois recorte de pessoa por máscara, depois API de imagem. **Nunca** se corta: contrato (Fase 1), portão automático, aprovação humana.

## Critério de prontidão para código

Uma fase só vira código quando: (1) seus documentos existem; (2) o dono validou; (3) as perguntas bloqueantes foram respondidas; (4) há um teste de aceite descrito.
