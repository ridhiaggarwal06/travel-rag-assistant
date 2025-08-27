# travel-rag-assistant

This project builds a Retrieval-Augmented Generation (RAG) pipeline to answer travel-related questions with grounded, reliable responses. Instead of relying solely on an LLM (which risks hallucination), the system retrieves context from a curated travel Q&A dataset and generates concise answers supported by real data.


Dataset:
- Source: Travel Q&A pairs (city, question, answer) inspired by Wikivoyage-style guides.
- Size: ~80% of data used for training the retriever, 20% held out for evaluation.
- Business Relevance: Provides accurate, context-based answers that help travellers make better decisions (when to visit, local tips, cultural notes).

Approach:
- Embedding & Indexing
  - Each travel Q&A pair was converted into vector representations using all-MiniLM-L6-v2, a compact embedding model that captures semantic meaning. These embeddings were stored in Chroma, enabling efficient similarity search. This approach ensures that queries like “best months to visit Prague” retrieve relevant answers even if the dataset phrases it differently.

- Reranking
  - To refine the retrieval process, a cross-encoder model (BAAI/bge-reranker-base) was applied to re-score the top candidates. While vector search provides broad recall, reranking improves precision by directly comparing each candidate against the query, ensuring the most relevant context surfaces first.

- Answer Generation
  - The reranked context was passed to a lightweight Llama-2 chat model (quantised via CTransformers). By conditioning the model on retrieved documents, the generated answers remain fluent yet grounded in source material, reducing hallucinations and producing concise, practical responses suitable for a travel assistant.
 


Analysis:
  - The retrieval-augmented pipeline combines vector search, reranking, and a lightweight language model to deliver accurate and grounded answers. Embedding ensures semantically similar questions map closely, while the cross-encoder reranker filters the best matches with higher precision. Passing this curated context to the Llama-2 model produces answers that are fluent but also tied to reliable sources, reducing the risk of misleading or irrelevant output. A small held-out evaluation set confirmed strong overlap with human reference answers, showing that the pipeline balances recall, precision, and response quality effectively.
