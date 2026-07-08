# Contributing

Thanks for helping grow this resource! A few simple rules keep quality high and consistent.

## Adding a new question

1. Pick the right topic file under `topics/`. If none fit, propose a new file in your PR description.
2. Follow this exact format:

```markdown
### Q: <question, phrased the way an interviewer would ask it>

**Answer:**
<Concise, correct, interview-ready answer. Prefer 3–8 sentences or a short structured
list over a wall of text. Include a diagram-element description or short example only
if it genuinely clarifies the answer.>

**Follow-up:** <one likely follow-up question an interviewer might ask next, optional>

<details>
<summary>Difficulty & tags</summary>

`Difficulty: Easy | Medium | Hard` · `Tags: e.g. sysml, requirements-traceability, incose`

</details>

---
```

3. Keep answers **interview-length**, not textbook-length. If it wouldn't fit in a 2–3 minute spoken answer, trim it.
4. For SysML/UML diagram questions, describe diagram elements/notation in words rather than trying to render actual diagrams in Markdown.
5. No duplicate questions — search the topic file first.

## Style

- Use `**bold**` for key terms, not whole sentences.
- Reference specific tools (Cameo, Rhapsody, Capella) or standards (ISO/IEC/IEEE 15288, INCOSE Handbook, ARP4754A) accurately, and note if an answer is tool/standard-version specific.
- Avoid opinionated claims presented as fact (e.g., "tool X is always better than tool Y") — frame trade-offs instead.

## Opening a PR

- One topic file per PR where possible, to keep reviews fast.
- Briefly describe what you added/changed in the PR description.

## Reporting an issue

Found a wrong or outdated answer? Open an issue tagged `correction` with the question text and a brief explanation, or submit a PR with the fix directly.
