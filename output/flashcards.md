{
  "title": "RAG Flashcards",
  "cards": [
    {
      "front": "What is the primary motivation for developing Retrieval-Augmented Generation (RAG) models?",
      "back": "To address the limitations of pre-trained language models in accessing/manipulating precise knowledge and updating world knowledge."
    },
    {
      "front": "In a RAG model, what constitutes the 'parametric memory'?",
      "back": "A pre-trained sequence-to-sequence (seq2seq) model, such as BART-large."
    },
    {
      "front": "In the RAG architecture, what serves as the 'non-parametric memory'?",
      "back": "A dense vector index of a text corpus (e.g., Wikipedia) accessed with a neural retriever."
    },
    {
      "front": "Which specific pre-trained model is used as the retriever in the original RAG study?",
      "back": "Dense Passage Retriever (DPR)."
    },
    {
      "front": "Which pre-trained model is utilized as the generator in the RAG architecture?",
      "back": "BART-large."
    },
    {
      "front": "How does the RAG-Sequence model determine which document to use for generating a sequence?",
      "back": "It uses the same retrieved document to condition the entire generated sequence."
    },
    {
      "front": "How does the RAG-Token model differ from the RAG-Sequence model regarding document usage?",
      "back": "It can use a different retrieved document to condition each individual token in the generated sequence."
    },
    {
      "front": "In RAG, the retriever component $p_{\\eta}(z|x)$ is based on what type of architecture?",
      "back": "A bi-encoder architecture consisting of a query encoder and a document encoder."
    },
    {
      "front": "What is the mathematical purpose of the Query Encoder $q(x)$ in RAG?",
      "back": "To produce a dense representation of the input $x$ for similarity matching."
    },
    {
      "front": "What is the purpose of the Document Encoder $d(z)$ in RAG?",
      "back": "To produce dense representations of documents $z$ to build the non-parametric memory index."
    },
    {
      "front": "Which method is used to efficiently find the top-$k$ documents in the dense vector index?",
      "back": "Maximum Inner Product Search (MIPS)."
    },
    {
      "front": "During RAG training, why is the document encoder usually kept fixed?",
      "back": "Updating it would require periodic, costly updates to the entire document index."
    },
    {
      "front": "How is the generator $p_{\\theta}$ conditioned on the retrieved content $z$ and input $x$?",
      "back": "The input $x$ and retrieved passage $z$ are simply concatenated."
    },
    {
      "front": "What training objective is minimized when fine-tuning RAG models?",
      "back": "The negative marginal log-likelihood of the target sequence."
    },
    {
      "front": "In RAG-Sequence, how is the probability $p(y|x)$ calculated?",
      "back": "By marginalizing the sequence probability over the top-$k$ retrieved documents."
    },
    {
      "front": "In RAG-Token, how is the probability of the next token $y_i$ determined?",
      "back": "By marginalizing the next-token probability over the top-$k$ retrieved documents."
    },
    {
      "front": "Define 'Thorough Decoding' in the context of RAG-Sequence.",
      "back": "Running beam search for each document and scoring all hypotheses by summing probabilities across beams."
    },
    {
      "front": "Define 'Fast Decoding' in the context of RAG-Sequence.",
      "back": "Assuming the probability of a hypothesis is zero if it was not generated during the beam search for a specific document."
    },
    {
      "front": "What defines a 'knowledge-intensive' NLP task according to the RAG paper?",
      "back": "A task that a human could not reasonably perform without access to an external knowledge source."
    },
    {
      "front": "What are the three open-domain QA tasks where RAG set a new state-of-the-art (SOTA)?",
      "back": "Natural Questions (NQ), WebQuestions (WQ), and CuratedTrec (CT)."
    },
    {
      "front": "How does RAG perform on questions where the answer is not present in any retrieved document?",
      "back": "It can still generate correct answers using parametric memory, achieving $11.8\\%$ accuracy on NQ in such cases."
    },
    {
      "front": "In Jeopardy question generation, which RAG variant performed better?",
      "back": "RAG-Token."
    },
    {
      "front": "According to human evaluation, how do RAG generations compare to BART in terms of factuality?",
      "back": "RAG was found to be significantly more factual, with BART being better in only $7.1\\%$ of cases."
    },
    {
      "front": "According to human evaluation, how do RAG generations compare to BART in terms of specificity?",
      "back": "RAG generations were rated as more specific by a large margin."
    },
    {
      "front": "What trend was observed regarding the diversity of RAG generations compared to BART?",
      "back": "RAG-Sequence and RAG-Token both produced significantly more diverse $n$-grams than BART."
    },
    {
      "front": "How can RAG's knowledge be updated at test time without retraining?",
      "back": "By 'hot-swapping' the non-parametric memory index with a newer version."
    },
    {
      "front": "In retrieval ablations, how did learned differentiable retrieval compare to BM25 for Open-Domain QA?",
      "back": "Differentiable retrieval performed significantly better than the word overlap-based BM25."
    },
    {
      "front": "In which task did BM25 retrieval outperform RAG's dense retriever?",
      "back": "FEVER fact verification (due to its entity-centric nature)."
    },
    {
      "front": "How does RAG-Token handle cases where a response requires combining facts from multiple documents?",
      "back": "It can shift the document posterior $p(z|x, y, i)$ to different documents for different tokens."
    },
    {
      "front": "What is the effect on RAG-Sequence performance as more documents are retrieved at test time?",
      "back": "Exact Match performance on Open-Domain QA tasks increases monotonically."
    },
    {
      "front": "How does the number of parameters in RAG compare to the SOTA closed-book T5-11B model?",
      "back": "RAG achieves SOTA results with approximately $626$ million parameters, far fewer than the $11$ billion in T5."
    },
    {
      "front": "What is the 'retrieval collapse' phenomenon observed in preliminary RAG experiments?",
      "back": "The retriever learns to retrieve the same documents regardless of the input, causing the generator to ignore them."
    },
    {
      "front": "What library was used to implement and open-source RAG models?",
      "back": "HuggingFace Transformers."
    },
    {
      "front": "How does RAG handle the FEVER fact verification task differently than SOTA pipeline models?",
      "back": "It does not require direct supervision on which evidence documents should be retrieved."
    },
    {
      "front": "In the 'Sun Also Rises' example, what role does parametric knowledge play after a document is selected?",
      "back": "The generator completes the titles using its internal parameters once the non-parametric memory provides the initial guide."
    },
    {
      "front": "What is the primary benefit of the non-parametric memory being raw text rather than distributed representations?",
      "back": "It makes the memory human-readable (interpretable) and human-writable (easily updatable)."
    },
    {
      "front": "For CuratedTrec, how did the authors handle answer regular expressions for a generative model?",
      "back": "They used a pre-processing step to find the most frequent regex match in retrieved documents as the target."
    },
    {
      "front": "Why did the authors experiment with a 'Null document' mechanism?",
      "back": "To model cases where no useful information could be retrieved for a given input."
    },
    {
      "front": "In RAG-Sequence, if a hypothesis $y$ does not appear in a specific document's beam, how is its probability estimated during Thorough Decoding?",
      "back": "An additional forward pass is run for that document to calculate the probability."
    },
    {
      "front": "What specific December 2018 dump was used for the non-parametric knowledge source in the main experiments?",
      "back": "Wikipedia."
    },
    {
      "front": "What tool is used for building and searching the MIPS index in RAG?",
      "back": "FAISS."
    },
    {
      "front": "Which variant of BART is used in the generator component of RAG?",
      "back": "BART-large ($400$M parameters)."
    },
    {
      "front": "How are Wikipedia articles processed before being added to the RAG document index?",
      "back": "They are split into disjoint $100$-word chunks."
    },
    {
      "front": "What is the total number of documents in the $2018$ Wikipedia index used by RAG?",
      "back": "$21$ million documents."
    },
    {
      "front": "How does RAG-Token's behavior regarding document count differ from RAG-Sequence at test time?",
      "back": "RAG-Token's performance peaks at around $10$ retrieved documents, whereas RAG-Sequence improves with more."
    },
    {
      "front": "Which metric was used to evaluate Jeopardy question generation?",
      "back": "Q-BLEU-1."
    },
    {
      "front": "On the MS-MARCO NLG task, how did RAG-Sequence compare to a BART-only baseline?",
      "back": "It outperformed BART by $2.6$ Bleu points and $2.6$ Rouge-L points."
    },
    {
      "front": "What is the primary advantage of RAG over extractive QA models when answering NQ questions?",
      "back": "It can generate answers even when the answer is not present verbatim in any retrieved document."
    },
    {
      "front": "In FEVER classification, how is the final label determined?",
      "back": "By marginalizing across the top-K retrieved documents to obtain class probabilities."
    },
    {
      "front": "What is the approximate memory requirement for storing the Wikipedia document index vectors on a CPU?",
      "back": "Approximately $100$ GB (uncompressed) or $36$ GB (compressed with FAISS)."
    },
    {
      "front": "In the index hot-swapping experiment, what was the accuracy when querying $2016$ world leaders with a $2018$ index?",
      "back": "$12\\%$ (demonstrating the specificity of the temporal knowledge index)."
    },
    {
      "front": "What is the relationship between the latent variable $z$ and the final prediction $y$ in the RAG probabilistic model?",
      "back": "The model treats $z$ as a latent variable and marginalizes over the seq2seq predictions given different documents."
    },
    {
      "front": "What pre-training objective was used for the BART model in the generator?",
      "back": "A denoising objective with various noising functions."
    },
    {
      "front": "Why is RAG considered more interpretable than purely parametric models?",
      "back": "Accessed knowledge (retrieved documents) can be inspected to understand the basis of a decision."
    },
    {
      "front": "How did the authors handle the small size of the WebQuestions (WQ) and CuratedTrec (CT) datasets during training?",
      "back": "They initialized the models with the Natural Questions (NQ) RAG model."
    },
    {
      "front": "Which component's parameters are denoted by $\\eta$ in the RAG framework?",
      "back": "The retriever parameters."
    },
    {
      "front": "Which component's parameters are denoted by $\\theta$ in the RAG framework?",
      "back": "The generator parameters."
    },
    {
      "front": "True or False: RAG-Sequence and RAG-Token are equivalent for sequence classification tasks.",
      "back": "True (when the target sequence is a single token)."
    },
    {
      "front": "In the FEVER analysis, how often was a 'gold' article title present in RAG's top $10$ retrieved documents?",
      "back": "$90\\%$ of cases."
    },
    {
      "front": "What is the effect of retrieving more documents on the Rouge-L and Bleu-1 scores for RAG-Token in MS-MARCO?",
      "back": "Rouge-L increases while Bleu-1 decreases."
    },
    {
      "front": "What optimizer was used for jointly training the retriever and generator?",
      "back": "Adam."
    },
    {
      "front": "What is the significance of the bi-encoder architecture in DPR for RAG?",
      "back": "It allows the document representations to be pre-computed, enabling fast retrieval via MIPS."
    },
    {
      "front": "How does the specificity of Jeopardy questions make them a good test for RAG?",
      "back": "They require precise factual statements about an entity, which tests the model's ability to access specific knowledge."
    },
    {
      "front": "In the RAG-Token decoding formula, what is used as the transition probability?",
      "back": "The sum of the generator's next-token probabilities weighted by the retriever's document priors."
    },
    {
      "front": "What are the two sub-tasks of FEVER, and which one does RAG directly address?",
      "back": "Label classification and evidence extraction; RAG addresses label classification."
    },
    {
      "front": "How many documents are typically retrieved during training for RAG experiments?",
      "back": "$k \\in \\{5, 10\\}$."
    },
    {
      "front": "What hardware was used to distribute the training of RAG models?",
      "back": "$8 \\times 32$GB NVIDIA V100 GPUs."
    },
    {
      "front": "What is the primary risk associated with advanced language models like RAG, as discussed in the 'Broader Impact' section?",
      "back": "Generation of biased, misleading, or abusive content and the potential automation of jobs."
    }
  ]
}