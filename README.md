# Simple Amharic RAG System

A small Retrieval-Augmented Generation (RAG) pipeline for **Amharic**, built and run in Google Colab. It retrieves relevant passages from an Amharic text and uses a multilingual LLM to answer questions grounded in that context, then compares the RAG answers against a no-context.

## What it does

1. **Load \& chunk** an Amharic source document (`adwa.txt`, on the Battle of Adwa / Ethiopian history) with `langchain`'s `RecursiveCharacterTextSplitter`, splitting on the Amharic sentence terminator `።` as well as newlines and spaces.
2. **Compare embedding models** for Amharic retrieval quality:

   * [`rasyosef/roberta-amharic-text-embedding-base`](https://huggingface.co/rasyosef/roberta-amharic-text-embedding-base) — native Amharic embedder
   * [`BAAI/bge-m3`](https://huggingface.co/BAAI/bge-m3) — strong general multilingual embedder (used as the default retriever)
   * [`intfloat/multilingual-e5-large`](https://huggingface.co/intfloat/multilingual-e5-large) — multilingual embedder
3. **Build FAISS vector stores** for each embedding model and run similarity search over the chunked document.
4. **Generate answers** with [`CohereLabs/tiny-aya-earth`](https://huggingface.co/CohereLabs/tiny-aya-earth), a 3.35B-parameter model from the Tiny Aya family tuned for West Asian and African languages, including Amharic.
5. **Evaluate** RAG vs. no-RAG answers on a small hand-built eval set using RAGAS metrics.

## Notebook

* [`Simple\_amharic\_rag\_system.ipynb`](./Simple_amharic_rag_system.ipynb)

## Running it

This notebook is designed to run on **Google Colab** with a GPU runtime (used for the embedding models and the `tiny-aya-earth` generator).

1. Open the notebook in Colab.
2. Upload your own Amharic `.txt` source document (the notebook expects `/content/adwa.txt` by default — swap in your own file or change the path). Note: `.gitignore` excludes `.txt` files by default, so if you want to commit your source document, add it with `git add -f adwa.txt`.
3. Run the cells top to bottom. The first two cells install the required packages.

To run locally instead:

```bash
pip install -r requirements.txt
```

Then launch Jupyter and open the notebook. A GPU is strongly recommended for the embedding and generation steps.



## Author

[Bedru Yimam Ahmed](https://github.com/bedr-ux)

