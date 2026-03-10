{
  "title": "RAG Quiz",
  "questions": [
    {
      "question": "What are the two primary components that comprise the Retrieval-Augmented Generation (RAG) architecture?",
      "answerOptions": [
        {
          "text": "Parametric memory and non-parametric memory",
          "isCorrect": true,
          "rationale": "RAG combines a pre-trained seq2seq model (parametric) with a dense vector index of Wikipedia (non-parametric)."
        },
        {
          "text": "Supervised learning and unsupervised learning modules",
          "isCorrect": false,
          "rationale": "While RAG involves pre-training and fine-tuning, the core architecture is defined by its memory components rather than learning paradigms."
        },
        {
          "text": "Dense and sparse vector indices",
          "isCorrect": false,
          "rationale": "RAG specifically utilizes a dense vector index; sparse indices are not a primary structural component of the hybrid model described."
        },
        {
          "text": "Encoder-only and decoder-only architectures",
          "isCorrect": false,
          "rationale": "RAG uses a full encoder-decoder transformer (BART) as its generator component."
        }
      ],
      "hint": "Think about the two ways the model stores and accesses factual knowledge."
    },
    {
      "question": "Which specific pre-trained model is used as the parametric generator in the RAG framework?",
      "answerOptions": [
        {
          "text": "BART",
          "isCorrect": true,
          "rationale": "BART-large is used as the generator because it is a pre-trained seq2seq transformer that performs well on diverse generation tasks."
        },
        {
          "text": "GPT-2",
          "isCorrect": false,
          "rationale": "GPT-2 is a decoder-only model, whereas RAG utilizes the encoder-decoder structure of BART."
        },
        {
          "text": "BERT",
          "isCorrect": false,
          "rationale": "BERT is used for the query and document encoders in the retriever component, but not as the generator."
        },
        {
          "text": "T5",
          "isCorrect": false,
          "rationale": "T5 is a comparable model mentioned in the paper, but BART was the specific choice for the RAG experiments."
        }
      ],
      "hint": "This model is a pre-trained seq2seq transformer known for its denoising objective."
    },
    {
      "question": "How does the non-parametric memory component in RAG access its knowledge base?",
      "answerOptions": [
        {
          "text": "Through a pre-trained neural retriever",
          "isCorrect": true,
          "rationale": "The retriever, based on Dense Passage Retrieval (DPR), uses a bi-encoder architecture to find relevant documents."
        },
        {
          "text": "Via keyword-based TF-IDF search",
          "isCorrect": false,
          "rationale": "The paper highlights that neural, dense retrieval outperforms word overlap-based methods like TF-IDF for most tasks."
        },
        {
          "text": "By querying a fixed relational database",
          "isCorrect": false,
          "rationale": "RAG queries a dense vector index of text passages rather than a structured relational database."
        },
        {
          "text": "Through a manual lookup table",
          "isCorrect": false,
          "rationale": "The access mechanism is a differentiable neural retriever capable of end-to-end training."
        }
      ],
      "hint": "Consider the role of the Dense Passage Retriever (DPR) mentioned in the text."
    },
    {
      "question": "What is the primary difference between the RAG-Sequence and RAG-Token models?",
      "answerOptions": [
        {
          "text": "RAG-Sequence uses the same document for the entire sequence, while RAG-Token can use different documents for each token.",
          "isCorrect": true,
          "rationale": "RAG-Sequence marginalizes across documents on a per-output basis, whereas RAG-Token does so for each generated token."
        },
        {
          "text": "RAG-Sequence is designed for classification, while RAG-Token is strictly for generation.",
          "isCorrect": false,
          "rationale": "Both models can be used for various tasks, though they are equivalent for sequence classification (length one)."
        },
        {
          "text": "RAG-Sequence uses a frozen retriever, while RAG-Token uses a fine-tuned retriever.",
          "isCorrect": false,
          "rationale": "Both models can undergo joint fine-tuning of the retriever (query encoder) and the generator."
        },
        {
          "text": "RAG-Sequence is parametric only, while RAG-Token is non-parametric only.",
          "isCorrect": false,
          "rationale": "Both models are hybrid systems combining parametric and non-parametric components."
        }
      ],
      "hint": "Focus on the granularity at which the model conditions on retrieved passages during text generation."
    },
    {
      "question": "During RAG training, how are the retrieved documents mathematically treated within the probabilistic model?",
      "answerOptions": [
        {
          "text": "As latent variables",
          "isCorrect": true,
          "rationale": "Documents are treated as latent variables that are marginalized out to produce the final output distribution."
        },
        {
          "text": "As fixed supervised labels",
          "isCorrect": false,
          "rationale": "The model is trained without direct supervision on which specific document should be retrieved for a given query."
        },
        {
          "text": "As noise-inducing outliers",
          "isCorrect": false,
          "rationale": "The documents provide essential factual context and are a core part of the predictive probability."
        },
        {
          "text": "As target sequences",
          "isCorrect": false,
          "rationale": "The target sequences are the generated answers or claims ($y$), not the retrieved documents ($z$)."
        }
      ],
      "hint": "Consider the term for variables that are not directly observed but are used to model the observed data."
    },
    {
      "question": "Which computational technique allows the RAG retriever to find the top-$K$ documents from millions of entries in sub-linear time?",
      "answerOptions": [
        {
          "text": "Maximum Inner Product Search (MIPS)",
          "isCorrect": true,
          "rationale": "MIPS, implemented via libraries like FAISS, allows for efficient retrieval by identifying vectors with the highest similarity to the query."
        },
        {
          "text": "Principal Component Analysis (PCA)",
          "isCorrect": false,
          "rationale": "PCA is a dimensionality reduction technique, not a search algorithm for large-scale retrieval."
        },
        {
          "text": "Stochastic Gradient Descent (SGD)",
          "isCorrect": false,
          "rationale": "SGD is an optimization algorithm used for training parameters, not for retrieving documents at inference time."
        },
        {
          "text": "Recursive Beam Search",
          "isCorrect": false,
          "rationale": "Beam search is used for decoding the generator's output, not for document retrieval."
        }
      ],
      "hint": "It involves finding the vectors that maximize the dot product with the query vector."
    },
    {
      "question": "What is the rationale provided in the paper for keeping the document encoder and its index fixed during training?",
      "answerOptions": [
        {
          "text": "Updating them is computationally expensive because it requires periodic re-indexing of the entire document collection.",
          "isCorrect": true,
          "rationale": "Re-encoding 21 million documents during every training step would be prohibitively costly, and fixed embeddings still yield strong results."
        },
        {
          "text": "The document encoder is already perfect and cannot be improved by fine-tuning.",
          "isCorrect": false,
          "rationale": "While the encoder is pre-trained, the decision to freeze it is based on efficiency rather than a claim of perfection."
        },
        {
          "text": "The query encoder and generator provide sufficient gradients for the document encoder to learn implicitly.",
          "isCorrect": false,
          "rationale": "The document encoder does not receive updates at all when it is frozen; only the query encoder and generator are fine-tuned."
        },
        {
          "text": "The document index uses a sparse representation that does not allow for gradient-based updates.",
          "isCorrect": false,
          "rationale": "The index uses dense representations, but updating them would require updating the entire FAISS index."
        }
      ],
      "hint": "Consider the logistics involved in updating representations for 21 million Wikipedia chunks."
    },
    {
      "question": "What significant advantage does RAG's non-parametric memory offer over \"closed-book\" models like T5 when world knowledge changes?",
      "answerOptions": [
        {
          "text": "The model's knowledge can be updated at test time by simply replacing the document index.",
          "isCorrect": true,
          "rationale": "By \"hot-swapping\" the index with a newer one, the model can answer questions about recent events without being retrained."
        },
        {
          "text": "It uses significantly fewer parameters to store the same amount of factual information.",
          "isCorrect": false,
          "rationale": "While T5 is parameter-heavy, the primary advantage cited for updating knowledge is the swapability of the index."
        },
        {
          "text": "It is completely immune to generating hallucinations.",
          "isCorrect": false,
          "rationale": "While RAG reduces hallucinations, the authors note it may still produce incorrect content if the retrieved documents are biased or incorrect."
        },
        {
          "text": "It eliminates the need for any parametric components.",
          "isCorrect": false,
          "rationale": "RAG still relies on a parametric generator to synthesize and format the retrieved information."
        }
      ],
      "hint": "Think about how the authors demonstrated updating the model with a 2018 Wikipedia dump compared to a 2016 dump."
    },
    {
      "question": "How did RAG models perform relative to a BART baseline in human evaluations for Jeopardy question generation?",
      "answerOptions": [
        {
          "text": "RAG models were judged as significantly more factual and specific.",
          "isCorrect": true,
          "rationale": "Human evaluators found RAG-generated content to be more grounded in truth and tailored to the input entities."
        },
        {
          "text": "RAG models were found to be less diverse in their choice of vocabulary.",
          "isCorrect": false,
          "rationale": "Diversity metrics actually showed that RAG models produce more varied n-grams than the BART baseline."
        },
        {
          "text": "Both models performed identically in terms of factuality, but RAG was faster.",
          "isCorrect": false,
          "rationale": "RAG was found to be more factual, and retrieval-based models are generally slower due to the additional retrieval step."
        },
        {
          "text": "BART was preferred for its superior ability to handle complex entities.",
          "isCorrect": false,
          "rationale": "Evaluators indicated that BART was more factual in only 7.1% of cases, whereas RAG was superior in 42.7%."
        }
      ],
      "hint": "Consider the qualitative metrics used to evaluate the generation of precise factual statements."
    },
    {
      "question": "In the RAG-Sequence decoding process, what characterizes the \"Thorough Decoding\" approach?",
      "answerOptions": [
        {
          "text": "It runs beam search for each document and calculates marginals by performing additional forward passes for hypotheses across all beams.",
          "isCorrect": true,
          "rationale": "This method ensures a more accurate estimation of $p(y|x)$ by accounting for hypotheses that may not have appeared in every document's beam."
        },
        {
          "text": "It uses a single beam search that approximates the marginal probability at each token step.",
          "isCorrect": false,
          "rationale": "This describes the decoding for RAG-Token; RAG-Sequence likelihood does not break down into a standard per-token likelihood."
        },
        {
          "text": "It assumes the probability is zero if a hypothesis was not generated during the initial beam search of a document.",
          "isCorrect": false,
          "rationale": "This describes the \"Fast Decoding\" approximation, which is more efficient but less thorough."
        },
        {
          "text": "It involves manually checking the top retrieved document for the correct answer span.",
          "isCorrect": false,
          "rationale": "RAG is a generative model and does not rely on manual extraction or checking of spans."
        }
      ],
      "hint": "This procedure is used to score hypotheses across multiple documents to estimate the full marginal probability."
    },
    {
      "question": "Regarding the FEVER fact verification task, what was a key result of the RAG experiments?",
      "answerOptions": [
        {
          "text": "RAG achieved competitive results without the need for intermediate retrieval supervision.",
          "isCorrect": true,
          "rationale": "Unlike traditional pipeline systems for FEVER, RAG learned to find evidence automatically through its latent variable approach."
        },
        {
          "text": "RAG outperformed state-of-the-art models by leveraging gold evidence sentences.",
          "isCorrect": false,
          "rationale": "RAG achieved results within 4.3% of the state-of-the-art without using the gold evidence provided in the task."
        },
        {
          "text": "The model failed to distinguish between 'Supported' and 'Refuted' claims without training on gold spans.",
          "isCorrect": false,
          "rationale": "RAG was able to perform both 3-way and 2-way classification with high accuracy using its own retrieved evidence."
        },
        {
          "text": "The model's retriever was unable to find articles annotated as gold evidence in FEVER.",
          "isCorrect": false,
          "rationale": "The paper reports that a gold article was present in the top 10 retrieved articles in 90% of cases."
        }
      ],
      "hint": "Consider how RAG's training process differs from traditional multi-stage pipelines that require specifically labeled evidence."
    },
    {
      "question": "How does increasing the number of retrieved documents ($K$) at test time typically affect the performance of RAG models?",
      "answerOptions": [
        {
          "text": "For RAG-Sequence, performance in Open-domain QA monotonically improves with more documents.",
          "isCorrect": true,
          "rationale": "More documents provide a better approximation of the marginal probability for RAG-Sequence, leading to higher accuracy."
        },
        {
          "text": "For RAG-Token, performance always peaks at exactly 50 documents.",
          "isCorrect": false,
          "rationale": "The paper notes that RAG-Token performance for NQ actually peaks around 10 retrieved documents."
        },
        {
          "text": "Retrieving more documents consistently reduces the diversity of the generated text.",
          "isCorrect": false,
          "rationale": "Diversity is generally higher in RAG models, and increasing $K$ can affect specific metrics like Rouge-L or Bleu-1 differently."
        },
        {
          "text": "Increasing $K$ beyond 5 has no measurable impact on performance due to retriever collapse.",
          "isCorrect": false,
          "rationale": "The number of retrieved documents significantly impacts both performance and runtime as shown in the ablation studies."
        }
      ],
      "hint": "Think about the graphs provided in the results section regarding the effect of retrieving more documents."
    },
    {
      "question": "What primary source of text was used to create the 21 million 100-word chunks for RAG's non-parametric memory?",
      "answerOptions": [
        {
          "text": "A December 2018 Wikipedia dump",
          "isCorrect": true,
          "rationale": "This specific snapshot of Wikipedia provided the foundational data for the document index used in the main experiments."
        },
        {
          "text": "The entire Common Crawl web corpus",
          "isCorrect": false,
          "rationale": "Common Crawl is too large and contains too much noise; Wikipedia is preferred for its high-quality factual content."
        },
        {
          "text": "A collection of curated medical journals",
          "isCorrect": false,
          "rationale": "While RAG could be used with such data, the paper specifically used Wikipedia for general-purpose NLP tasks."
        },
        {
          "text": "All books published before the 20th century",
          "isCorrect": false,
          "rationale": "Wikipedia provides more contemporary and structured factual knowledge required for tasks like NQ and TriviaQA."
        }
      ],
      "hint": "This resource is the 'workhorse' of open-domain knowledge in NLP research."
    },
    {
      "question": "In the RAG-Token model, what is the role of marginalization across the top-$K$ documents?",
      "answerOptions": [
        {
          "text": "It blends the distributions for the next output token from each document given the current context.",
          "isCorrect": true,
          "rationale": "This allows the model to potentially combine information from multiple different documents to form a single answer."
        },
        {
          "text": "It selects the single most likely document to use for all future tokens in the sequence.",
          "isCorrect": false,
          "rationale": "Selecting a single document for the entire sequence is the defining characteristic of the RAG-Sequence model."
        },
        {
          "text": "It calculates the average length of the retrieved documents to adjust the generation speed.",
          "isCorrect": false,
          "rationale": "Marginalization is a probabilistic step to find the likelihood of the output tokens, not a structural or speed adjustment."
        },
        {
          "text": "It identifies the document with the highest word overlap with the input query.",
          "isCorrect": false,
          "rationale": "The retriever uses dense embeddings to find documents, and marginalization uses those documents to calculate token probabilities."
        }
      ],
      "hint": "Consider how the model uses the top $K$ documents to decide what the next word should be."
    },
    {
      "question": "According to the paper, how does the RAG model handle situations where no correct answer is contained within any of the retrieved documents?",
      "answerOptions": [
        {
          "text": "It can still generate correct answers using its internal parametric knowledge.",
          "isCorrect": true,
          "rationale": "The paper notes that RAG achieved 11.8% accuracy on NQ even when the answer was not in any retrieved document."
        },
        {
          "text": "It returns a standard 'no information found' token.",
          "isCorrect": false,
          "rationale": "The model is designed to always attempt a generation based on the provided input and context."
        },
        {
          "text": "It automatically triggers a new search with a different retrieval strategy.",
          "isCorrect": false,
          "rationale": "The retriever is fixed at test time and does not have an iterative re-searching mechanism described in the paper."
        },
        {
          "text": "The model fails to decode and produces an empty string.",
          "isCorrect": false,
          "rationale": "RAG leverages its pre-trained BART generator, which contains extensive knowledge in its own parameters."
        }
      ],
      "hint": "Recall the performance of 'closed-book' models that have no retrieval at all."
    }
  ]
}