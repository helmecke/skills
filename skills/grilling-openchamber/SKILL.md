---
name: grilling-openchamber
description: Companion to the grilling skill, asking one frontier question per OpenChamber form. Load whenever grilling runs in OpenChamber.
---

# Grilling in OpenChamber

Grilling asks its whole frontier as text. In OpenChamber, ask **one frontier question per form** instead: the form's own note field becomes that question's free-text answer, so the user picks the recommendation or writes what the options don't cover, with no extra fields on the card.

Render with `openchamber_render`, passing `ui` and `awaitAnswer`:

```openui-lang
root = Stack([q])
q = Card("<question title>", [Text("<question body and the choices>"), Choice("q", [{label: "<recommendation> (Recommended)", description: "<why>"}, "<alternative>", "<alternative>"], "Your answer")])
```

Use one `Choice` and nothing else; the form supplies the send button and the note field. Put the recommendation first, labelled `(Recommended)`, so accepting it is one click. Word the question so taking the recommendation is the natural default.

Ask exactly one question per form. Never put two questions or two fields on one form. When it returns, read `data.values.q` (the picked label) and `data.note` (the user's own words, which win over or refine the pick). When `answered` is false, the user replied in chat: that message is the answer.

Then recompute the frontier and ask the next single question. Order questions so an answer can reshape the ones asked after it.

Without `openchamber_render` — OpenCode's TUI, the VS Code extension, or a session another agent started — keep grilling's own text round, whole frontier in one message.
