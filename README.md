# esforêÂ§o medido

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![esforêÂ§o medido](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## O que êÂ©

O esforêÂ§o medido êÂ© um kit gratuito que mostra, com nêÂºmeros, o que o nêÂ­vel de esforêÂ§o (`low`, `medium`, `high`, `xhigh`,
`max`) muda de verdade no **Claude Fable 5.2** e no **Claude Opus 5.5**. Ele roda a mesma tarefa em cada nêÂ­vel, pelo
Claude Code da sua assinatura, e conta o que o modelo fez: quantas ferramentas chamou, quantas vezes conferiu o prêÂ³prio
trabalho, quanto pensou, quanto tempo levou e se acertou, segundo um checador oculto que o modelo nêÂ£o vêÂª. Junto vêÂªm 14
dicas de esforêÂ§o, cada uma com a fonte. Precisa de Python 3 e do Claude Code; nêÂ£o usa API.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## Por quêÂª

EsforêÂ§o nêÂ£o êÂ© cronêÂ´metro. Pela documentaêÂ§êÂ£o da Anthropic, ele controla quanto o modelo pensa **e age** em cada passo
("the level also changes how the model thinks and how many tool calls it makes"). O tempo êÂ© consequêÂªncia. E o mesmo nome
nêÂ£o quer dizer a mesma coisa nos dois modelos: na API o Fable 5.2 vem em `high` e o Opus 5.5 em `medium`, e o `medium` do
Opus 5.5 jêÂ¡ passa o `high` do Opus 5. O jeito de saber qual nêÂ­vel serve para vocêÂª êÂ© medir na sua tarefa.

## O que eu medi (12/11/2026)

3 tarefas de cêÂ³digo, cada uma em 5 nêÂ­veis nos dois modelos: 30 rodadas, cêÂ³pia limpa, checador escondido, uma rodada por
combinaêÂ§êÂ£o. **Todas as 30 tiraram nota mêÂ¡xima.** O que mudou foi o resto (soma das 3 tarefas):

| | Nota | Tempo | Pensamento (tokens) | ConferêÂªncias | Custo equivalente* |
|---|---|---|---|---|---|
| Fable 5.2 êÂ· low | 48/48 | 1 min 05 s | 1.015 | 3 | US$ 0,72 |
| Fable 5.2 êÂ· high | 48/48 | 1 min 53 s | 3.174 | 7 | US$ 1,05 |
| Fable 5.2 êÂ· max | 48/48 | 26 min 42 s | 132.112 | 25 | US$ 9,78 |
| Opus 5.5 êÂ· low | 48/48 | 1 min 04 s | 253 | 5 | US$ 0,30 |
| Opus 5.5 êÂ· medium | 48/48 | 1 min 29 s | 760 | 4 | US$ 0,39 |
| Opus 5.5 êÂ· max | 48/48 | 18 min 06 s | 92.889 | 16 | US$ 3,77 |

O pensamento subiu em todo degrau; as conferêÂªncias cresceram pouco atêÂ© o `high` (Fable 3 ê¢â â 7; no Opus quase nêÂ£o mudaram, 5 ê¢â â 4) e saltaram
no `xhigh` e no `max` (Fable 25, Opus 16). SêÂ£o tarefas curtas: elas nêÂ£o mostram onde o esforêÂ§o alto ajuda (trabalho longo, problema aberto). Os quadros
completos, com os 5 nêÂ­veis, estêÂ£o em `exemplos/`.

RepetiêÂ§êÂ£o (ruêÂ­do): Fable 5.2 no carrinho, `low` e `high` 3 vezes cada ê¢â â todas 12/12; `low` 26ê¢â¬â33 s, `high` 39ê¢â¬â49 s, e o
`high` sempre pensou mais. JêÂ¡ o nêÂºmero de ferramentas variou de 3 a 12 entre rodadas iguais: compare pensamento e tempo,
nêÂ£o uma contagem solta de ferramentas (`exemplos/carrinho-repeticao/`).

\* Custo que o Claude Code calcula pela tabela da API; na assinatura vocêÂª paga em limite.

## ComeêÂ§o rêÂ¡pido

```bash
git clone https://github.com/linnoff88-dev/esforco-medido.git
cd esforco-medido
./medir.sh carrinho -m opus -e low,medium        # 2 rodadas rêÂ¡pidas para ver funcionando
./medir.sh calendario                            # fable e opus, low ê¢â â max (10 rodadas; o max demora)
cat dicas/DICAS.md                               # as 14 dicas
```

## ConteêÂºdo

| Arquivo | O que faz |
|---|---|
| `medir.sh` | Roda a tarefa em cada `modelo êâ esforêÂ§o` (`-m fable,opus -e low,ê¢â¬Â¦,max -r N`), cada rodada numa cêÂ³pia limpa e com um HOME temporêÂ¡rio: o modelo nêÂ£o lêÂª o seu CLAUDE.md, hooks, plugins, skills nem MCP. SêÂ³ a credencial da assinatura êÂ© ligada. `--sem-isolar` roda com tudo seu. |
| `relatorio.py` | LêÂª o stream-json de cada rodada e conta ferramentas (por tipo), conferêÂªncias (cêÂ³digo rodado para checar, inclusive a suêÂ­te), ediêÂ§êÂµes, releituras depois de editar, subagentes, erros, tokens de pensamento, tempo, custo equivalente e se o fim oferece continuar em vez de terminar. `sessao <arquivo.jsonl>` faz as mesmas contagens numa sessêÂ£o sua. |
| `tarefas/` | 3 tarefas com checador oculto (`oculto/checar.py`, nota `n/total`): `intervalos` (fêÂ¡cil, 16 casos), `carrinho` (caêÂ§a a 6 erros com as regras num arquivo, 12 casos), `calendario` (5 funêÂ§êÂµes com feriados mêÂ³veis, 20 casos). |
| `dicas/DICAS.md` | 14 dicas de esforêÂ§o para Fable 5.2 e Opus 5.5, com a fonte de cada uma. |
| `dicas/QUAL-USAR.md` | Fable 5.2 ou Opus 5.5: por onde comeêÂ§ar, com os nêÂºmeros medidos aqui e o que vem do guia da Anthropic (1 pêÂ¡gina). |
| `dicas/terminar-o-trabalho.pt.txt` | O bloco "terminar o trabalho" da Anthropic, para colar no CLAUDE.md quando o modelo parar antes da hora (original em `.en.txt`, versêÂ£o em espanhol em `.es.txt`). |
| `exemplos/` | As rodadas reais deste kit (12/11/2026): `RESULTADO.md` e `resultado.json` de cada tarefa. |
| `tests/` | 18 testes sem rede: `python3 -m unittest discover tests`. |

## Tarefa sua

Crie `tarefas/<nome>/` com `projeto/` (o cêÂ³digo que o modelo recebe), `pedido.txt` (o pedido, igual ao que vocêÂª
digitaria) e `oculto/checar.py <pasta>` imprimindo `NOTA n/total`. Rode `./medir.sh <nome>`.

## O processo, do inêÂ­cio ao fim

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/linnoff88-dev/esforco-medido.git` | ê°Å¸Â§Â  vocêÂª |
| 2 | Escolher ou criar a tarefa | `ls tarefas/` ou `tarefas/<nome>/` | ê°Å¸Â§Â  vocêÂª |
| 3 | Medir os nêÂ­veis | `./medir.sh <tarefa> [-m ê¢â¬Â¦] [-e ê¢â¬Â¦] [-r 3]` | ê¢Å¡â¢ê¯Â¸Â script + ê°Å¸Â¤â `claude -p` |
| 4 | Ler o quadro | `resultados/<tarefa>-<data>/RESULTADO.md` | ê°Å¸Â§Â  vocêÂª |
| 5 | Fixar o nêÂ­vel | `/effort <nêÂ­vel>` ou `effortLevel` / `modelSettings` no settings | ê°Å¸Â§Â  vocêÂª |
| 6 | Se parar cedo | colar `dicas/terminar-o-trabalho.pt.txt` no CLAUDE.md | ê°Å¸Â§Â  vocêÂª |

## Cuidados

- Cada rodada gasta o limite da sua assinatura. O `max` pode levar muito tempo; comece com `-e low,medium,high`.
- Uma rodada êÂ© a medida daquela tarefa, naquele dia. Antes de virar regra, repita (`-r 3`).
- O custo no quadro êÂ© o que o Claude Code calcula pela tabela da API; na assinatura vocêÂª paga em limite.
- No macOS o login fica no Keychain: se o isolamento nêÂ£o achar a credencial, o script avisa e roda sem isolar.

## Kit irmêÂ£o

[esforêÂ§o certo](https://github.com/linnoff88-dev/esforco-certo): audita em que modelo e esforêÂ§o cada parte do seu projeto
roda e compara dois nêÂ­veis num trabalho seu (`ab.sh`).

## LicenêÂ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>