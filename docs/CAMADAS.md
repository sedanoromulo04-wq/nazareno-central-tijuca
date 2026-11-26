# Camadas: como o esqueleto se adapta

## 1. Por que existe

O repositório `minha-empresa` ficou com "contexto manchado": regras de um projeto (ex.: limite de hashtags do Instagram) viraram constituição para todos. Este documento impede isso por construção.

## 2. As três camadas

| Camada | Pasta | O que contém | Muda entre projetos? | Quem edita |
| --- | --- | --- | --- | --- |
| **Núcleo** | `nucleo/` | Entidades, máquina de estados, ledger, aprovações, servidor local, interface de revisão, renderizador, índice de mídia, skills genéricas | Não | Mantenedor do esqueleto |
| **Canal** | `canais/<plataforma>/` | Formatos, limites, sinais que a plataforma premia, verificadores de publicação | Por plataforma | Mantenedor + pesquisa |
| **Projeto** | `projeto/` | Voz, restrições, público, aprovadores, estilo visual, fontes de material, motores ligados, regras aprendidas | Sempre | Dono do projeto |

## 3. Regra de dependência

```
projeto  ──lê──▶  canal  ──lê──▶  núcleo
```

- O **núcleo** não cita canal nem projeto. Conhece só contratos ("toda peça tem estado, formato, aprovador").
- O **canal** pode usar o núcleo, mas não cita projeto.
- O **projeto** configura o que o núcleo e o canal oferecem, nunca os edita.

Teste prático: se uma frase em `nucleo/` ou `canais/` contém "igreja", "culto", "pastor" ou o nome de um cliente, a fronteira foi violada.

## 4. Pontos de variação (os "5 arquivos" mais 3)

Só entra aqui o que já variou entre a igreja e o Rio Sol (cliente da empresa anterior). O resto é fixo.

| Arquivo em `projeto/` | Responde | Exemplo na igreja |
| --- | --- | --- |
| `perfil.md` | Quem somos, objetivo, público | Evangelização e crescimento da congregação na Tijuca |
| `voz.md` | Como falamos, o que nunca dizemos | Acolhedor, bíblico, sem sensacionalismo |
| `restricoes.md` | Limites legais, éticos e de direito | LGPD, menores, música, claims proibidos |
| `estilo.json` | Fonte, cor, legenda, enquadramento | Legenda branca com palavra-chave em vermelho |
| `aprovadores.json` | Quem aprova o quê | Dono do projeto; especialista a nomear |
| `capacidades.json` | Quais motores estão ligados | Corte, legenda, imagem por IA: sim; avatar: não |
| `fontes.md` | De onde vem o material | Pasta do Drive, por culto |
| `regras-aprendidas.md` | O que as reprovações ensinaram | Começa vazio |

Os modelos de cada arquivo estão em `projeto/`.

## 5. O que fica fixo no núcleo (e por quê)

| Fixo | Motivo |
| --- | --- |
| Humano aprova antes de publicar | Princípio de segurança, não de gosto |
| Quem gera não aprova | Independência do portão |
| Limite de 2 tentativas | Evita laço infinito de reprovação |
| Peso no Drive, índice no git | Repositório leve e limpo |
| Peça = JSON de parâmetros | Habilita edição humana e re-render |

Se um projeto futuro precisar mudar um item fixo, isso é **sinal de mudança no núcleo** (decisão registrada em `DECISOES.md`), não de configuração local.

## 6. Capacidades: ligar e desligar motores

`projeto/capacidades.json` declara os motores usados. Exemplo conceitual:

```json
{
  "corte_video": true,
  "transcricao": true,
  "legenda_queimada": true,
  "imagem_ia": true,
  "avatar_ia": false,
  "carrossel": false
}
```

Um projeto sem vídeo (ex.: só carrosséis) desliga o que não usa e não carrega o peso.

## 7. Skills parametrizadas

Uma skill do núcleo nunca embute voz, restrição ou estilo. Ela **lê** `projeto/` em tempo de execução. A mesma skill "cortar e legendar" atende qualquer projeto; o que muda é o que lê.

## 8. Canais

Cada pasta em `canais/` declara:

1. **Formatos** aceitos (proporção, duração, tamanho).
2. **Sinais** que a plataforma premia, cada um com fonte e nível de confiança.
3. **Regras verificáveis** (limites que código confere).
4. **Hipóteses típicas** a testar nesse canal.

O Instagram é o primeiro canal. YouTube Shorts, TikTok ou e-mail entrariam como pastas irmãs, sem tocar o núcleo.

## 9. Distribuição do esqueleto

| Opção | Vantagem | Custo |
| --- | --- | --- |
| Repositório-modelo (template) no GitHub; cada projeto é uma cópia | Contexto isolado por projeto | Melhorias do núcleo não propagam sozinhas |
| Um repositório com várias pastas `projeto/` | Núcleo único | Volta a misturar contexto |

**Decisão da mesa:** repositório-modelo. O núcleo tem `nucleo/VERSAO`. Um script de atualização do núcleo só se justifica depois do segundo projeto. Onde o esqueleto passa a viver (repositório próprio ou extração deste) está em aberto em `PENDENCIAS.md`.

## 10. Critério anti-framework

Antes de criar uma configuração nova, a pergunta é: **isto já variou entre dois projetos reais?** Se não, fica fixo e documentado. Um esqueleto que tudo permite é um esqueleto que ninguém entende.
