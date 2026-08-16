# Ch 15 · Query–history relatedness（相关）

Discourse segmentation: is this utterance still the **same conversation thread**? Not intent classification.

## Concept

**Definition.** Relatedness asks whether the new utterance continues the *same thread* as the history (CONTINUE) or has shifted (SHIFT / NEW). Retrospective: see the next turn, then mark shift (TIAGE). Predictive: history only — will the *next* turn drift? It is discourse, not NLU intent.

**Why it exists.** The context layer’s stage 1 needs this label only. Intent can stay “write code” while the topic jumps (auth → CSS). Intent can change while entities stay (same file, “why did tests fail?”). If you put “refactor vs Q&A” in the same softmax as topic shift, RESET and CONTINUE both go wrong.

**Mechanism.** Features: anaphora (“continue”, “that function”), new entity (path never seen), time gap, code-symbol Jaccard, task-statement NLI (gist as hypothesis → entail / contradict / neutral（蕴含 / 矛盾 / 中性）), user explicit (“new topic”). Judges: lexical / embedding / NLI / topic-shift / entity continuity. Hot path should be <20ms; a second LLM classifier is a cost, not a default.

**Boundaries.** Not [intent](../part-04-routing-nlu/ch-20-intent.md). Not compacting. Not model routing. TextTiling / C99 / LCSeg are unsupervised *linear* segmentation vocabulary — weak on short chat, still the words for “boundary.”

**Example.** TIAGE augments PersonaChat with 7,861 gold topic-shift marks and splits detection vs generation. Chinese topic-shift papers (e.g. 2305.01195) are still shift, still not BANKING77 intents.

**Failure modes.** Training relatedness on ATIS intents. RESET that keeps slot inherit. Treating embedding cosine 0.7 as a universal SHIFT threshold.

## Comparison

| Setting | Definition | Literature |
| --- | --- | --- |
| Retrospective | see the next turn, then mark shift | TIAGE (EMNLP 2021) |
| Predictive | history only, will the *next* turn drift? | TIAGE TSManager |
| Topic segmentation | boundaries on a long stream | TextTiling, C99, LCSeg, BERT-Wiki727k |
| Unsupervised chat | embed + time gap + speaker → cluster | 2025 chat-shift papers |

| Feature | Example |
| --- | --- |
| Anaphora | “continue”, “that function” → CONTINUE prior |
| New entity | path / URL / product never seen |
| Time gap | same session, hours later → higher shift prior |
| Code-symbol overlap | identifier Jaccard |
| Task-statement NLI | gist as hypothesis → entail / contradict / neutral |
| User explicit | “new topic”, “scratch that, do X” |

**Read (读):**
- [paper — TIAGE (Xie et al., EMNLP 2021): topic-shift dataset on PersonaChat; retrospective vs predictive. Not an intent table.](https://arxiv.org/abs/2109.04562)
- [paper — Topic Shift Detection in Chinese Dialogues: Chinese-specific shift labels; still not NLU intent.](https://arxiv.org/abs/2305.01195)
- [paper — TextTiling (Hearst 1997): classic unsupervised linear topic segmentation.](https://aclanthology.org/J97-1003/)
- [paper — C99 (Choi): another classic linear segmenter; vocabulary for “boundary.”](https://aclanthology.org/C00-1070/)
- [paper — Text Segmentation as a Supervised Learning Task (Koshorek et al.): Wiki-727K neural boundaries on long streams, not chat intent.](https://arxiv.org/abs/1803.05355)
- [paper — MNLI (Williams et al.): the entail/contradict/neutral labels task-statement NLI borrows.](https://arxiv.org/abs/1704.05426)

[Part III](./README.md)
