<!-- Header -->
<div align="center">
  <img src="header_hero.svg" width="100%"/>
  <h1>Sanjay Kumar</h1>
  <p><b>Full Stack Developer (MERN + Java) | Building AI-Powered Dev Tools</b></p>
</div>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=22&pause=1000&color=00FFCC&center=true&vCenter=true&width=750&height=55&lines=Full+Stack+Developer+%7C+MERN+%26+Java;Building+DriftGuard%3A+AI+Code+Governance+Platform;React+%7C+Node.js+%7C+Spring+Boot+%7C+MongoDB;AST+Parsing+%2B+LLM-Assisted+Code+Review;BCA+Final+Year+%E2%80%94+Graduating+2026;Open+to+Remote+Startup+Roles+%F0%9F%9A%80" />
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/sanjayjaidev"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0A0F2E"/></a>
  <a href="mailto:sanjayk.dev.ai@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0A0F2E"/></a>
  <a href="https://x.com/SanjayJava6006"><img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=0A0F2E"/></a>
  <a href="https://github.com/fullspectrum-dev"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white&labelColor=0A0F2E"/></a>
  <img src="https://komarev.com/ghpvc/?username=fullspectrum-dev&style=for-the-badge&color=00FFCC&labelColor=0A0F2E&label=PROFILE+VIEWS"/>
</p>

<div align="center">

|  |  |  |  |
|:---:|:---:|:---:|:---:|
| 🛡️ **Building DriftGuard — AI Code Governance** | 🏢 **Ex-SAG Infotech** | 🧠 **225+ DSA Problems Solved** | 🎓 **BCA — Grad 2026** |

</div>

---

## 👋 About Me

> *"I don't just write code — I build systems that scale."*

I'm a **Full Stack Developer** from Jaipur, India. I started as a **Java backend developer** with real production experience at **SAG Infotech** — shipping REST APIs and working in Agile teams. I've since gone full-stack with **React, Node.js, Express, and MongoDB**, and I'm currently deep in **GenAI-backed backend systems** — building **DriftGuard**, an AI code-governance platform that parses code with ASTs and uses an LLM to catch architectural drift on pull requests.

I believe in **learning by building** — every concept I study turns into a project, a PR, or a bug fixed in production.

```
🔭  Building        → DriftGuard (AI code governance / drift detection SaaS)
🧩  Shipping         → HireFlow (role-based recruitment platform, MERN)
🌱  Learning         → LLM orchestration, queue-based system design
🎯  Goal             → Remote Full Stack / Backend role at a high-growth startup
💬  Ask me about     → React, Node.js, Java, Spring Boot, System Design
⚡  Fun fact         → I debug faster with chai ☕ than coffee
```

---

## 🛡️ Flagship Project — DriftGuard

<p align="left">
  <img src="https://img.shields.io/badge/STATUS-ACTIVE%20DEVELOPMENT-00FFCC?style=for-the-badge&labelColor=0A0F2E"/>
  <img src="https://img.shields.io/badge/CATEGORY-DEV%20TOOLS%20%2F%20B2B%20SAAS-00FFCC?style=for-the-badge&labelColor=0A0F2E"/>
</p>

**DriftGuard is an AI-powered code governance platform that catches architectural drift on GitHub pull requests before they merge.**

Teams write their engineering conventions in plain markdown ("handlers must not call the database directly," "always wrap errors," etc.). DriftGuard parses that markdown into a structured rule using an LLM, then checks every pull request's diff against it — using AST-based code parsing, not regex — and reports back the exact file, line range, a plain-language explanation, and a suggested fix.

<img src="https://skillicons.dev/icons?i=react,vite,ts,nodejs,express,mongodb,redis&theme=dark" />

**Core Capabilities**
- 🔐 GitHub OAuth login + GitHub App installation flow to connect repositories
- 📝 Markdown-to-rule engine — conventions are parsed into category, severity, confidence score, and good/bad examples
- 🌳 AST-based code analysis — function-level extraction and import/export extraction, not text matching
- 🗂️ File ingestion pipeline — fetches the repo tree, filters relevant files, and classifies each file's architectural layer
- ⚙️ Webhook-driven async pipeline — GitHub webhook events are signature-verified, acknowledged instantly, and processed in the background via **Redis + BullMQ**
- 🤖 LLM-assisted violation detection — violations returned with an explanation and a suggested fix, not just a flag
- 📊 Drift analytics — per-repo violation trends over time, plus AI token usage and cache-hit tracking per organization
- 💳 Razorpay-based billing — trial gating, plan upgrades, and signature-verified payment webhooks
- 👥 Org-based multi-tenancy — admin-controlled invites and role assignment

**[View Repository →](https://github.com/fullspectrum-dev/driftguard)**

<br/>

<details open>
<summary><b>🏗️ High-Level Design (HLD)</b></summary>
<br/>

```mermaid
graph TD
    U[React Dashboard<br/>Vite + TS + Tailwind + TanStack Query] -->|REST + JWT| API[Express API<br/>Node.js + TypeScript]

    API --> AUTH[Auth Service<br/>GitHub OAuth + JWT access/refresh]
    API --> REPO[Repo Service<br/>GitHub App install + connect]
    API --> CONV[Convention Service<br/>markdown → structured rule]
    API --> BILL[Billing Service<br/>Razorpay orders + verification]
    API --> USAGE[Usage Service<br/>token / cost tracking]
    API --> NOTIF[Notification Service]
    API --> INVITE[Invite Service<br/>org onboarding + roles]

    GH[GitHub App] -->|Signed Webhook Event| WH[Webhook Controller<br/>x-hub-signature-256 verified]
    WH -->|enqueue job| QUEUE[(Redis + BullMQ Queue)]
    QUEUE --> WORKER[Async Worker]

    WORKER --> INGEST[Ingestion Engine<br/>fetch repo tree → filter files → classify layer]
    INGEST --> AST[AST Engine<br/>function + import/export extraction]
    AST --> DRIFT[Drift Analysis Engine<br/>LLM-assisted rule matching]
    CONV -.Ready conventions.-> DRIFT
    DRIFT --> RESULT[(AnalysisResult<br/>violations + explanation + fix)]
    DRIFT --> NOTIF

    AUTH --> DB[(MongoDB<br/>User · Repo · Convention · AnalysisResult · TokenUsage · Invite)]
    REPO --> DB
    CONV --> DB
    RESULT --> DB
    BILL --> DB
    USAGE --> DB

    RZ[Razorpay] -->|Signed Webhook| BILL
```

</details>

<details>
<summary><b>🔩 Low-Level Design (LLD) — PR Analysis Pipeline</b></summary>
<br/>

```mermaid
sequenceDiagram
    participant GH as GitHub
    participant WH as Webhook Controller
    participant Q as BullMQ Queue (Redis)
    participant W as Worker
    participant ING as Ingestion Service
    participant AST as AST Service
    participant CLS as Classification Service
    participant DR as Drift Analysis (LLM)
    participant DB as MongoDB

    GH->>WH: POST /webhook (pull_request event)
    WH->>WH: verify x-hub-signature-256
    WH-->>GH: 200 OK (immediate ack)
    WH->>Q: enqueue analysis job

    Q->>W: pick up job
    W->>ING: fetchRepoTreeService(installationId, repo, branch)
    ING->>ING: filterRelevantFilesService(tree)
    ING->>CLS: classifyFileLayerService(filePath)
    CLS-->>ING: layer (handler / service / repository / ...)

    loop for each changed file
        ING->>AST: extractFunctionService(code)
        ING->>AST: extractImportExportService(code)
        AST-->>ING: function chunks + import/export graph
    end

    ING->>DB: load Ready conventions for repo
    DB-->>ING: structured rules (category, severity, pattern)

    ING->>DR: compare AST chunks against Ready conventions
    DR-->>ING: violations [{file, lineRange, severity, explanation, suggestedFix, conventionRef}]

    ING->>DB: save AnalysisResult (repoId, prNumber, headSha, violations)
    ING->>GH: post PR comment / commit status check
    ING->>DB: create notification (unread)
```

</details>

<details>
<summary><b>🔩 Low-Level Design (LLD) — Convention Parsing Pipeline</b></summary>
<br/>

```mermaid
sequenceDiagram
    participant U as User (Dashboard)
    participant API as Convention Controller
    participant ORG as Org Access Guard
    participant SVC as Convention Service (LLM)
    participant DB as MongoDB

    U->>API: POST /conventions { repoId, rawMarkdown }
    API->>ORG: isOrgActive(orgId)
    ORG-->>API: active / trial expired (403)
    API->>DB: save Convention (status: Processing)
    API->>SVC: parse rawMarkdown

    SVC->>SVC: extract title, category, severity
    SVC->>SVC: extract good/bad examples
    SVC->>SVC: compute confidence score

    alt confidence high
        SVC->>DB: update status → Ready
    else confidence low
        SVC->>DB: update status → Needs Review
    else parse failed
        SVC->>DB: update status → Failed
    end

    DB-->>U: GET /conventions reflects updated status
```

</details>

---

## 🔥 Other Projects

### 💼 HireFlow — Full-Stack Recruitment Platform

<table>
<tr>
<td width="55%" valign="top">

**A role-aware hiring platform connecting candidates and recruiters through one clean workflow.**

<img src="https://skillicons.dev/icons?i=react,vite,nodejs,express,mongodb,sass&theme=dark" />

**Key Features**
- 🔐 JWT auth with refresh token rotation + role-based access control
- 🔍 Job listing with pagination, filters, debounced search
- 📝 Apply flow with duplicate-apply conflict handling
- 📊 Candidate & Recruiter dashboards with pipeline funnel charts
- 🌗 Centralized dark/light theme system

**[View Repository →](https://github.com/fullspectrum-dev/hireflow)**

</td>
<td width="45%" valign="top">

```mermaid
graph TD
    A[React + Vite Client] -->|Axios + JWT| B[Express 5 API]
    B -->|Access/Refresh Tokens| A
    B --> C{Role Check}
    C -->|Candidate| D[Jobs · Applications · Saved]
    C -->|Recruiter| E[Post Jobs · Review Pipeline]
    D --> F[(MongoDB)]
    E --> F
```

</td>
</tr>
</table>

---

### 🗂️ TeamBoard — Project Management App

<table>
<tr>
<td width="55%" valign="top">

**A full-stack task & project tracker built for small teams to plan, assign, and ship work.**

<img src="https://skillicons.dev/icons?i=react,vite,nodejs,express,mongodb,sass&theme=dark" />

**Key Features**
- 📋 Project & task CRUD with soft-delete and restore
- 👤 Shared layout with avatar dropdown & theme toggle
- 📈 CSS-only donut chart dashboard for progress tracking
- 🔐 JWT-protected routes with `ProtectedRoute` guard
- 🌗 Centralized dark/light theme system

**[View Repository →](https://github.com/fullspectrum-dev/teamboard)**

</td>
<td width="45%" valign="top">

```mermaid
graph TD
    A[React + Vite Client] -->|Axios + JWT| B[Express API]
    B --> C[Controller Layer]
    C --> D[Service Layer]
    D --> E[(MongoDB + Mongoose)]
    D --> F[Soft-Delete / Restore Logic]
    B --> G[Auth Middleware]
```

</td>
</tr>
</table>

---

### 🔐 Full-Stack Authentication System

<table>
<tr>
<td width="55%" valign="top">

**A production-grade auth system built with a security-first mindset.**

<img src="https://skillicons.dev/icons?i=react,java,spring,mysql&theme=dark" />

**Key Features**
- 🔑 JWT auth with access & refresh token expiry handling
- 🌐 OAuth2 login — Google / GitHub
- 🛡️ Spring Security role-based access control (ADMIN / USER)
- ⚠️ Global exception handling with custom error responses
- 🗄️ Optimized MySQL queries with proper indexing
- 🧱 Clean Controller → Service → Repository architecture

**[View Repository →](https://github.com/fullspectrum-dev/full-stack-Authentication-App-)**

</td>
<td width="45%" valign="top">

```mermaid
graph TD
    A[React Client] -->|REST + JWT| B[Spring Boot API]
    B --> C[Spring Security Filter]
    C --> D{Role Check}
    D -->|Admin| E[Admin Endpoints]
    D -->|User| F[User Endpoints]
    B --> G[(MySQL)]
    B --> H[OAuth2 Provider]
```

</td>
</tr>
</table>

---

> 📌 *DriftGuard is under active development — billing, drift analytics and the AST engine are shipped; more is on the way.*

---

## 🛠️ Tech Stack

<div align="center">

**Frontend**
<br/>
<img src="https://skillicons.dev/icons?i=react,vite,redux,sass,angular,html,css,js&theme=dark" />

<br/><br/>

**Backend**
<br/>
<img src="https://skillicons.dev/icons?i=nodejs,express,java,spring,graphql&theme=dark" />

<br/><br/>

**Database & Cache**
<br/>
<img src="https://skillicons.dev/icons?i=mongodb,mysql,redis&theme=dark" />

<br/><br/>

**DevOps & Tools**
<br/>
<img src="https://skillicons.dev/icons?i=docker,kafka,git,github,postman,vscode&theme=dark" />

<br/><br/>

**GenAI & LLM Engineering**
<br/>
<img src="https://skillicons.dev/icons?i=openai,py,pytorch,tensorflow&theme=dark" />
<br/><br/>
<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=00FFCC&labelColor=0A0F2E"/>
<img src="https://img.shields.io/badge/LlamaIndex-000000?style=for-the-badge&logoColor=00FFCC&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Hugging_Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=0A0F2E&labelColor=0A0F2E"/>
<img src="https://img.shields.io/badge/OpenAI_API-412991?style=for-the-badge&logo=openai&logoColor=white&labelColor=0A0F2E"/>
<br/><br/>
<img src="https://img.shields.io/badge/RAG-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Embeddings-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Prompt_Engineering-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Fine--Tuning-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Semantic_Search-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/AI_Agents-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<br/><br/>

**Vector Databases**
<br/>
<img src="https://img.shields.io/badge/Pinecone-000000?style=for-the-badge&logoColor=00FFCC&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/ChromaDB-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/FAISS-00FFCC?style=for-the-badge&logo=meta&logoColor=00FFCC&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Weaviate-00FFCC?style=for-the-badge&labelColor=0A0F2E&color=1a1a2e"/>
<img src="https://img.shields.io/badge/Qdrant-DC244C?style=for-the-badge&labelColor=0A0F2E"/>

</div>

---

## 📊 GitHub Stats

<div align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=fullspectrum-dev&show_icons=true&theme=tokyonight&hide_border=true&count_private=true&bg_color=0A0F2E&title_color=00FFCC&icon_color=00FFCC&text_color=FFFFFF" width="420"/>
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=fullspectrum-dev&layout=compact&theme=tokyonight&hide_border=true&bg_color=0A0F2E&title_color=00FFCC&text_color=FFFFFF" width="340"/>
</div>

<div align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=fullspectrum-dev&theme=tokyonight&hide_border=true&background=0A0F2E&stroke=00FFCC&ring=00FFCC&fire=00FFCC&currStreakLabel=00FFCC" width="500"/>
</div>

---

## 📈 Contribution Graph

<div align="center">
  <img src="https://github-readme-activity-graph.vercel.app/graph?username=fullspectrum-dev&theme=react-dark&bg_color=0A0F2E&color=00FFCC&line=00FFCC&point=FFFFFF&area=true&hide_border=true" width="95%"/>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/fullspectrum-dev/fullspectrum-dev/output/github-contribution-grid-snake-dark.svg" width="95%" alt="contribution snake"/>
</div>

---

## 🏆 GitHub Trophies

<div align="center">
  <img src="https://github-profile-trophy.vercel.app/?username=fullspectrum-dev&theme=algolia&no-frame=true&margin-w=6&column=6" width="95%"/>
</div>

---

## ⚡ LeetCode & GeeksForGeeks

<div align="center">
  <table border="0" cellspacing="0" cellpadding="0">
    <tr>
      <td align="center" style="padding: 15px;">
        <img src="https://img.shields.io/badge/LeetCode-122%2B%20Solved-00FFCC?style=for-the-badge&logo=leetcode&logoColor=white&labelColor=0A0F2E"/>
        <br/><br/>
        <img src="https://img.shields.io/badge/Easy-Solved-green?style=for-the-badge&labelColor=0A0F2E"/>
        <img src="https://img.shields.io/badge/Medium-Solving-yellow?style=for-the-badge&labelColor=0A0F2E"/>
        <img src="https://img.shields.io/badge/Hard-Grinding-red?style=for-the-badge&labelColor=0A0F2E"/>
      </td>
      <td align="center" style="padding: 15px;">
        <img src="https://img.shields.io/badge/GeeksforGeeks-103%20Solved-00FFCC?style=for-the-badge&logo=geeksforgeeks&logoColor=white&labelColor=0A0F2E"/>
        <br/><br/>
        <img src="https://img.shields.io/badge/Coding%20Score-275-brightgreen?style=for-the-badge&labelColor=0A0F2E"/>
        <img src="https://img.shields.io/badge/Institute%20Rank-Top%2030%25-orange?style=for-the-badge&labelColor=0A0F2E"/>
      </td>
    </tr>
  </table>
</div>

---

## 🌱 Currently Learning

<div align="center">

| Track | Status |
|---|---|
| 🤖 RAG (Retrieval-Augmented Generation) | 🚧 In Progress |
| 🧬 Embeddings & Semantic Search | 🚧 In Progress |
| 🗄️ Vector Databases (Pinecone, ChromaDB, FAISS) | 🚧 In Progress |
| 🔗 LangChain / LlamaIndex Frameworks | 🌱 Learning |
| ✍️ Prompt Engineering & Fine-Tuning | 🌱 Learning |
| 🧠 DSA — Two Pointers, Fast & Slow | ✅ Completed |
| 🧠 DSA — Sliding Window | 🚧 In Progress |
| 🧠 DSA — Intervals, Heaps, Top-K, K-way Merge | ⏭️ Upcoming |
| 🏗️ System Design — HLD/LLD, CAP theorem | 🌱 Learning |
| 📦 Kubernetes | 🌱 Learning |
| 📐 Design Patterns (GoF) | 🌱 Learning |

</div>

---

## 🤝 Connect With Me

<p align="center">
  <a href="https://www.linkedin.com/in/sanjayjaidev">
    <img src="https://img.shields.io/badge/LinkedIn-Sanjay%20Kumar-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0A0F2E"/>
  </a>
  <a href="mailto:sanjayk.dev.ai@gmail.com">
    <img src="https://img.shields.io/badge/Gmail-sanjayk.dev.ai@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0A0F2E"/>
  </a>
  <a href="https://x.com/SanjayJava6006">
  <img src="https://img.shields.io/badge/Twitter-@SanjayJava6006-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=0A0F2E"/>
</a>
  <a href="https://github.com/fullspectrum-dev">
    <img src="https://img.shields.io/badge/GitHub-fullspectrum--dev-181717?style=for-the-badge&logo=github&logoColor=white&labelColor=0A0F2E"/>
  </a>
</p>

<div align="center">
  <i>Open to remote Full Stack / Backend roles at startups — let's build something great together.</i>
</div>

---

<!-- Footer -->
<div align="center">
  <img src="footer_hero.svg" width="100%"/>
</div>
