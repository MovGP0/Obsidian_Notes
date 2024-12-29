A **GraphRAG** is a RAG that uses a graph database as information storage.

## Knowledge Graph embedding

- embedding of entities and relationships in a continous vector space
- allows measuring of similarity for efficient retrieval and reasoning
- Methods include TransE, TransR, RotatE

See also: [Simple Schemes for Knowledge Graph Embedding](https://medium.com/stanford-cs224w/simple-schemes-for-knowledge-graph-embedding-dd07c61f3267)

## Graph Neural Networks (GNN)

Used to encode the information from the knowledge graph in a latent representation.

## Retrieval Methods

- [KAPING](https://arxiv.org/abs/2306.04136)
	- Retrieves relevant facts from a knowledge graph based on semantic similarities to the input question. These facts are then verbalized and prepended to the question as a prompt, which is fed into an LLM to generate the answer.
- [KG-GPT](https://arxiv.org/abs/2310.11220) operates in three steps:
		1. **Sentence Segmentation:** Breaks down the input into sub-sentences, each corresponding to a potential KG triple.
		2. **Graph Retrieval:** Identifies and retrieves relevant subgraphs from the KG that align with these sub-sentences.
		3. **Inference:** Uses the retrieved subgraphs to perform reasoning and generate answers or verify facts.
- [KELP](https://arxiv.org/abs/2406.13862)
	- enhances LLM outputs by selecting relevant knowledge paths from a KG. It employs a trained model to score and select paths that have latent semantic alignment with the input text, allowing the incorporation of both direct and indirect knowledge.
- [G-Retriever](https://arxiv.org/abs/2402.07630)
	- integrates Graph Neural Networks (GNNs) with LLMs in a retrieval-augmented generation framework. It retrieves relevant subgraphs from a textual graph by formulating the task as a Prize-Collecting Steiner Tree optimization problem, which are then encoded and used to generate answers.
- [SimGraphRAG](https://github.com/YZ-Cai/SimGRAG)
	- SimGRAG introduces a method to align query text with KG structures by generating a pattern graph using an LLM. It then retrieves subgraphs from the KG that semantically align with this pattern graph, quantified by a novel metric called Graph Semantic Distance (GSD).

See also: [DiscoverAI: Knowledge Graph based RAG: SimGRAG](https://www.youtube.com/watch?v=aPsfAkrkma0)

## GraphRAG Variants

- **Graph-to-Text** generate text based on the extracted information of the graph
- **Text-to-Graph** extract entities from text and construct a corresponding graph
- **Graph-to-Graph** transforming one graph into another
