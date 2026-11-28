# english_dictation
Word-by-word dictation

## Guided learning flow and AI evaluation

The app runs a four-step flow: full-passage listening (up to 5 plays), local summary matching, vocabulary selection and example sentences (up to 8 words), then sentence-by-sentence dictation. Learners can skip each preparation step and skip individual dictation sentences. Dictation allows 10 listens per sentence; an additional manual listen resets the exercise to Step 1. Hints per sentence are capped at half the sentence's word count, rounded down.

The summary step extracts up to three salient sentences as a reference and compares content-word overlap with the learner's summary using an F1 score. This is a local lexical estimate, not semantic or translation-aware AI evaluation; the UI explains that limitation. A 50% score unlocks the next step, and learners may also skip the comparison.

Vocabulary example checks are sent to the same-origin `POST /api/ai/evaluate` endpoint. The repository is a static frontend and does not include that server route or an AI provider credential. Implement the route on a trusted server; do not put provider API keys in `index.html`. The frontend sends:

```json
{
  "task": "vocabulary",
  "prompt": {
    "system": "Use the strict vocabulary evaluator prompt from index.html.",
    "responseSchema": {}
  },
  "input": {
    "sourcePassage": "Original passage",
    "targetWord": "example",
    "exampleSentence": "This is an example sentence."
  }
}
```

The proxy must pass the system prompt and JSON schema to the selected LLM, enforce JSON output, and return the validated object in this envelope:

```json
{
  "result": {
    "grammarCorrect": true,
    "contextCorrect": true,
    "feedback": "Câu đúng ngữ pháp và dùng từ phù hợp."
  }
}
```

Vocabulary results require boolean `grammarCorrect` and `contextCorrect` fields plus string `feedback`; both booleans must be true to pass. Invalid responses and network errors are shown to the learner, but the vocabulary step can be skipped.
