- Read large corpus of text
- Vocabulary = sorted list of unique characters/words in the input text
- Use encoder/decoder to tokenize the input text 
	- Simple: char2int
	- [google/sentencepiece](https://github.com/google/sentencepiece)
	- [openai/tiktoken](https://github.com/openai/tiktoken)
- Build a tensor representing the vector-encoded text
	- Dim 0 represents the set of tokens
	- Dim 1 represents the dimensions of the vector (or scalar) representing the individual token
- Split input data into training and test datasets
- Create spans with a given window size of the training data
	- last token should be predicted
- 

## See also

- [Let's build GPT: from scratch, in code, spelled out](https://www.youtube.com/watch?v=kCc8FmEb1nY)
- [karpathy/nanoGPT](https://github.com/karpathy/nanoGPT)