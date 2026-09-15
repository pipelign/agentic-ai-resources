# How Language Models Work

**Last reviewed:** September 15, 2026

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

A large language model (LLM) is a neural network trained on language at scale. To understand an assistant's response, it helps to follow both how the model generates text and how it learned to respond to requests.

**Audience and prerequisites:** Curious readers exploring AI and agents. No programming required. The [neural-network introduction](../neural-networks/neural-networks-embeddings-and-language-models.md#1-neural-networks-and-learning) explains weights and learning from examples.

## Start with these explainers

| Resource | What it explains | Background and scope |
| --- | --- | --- |
| Grant Sanderson / 3Blue1Brown: [Large Language Models explained briefly](https://www.youtube.com/watch?v=LPZh9BOjkQs), with an [illustrated companion](https://www.3blue1brown.com/lessons/mini-llm/) | A short animated overview of prediction, training, and transformers. | Beginner; start here. Uses words to simplify the explanation of tokens. |
| Josh Starmer / StatQuest: [Reinforcement Learning with Human Feedback, Clearly Explained](https://www.youtube.com/watch?v=qPN_XZcJf_s) | Pretraining, supervised fine-tuning, and learning from preferences. | Basic neural-network knowledge helps. Explains the InstructGPT approach; training recipes vary. |
| Andrej Karpathy: [Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI) | A longer account of the training process and model behavior. | Optional lecture of over three hours. Start at [59:23 for the transition to assistant training](https://www.youtube.com/watch?v=7xTGNNLPyMI&t=3563s). Product examples reflect February 2025. |

These are creator-produced educational videos. The papers and documentation below support the technical distinctions. Resource descriptions were reviewed on **2026-09-15**.

## Neural networks, transformers, and LLMs

These names describe different aspects of a model:

| Term | Meaning |
| --- | --- |
| **Neural network** | A model whose numerical parameters are adjusted during training. Networks can classify images, predict quantities, generate language, and perform other tasks. |
| **Transformer** | A neural-network architecture that uses attention to combine information across positions in an input. |
| **Language model** | A model trained to predict language. A generative model can repeatedly predict the next token to produce text. |

Modern generative LLMs commonly use transformers. The original transformer contained an encoder and a decoder for translation; GPT-style models use a decoder architecture for generating continuations. This guide focuses on that common design. See [Hugging Face's architecture overview](https://huggingface.co/learn/llm-course/en/chapter1/4) and the original [Attention Is All You Need paper](https://arxiv.org/abs/1706.03762).

## How a response is generated

1. **Tokenize the input.** A tokenizer converts text into vocabulary IDs. A token can represent a word, part of a word, punctuation, or another text unit; tokens and words are not interchangeable. See [Hugging Face's tokenizer introduction](https://huggingface.co/learn/llm-course/en/chapter2/4).
2. **Compute representations.** Token IDs select learned embedding vectors. The network also accounts for position. Attention and feed-forward layers transform those vectors using the available context.
3. **Predict the next token.** The network produces scores that become probabilities over possible next tokens.
4. **Select a token.** A decoding procedure chooses one. Sampling allows variation; settings such as temperature change how concentrated the sampling probabilities are.
5. **Continue.** The selected token becomes part of the context for the next prediction. Generation ends at a stopping condition, such as an end marker or output limit.

This repeated use of earlier output is called **autoregressive generation**. The [3Blue1Brown transformer lesson](https://www.3blue1brown.com/lessons/gpt/) illustrates the computation. In a standard causal decoder, a position can attend to earlier positions and itself. Training can process many positions in parallel while masking future tokens; generating new text still depends on tokens already selected. [Hugging Face architecture overview](https://huggingface.co/learn/llm-course/en/chapter1/4).

For an illustrative prompt such as `Write a subject line for a delayed delivery`, the model generates a continuation one token at a time. Changing the prompt to include the delivery date or a sample subject line changes the information available at every subsequent prediction.

## What pretraining teaches

During **pretraining**, a generative model processes large collections of text and learns to predict tokens from preceding context. The text supplies its own training targets, which is why this is called **self-supervised learning**. Prediction error guides updates to the model's weights. [3Blue1Brown's training overview](https://www.3blue1brown.com/lessons/mini-llm/).

Predicting text across many domains can support capabilities such as translation, question answering, and following examples supplied in a prompt. The [GPT-3 paper, Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165), demonstrated such uses without updating weights for each task. It also documented substantial variation in performance across tasks. Next-token prediction describes the training objective; assessing what the resulting model can do requires testing it.

## How a pretrained model becomes an assistant

A pretrained **base model** learns to continue text. **Post-training** further changes its weights to shape behavior such as answering questions and following instructions. One influential recipe is:

| Stage | Training signal | Intended effect |
| --- | --- | --- |
| **Supervised fine-tuning (SFT)** | Demonstrations of useful responses to prompts. | Make the model more likely to respond in the demonstrated ways. |
| **Reinforcement learning from human feedback (RLHF)** | People compare responses; a reward model learns those preferences, and the language model is optimized against that reward. | Increase responses judged helpful or otherwise preferred. |

The [InstructGPT paper](https://arxiv.org/abs/2203.02155) describes this sequence and its evaluations. Other methods exist: [Direct Preference Optimization (DPO)](https://arxiv.org/abs/2305.18290) trains directly from preference comparisons without the same separate reward-model and reinforcement-learning loop. These are examples of post-training methods, rather than a mandatory recipe for every assistant.

[Reasoning training](reasoning-models.md#how-models-learn-to-reason) can further develop problem-solving behavior through demonstrations and rewards. Assistant training and reasoning training can overlap within a model's development.

## Learned parameters and current context

| Information | How it affects a response |
| --- | --- |
| **Learned parameters, or weights** | Determine the model's learned computation. Training updates them. |
| **Current context** | Supplies instructions, conversation, examples, retrieved passages, and tool results for this invocation. |

Providing examples in a prompt is often called **in-context learning**. Ordinary inference can adapt to those examples while keeping the weights fixed, as in the [GPT-3 experiments](https://arxiv.org/abs/2005.14165).

Suppose a delivery assistant receives `Order 42 now arrives on Friday` from a lookup tool. It can use that fact in its next response because the tool result is in context. Retaining the fact for another conversation requires a persistence mechanism, such as an order database or stored conversation. See [context engineering and memory](../context-and-memory/context-engineering-and-memory.md#state-memory-compaction-and-caching).

## Where BERT and embeddings fit

**BERT** uses an encoder to build contextual representations, with original pretraining that included predicting masked tokens. **GPT-style models** use preceding context to predict the next token. Both learn language representations, with different training objectives and typical uses. [Original BERT paper](https://arxiv.org/abs/1810.04805); [GPT-3 paper](https://arxiv.org/abs/2005.14165).

An embedding model can help retrieve relevant delivery policies; a generative model can then use those passages to draft a reply. Follow the [word2vec, BERT, and sentence-embedding explanations](../neural-networks/neural-networks-embeddings-and-language-models.md#2-vector-embeddings-and-word2vec) for that branch of the story.

## Why convincing responses can be wrong

Useful language generation and factual correctness are separate things to evaluate. Post-training can improve helpfulness and truthfulness while leaving errors, as the [InstructGPT results](https://arxiv.org/abs/2203.02155) show. A response may supply an unsupported fact or a plausible explanation for a faulty conclusion. Sampling variation also means repeated requests can produce different responses.

For the delivery example, a polished message still needs the correct order details. Retrieval and tools can supply evidence; checking that the response uses that evidence correctly remains part of the task.

## Continue

- [Reasoning models](reasoning-models.md) — intermediate work, training, and computation while answering.
- [How agents work](../agent-loops-and-autonomy/how-agents-work.md) — the runtime and tools around the model.
- [Language-model index](README.md)
