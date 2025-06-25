
# LLM Portfolio: Summarisation, Brochure Generation & RAG Ingestor 🚀

This is an evolving portfolio of hands-on LLM projects. It documents my learning journey through building real-world applications using tools like **OpenAI API**, **Ollama**, **LangChain**, and **Hugging Face**. The focus is on practical skills development, starting with three key projects:

### ✅ Current Projects:

1. **Text Summarisation & Brochure Generation**  
   Scrapes and summarises company websites, then generates concise brochures tailored for different stakeholders like customers, investors, and job candidates. Uses OpenAI + Ollama with streaming output and structured prompting.  
   📖 [Read the blog](https://medium.com/@vishalkhushlani123/building-with-llms-intelligent-web-scraping-ai-powered-brochures-f93c618c0087)

2. **Markdown Knowledge Ingestor for RAG**  
   Loads structured `.md` documents (e.g., internal wikis), adds metadata, and splits them into overlapping chunks using LangChain. Prepares content for use in vector stores and RAG pipelines. Also includes an optional conversational RAG chain powered by OpenAI or local models via Ollama.  
   📖 [Read the blog](https://medium.com/@vishalkhushlani123/building-a-markdown-knowledge-ingestor-for-rag-with-langchain-ba201515f6c4)

---

## 🛠️ Setup Instructions

### 1. Clone this Repository
```bash
git clone https://github.com/VishalKhushlani/llm-portfolio
cd llm-portfolio
```

### 2. Install Python Dependencies

You can use `conda`:
```bash
conda create -n llm-env python=3.10
conda activate llm-env
pip install -r requirements.txt
```

Or `pip` directly:
```bash
python -m venv llm-env
source llm-env/bin/activate  # On Windows use `llm-env\Scripts\activate`
pip install -r requirements.txt
```

### 3. Set Environment Variable

To use OpenAI's API, set the following environment variable in a `.env` file or directly in your shell:

```
OPENAI_API_KEY=your_openai_api_key_here
```

> You can obtain an API key from https://platform.openai.com/account/api-keys

---

🔗 **Connect on LinkedIn:**  
[linkedin.com/in/vishal-khushlani-982177175](https://www.linkedin.com/in/vishal-khushlani-982177175/)

---

Happy learning! More projects coming soon.
