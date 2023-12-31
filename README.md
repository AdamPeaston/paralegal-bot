# paralegal-bot
The law is too inaccessible to the public, partly because the language is full of terms of art (jargon) and partly because there is just so much reading to do that people with day jobs could not possibly hope to adequately know where they stand in respect of the law. Both of these problems are eminently solvable now. Let's do that.

## The objective
Goal here is to obtain a modest corpus of legal precedent and to turn that corpus into a ChatBot that you can consult with for answers to your legal questions. To achieve this I'm going to use the technique of Retrieval Augmented Generation (RAG), using technologies like LangChain to store my corpus in vector-searchable format and Llama 2 prompted with context + query to produce natural language synthesis of the answer to the user.  

I am looking forward to experimenting with different prompting techniques for different base LLMs, and also seeing how well I can get the system to properly cite the relevant cases.

## Approach
1. Obtain a corpus of legal research data - a simple script for this can be found in the __paralegal_research.py__ file.
2. Pre-process and ingest this corpus into a vector database - hopefully find an open-source document embedding model or free service. Scottish heritage here.
3. Write the supervisor logic to take a user inquiry, retrieve relevant documents from the corpus, assemble these documents into a suitably formatted context and pass context and prompt to an LLM for synthesis of the response to the user. Expectation is that some careful formatting of the context and prompt will be necessary to ensure the LLM includes consistently formatted references and citations.

The supervisor logic can be tailored to the use case. For legal predecent, it will be prudent to explore the dataset to determine typical and extremal lengths of judgement documents. It is expected that these will exceed the maximum context length for the LLM that I can run locally of 2,048 tokens. Therefore it will be necessary to consider how to process whole documents, potentially in chunks. At the same time, intuition tells me that it would be sensible for the model to consider each whole judgement in its entirety rather than only be shown small bits aligning with the user's inquiry.

This raises two questions:
1. Can we develop an embedding scheme (or embedding summary scheme) which can be used to prioritise whole judements for consideration by the model, rather than typically vectorised chunks.
2. Can we get the model to read and summarise judgements in chunks serially so as to produce shortened versions of judgements under consideration that can ultimately fit into the context for the final prompt. Developing this algorithm will be challenging because judgements are all of different lengths, there will be a different number of potentially relevant judgements aligning with each user inquiry, and the user inquiry also can be of different lengths. Sounds like fun!

## Stretch goals
1. Experiment with LoRA fine-tuning of an LLM on the legal precedent corpus.

## The journey
 - Finding open source tools to embed sentences / paragraphs is a whole lot easier than I was expecting. Getting started with the `paraphrase-MiniLM-L6-v2` model for MVP from who else but Hugging Face which is incredibly simple to download and apply.
 - Experimenting with LanceDB as an open-source light-weight serverless vector database that looks super easy to get started with: `https://lancedb.github.io/lancedb/notebooks/youtube_transcript_search/`
 - Splashing out a bit on the LLM, let's see if we can run a whole LLama 2 model. Probably the hardest thing yet has just been working out how to run bash scripts on Windows.
