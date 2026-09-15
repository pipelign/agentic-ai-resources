# Neural Networks, Embeddings, and Language Models: A Viewing Guide

**Last reviewed:** September 15, 2026

> AI Use Disclosure: Codex was used to research, organize, and draft this guide.

This guide follows the ideas behind neural networks: learning from examples, representing information as vectors, using context to interpret language, and turning those representations into useful predictions. It includes a language-model learning path and additional explainers on images and sequences.

**Audience and prerequisites:** Curious readers who want a conceptual understanding of AI. Basic algebra and an understanding of coordinates help; the main viewing path requires no programming. The papers are optional references for readers who want the original methods and historical context.

**Start with [3Blue1Brown's neural-networks playlist](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi).** Grant Sanderson's animations provide the backbone of this guide. Jay Alammar and Josh Starmer add focused explanations of embeddings, word2vec, BERT, and similarity search.

## Contents

- [Suggested viewing order](#suggested-viewing-order)
- [Core explainers](#core-explainers)
- [How the ideas connect](#how-the-ideas-connect)
- [Selected historical milestones](#selected-historical-milestones)
- [Beyond language models](#beyond-language-models)
- [Connecting the fundamentals to agents](#connecting-the-fundamentals-to-agents)

## Suggested viewing order

| Step | Question | Start here |
| --- | --- | --- |
| 1 | What is a neural network, and how does it learn? | [3Blue1Brown's network overview](#1-neural-networks-and-learning) |
| 2 | How does a language model generate a response? | [How language models work, with a short 3Blue1Brown explainer](../language-models/how-language-models-work.md) |
| 3 | How does a representation change with its context? | [3Blue1Brown on transformers and attention](#3-transformers-and-attention) |
| 4 | How does a pretrained model become an assistant? | [Demonstrations and feedback](../language-models/how-language-models-work.md#how-a-pretrained-model-becomes-an-assistant) |
| 5 | What do reasoning models add? | [Intermediate work, training, and inference](../language-models/reasoning-models.md) |

For embeddings and retrieval, follow [word2vec](#2-vector-embeddings-and-word2vec), [BERT](#4-bert-and-contextual-representations), and [sentence embeddings](#5-sentence-embeddings-and-semantic-search). For applications beyond language, explore [CNNs, recurrent networks, and diffusion](#beyond-language-models).

The alternative videos offer a second explanation when a concept needs more time. Creator-produced videos teach intuition; the linked papers document particular methods and results. All resource annotations below were reviewed on **2026-09-15**.

## Core explainers

### 1. Neural networks and learning

**Creator:** Grant Sanderson / 3Blue1Brown. **Format:** Animated explainers with illustrated text companions. **Level:** Beginner; calculus is optional for the initial intuition.

**Watch first:**

- **[But what is a neural network?](https://www.youtube.com/watch?v=aircAruvnKk)** — watch **0:00–16:27**, about **16 minutes**, through the recap. Uses handwritten digits to introduce layers, activations, weights, biases, and their relationship to learning. The [illustrated lesson](https://www.3blue1brown.com/lessons/neural-networks/) is useful for revisiting the network diagram.

**Go deeper into learning:**

1. **[Gradient descent, how neural networks learn](https://www.3blue1brown.com/lessons/gradient-descent/)** — connects prediction errors to a loss function and shows how parameter changes can reduce that loss. This link opens the video lesson and written explanation.
2. **[Backpropagation, intuitively](https://www.youtube.com/watch?v=Ilg3gGewQ5U)** — explains how a training example supplies information about changes throughout the network. The [companion lesson](https://www.3blue1brown.com/lessons/backpropagation/) lets you inspect the steps at your own pace.

**Takeaway:** Training adjusts parameters using examples and a learning objective. Backpropagation calculates gradients; an optimizer uses them to update parameters. Inference uses the resulting parameters to process an input. The small digit classifier makes those roles visible.

### 2. Vector embeddings and word2vec

**Start with [The Illustrated Word2vec — A Gentle Intro to Word Embeddings in Machine Learning](https://www.youtube.com/watch?v=ISPId9Lhc1g), by Jay Alammar.** This narrated visual explanation introduces learned word vectors and the ideas behind word2vec. **Level:** Beginner after the network introduction; no code required.

The [illustrated article](https://jalammar.github.io/illustrated-word2vec/) is especially useful for the training story. It connects neighboring words, prediction tasks, and negative sampling: learning to distinguish observed word-context pairs from sampled comparison pairs. Word2vec includes continuous bag-of-words (CBOW), which predicts a word from surrounding words, and skip-gram, which predicts surrounding words from a word.

**Alternative: [Word Embedding and Word2Vec, Clearly Explained!!!](https://www.youtube.com/watch?v=viZrOnJclY0), by Josh Starmer / StatQuest.** Choose this for another step-by-step explanation of how a network learns word representations. **Level:** Basic familiarity with neural-network inputs, outputs, and training.

**Takeaway:** An embedding represents an item using a vector—a list of numbers. Training gives relationships between those vectors practical value. In ordinary word2vec use, a vocabulary word has a fixed learned representation; its vector does not change for each new sentence. The training objective and data shape which relationships the space captures.

### 3. Transformers and attention

**Creator:** Grant Sanderson / 3Blue1Brown. **Format:** Animated explainers. **Level:** Intermediate intuition; vectors and matrix multiplication are helpful.

- **[Transformers, the tech behind LLMs](https://www.youtube.com/watch?v=wjZofJX0v4M)** — connects tokenization, embeddings, transformer blocks, and predictions. Use the [illustrated companion](https://www.3blue1brown.com/lessons/gpt/) to follow which numbers are learned parameters and which are changing input representations.
- **[Attention in transformers, step-by-step](https://www.youtube.com/watch?v=eMlx5fFNoYc)** — explains how query, key, and value operations allow a token's representation to incorporate information from other positions. The [companion lesson](https://www.3blue1brown.com/lessons/attention/) expands the diagrams and calculations.

**Takeaway:** A token begins with a lookup embedding. As it passes through transformer layers, its representation can incorporate surrounding information. The word *bank* in a river description and in a financial report can consequently acquire different contextual representations. This example illustrates the mechanism; it is not a measured model output.

### 4. BERT and contextual representations

**Start with [Language Processing with BERT: The 3 Minute Intro](https://www.youtube.com/watch?v=ioGry-89gqE), by Jay Alammar.** It introduces applications and then gives a high-level account of the model and search. **Level:** Beginner overview; the transformer lessons help with the mechanism. The video continues beyond the introductory portion named in its title.

**Continue with [Encoder-Only Transformers (like BERT) for RAG, Clearly Explained!!!](https://www.youtube.com/watch?v=GDN649X_acE), by Josh Starmer / StatQuest.** Use this to connect encoder models with retrieval workflows. **Level:** Intermediate; watch after embeddings and attention. RAG means retrieval-augmented generation: bringing retrieved material into a generation task.

**Historical companion: [The Illustrated BERT, ELMo, and co.](https://jalammar.github.io/illustrated-bert/), by Jay Alammar.** This illustrated article traces the transition from fixed word vectors to contextual representations and reusable pretrained language models. It is a useful bridge between word2vec, recurrent models, transformers, and BERT.

**Takeaway:** BERT stands for *Bidirectional Encoder Representations from Transformers*. Its encoder builds representations using context on both sides of a position. Its original pretraining tasks included predicting selected obscured tokens and whether two segments followed one another. It could then be adapted to tasks such as classification or question answering. See the [original BERT paper](https://arxiv.org/abs/1810.04805).

For retrieval, distinguish original BERT pretraining from additional training that makes whole-text vectors useful for similarity search. The next section explains that connection.

### 5. Sentence embeddings and semantic search

**Watch [Cosine Similarity, Clearly Explained!!!](https://www.youtube.com/watch?v=e9U0QAFbfLI), by Josh Starmer / StatQuest.** This explains comparing vector directions. **Level:** Basic vectors and algebra; useful after the word2vec explanation.

**Then read the [Sentence-BERT paper](https://arxiv.org/abs/1908.10084), by Nils Reimers and Iryna Gurevych.** Its abstract provides the key idea: adapt BERT to produce sentence vectors that can be compared efficiently. The full paper is optional and assumes familiarity with neural-network training.

The project's [semantic-search guide](https://sbert.net/examples/sentence_transformer/applications/semantic-search/README.html) shows the application: encode the query and candidate texts into a compatible vector space, then retrieve close matches. **Format:** Maintainer documentation with Python examples. The conceptual introduction can be read without running code.

**Takeaway:** A contextual vector for one token and a vector intended to represent an entire sentence serve different purposes. Sentence-level pooling and training objectives matter. A similarity score helps rank candidates; judging whether a retrieved passage answers the question still requires checking its contents.

## How the ideas connect

This table summarizes distinctions that the viewing path develops:

| Concept | What it represents or does | Useful distinction |
| --- | --- | --- |
| Token | A unit produced by a tokenizer, such as a word or part of a word. | A token ID identifies a vocabulary entry; an embedding supplies a numerical representation. |
| Static word embedding | A learned vector associated with a vocabulary word, as in ordinary word2vec or GloVe use. | Its lookup value stays the same across input sentences. |
| Contextual token representation | A vector computed from a token and the surrounding input, as in ELMo or BERT. | Its value can change between occurrences of the same word. |
| Sentence or passage embedding | A vector intended to represent a larger text for tasks such as retrieval or clustering. | The method of combining token information and its training objective affect usefulness. |
| Autoregressive language model | Predicts a next token from the preceding context and can repeat that process to generate text. | Generation and embedding-based retrieval are different operations that a system can combine. |

Sources for these distinctions: [transformer representations](https://www.3blue1brown.com/lessons/gpt/), [GloVe](https://nlp.stanford.edu/projects/glove/), [ELMo](https://arxiv.org/abs/1802.05365), and [Sentence-BERT](https://arxiv.org/abs/1908.10084).

Two points help interpret the visual explanations:

- **Closeness depends on what the model learned.** A vector space can capture topical association, similar usage, or task-specific relevance. Even opposite words can appear in similar contexts. Alammar discusses this in his [word2vec explanation](https://jalammar.github.io/illustrated-word2vec/).
- **A useful retrieval space needs compatible representations.** Query and document encoders must be trained to produce vectors that can be compared meaningfully. The [Sentence Transformers guide](https://sbert.net/examples/sentence_transformer/applications/semantic-search/README.html) also distinguishes finding similar texts from finding a passage that answers a short query.

## Selected historical milestones

These publications connect language representations, generative models, assistant training, and reasoning. Dates identify the cited publications or initial arXiv releases; the developments overlap and build on earlier work. The papers are primary research references, and their mathematical details are optional for the viewing path.

| Date | Milestone and primary source | Why it belongs in the story |
| --- | --- | --- |
| 1986 | Rumelhart, Hinton, and Williams: [Learning representations by back-propagating errors](https://www.nature.com/articles/323533a0) | Demonstrated how error-driven weight updates can train hidden units to learn useful features. The publisher provides an abstract; full text may require institutional or paid access. |
| 2003 | Bengio and colleagues: [A Neural Probabilistic Language Model](https://jmlr.org/papers/v3/bengio03a.html) | Learned word representations jointly with a language prediction model, establishing an important precursor to later embedding methods. |
| 2013 | Mikolov and colleagues: [Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/abs/1301.3781) | Introduced efficient CBOW and skip-gram architectures for learning word vectors from large text collections. |
| 2013 | Mikolov and colleagues: [Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/abs/1310.4546) | Extended skip-gram with methods including negative sampling and phrase representations. |
| 2014 | Pennington, Socher, and Manning: [GloVe](https://aclanthology.org/D14-1162/) | Learned word vectors from aggregated co-occurrence statistics. The [authors' project page](https://nlp.stanford.edu/projects/glove/) illustrates the resulting geometry. |
| 2017 | Vaswani and colleagues: [Attention Is All You Need](https://arxiv.org/abs/1706.03762) | Introduced the transformer architecture, using attention to process relationships in sequences without recurrent layers. |
| 2018 | Peters and colleagues: [ELMo / Deep contextualized word representations](https://arxiv.org/abs/1802.05365) | Produced representations that change with a word's context using a bidirectional recurrent language model. Contextual embeddings also developed outside transformer architectures. |
| 2018 | Devlin and colleagues: [BERT](https://arxiv.org/abs/1810.04805) | Combined transformer encoders with bidirectional pretraining and adaptation to language-understanding tasks. The initial preprint appeared in 2018; the conference publication followed in 2019. |
| 2019 | Reimers and Gurevych: [Sentence-BERT](https://arxiv.org/abs/1908.10084) | Adapted pretrained encoders to produce sentence embeddings suitable for efficient similarity comparisons. |
| 2020 | Brown and colleagues: [GPT-3 / Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) | Demonstrated task adaptation through instructions and examples in context without task-specific weight updates. |
| 2022 | Wei and colleagues: [Chain-of-Thought Prompting](https://arxiv.org/abs/2201.11903) | Showed how examples of intermediate reasoning could improve performance on studied reasoning tasks. |
| 2022 | Ouyang and colleagues: [InstructGPT](https://arxiv.org/abs/2203.02155) | Combined demonstrations and human preference feedback to improve instruction following. |
| 2023 | Rafailov and colleagues: [Direct Preference Optimization](https://arxiv.org/abs/2305.18290) | Offered a direct method for learning from response preferences. |
| 2025 | DeepSeek-AI: [DeepSeek-R1, original report](https://arxiv.org/html/2501.12948v1) | Documented reasoning training using reinforcement learning, supervised data, and distillation. |

For a visual narrative connecting several of these developments, return to [Alammar's illustrated history of BERT and its predecessors](https://jalammar.github.io/illustrated-bert/).

## Beyond language models

These additional creator-produced videos widen the picture after the neural-network basics:

| Topic and video | What to look for | Prerequisites |
| --- | --- | --- |
| **Vision:** Josh Starmer / StatQuest, [Image Classification with Convolutional Neural Networks](https://www.youtube.com/watch?v=HGwBXDKFk9I) | How filters, feature maps, and pooling let a network work with image structure. | Network basics, backpropagation, and multiple inputs and outputs. |
| **Sequences:** Josh Starmer / StatQuest, [Recurrent Neural Networks, Clearly Explained!!!](https://www.youtube.com/watch?v=AsNTP8Kwu80) | How a network carries state between sequence steps and why this creates training challenges. This supplies background for the recurrent models in the history above. | Network basics and backpropagation. |
| **Image generation:** Stephen Welch / Welch Labs, hosted by 3Blue1Brown, [But how do AI images and videos actually work?](https://www.youtube.com/watch?v=iv-5mZ_9CPY) | A visual introduction to diffusion, text-image representations, and conditioning image generation on text. See the [creator's lesson page](https://www.3blue1brown.com/lessons/diffusion-models/). | Network and embedding intuition; probability is helpful. Read the video's description for the creator's technical corrections. |

## Connecting the fundamentals to agents

The next step is understanding how a model participates in a larger system:

- [How language models work](../language-models/how-language-models-work.md) connects neural networks to generation, assistant training, and current context.
- [Reasoning models](../language-models/reasoning-models.md) explains intermediate work and how it connects to evidence from tools.
- [How agents work](../agent-loops-and-autonomy/how-agents-work.md) introduces the runtime, tools, environment, and action loop around a model.
- [Context engineering and memory](../context-and-memory/context-engineering-and-memory.md) explains how retrieved information and working state become available during a task.
- [Skills, tools, hooks, and delegation](../tool-use-and-integrations/skills-tools-hooks-and-delegation.md) explains the mechanisms for organizing and executing agent work.
