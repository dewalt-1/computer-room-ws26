# Day 4: Hermes Agent

[← Day 3: Ollama](../03-ollama/README.md) · [Schedule](../README.md) · Next: [Day 5: Ideation →](../05-ideation/README.md)

Slides: [Hermes + KI:connect](https://www.figma.com/slides/Ob0je5GTJ8NS8aCFPxky6B)

## A harness around a model

A model on its own does one thing: text goes in, text comes out. It cannot open a file, run
a command or send anything anywhere. [Hermes Agent](https://hermes-agent.nousresearch.com)
(Nous Research) is the program wrapped around it. It reads what the model asks for, carries
that out, and hands back the result. Then the model picks the next step.

| The model **chats** | The harness **acts** |
|---|---|
| Reads text, writes text, works out what should happen next. It never touches your machine. | Offers the tools, runs whichever one the model picks, feeds the result back, keeps the context, and stops to ask you when something matters. |

Put the same model in a different harness and you get different behaviour. Hermes, a chat
website and a terminal tool can all call the same model and do very different amounts of work.

**A chat and an agent.** Ask a chat for five summary bullets: you get five bullets and it is
over. Ask an agent the same thing: it reads the files, searches the mailbox, then comes back
with a summary and the three items that need a reply. Every step is added to the context for
the next call. That is how it keeps track, and why long runs get expensive.

## Tool calls

Reading a file, running a command, fetching a page: each one is written into your transcript
as it happens.

```
read_file
  path: notes.md
→ 2,481 characters

bash
  ls ~/Documents
→ 14 items
```

When something goes wrong, read the calls before the summary. The summary is what the agent
says it did. The calls are the record of what ran.

An agent only works with what it can reach. Access is not the same thing as intelligence.

## You decide what it may do

| Manual | Smart | Off |
|---|---|---|
| Every action waits for you. Nothing runs until you say yes. | Reads freely, asks to change. The setting most people end up on. | No prompts at all. Know why you want this before you pick it. |

**Stay on manual today**, so you can watch each step before it runs.

## Context

**The context window** is the model's working memory for one turn. Every time Hermes calls
the model it sends a fixed amount of text: the system prompt, your context files, the
conversation so far and every tool result share that space. Whatever they take up is not
available for the reply.

**Sessions.** A session begins with the system prompt, `SOUL.md`, the project file and a
snapshot of memory. From there it grows with your messages, the replies and every tool result.
Open a new one and none of that conversation comes with it.
**A session is temporary. Anything you want to keep goes in a file.**

**Context layers**, loaded in this order:

| Layer | What it is |
|---|---|
| Core system prompt | Hermes' own instructions for the loop, its tools and safety. You do not edit this one. |
| `SOUL.md` | Identity, tone and defaults. Hermes writes a starter file if you do not have one. |
| Project file | Whichever it finds first: `.hermes.md`, `AGENTS.md` or `CLAUDE.md`. |
| `USER.md` · `MEMORY.md` | Your profile, and facts it has picked up along the way. Loaded when memory is on. |

**Two files worth writing.** `~/.hermes/SOUL.md` follows the agent into every project
("Say when you are uncertain. Avoid hype and filler."). `AGENTS.md` stays with one project
("App code is in src/. Never commit .env files."). Keep both short: they cost context on
every turn. After you edit `SOUL.md`, start a new session.

**When it fills up**, Hermes summarises the older conversation to make room. Summaries lose
detail, and what goes first is usually the specific: an exact filename, the folder you said
not to touch, the reason you rejected the last version. The agent keeps going, just on a
slightly different question than the one you asked. That is drift.

- Finish one task per session.
- Put the rules in a file it can reread.
- Start fresh rather than pushing a long session further.

**The model is a setting.** Hermes does not contain a model; you pick one per task. Larger
models hold more context and cost more to run. Hermes needs **at least 64,000 tokens of
context**: enough for the prompt, the tool definitions and several rounds of results. Below
that, the agent loses track partway through.

## Install Hermes

**Check first: Apple Silicon only** (M1 or newer). Intel Macs are not supported: look under
Apple menu → About This Mac.

1. Open https://hermes-agent.nousresearch.com/desktop
2. Download the Mac version (a `.dmg` file).
3. Open the `.dmg` and drag Hermes into Applications.
4. Launch it from Applications, not from the `.dmg` window.

If macOS blocks it: right-click → **Open**, then **Open** again in the box that appears.
This happens on the first launch only.

## Connect it to KI:Connect

KI:Connect NRW gives TH Köln students access to language models. Get your own API key at
https://ki.th-koeln.de.

In Hermes: **Settings → Providers → Custom Endpoints → Add Endpoint**
(German: Einstellungen → Eigene Endpunkte → Endpunkt hinzufügen).

| Field | Value |
|---|---|
| Name | `KI:connect` |
| Provider ID | `kiconnect` |
| Endpoint URL | `https://chat.kiconnect.nrw/api/v1` |
| API Mode | Auto-detect |
| Default Model | `openai-gpt5.5` |
| Context | leave it empty |
| API Key | your own key. Nothing shows as you paste it. |

Press **Test**. You want to see **"Endpoint is reachable."** ("Der Endpunkt ist erreichbar.").
Then **Save** (Speichern).

### Models on KI:Connect

| Model | Use for | Context |
|---|---|---|
| `openai-gpt5.5` | Your default. The hardest tasks. | 400K |
| `openai-gpt5.4-mini` | Quick work, lighter on quota | 400K |
| `glm-4.7` | Built for agent loops | 203K |
| `qwen3.5-397b-a17b` | Large open model | 262K |
| `mistral-small-4-119b` | Very long documents | 1.05M |
| `openai-gpt-oss-120b` | Open weights, self-hosted | 131K |

### When it breaks

| You see | Fix |
|---|---|
| Validation failed | The URL has to end in `/v1`. Check it character by character. |
| The endpoint rejected the API key | Clear the field and paste once more. Pasting twice is the usual cause. |
| No models in the list | The key is wrong, or your account does not have access yet. |
| Replies make no sense | You picked an embedding model. Go back and choose `openai-gpt5.5`. |

## Pick a model and start

Just left of the microphone, choose `openai-gpt5.5` and type something. To keep that choice:
**Settings → Model → Main model → Apply**.

Try this one, small enough to check:

> "Read the three files in this folder and tell me where they disagree."

Then read the tool calls rather than the answer. You are checking whether it opened the files
at all, or whether it wrote you something plausible instead.

## Keep this

| | |
|---|---|
| Endpoint URL | `https://chat.kiconnect.nrw/api/v1` |
| Default model | `openai-gpt5.5` |
| Lighter on quota | `openai-gpt5.4-mini` |
| Where you set it up | Settings → Providers → Custom Endpoints |

*Agent and context material adapted from Agentic Creativity, Week 2, used with permission.*
