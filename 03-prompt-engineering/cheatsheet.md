# Prompt Engineering — Cheatsheet

> Just the topic list. Write your own one-line explanation next to each in your own words.
> (Full explanations are in `concepts.md`.)

## Big picture
- What is prompt engineering (cheapest quality lever) —
- Prompt vs context engineering —

## Anatomy
- The 3 roles: system / user / assistant —
- System prompt & role framing —
- Prompt skeleton: Role → Task → Context → Examples → Format → Constraints —

## Core techniques
- Zero-shot vs few-shot —
- Picking good examples (match real data) —
- Chain-of-thought (CoT) —
- When to SKIP CoT —
- Structured output / JSON mode / function calling —
- Validate + retry on bad output —
- Delimiters (separate instructions from data) —
- Prompt injection (intro) —
- Give the model an "out" (say "I don't know") —

## Production
- Prompts as code (version them) —
- Regression eval set / A/B test changes —
- Log prompt version → output —

## Bonus techniques (name them)
- Prompt chaining —
- Self-consistency —
- ReAct (reason + act) —
- Decomposition —
