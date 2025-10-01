### Literature Survey

* base benchmark

  * *AGIEval: A Human-Centric Benchmark for Evaluating Foundation Models*
    * This paper creates a benchmark that uses real human exams, like the SAT and LSAT, to test large language models. It shows that models like GPT-4 do well on some tasks, but still have problems with hard reasoning questions.
  * *HumanEval-XL: A Multilingual Code Generation Benchmark for Cross-lingual Natural Language Generalization*
    * This study builds a big dataset to test how well language models can write code in many languages. It includes 23 natural languages and 12 programming languages to test cross-language understanding.

* related benchmark

  * *Ad-hoc Concept Forming in the Game Codenames as a Means for Evaluating Large Language Models*

    * The paper uses the game Codenames to see if language models can make smart clues. It helps check how models understand meaning and form new ideas during the game.

    * *clembench: Using Game Play to Evaluate  Chat-Optimized Language Models as Conversational Agents*
      * clembench is a test that uses games to check if chat-based language models can follow rules and talk naturally. It shows how models behave in simple game settings.

    * *GAMEARENA: EVALUATING LLM REASONING  THROUGH LIVE COMPUTER GAMES*
      * GameArena tests the reasoning skills of models by putting them into live computer games. It focuses on different types of thinking, like logical and step-by-step reasoning.

  * *GameEval: Evaluating LLMs on Conversational Games*

    * GameEval uses talking games to check if models can work together to solve goals. The games test how well models can discuss, ask questions, and make decisions in conversation.

    

## Methods and Techniques

### Benchmark Design and Evaluation Dimensions

**Goal**

- Build a Taboo-inspired benchmark using LLMs as both clue-giver (hinter) and guesser
- Ensure reproducibility and consistency by avoiding human participants

**Core Evaluation Dimensions**

1. **Rule-following**
   - Evaluates whether the hinter avoids using the target or taboo words
   - Detected via regular expressions
2. **Reasoning-related Abilities**
   - Circumlocution understanding
   - Multi-hop reasoning
   - Domain-specific knowledge
   - Semantic disambiguation



### Implementation, Dataset, and Evaluation Methods

**Evaluation Setup**

- LLMs interact through API in a prompt-response loop
- Controlled setup:
  - Fix hinter, vary guesser → test reasoning
  - Fix guesser, vary hinter → test clue generation
- Enables role-isolated evaluation and fair model comparison

**Dataset Construction**

- Target words sourced from WordNet
- Categorized into concrete, abstract, technical, and polysemous types
- Taboo lists generated from semantically related terms

**Reasoning Ability Evaluation**

| Capability          | What It Tests                                | Evaluation Method                                            |
| ------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| Circumlocution      | Guess from indirect or abstract clues        | WordNet, sentence embeddings, semantic similarity            |
| Multi-hop reasoning | Combine multiple clues to infer target       | not sure[[2402.16837] Do Large Language Models Latently Perform Multi-Hop Reasoning?](https://arxiv.org/abs/2402.16837) |
| Domain knowledge    | Understand technical vocabulary              | Categorized targets, domain-specific accuracy                |
| Disambiguation      | Interpret polysemous words under constraints | Sense alignment via taboo context, LLM-as-Judge              |

**Scoring and Output**

- Regular expressions for rule-checking
- Embedding similarity and lexical tools for clue interpretation
- LLM-as-Judge framework for subjective/semantic evaluation
- Output includes success rates, violation counts, and comparative charts









Use the author or titie

prompts ambiguous