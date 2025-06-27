Creates a vector database based on either:
- Azure AI Search Index
- [Facebook AI Similarity Search](https://github.com/facebookresearch/faiss) (FAISS) 

Only some file formats are supported: `md`, `txt`, `html`, `htm`, `py`, `doc`, `docx`, `ppt`, `pptx`, `pdf`, `xls`, `xlsx`

1. Create a search index
2. Index the documents
3. in PromptFlow:
	1. Create an Embedding from the user input (convert to vector)
	2. Execute a lookup to get the search result
	3. Transform/format the search result
	4. Feed the output into a [[PromptFlow LLM Nodes|LLM Node]] for question answering
