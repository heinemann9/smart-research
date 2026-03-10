{
  "title": "RAG Flashcards",
  "cards": [
    {
      "front": "What two primary components are combined in a Retrieval-Augmented Generation (RAG) system?",
      "back": "A retrieval model and a generative model."
    },
    {
      "front": "In the context of LLMs, what is 'parametric memory'?",
      "back": "The knowledge stored within the pre-trained weights of the model."
    },
    {
      "front": "What constitutes the 'non-parametric memory' in a RAG architecture?",
      "back": "External data stored in a source like a vector database or document library."
    },
    {
      "front": "According to Andrej Karpathy's analogy, if the LLM is the CPU, what is the context window?",
      "back": "The RAM."
    },
    {
      "front": "How does 'Context Engineering' differ from 'Prompt Engineering'?",
      "back": "Context engineering designs the entire information flow and architecture, while prompt engineering optimizes single instructions."
    },
    {
      "front": "What is the 'Lost in the Middle' effect in LLMs?",
      "back": "The tendency for models to struggle with reasoning from information buried in the middle of a long context window."
    },
    {
      "front": "Which RAG context failure occurs when incorrect or hallucinated data enters the model's memory and propagates through the workflow?",
      "back": "Context Poisoning"
    },
    {
      "front": "What failure mode describes a model losing its ability to generalize because the context window is overstuffed with accumulated history?",
      "back": "Context Distraction"
    },
    {
      "front": "What is 'Context Confusion' in an agentic LLM system?",
      "back": "A performance drop caused by presenting the model with too much irrelevant data or too many tools."
    },
    {
      "front": "Which RAG failure mode involves contradictory or duplicate information causing logical inconsistencies in reasoning?",
      "back": "Context Clash"
    },
    {
      "front": "What are the three primary metrics provided by the RAGAS framework for quality assessment?",
      "back": "Answer relevance, context precision/recall, and faithfulness."
    },
    {
      "front": "Which RAG evaluation tool is specialized for deep tracing visibility and experiment management for teams using LangChain?",
      "back": "LangSmith"
    },
    {
      "front": "Which open-source evaluation solution uses OpenTelemetry (OTEL) for automated instrumentation of LLM request paths?",
      "back": "Arize Phoenix"
    },
    {
      "front": "How does TruLens programmatically evaluate components of an application's execution flow?",
      "back": "Through the use of feedback functions."
    },
    {
      "front": "Which evaluation framework functions as a 'unit-testing' solution for LLMs and integrates natively with Pytest?",
      "back": "DeepEval"
    },
    {
      "front": "What is the primary focus of the tool 'Promptfoo' in RAG development?",
      "back": "Automated security testing and test-driven prompt engineering within CI/CD pipelines."
    },
    {
      "front": "What is the core purpose of 'Recursive Character Chunking'?",
      "back": "Splitting text using a hierarchy of separators like paragraphs, then lines, then words to keep related content together."
    },
    {
      "front": "Which chunking strategy is best for maintaining structural integrity in technical documentation or structured data?",
      "back": "Document-based (Specialized) Chunking"
    },
    {
      "front": "What are the disadvantages of using chunks that are too large in a RAG system?",
      "back": "Imprecise retrieval and bloating the LLM context with irrelevant content."
    },
    {
      "front": "Why is 'Semantic Chunking' often preferred for production-grade RAG applications despite higher indexing costs?",
      "back": "It groups text by meaning rather than arbitrary character counts, leading to higher retrieval precision."
    },
    {
      "front": "What is the 'Hierarchical' or 'Parent-Child' chunking strategy?",
      "back": "Retrieving small 'child' chunks for precision but sending the larger 'parent' context to the LLM."
    },
    {
      "front": "In RAG systems, why is 'overlap' included between text chunks?",
      "back": "To prevent context loss where a critical piece of information might be cut in half by a split point."
    },
    {
      "front": "What is the primary problem 'Graph of Records' (GoR) aims to solve in RAG systems?",
      "back": "The neglect of informative historical LLM responses in long-context global summarization."
    },
    {
      "front": "How is a 'Graph of Records' constructed during the RAG process?",
      "back": "By establishing edges between retrieved text chunks and the corresponding LLM-generated responses."
    },
    {
      "front": "What objective function does GoR use to optimize node embeddings in a self-supervised manner?",
      "back": "A BERTScore-based contrastive loss."
    },
    {
      "front": "Why does supervised training on global summarization queries often perform worse than self-supervised training in GoR?",
      "back": "Global reference summaries are highly correlated with many nodes, confusing the model's optimization direction."
    },
    {
      "front": "In the GoR architecture, what is the role of Graph Neural Networks (GNNs)?",
      "back": "Learning node embeddings that capture both semantic content and structural relationships."
    },
    {
      "front": "What does the 'faithfulness' metric specifically measure in RAG evaluation?",
      "back": "Whether the generated answer is factually consistent with the retrieved context."
    },
    {
      "front": "Which evaluation approach uses a separate LLM to grade the accuracy and relevance of a RAG system's responses?",
      "back": "LLM-as-a-Judge"
    },
    {
      "front": "According to research, by what percentage can RAG reduce hallucinations compared to pure generation systems?",
      "back": "$30-50\\%$"
    },
    {
      "front": "In vector search, what are 'embeddings'?",
      "back": "Mathematical representations of concepts that allow the system to match queries based on meaning rather than keywords."
    },
    {
      "front": "What is the purpose of 'Hybrid Retrieval' in modern RAG pipelines?",
      "back": "Combining dense semantic similarity with traditional keyword filters to improve recall."
    },
    {
      "front": "Which specific open-source framework by Iguazio orchestrates RAG evaluation tools as part of the AI lifecycle?",
      "back": "MLRun"
    },
    {
      "front": "What is the 'lost in the middle' mitigation strategy for context engineering?",
      "back": "Periodically summarizing conversation history or prioritizing recent context via scoring."
    },
    {
      "front": "How does 'Semantic Search' differ from 'Keyword Search'?",
      "back": "Semantic search understands the meaning and conceptual closeness of terms, while keyword search only matches exact strings."
    },
    {
      "front": "What is the 'Augmentation Phase' in a RAG pipeline?",
      "back": "Combining the user's original query with the retrieved context to create a detailed prompt for the LLM."
    },
    {
      "front": "Why is RAG considered more cost-effective than fine-tuning for maintaining up-to-date knowledge?",
      "back": "It allows updating the knowledge base without the high resource requirements of retraining the model."
    },
    {
      "front": "What is a 'Golden Standard' dataset in RAG evaluation?",
      "back": "A trusted set of ground-truth Q&A pairs used for bulk performance testing."
    },
    {
      "front": "In GoR, what is 'Query Simulation'?",
      "back": "Using an LLM to generate diverse, meaningful questions based on text chunks to create training data."
    },
    {
      "front": "What is the 'retrieve-then-generate' paradigm?",
      "back": "The core RAG workflow of fetching relevant data before using an LLM to produce a grounded response."
    },
    {
      "front": "Which chunking strategy is specifically designed for codebases to capture dependencies and architecture?",
      "back": "Coding Assistant (Cursor/Copilot style) Context Engineering"
    },
    {
      "front": "In the context of RAG, what is 'groundedness'?",
      "back": "The degree to which the LLM's response is supported by and derived from the retrieved source documents."
    },
    {
      "front": "Which tool would you use for 'test-driven prompt engineering' to identify quality issues during development?",
      "back": "Promptfoo"
    },
    {
      "front": "What does the 'Hit Rate' metric measure in RAG retrieval?",
      "back": "Whether the correct or relevant chunk was successfully retrieved among the top results."
    },
    {
      "front": "How does 'Context Pruning' affect a RAG pipeline's efficiency?",
      "back": "It reduces token costs and latency by removing irrelevant information from the retrieved context."
    },
    {
      "front": "What is the 'lost in the middle' effect's impact on token limits?",
      "back": "It proves that simply having a large context window (e.g., 100K tokens) does not guarantee the model will find information buried in the middle."
    },
    {
      "front": "Why is 'Vector Search' preferred over keyword search in healthcare or legal RAG systems?",
      "back": "It captures conceptual meaning and context, which are essential for navigating complex professional terminology."
    },
    {
      "front": "In GoR, what is the 'Self-evolving' manner of historical responses?",
      "back": "Appending previously generated responses back into the retrieval corpus to help generate more refined future knowledge."
    },
    {
      "front": "What is 'HITL' in the context of RAG evaluation?",
      "back": "Human-In-The-Loop feedback, where experts review and score system outputs."
    },
    {
      "front": "Which evaluation framework uses 'unit-testing' style metrics like contextual recall and contextual precision?",
      "back": "DeepEval"
    },
    {
      "front": "What is the primary risk of using 'Parametric Memory' alone for enterprise applications?",
      "back": "Hallucinations and outdated information due to the model's fixed training cutoff."
    },
    {
      "front": "In GoR training, what is the function of the 'Pair-wise Ranking Loss'?",
      "back": "It imposes stricter constraints on node embeddings by explicitly optimizing the relative order of positive and negative samples."
    },
    {
      "front": "Which chunking strategy is recommended for most general applications as a 'solid default'?",
      "back": "Recursive chunking (at 512 tokens with 100 token overlap)."
    },
    {
      "front": "What is 'Semantic Caching' in a RAG system?",
      "back": "Storing and reusing responses for queries that are semantically similar to previous ones to improve performance."
    },
    {
      "front": "What does the 'ROUGE' metric generally assess in summarization tasks?",
      "back": "The overlap of n-grams between the generated summary and a reference summary."
    },
    {
      "front": "In the 'Graph of Records' paper, what was the observed improvement over standard retrievers on the WCEP dataset?",
      "back": "$15\\%$ in Rouge-L, $8\\%$ in Rouge-1, and $19\\%$ in Rouge-2."
    },
    {
      "front": "What is the 'Lost in the Middle' research finding for top-tier models like Llama 3.1 405b?",
      "back": "Accuracy begins to dip significantly beyond 32,000 tokens."
    },
    {
      "front": "How does 'Knowledge Decomposition' improve RAG performance?",
      "back": "By breaking documents into smaller, self-contained units that are easier for retrievers to match and models to process."
    },
    {
      "front": "Which evaluation tool specifically supports 'LLM-as-a-Judge' metrics out of the box?",
      "back": "DeepEval (or LangSmith/RAGAS)."
    },
    {
      "front": "What are the two 'failure modes' of chunking identified by Seenivasa Ramadurai?",
      "back": "Chunks that are too large (imprecise retrieval) and chunks that are too small (orphaned fragments lacking context)."
    },
    {
      "front": "In GoR, why is 'BERTScore' preferred over 'SBERT' for modeling correlations between complex text?",
      "back": "BERTScore better quantifies semantic similarity between paragraphs for ranking purposes in global summarization."
    },
    {
      "front": "What is 'Contextual Relevancy' in the generator phase?",
      "back": "The measure of how much of the retrieved context is actually useful and relevant to answering the query."
    },
    {
      "front": "Which framework allows for teams of specialized sub-agents to share and update context?",
      "back": "LangGraph (or orchestrators like CrewAI)."
    },
    {
      "front": "What is the primary benefit of 'Open Source Embeddings' (like Sentence Transformers) for large-scale indexing?",
      "back": "Cost sustainability when embedding millions of document chunks compared to cloud APIs."
    },
    {
      "front": "Which evaluation tool provides a 'metrics leaderboard' to compare different LLM application versions?",
      "back": "TruLens"
    },
    {
      "front": "What is the 'lost in the middle' effect's impact on cost and latency?",
      "back": "Larger context windows increase cost and latency without guaranteeing better answer retrieval."
    },
    {
      "front": "How often should production RAG systems be evaluated according to Iguazio?",
      "back": "Continuously when telemetry is available, or at least weekly for baseline scoring."
    },
    {
      "front": "In the context of 'Context Engineering', what is a 'quarantine buffer'?",
      "back": "A temporary storage area for unverified data to prevent it from poisoning the agent's memory."
    },
    {
      "front": "What is 'Agentic LLM failure' usually attributed to, according to Turing College?",
      "back": "A failure to pass the required context (missing facts, poor formatting) rather than model incapability."
    },
    {
      "front": "What is 'semantic similarity' in RAG retrieval?",
      "back": "A measure of how conceptually related two pieces of text are, calculated using vector distance."
    },
    {
      "front": "Which RAG component acts as the 'operating system' deciding what data to load into the context window?",
      "back": "Context Engineering"
    },
    {
      "front": "What is the 'DPR-style embedding score' used for?",
      "back": "Evaluating retrieval quality in dense passage retrieval architectures."
    },
    {
      "front": "What does the 'MRR' (Mean Reciprocal Rank) metric evaluate?",
      "back": "How high up in the retrieved results list the first relevant document appears."
    },
    {
      "front": "Which RAG architecture framework was introduced by Lewis et al. in 2020?",
      "back": "A model combining a pre-trained seq2seq parametric memory with a dense vector index of Wikipedia."
    }
  ]
}