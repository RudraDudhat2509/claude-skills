---
name: cold-email-outreach
description: Generate hyper-specific cold emails for internship/job outreach. Use this skill whenever the user wants to write a cold email, outreach email, or internship application email to a company. Trigger on phrases like "write a cold email to X", "outreach for X", "email for internship at X", "draft an email to X company", or any variation of reaching out to a company for an internship or role. Always use this skill for outreach — never write generic emails from memory.
---

# Cold Email Outreach Skill

Generates hyper-specific, angle-driven cold emails for internship outreach. NOT generic. Every email must have a concrete hook tied to something real the company is doing.

## About the Sender (Rudra Dudhat)

- **Name**: Rudra Dudhat
- **College**: IIT Bhilai, B.Tech Data Science & AI (2nd year, graduating 2028)
- **CGPA**: 9.48
- **Target role**: Applied Multimodal AI Engineering — vision-language models (VLMs), multimodal systems
- **Portfolio**: rudradudhat2509.github.io
- **GitHub**: github.com/RudraDudhat2509
- **X/Twitter**: @rudrabuilds

### Key Projects (pick the most relevant 1-2 per email)

1. **VLM Research Paper Tutor** — ingests papers including figures and citations, teaches content actively through coding assignments, maintains a skill tree. Stack: Docling, Qdrant, LangGraph, RAGAS. Strong research angle.
2. **Cascade AI** — multi-agent LLM backend on Firebase with self-healing routing and per-user memory via Firestore.
3. **OptiQuant** — ensemble ML system with SHAP explainability, deployed on AWS EC2 with Docker and GitHub Actions CI/CD.
4. **diffprompt** — CLI tool for behavioral prompt diffing (github.com/RudraDudhat2509/diffprompt). In active development.
5. **Multi-agent GAIA benchmark system** — built using smolagents targeting HuggingFace Agents Course leaderboard.
6. **Personal Outreach Engine** — automated CCP outreach pipeline using Groq LLM, Gmail SMTP/IMAP, Google Sheets, CLI-based human-in-the-loop approval.

### Skills
- VLMs: CLIP, LLaVA/Idefics, Florence-2
- Fine-tuning: LoRA/QLoRA, PEFT
- Inference: vLLM, ONNX
- Agents/infra: LangGraph, FAISS/Qdrant, smolagents
- Eval: RAGAS, LangSmith
- Stack: Python, PyTorch, HuggingFace, FastAPI, Docker, AWS

---

## Workflow

### Step 1: Research the Company

Use web search to find:
- What the company actually builds (not just their homepage blurb)
- Their **specific AI/ML work** — models, papers, products, recent launches
- Any **pain points or open problems** visible in their blog, GitHub, job postings, or press
- Key people (founders, AI leads, research heads) — find a real name to address

Search queries to use:
- `"[company name]" AI research blog`
- `"[company name]" machine learning paper`
- `"[company name]" internship OR "applied AI" site:linkedin.com`
- `"[company name]" GitHub`

### Step 2: Find the Angle

The angle is the ONE specific thing that connects Rudra's work to the company's real problems. It must be:
- Tied to something the company actually does (cite it specifically — a product feature, a paper, a blog post)
- Connected to a specific project or skill of Rudra's
- Something that shows he's done his homework, not just read the About page

Bad angle: "I am passionate about AI and your company does great work."
Good angle: "Your qXR-Detect product surfaces lesion-level confidence scores — I've been exploring attention map visualization in VLMs and built a research paper tutor that reasons over figure-level embeddings, which maps directly to the explainability pipeline you'd need at scale."

### Step 3: Draft the Email

**Structure:**
1. **Hook (1-2 lines)** — The angle. Specific thing they do + why it caught your attention. Name the product/paper/feature.
2. **What you've built (2-3 lines)** — 1-2 most relevant projects. Be concrete: what it does, what stack, what result.
3. **The ask (1-2 lines)** — Clear, low-friction. Internship or a short conversation. Not begging, not entitled.
4. **Links** — Portfolio + GitHub. One line, clean.

**Rules:**
- Address a real person by name if found. If not, use a role ("Hi [AI team]" or "Hi [Hiring team]").
- No em dashes. Use semicolons or periods instead.
- No filler phrases: "I am passionate about", "I would love the opportunity", "As a quick introduction"
- Subject line must be specific — reference the company's actual work, not just "Internship Inquiry"
- Length: whatever the angle demands. Don't pad. Don't cut substance.

**Subject line formula:**
`[Their specific product/problem] + [Your relevant angle]`
Example: `VLM explainability for qXR-Detect | IIT Bhilai undergrad`

### Step 4: Output Format

Present the email like this:

```
SUBJECT: [subject line]

[email body]

---
Links:
Portfolio: rudradudhat2509.github.io
GitHub: github.com/RudraDudhat2509
```

Then add a short note (2-3 lines) explaining:
- What angle was used and why
- Which projects were picked and why those specifically
- Any uncertainty (e.g., "couldn't find a specific person to address, used team instead")

---

## Edge Cases

- **No AI/ML work found at the company**: Flag it. Ask the user if they still want to proceed or want a different company.
- **Very small/stealth company**: Use GitHub or LinkedIn to find their stack. If still nothing, write around their problem domain (e.g., "companies building X typically face Y problem").
- **User provides a JD**: Extract required skills from it and prioritize matching projects accordingly.
- **User already has context**: If they paste a company description or specific detail, use that as the primary source; supplement with search.
