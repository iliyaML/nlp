References:

- [20230523 State of GPT by Andrej Karpathy](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)
- [Notes](https://iliya.web.app/notes/nlp/state-of-gpt-2023/)

---

1. Training
   - GPT Assistant Training Pipeline - consists of 3 main steps
     1. **Pre-training** - require significant compute (~1000 GPUs), end up with a Base model
        - Data Collection
          - Huge corpus of text data from the Internet (Wikipedia, Books, GitHub, etc.)
          - High quantity/Low quality
        - Tokenization
          - Convert text to a list of integers
        - 2 Example Models
          - GPT-3-175B (2020) vs. LLaMA-65B (2023) comparison
          - Size is not everything, LLaMA is a much better model as it has been trained much longer on much bigger dataset (1T tokens)
          - Context length: 1k-100k tokens (working memory)
        - Pre-training
          - Training Process
            - Inputs to the transformer of the shape (B, T) tensor
              - B is the batch size, T is the maximum context length
              - Training sequences are laid out as rows, delimited by special <|endoftext|> tokens
          - Training Curve - loss decreases over time
        - Base Models Learn Powerful, General Representations (GPT-1)
          - Do pre-training then fine-tune on a particular task like sentiment classification
        - Base Models can be Prompted into Completing Tasks (GPT-2)
          - Started the era of prompting as these models can be tricked to do question-answering tasks using clever prompting
        - Base Models in the Wild
          - GPT-4 (base model not released), GPT-3 (available via API), GPT-2 (weights are released), LLaMA (not commercially licensed)
        - Base Models are NOT ‘Assistants’
          - They are basically document completers, though they can be tricked into being AI Assistants using some clever prompting
     2. **Supervised Fine-tuning (SFT)** - require less compute than pre-training (1-100 GPUs)
        - SFT Dataset - low quantity compared to Pre-training, but high quality
          - QA prompt responses
     3. **Reinforcement Learning from Human Feedback (RLHF)** - still research/experimental territory
        1. Reward Modeling (RM) - rank outputs of a model
           - RM Dataset
           - RM Training
        2. Reinforcement Learning (RL)
           - RL Training
        - Why RLHF? - it just works better
        - Mode Collapse
          - Not strictly an improvement on the base models, they lose some entropy (outputs of the model lose some diversity)
     - Assistant Models in the Wild
       - Best Models: GPT-4, Claude 1, …
2. Applications
   - Human Text Generation vs. LLM Text Generation
     - For GPT’s, it’s just a sequence of tokens, each chunk is roughly the same amount of computation work for each token
     - Transformers are just like token simulators
       - They do have a lot of storage (10B parameters), and a relatively large working memory (context length or window)
       - Anything that fits into the context window is immediately available (direct access) to the Transformer through the self-attention mechanism
   - Chain of Thought
     - “Transformers need tokens to think”
       - Few shot prompting - give examples that shows the Transformer that it should show its work
     - Can elicit this behavior by adding “Let’s think step by step” in the prompt
       - Conditions the Transformer to show its work
       - Spread out the reasoning over many tokens
   - Ensemble Multiple Attempts
     - Self-Consistency - sample multiple answers as sometimes it can get unlucky with its generation of output
   - Ask for Reflection
     - Ask the model if it achieved the target of your prompt (works for GPT-4)
   - Recreate Our ‘System 2’ - slower, deliberate planning part of our brain
     - Tree of Thoughts paper - maintaining multiple completions for any given prompt, scoring them along the way and keeping the ones that are going well
   - Chains / Agents
     - ReAct paper
     - AutoGPT - allow an LLM to sort of keep a task list and continue to recursively break down tasks
   - Condition on Good Performance
     - “LLMs don’t want to succeed. They want to imitate. You want to succeed, and you should ask for it.”
     - Ask the model to pretend that its a leading expert in something
     - “Let’s work this out in a step by step way to be sure we have the right answer”
       - Conditions it on getting a right answer
   - Tool Use / Plugins - calculator
   - Retrieval-Augmented LLMs
     - Load related information into the model’s working memory (context window)
     - Recipe
       - Take relevant documents, split them up into chunks, embed all of them, and basically get embedding vectors that represent that data
       - Store the embeddings and chunks of text in the vector store
       - At test time, query your vector store, fetch chunks that might be relevant to your task, and stuff them into the prompt, and the model generates the response
   - Constrained Prompting
     - Techniques for enforcing a certain template in the outputs of LLMs
     - Guidance by Microsoft
   - Fine-tuning
     - Parameter Efficient Finetuning Techniques (PEFT) like LORA
       - Training only small, sparse pieces of your model while most of the base model is kept clamped
       - Works pretty well, empirically, and is cheaper
     - RLHF still experimental
   - Default Recommendations
     1. Achieve your top possible performance
     2. Optimize costs
   - Use Cases - GPTs/LLMs as Copilots
     - Use in low-stakes applications, combine with human oversight
     - Source of inspiration, suggestions
     - Copilots over autonomous agents
   - GPT-4 & Looking Forward
   - OpenAI API
