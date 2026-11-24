# esforÃ§o medido

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![esforÃ§o medido](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O esforÃ§o medido Ã© um kit gratuito que mostra, com nÃºmeros, o que o nÃ­vel de esforÃ§o (`low`, `medium`, `high`, `xhigh`,
`max`) muda de verdade no **Claude Fable 5.2** e no **Claude Opus 5.5**. Ele roda a mesma tarefa em cada nÃ­vel, pelo
Claude Code da sua assinatura, e conta o que o modelo fez: quantas ferramentas chamou, quantas vezes conferiu o prÃ³prio
trabalho, quanto pensou, quanto tempo levou e se acertou, segundo um checador oculto que o modelo nÃ£o vÃª. Junto vÃªm 14
dicas de esforÃ§o, cada uma com a fonte. Precisa de Python 3 e do Claude Code; nÃ£o usa API.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## Por quÃª

EsforÃ§o nÃ£o Ã© cronÃ´metro. Pela documentaÃ§Ã£o da Anthropic, ele controla quanto o modelo pensa **e age** em cada passo
("the level also changes how the model thinks and how many tool calls it makes"). O tempo Ã© consequÃªncia. E o mesmo nome
nÃ£o quer dizer a mesma coisa nos dois modelos: na API o Fable 5.2 vem em `high` e o Opus 5.5 em `medium`, e o `medium` do
Opus 5.5 jÃ¡ passa o `high` do Opus 5. O jeito de saber qual nÃ­vel serve para vocÃª Ã© medir na sua tarefa.

## O que eu medi (12/11/2026)

3 tarefas de cÃ³digo, cada uma em 5 nÃ­veis nos dois modelos: 30 rodadas, cÃ³pia limpa, checador escondido, uma rodada por
combinaÃ§Ã£o. **Todas as 30 tiraram nota mÃ¡xima.** O que mudou foi o resto (soma das 3 tarefas):

| | Nota | Tempo | Pensamento (tokens) | ConferÃªncias | Custo equivalente* |
|---|---|---|---|---|---|
| Fable 5.2 Â· low | 48/48 | 1 min 05 s | 1.015 | 3 | US$ 0,72 |
| Fable 5.2 Â· high | 48/48 | 1 min 53 s | 3.174 | 7 | US$ 1,05 |
| Fable 5.2 Â· max | 48/48 | 26 min 42 s | 132.112 | 25 | US$ 9,78 |
| Opus 5.5 Â· low | 48/48 | 1 min 04 s | 253 | 5 | US$ 0,30 |
| Opus 5.5 Â· medium | 48/48 | 1 min 29 s | 760 | 4 | US$ 0,39 |
| Opus 5.5 Â· max | 48/48 | 18 min 06 s | 92.889 | 16 | US$ 3,77 |

O pensamento subiu em todo degrau; as conferÃªncias cresceram pouco atÃ© o `high` (Fable 3 â†’ 7; no Opus quase nÃ£o mudaram, 5 â†’ 4) e saltaram
no `xhigh` e no `max` (Fable 25, Opus 16). SÃ£o tarefas curtas: elas nÃ£o mostram onde o esforÃ§o alto ajuda (trabalho longo, problema aberto). Os quadros
completos, com os 5 nÃ­veis, estÃ£o em `exemplos/`.

RepetiÃ§Ã£o (ruÃ­do): Fable 5.2 no carrinho, `low` e `high` 3 vezes cada â†’ todas 12/12; `low` 26â€“33 s, `high` 39â€“49 s, e o
`high` sempre pensou mais. JÃ¡ o nÃºmero de ferramentas variou de 3 a 12 entre rodadas iguais: compare pensamento e tempo,
nÃ£o uma contagem solta de ferramentas (`exemplos/carrinho-repeticao/`).

\* Custo que o Claude Code calcula pela tabela da API; na assinatura vocÃª paga em limite.

## ComeÃ§o rÃ¡pido

```bash
git clone https://github.com/linnoff88-dev/esforco-medido.git
cd esforco-medido
./medir.sh carrinho -m opus -e low,medium        # 2 rodadas rÃ¡pidas para ver funcionando
./medir.sh calendario                            # fable e opus, low â†’ max (10 rodadas; o max demora)
cat dicas/DICAS.md                               # as 14 dicas
```

## ConteÃºdo

| Arquivo | O que faz |
|---|---|
| `medir.sh` | Roda a tarefa em cada `modelo Ã— esforÃ§o` (`-m fable,opus -e low,â€¦,max -r N`), cada rodada numa cÃ³pia limpa e com um HOME temporÃ¡rio: o modelo nÃ£o lÃª o seu CLAUDE.md, hooks, plugins, skills nem MCP. SÃ³ a credencial da assinatura Ã© ligada. `--sem-isolar` roda com tudo seu. |
| `relatorio.py` | LÃª o stream-json de cada rodada e conta ferramentas (por tipo), conferÃªncias (cÃ³digo rodado para checar, inclusive a suÃ­te), ediÃ§Ãµes, releituras depois de editar, subagentes, erros, tokens de pensamento, tempo, custo equivalente e se o fim oferece continuar em vez de terminar. `sessao <arquivo.jsonl>` faz as mesmas contagens numa sessÃ£o sua. |
| `tarefas/` | 3 tarefas com checador oculto (`oculto/checar.py`, nota `n/total`): `intervalos` (fÃ¡cil, 16 casos), `carrinho` (caÃ§a a 6 erros com as regras num arquivo, 12 casos), `calendario` (5 funÃ§Ãµes com feriados mÃ³veis, 20 casos). |
| `dicas/DICAS.md` | 14 dicas de esforÃ§o para Fable 5.2 e Opus 5.5, com a fonte de cada uma. |
| `dicas/QUAL-USAR.md` | Fable 5.2 ou Opus 5.5: por onde comeÃ§ar, com os nÃºmeros medidos aqui e o que vem do guia da Anthropic (1 pÃ¡gina). |
| `dicas/terminar-o-trabalho.pt.txt` | O bloco "terminar o trabalho" da Anthropic, para colar no CLAUDE.md quando o modelo parar antes da hora (original em `.en.txt`, versÃ£o em espanhol em `.es.txt`). |
| `exemplos/` | As rodadas reais deste kit (12/11/2026): `RESULTADO.md` e `resultado.json` de cada tarefa. |
| `tests/` | 18 testes sem rede: `python3 -m unittest discover tests`. |

## Tarefa sua

Crie `tarefas/<nome>/` com `projeto/` (o cÃ³digo que o modelo recebe), `pedido.txt` (o pedido, igual ao que vocÃª
digitaria) e `oculto/checar.py <pasta>` imprimindo `NOTA n/total`. Rode `./medir.sh <nome>`.

## O processo, do inÃ­cio ao fim

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/linnoff88-dev/esforco-medido.git` | ðŸ§  vocÃª |
| 2 | Escolher ou criar a tarefa | `ls tarefas/` ou `tarefas/<nome>/` | ðŸ§  vocÃª |
| 3 | Medir os nÃ­veis | `./medir.sh <tarefa> [-m â€¦] [-e â€¦] [-r 3]` | âš™ï¸ script + ðŸ¤– `claude -p` |
| 4 | Ler o quadro | `resultados/<tarefa>-<data>/RESULTADO.md` | ðŸ§  vocÃª |
| 5 | Fixar o nÃ­vel | `/effort <nÃ­vel>` ou `effortLevel` / `modelSettings` no settings | ðŸ§  vocÃª |
| 6 | Se parar cedo | colar `dicas/terminar-o-trabalho.pt.txt` no CLAUDE.md | ðŸ§  vocÃª |

## Cuidados

- Cada rodada gasta o limite da sua assinatura. O `max` pode levar muito tempo; comece com `-e low,medium,high`.
- Uma rodada Ã© a medida daquela tarefa, naquele dia. Antes de virar regra, repita (`-r 3`).
- O custo no quadro Ã© o que o Claude Code calcula pela tabela da API; na assinatura vocÃª paga em limite.
- No macOS o login fica no Keychain: se o isolamento nÃ£o achar a credencial, o script avisa e roda sem isolar.

## Kit irmÃ£o

[esforÃ§o certo](https://github.com/linnoff88-dev/esforco-certo): audita em que modelo e esforÃ§o cada parte do seu projeto
roda e compara dois nÃ­veis num trabalho seu (`ab.sh`).

## LicenÃ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>