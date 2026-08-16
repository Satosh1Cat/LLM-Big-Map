# Ch 20 · Intent（意图识别）

Task-oriented NLU left half: map one utterance onto a label table. **Single-sentence classification**, not “does this look like the history?”.

## Concept

**Definition.** Intent is a label from *your* table: `code-edit` / `ask` / `search` / `browser` / `compact-now`, or ATIS `flight`. Metrics: Accuracy, Macro-F1, OOS recall — reported **apart** from relatedness and slots. LLM era: small classifier, JSON enum, or `function_call.name` as the intent.

**Why it exists.** Agents still need a task type to pick tools, SKUs, or a model family. Classic datasets exist so you can learn the *format* (OOS or not), not so you copy airline labels into a coding agent.

**Mechanism.** Classic: SVM+n-gram → BERT sentence clf → joint intent-slot (Joint BERT, Slot-Gated) → Rasa DIET. OOS: CLINC150 exists to reject out-of-scope. Multi-intent: one utterance, two labels. Production: function calling makes `name` the intent and arguments the slots ([Ch 21](./ch-21-slot-filling.md)).

**Boundaries.** Not topic shift ([Ch 15](../part-03-context-router/ch-15-relatedness.md)). Not model routing (though intent can be a *feature* of the router). Product tables are **yours to label**, not ATIS.

**Failure modes.** Training on BANKING77 and deploying on GitHub issues. Fusing intent and relatedness into one softmax. Treating OOS as “low confidence intent” without a reject class.

## Comparison

| Block | Content |
| --- | --- |
| Classic data | ATIS (air), SNIPS, CLINC150 (OOS), BANKING77, HWU64, MASSIVE (multilingual) |
| Classic models | SVM+n-gram → BERT sentence clf → joint intent-slot (Joint BERT, Slot-Gated) → Rasa DIET |
| OOS | must reject out-of-scope; CLINC150 exists for this |
| Multi-intent | one utterance, two labels (“lights on and AC to 26”) |
| Routing table | not dialogue NLU — you annotate product intents |

**Read (读):**
- [paper — Joint BERT (Chen et al. 2019): shared encoder, intent head + slot head. Not topic shift.](https://arxiv.org/abs/1902.10909)
- [paper — ATIS (Hemphill et al. 1990): airline travel utterances; the classic intent/slot format, not your coding-agent labels.](https://aclanthology.org/H90-1021/)
- [paper — Snips NLU (Coucke et al. 2018): crowdsourced voice intents; small class set.](https://arxiv.org/abs/1805.10190)
- [paper — CLINC150 (Larson et al. EMNLP 2019): 150 in-scope intents plus out-of-scope queries.](https://aclanthology.org/D19-1131/)
- [repo — clinc/oos-eval: the CLINC150 dataset files.](https://github.com/clinc/oos-eval)
- [paper — BANKING77 (Casanueva et al. 2020): 77 fine-grained banking intents, single domain.](https://arxiv.org/abs/2003.04807)
- [repo — PolyAI-LDN/task-specific-datasets: BANKING77 data release.](https://github.com/PolyAI-LDN/task-specific-datasets)
- [docs — OpenAI function calling: production intent as `name`, slots as JSON.](https://platform.openai.com/docs/guides/function-calling)

[Part IV](./README.md)
