# english_dictation
Word-by-word dictation

## Guided learning flow and AI evaluation

The app runs a four-step flow: full-passage listening (up to 5 plays), AI comprehension check (minimum score 50/100), vocabulary selection and example sentences (up to 8 words), then sentence-by-sentence dictation. Dictation allows 10 listens per sentence; an additional manual listen resets the exercise to Step 1. Hints per sentence are capped at half the sentence's word count, rounded down.

AI checks are sent to the same-origin `POST /api/ai/evaluate` endpoint. The repository is a static frontend and does not include that server route or an AI provider credential. Implement the route on a trusted server; do not put provider API keys in `index.html`. The frontend sends:

```json
{
  "task": "comprehension",
  "prompt": {
    "system": "Use the task-specific strict evaluator prompt from index.html.",
    "responseSchema": {}
  },
  "input": {
    "sourcePassage": "Original passage",
    "userSummary": "Learner summary"
  }
}
```

For vocabulary evaluation, `task` is `"vocabulary"` and `input` contains `sourcePassage`, `targetWord`, and `exampleSentence`. The proxy must pass the system prompt and JSON schema to the selected LLM, enforce JSON output, and return the validated object in this envelope:

```json
{ "result": { "score": 72, "feedback": "The main idea is captured." } }
```

Comprehension results require an integer `score` from 0 to 100 and string `feedback`. Vocabulary results require boolean `grammarCorrect` and `contextCorrect` fields plus string `feedback`; both booleans must be true to pass. Invalid responses and network errors are shown to the learner and never unlock the next step.
