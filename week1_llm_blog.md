
# 🚀 Week 1 of Building with LLMs: Intelligent Web Scraping + AI-Powered Brochures

Over the past week, I’ve kicked off my LLM portfolio with two tightly connected sub-projects that form the foundation of **Project 1: AI-Powered Brochure Generator**. The goal? To automate summarizing and brochure-creation for any company website using LLMs and basic web scraping.

In this post, I’ll walk through what I built, what I learned, and share code snippets that you can reuse. It includes both **Ollama (free, local LLMs)** and **OpenAI GPT (cloud, paid)** integrations.

---

## 💡 Project Goal

Build a smart system that:
- Scrapes a website intelligently
- Identifies the most relevant pages (e.g., *About*, *Careers*, *Customers*)
- Summarizes the content
- Creates a concise and well-formatted brochure in markdown using an LLM

---

## ⚙️ Tech Stack
- **Python**, **Jupyter**, **BeautifulSoup**
- **OpenAI GPT-4o-mini** for production-quality outputs
- **Ollama (LLaMA3 / DeepSeek 1.5b)** for offline/experimental use
- `.env` config for API keys
- IPython’s `display(Markdown())` to render output

---

## 🔌 Step 1: Local + Cloud LLM Setup

You can choose between:
- **OpenAI (cloud)**: via `openai-python`
- **Ollama (local)**: via OpenAI-compatible endpoint at `http://localhost:11434`

Example setup:
```python
from openai import OpenAI

ollama_via_openai = OpenAI(
    base_url='http://localhost:11434/v1',
    api_key='ollama'  # dummy value just to satisfy client
)

response = ollama_via_openai.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Hello, LLM!"}]
)
```

---

## 🌐 Step 2: Scrape and Clean Website

I created a `Website` class that loads a URL, cleans up the body text, and optionally grabs links:

```python
from bs4 import BeautifulSoup
import requests

headers = {
    "User-Agent": "Mozilla/5.0"
}

class Website:
    def __init__(self, url):
        self.url = url
        response = requests.get(url, headers=headers)
        soup = BeautifulSoup(response.content, 'html.parser')
        self.title = soup.title.string if soup.title else "No title found"
        if soup.body:
            for tag in soup.body(["script", "style", "img", "input"]):
                tag.decompose()
            self.text = soup.body.get_text(separator="\n", strip=True)
        else:
            self.text = ""
        self.links = [a.get("href") for a in soup.find_all("a") if a.get("href")]

    def get_contents(self):
        return f"Webpage Title:\n{self.title}\nWebpage Contents:\n{self.text}\n\n"
```

---

## 🧠 Prompt Design: System + User

Here’s how I structured LLM prompts:

```python
# System prompt tells the model what it's doing
system_prompt = "You are an assistant that summarizes company websites for brochures. Respond in markdown."

# User prompt injects content scraped from the page
def user_prompt_for(website):
    return f"You are looking at a website titled {website.title}\nHere is the content:\n{website.text}"
```

The message format follows OpenAI's standard:
```python
[
    {"role": "system", "content": system_prompt},
    {"role": "user", "content": user_prompt_for(website)}
]
```

---

## 🔗 Step 3: Identify Relevant Pages for the Brochure

This is where things get clever. Instead of hardcoding logic to find "about" or "careers" pages, I used GPT-4o to choose them for me — **using one-shot prompting**:

```python
link_system_prompt = "You are provided with a list of links found on a webpage. Decide which links are relevant for a brochure. Respond in JSON format like this:\n{\n  'links': [\n    {'type': 'about', 'url': 'https://example.com/about'},\n    ...\n  ]\n}"

def get_links_user_prompt(website):
    return f"Here are links from {website.url}. Choose only relevant ones for a brochure:\n" +            "\n".join(website.links)

def get_links(url):
    website = Website(url)
    response = openai.chat.completions.create(
        model=MODEL,
        messages=[
            {"role": "system", "content": link_system_prompt},
            {"role": "user", "content": get_links_user_prompt(website)}
        ],
        response_format={"type": "json_object"}
    )
    return json.loads(response.choices[0].message.content)
```

---

## 🧾 Step 4: Fetch All Page Contents and Build the Brochure

Now that we know which links matter, fetch and concatenate their contents:

```python
def get_all_details(url):
    result = "Landing page:\n"
    result += Website(url).get_contents()
    links = get_links(url)
    for link in links["links"]:
        result += f"\n\n{link['type']}\n"
        result += Website(link["url"]).get_contents()
    return result
```

And prompt the LLM to turn this into a brochure:

```python
def get_brochure_user_prompt(company_name, url):
    return f"You are looking at a company called: {company_name}\n"            f"Here are the contents of its website and subpages:\n" +            get_all_details(url)[:5000]  # truncate to stay under token limit

def create_brochure(company_name, url):
    response = openai.chat.completions.create(
        model=MODEL,
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": get_brochure_user_prompt(company_name, url)}
        ]
    )
    display(Markdown(response.choices[0].message.content))
```

---

## 🖋️ Learnings from Week 1

✅ Ollama is great for fast local prototyping and avoids API costs  
✅ System vs User prompts are powerful for shaping tone and scope  
✅ LLMs are surprisingly good at link filtering and classification  
✅ One-shot prompting is a game-changer  
✅ Markdown makes LLM outputs instantly usable  
✅ Streaming GPT output adds polish with minimal extra code

---

## 🔮 What’s Next?

Over the coming weeks, I’ll be building out the rest of this LLM portfolio:

- 🧑‍✈️ Multi-modal airline support agent (with UI + tools)
- 🧑‍💼 Audio-to-action-item pipeline using Whisper + GPT
- 🔁 Python → Optimized C++ code converter
- 🧠 RAG-powered AI Knowledge Worker
- 📦 Capstone: Product price prediction (Open + Closed models)

Stay tuned and follow along!
