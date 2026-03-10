{
  "title": "RAG Quiz",
  "questions": [
    {
      "question": "In the context of RAG evaluation, which open-source framework is specifically noted for its integration with Pytest to provide unit-testing capabilities for LLMs?",
      "answerOptions": [
        {
          "text": "DeepEval",
          "isCorrect": true,
          "rationale": "DeepEval is designed as a unit-testing solution that fits natively into the developer workflow using Pytest."
        },
        {
          "text": "RAGAS",
          "isCorrect": false,
          "rationale": "While RAGAS is a popular evaluation framework, it focuses on metrics like faithfulness and context precision rather than native Pytest unit-testing integration."
        },
        {
          "text": "LangSmith",
          "isCorrect": false,
          "rationale": "LangSmith focuses on observability, tracing, and dataset management, particularly for teams using LangChain."
        },
        {
          "text": "Arize Phoenix",
          "isCorrect": false,
          "rationale": "Arize Phoenix is primarily an observability and tracing solution built on OpenTelemetry for visualizing execution paths."
        }
      ],
      "hint": "Think about a framework that treats LLM outputs like traditional code unit tests."
    },
    {
      "question": "Which specific 'Context Failure' occurs when the model is overwhelmed by irrelevant documents or extraneous tool definitions, even if the context window is not full?",
      "answerOptions": [
        {
          "text": "Context Confusion",
          "isCorrect": true,
          "rationale": "Context confusion arises when the model is presented with too much non-essential data, leading to a drop in accuracy despite available space in the context window."
        },
        {
          "text": "Context Poisoning",
          "isCorrect": false,
          "rationale": "Poisoning refers specifically to the inclusion of incorrect or hallucinated information that derails subsequent reasoning."
        },
        {
          "text": "Context Clash",
          "isCorrect": false,
          "rationale": "A clash occurs when contradictory or duplicate information exists within the input, causing logical inconsistencies."
        },
        {
          "text": "Context Distraction",
          "isCorrect": false,
          "rationale": "Distraction is associated with very large context windows where the model loses focus on relevant information due to anchoring on history."
        }
      ],
      "hint": "This failure happens when the quantity of low-value information interferes with the model's decision-making."
    },
    {
      "question": "According to the 'Graph of Records' ($GoR$) methodology, what is the primary purpose of using $BERTScore$ in the self-supervised training phase?",
      "answerOptions": [
        {
          "text": "To rank nodes based on their similarity to reference summaries and fill the supervision gap",
          "isCorrect": true,
          "rationale": "BERTScore is used to quantify semantic similarity between reference summaries and nodes, allowing indirect supervision signals to optimize node embeddings."
        },
        {
          "text": "To act as a primary retriever for finding relevant text chunks during inference",
          "isCorrect": false,
          "rationale": "BERTScore is an evaluation and training objective component, whereas the retrieval during inference uses learned node embeddings."
        },
        {
          "text": "To generate diverse user queries from a long document using temperature sampling",
          "isCorrect": false,
          "rationale": "Query simulation is handled by LLMs using temperature sampling, not by the BERTScore objective function."
        },
        {
          "text": "To compress historical responses into a more compact vector space",
          "isCorrect": false,
          "rationale": "While GoR uses historical responses, BERTScore is specifically utilized for ranking and providing training objectives rather than compression."
        }
      ],
      "hint": "Consider how the system provides a 'labels' signal when actual text chunk indices are missing."
    },
    {
      "question": "Which chunking strategy is specifically recommended for complex legal or medical documents to ensure that units of text are semantically self-contained?",
      "answerOptions": [
        {
          "text": "Semantic Chunking",
          "isCorrect": true,
          "rationale": "Semantic chunking groups text by meaning rather than character count, ensuring that critical qualifiers in legal or medical text aren't cut off."
        },
        {
          "text": "Recursive Character Splitting",
          "isCorrect": false,
          "rationale": "Recursive splitting is based on separators like newlines and spaces, which may still split a single semantic idea across chunks."
        },
        {
          "text": "Fixed-Size Chunking",
          "isCorrect": false,
          "rationale": "Fixed-size chunking uses a set number of tokens and is prone to breaking sentences or paragraphs mid-thought."
        },
        {
          "text": "Simple Overlap Splitting",
          "isCorrect": false,
          "rationale": "Overlap splitting helps bridge context between chunks but does not inherently group text by conceptual meaning."
        }
      ],
      "hint": "This method focuses on 'topic shifts' rather than character or token counts."
    },
    {
      "question": "In the RAG architecture described by Lewis et al., how is the 'Non-parametric memory' distinct from 'Parametric memory'?",
      "answerOptions": [
        {
          "text": "It consists of an external dense vector index of a knowledge source that can be updated without retraining",
          "isCorrect": true,
          "rationale": "Non-parametric memory refers to the external database that allows the system to access fresh or specific information without modifying model weights."
        },
        {
          "text": "It refers to the factual knowledge stored within the weights of the pre-trained seq2seq model",
          "isCorrect": false,
          "rationale": "The knowledge stored within the model's weights is known as parametric memory."
        },
        {
          "text": "It is the part of the system responsible for generating the final natural language response",
          "isCorrect": false,
          "rationale": "The generative part of the model is the parametric component that uses the retrieved data to craft an answer."
        },
        {
          "text": "It is a temporary buffer that stores the current user prompt during the inference cycle",
          "isCorrect": false,
          "rationale": "While the prompt is held in memory, non-parametric memory refers specifically to the large-scale external knowledge base."
        }
      ],
      "hint": "Think about the component that acts as the 'external library' for the model."
    },
    {
      "question": "The 'lost in the middle' effect suggests that LLMs tend to struggle with which of the following?",
      "answerOptions": [
        {
          "text": "Reasoning from information buried in the center of a very large context window",
          "isCorrect": true,
          "rationale": "Research indicates that LLMs effectively attend to the beginning and end of a context window but lose track of information in the middle."
        },
        {
          "text": "Processing the very first tokens of a document when using semantic embeddings",
          "isCorrect": false,
          "rationale": "Models generally perform well at attending to the beginning of the context window."
        },
        {
          "text": "Connecting the retrieved context to the generative parametric memory during the final stage",
          "isCorrect": false,
          "rationale": "This describes a general RAG integration challenge, not the specific 'lost in the middle' phenomenon related to token position."
        },
        {
          "text": "Maintaining a high hit rate when the vector database contains more than one million vectors",
          "isCorrect": false,
          "rationale": "This is a retrieval latency or scaling issue rather than a model reasoning limitation based on context position."
        }
      ],
      "hint": "Focus on where the model's attention is weakest within a long prompt."
    },
    {
      "question": "Which RAG evaluation tool uses 'feedback functions' to programmatically evaluate groundedness and context relevance through OpenTelemetry traces?",
      "answerOptions": [
        {
          "text": "TruLens",
          "isCorrect": true,
          "rationale": "TruLens uses programmatic feedback functions to evaluate different stages of the RAG application's execution flow."
        },
        {
          "text": "Promptfoo",
          "isCorrect": false,
          "rationale": "Promptfoo is primarily used for test-driven prompt engineering and proactive security testing in CI/CD pipelines."
        },
        {
          "text": "LlamaIndex Evaluation Suite",
          "isCorrect": false,
          "rationale": "LlamaIndex offers component-wise evaluation but is not primarily defined by the 'feedback function' terminology used by TruLens."
        },
        {
          "text": "LangSmith",
          "isCorrect": false,
          "rationale": "LangSmith provides tracing and observability but uses a 'judge' or custom logic approach rather than the specific feedback function architecture."
        }
      ],
      "hint": "This tool features a metrics leaderboard to compare different application versions."
    },
    {
      "question": "What is a major risk of using a 'Full Context' approach (stuffing the entire document into the prompt) instead of a structured RAG pipeline?",
      "answerOptions": [
        {
          "text": "Higher latency and cost without a guarantee that the model finds the correct answer",
          "isCorrect": true,
          "rationale": "Processing excessive tokens is expensive and slow, and the model may still miss relevant details due to context reasoning limitations."
        },
        {
          "text": "It completely prevents the model from experiencing hallucinations",
          "isCorrect": false,
          "rationale": "Even with full context, models can still misinterpret data or hallucinate, especially if the context is confusing."
        },
        {
          "text": "It eliminates the need for parametric memory in the LLM",
          "isCorrect": false,
          "rationale": "Parametric memory is still required to process and generate language based on the provided context."
        },
        {
          "text": "It forces the model to use only keyword matching rather than semantic understanding",
          "isCorrect": false,
          "rationale": "Full context allows the model to use its full reasoning capability, but it does so inefficiently compared to RAG."
        }
      ],
      "hint": "Consider the trade-offs involving efficiency and model focus."
    },
    {
      "question": "How does 'Hierarchical Chunking' function in a RAG system?",
      "answerOptions": [
        {
          "text": "It retrieves small 'child' chunks for precision but passes larger 'parent' contexts to the LLM",
          "isCorrect": true,
          "rationale": "This strategy balances the need for accurate retrieval matching with the LLM's need for sufficient surrounding context to reason."
        },
        {
          "text": "It organizes documents based on a hierarchy of metadata tags like department or date",
          "isCorrect": false,
          "rationale": "This describes metadata filtering or data mesh organization rather than hierarchical text chunking."
        },
        {
          "text": "It uses a sequence of LLMs to progressively summarize text into a single root node",
          "isCorrect": false,
          "rationale": "While summarization can be hierarchical, this specific chunking strategy refers to the relationship between retrieved and injected text."
        },
        {
          "text": "It prioritizes chunks that appear at the beginning of a document over those in the middle",
          "isCorrect": false,
          "rationale": "This might be a ranking heuristic, but it is not the definition of hierarchical parent-child chunking."
        }
      ],
      "hint": "This involves using different chunk sizes for the retrieval stage versus the generation stage."
    },
    {
      "question": "According to the Deepchecks source, incorporating retrieval into generation in 2025 was found to reduce hallucinations by what percentage?",
      "answerOptions": [
        {
          "text": "$30-50\\%$",
          "isCorrect": true,
          "rationale": "A 2025 study on retrieval-augmented models demonstrated this significant reduction in hallucinations compared to generation-only systems."
        },
        {
          "text": "$15-20\\%$",
          "isCorrect": false,
          "rationale": "This range is closer to the reported hallucination rate of business LLMs like GPT-4.5, not the reduction achieved by RAG."
        },
        {
          "text": "$70-85\\%$",
          "isCorrect": false,
          "rationale": "While RAG is effective, a $70-85\\%$ reduction is higher than the findings reported in the specific 2025 studies cited."
        },
        {
          "text": "$10\\%$",
          "isCorrect": false,
          "rationale": "This represents a minor improvement and understates the impact reported in the architectural research."
        }
      ],
      "hint": "RAG provides a substantial, often near-half, reduction in factual errors."
    }
  ]
}