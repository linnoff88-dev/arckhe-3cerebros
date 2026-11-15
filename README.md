<div align="center">

# ðŸ§ ðŸ§ ðŸ§  3cerebros

**Um cÃ©rebro de IA com 3 cÃ©rebros â€” isolado e embutÃ­vel por qualquer agente.**

Projeto Â· Self Â· Conhecimento Â· `.md` + SQLite Â· zero dependÃªncias no nÃºcleo

</div>

---

## O que Ã©

`3cerebros` Ã© o **cÃ©rebro**, num repo isolado pra vocÃª **incorporar no seu sistema**
(jarvis, bot, agente CLI, o que for). Ele separa a memÃ³ria em **3 cÃ©rebros com
necessidades opostas** â€” em vez de um monte sÃ³ que incha:

| CÃ©rebro | Guarda | Natureza |
|---|---|---|
| **1 Â· Projeto** | o que rolou, decisÃµes, estado | episÃ³dico |
| **2 Â· Self** | quem vocÃª Ã©, valores, dÃºvidas | identidade |
| **3 Â· Conhecimento** | fatos, o que aprendeu | referÃªncia |

Mais o diferencial que nenhum outro segundo cÃ©rebro tem: uma **polÃ­tica de
automaÃ§Ã£o** (o que o cÃ©rebro pode fazer sozinho vs pedir confirmaÃ§Ã£o).

> A pesquisa e o racional estÃ£o em [inematds/2cerebrox](https://github.com/inematds/2cerebrox).

## Status: operacional (F1â€“F4, menos SDK TS)

NÃºcleo rodÃ¡vel **com tudo ligado**: entrevista que gera a polÃ­tica (F1),
autonomia sob polÃ­tica (F2), conectores de ingestÃ£o (F3), e CLI + MCP server +
Agent Skills (F4). Ãšnico pendente: o SDK TypeScript (qualquer host TS usa o **MCP
server** enquanto isso). NÃºcleo sÃ³ stdlib; `mcp` Ã© dependÃªncia **opcional**.

## Uso (Python)

```python
from cerebros import Brain

brain = Brain("~/.cerebros")                 # cria os 3 cÃ©rebros se nÃ£o existirem

brain.interview(answers={...})               # F1: entrevista â†’ SOUL/values/POLICY
system = brain.build_context(query, who)     # Self + memÃ³rias relevantes â†’ system prompt
brain.observe(who, user_msg, ai_reply)       # triagem + save autÃ´nomo (sob polÃ­tica)
brain.recall("postgres", sector="projeto")   # busca por relevÃ¢ncia (BM25)
brain.note("texto cru")                       # destila em nota no Conhecimento
brain.ingest("chatgpt_export", path="â€¦")     # F3: importa fonte externa â†’ inbox
brain.run_jobs()                              # F2: manutenÃ§Ã£o autÃ´noma (cron chama)
brain.act("enviar_mensagem", do=enviar)      # gate de aÃ§Ã£o SOB a polÃ­tica
brain.policy.check("enviar_mensagem")         # 'allow' | 'confirm' | 'deny'
```

Veja [`examples/minimal_host.py`](examples/minimal_host.py) (host completo) e
[`examples/intelecto_seam.py`](examples/intelecto_seam.py) (ponto de costura no `core.py`).

## Uso (CLI)

```bash
pip install -e .                  # instala o comando `cerebros`
cerebros init                     # entrevista de onboarding (gera a polÃ­tica)
cerebros observe "decidi migrar pra Postgres na sexta" --who nei
cerebros ingest chatgpt_export --path conversations.json
cerebros jobs                     # roda a manutenÃ§Ã£o autÃ´noma (sob polÃ­tica)
cerebros recall "postgres" --sector projeto
cerebros policy enviar_mensagem   # â†’ allow | confirm | deny
cerebros mcp                      # sobe o MCP server (pip install 'cerebros[mcp]')
```

## Agent Skills (Claude Code / Codex / Gemini)

Em [`skills/`](skills): `cerebro-triagem` (capturar/rotear), `cerebro-nota`
(destilar conhecimento) e `cerebro-socratica` (pensar apoiado no Self). Operam o
mesmo cÃ©rebro em `~/.cerebros/` via a CLI.

## Como incorporar no seu sistema

| Host | Como |
|---|---|
| Python (intelecto, jarvis caseiro) | `pip install -e .` ou git submodule â†’ `from cerebros import Brain` |
| Claude Code / Codex / Gemini | Agent Skills de [`skills/`](skills) + MCP |
| Qualquer runtime | subir o MCP server (`cerebros mcp`) |
| TypeScript (ex.: openpcbot) | falar com o **MCP server** (SDK ts ainda nÃ£o â€” Ãºnico pendente) |
| SÃ³ o formato | apontar a ferramenta pra `~/.cerebros/` â€” spec aberta |

## Formato e polÃ­tica

- [`spec/FORMAT.md`](spec/FORMAT.md) â€” a estrutura do cÃ©rebro (pasta + schema SQLite).
- [`spec/POLICY.md`](spec/POLICY.md) â€” o formato da polÃ­tica de automaÃ§Ã£o.

## Rodar o teste

```bash
python -m pytest -q          # ou: python tests/test_smoke.py
```

## Roadmap

- **F0 âœ…** nÃºcleo: Brain, memÃ³ria FTS5, triagem, polÃ­tica, destilaÃ§Ã£o
- **F1 âœ…** entrevista â†’ `POLICY.md` do usuÃ¡rio + `brain.act()` (o diferencial)
- **F2 âœ…** autonomia: `run_jobs()` real (triagem/briefing/saÃºde) via cron
- **F3 âœ…** conectores: ChatGPT export, Telegram, email/mbox, arquivos â†’ inbox
- **F4 âœ…** empacotar: CLI `cerebros` + MCP server + Agent Skills
- **F4 (resta)** SDK TypeScript â€” hosts TS usam o MCP server enquanto isso

---

<div align="center">
Um projeto <a href="https://inema.club">INEMA.CLUB</a>
</div>

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe