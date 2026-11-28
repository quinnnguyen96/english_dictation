# english_dictation
Word-by-word dictation

## Guided learning flow and AI evaluation

The app runs a four-step flow: full-passage listening (up to 5 plays), local summary matching, vocabulary selection and example sentences (up to 8 words), then sentence-by-sentence dictation. Learners can skip each preparation step and skip individual dictation sentences. Dictation allows 10 listens per sentence; an additional manual listen resets the exercise to Step 1. Hints per sentence are capped at half the sentence's word count, rounded down.

The summary step extracts up to three salient sentences as a reference and compares content-word overlap with the learner's summary using an F1 score. This is a local lexical estimate, not semantic or translation-aware AI evaluation; the UI explains that limitation. A 50% score unlocks the next step, and learners may also skip the comparison.

Vocabulary example checks run entirely in the browser without an AI endpoint. The local heuristic checks capitalization, terminal punctuation, sentence length, a basic subject/verb pattern, target-word occurrence (including common inflections), and overlap between content words in the example and the source sentence containing the target word. It reports separate grammar/context estimates and only unlocks dictation when both reach the threshold. This is intentionally a lightweight fallback, not a full grammar parser or semantic model; learners can skip the vocabulary step if the estimate is not appropriate.
