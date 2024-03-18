# PDF Chatbot with GPT-4 and LangChain

Build a ChatGPT-style conversational interface over your documents using the GPT-4 API. Supports multiple large PDFs as well as DOCX, PPTX, HTML, TXT, and CSV.

**Stack:** LangChain, Chroma (vector store), TypeScript, OpenAI, Next.js. LangChain simplifies building scalable AI/LLM apps; Chroma stores embeddings and document text for similarity retrieval.

A [tutorial video](https://www.youtube.com/watch?v=ih9PBGVVOO4) is available using Pinecone instead of Chroma. A visual guide lives in the `visual guide` folder.

**Prerequisites:** Node.js 18 or higher. If you hit errors, see the Troubleshooting section below.

---

## Features

- **Multiple formats:** DOCX, PPTX, HTML, TXT, CSV.
- **Bulk ingestion:** Place files in the `docs` folder and run `npm run ingest`.
- **Model choice:** GPT-3.5 or GPT-4 (GPT-4 recommended for quality; slower).
- **Local vector DB:** Chroma runs on your host; no cloud vector database required.

---

## Deployment

### 1. Get the code

Clone or download the repository:

```bash
git clone [repository URL]
```

### 2. Install dependencies

Install Yarn globally if needed: `npm install yarn -g`. Then:

```bash
yarn install
```

A `node_modules` folder should appear after installation.

### 3. Environment and Chroma

- Copy `.env.example` to `.env`. It should include:

  ```
  OPENAI_API_KEY=
  COLLECTION_NAME=
  ```

  `COLLECTION_NAME` must be a UUID (e.g. generate with `uuid` on Linux/macOS).

- Get an API key from [OpenAI](https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key) and add it to `.env`.
- Run Chroma locally (e.g. via [Chroma docs](https://docs.trychroma.com/getting-started#2-get-the-chroma-client)):

  ```bash
  git clone git@github.com:chroma-core/chroma.git
  cd chroma
  docker-compose up -d --build
  ```

### 4. Collection namespace

In `.env`, set `COLLECTION_NAME` to the namespace where embeddings will be stored when you run `npm run ingest`. The same namespace is used at query time.

### 5. Customization

- In `utils/makechain.ts`, adjust `QA_PROMPT` for your use case and set `modelName` in `new OpenAI` to `gpt-4` if you have GPT-4 access. Confirm GPT-4 access outside this repo or the app may fail.

---

## Ingest documents into embeddings

The app can load multiple PDFs and other supported files from `docs` (including subfolders).

1. Add your PDFs (or other supported files) to the `docs` folder. An example file is included.
2. Run: `npm run ingest`.
3. Check the Chroma Docker logs to confirm successful POST requests.

---

## Run the app

After embeddings are in Chroma, start the dev server:

```bash
npm run dev
```

Open **http://localhost:3000** and ask questions in the chat interface.

---

## Product snapshot

![Chat with your docs (EN)](images/chat_with_docs_en.jpg)

![Chat with your docs (CN)](images/chat_with_docs_cn.jpg)

---

## Troubleshooting

Check the repository **Issues** and **Discussions** for community solutions.

**General:**

- Ensure a recent Node version: `node -v`
- Try another PDF or convert to text first; some PDFs are scanned or need OCR.
- Log `env` variables to confirm they are set.
- Use the same LangChain and Chroma versions as in this project.
- Ensure `.env` has valid API keys and index/collection name.
- If you switch `modelName` in `OpenAI`, verify API access for that model.
- Confirm OpenAI billing and sufficient credits.
- Avoid multiple `OPENAPI` keys in the global environment; the project `.env` can be overridden.
- As a last resort, hardcode API keys in `process.env` for debugging.

## Credits

Frontend inspired by [gpt4-pdf-chatbot-langchain](https://github.com/mayooear/gpt4-pdf-chatbot-langchain) and [gpt4-pdf-chatbot-langchain-chroma](https://github.com/sagarsaija/gpt4-pdf-chatbot-langchain-chroma).
