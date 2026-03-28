<div align="center">
  <img src="logo.png" width="120" alt="Rahul Pamula Logo">
  <h1>Rahul Pamula</h1>
  <p><b>Software Developer | Cloud Architecture | AI Integration</b></p>
  <img src="profile.jpg" width="160" style="border-radius: 50%; border: 4px solid #4facfe; box-shadow: 0 4px 14px rgba(0, 242, 254, 0.4);" alt="Rahul Pamula">
  <br><br>
  
  [![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rahul-pamula/)
  [![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=leetcode&logoColor=white)](https://leetcode.com/u/rahulpamula)
  [![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/rahul.pamula_/)
  [![Microsoft AZ-900](https://img.shields.io/badge/Azure_AZ--900-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://www.credly.com/badges/cb01588f-556d-448d-84f3-aebeef100485/public_url)
</div>

---

## 👨‍💻 About Me

Welcome to my portfolio space! I am a Software Developer who thrives on complex logic and architecture. When a problem is difficult, my interest peaks. I specialize in building complete full-stack ecosystems—from highly polished frontends to deeply integrated API backends, scalable message queues, and cutting-edge **RAG (Retrieval-Augmented Generation) Systems**.

**Hobbies:** Playing chess ♟️ & Mobile gaming 🎮

---

## 🛠️ My Tech Stack

* **Languages & Frameworks:** Python, Node.js, React Native, React, Next.js, FastAPI, TypeScript
* **Infrastructure & AI:** AWS SES, RabbitMQ, Supabase, PostgreSQL, Azure AI Services, Google Gemini, **RAG Systems**
* **Design & UI:** Tailwind CSS, shadcn-ui

---

## 🚀 Major Projects & Architectures

### 1. 📧 [Sh_R_Mail](https://github.com/Rahul-pamula/Sh_R_Mail)
A self-hosted, multi-tenant email marketing and campaign management platform. Built to route massive scales of emails reliably using background workers and AWS SES.

<details open>
<summary><b>View Architecture Flow</b></summary>

```mermaid
graph TD
    classDef userNode fill:#3b82f6,stroke:#1d4ed8,stroke-width:2px,color:#fff,font-weight:bold,rx:10px,ry:10px;
    classDef coreApp fill:#10b981,stroke:#047857,stroke-width:2px,color:#fff,font-weight:bold,rx:5px,ry:5px;
    classDef sysWorker fill:#8b5cf6,stroke:#6d28d9,stroke-width:2px,color:#fff,font-weight:bold,rx:5px,ry:5px;
    classDef tenWorker fill:#f59e0b,stroke:#b45309,stroke-width:2px,color:#fff,font-weight:bold,rx:5px,ry:5px;
    classDef provider fill:#ef4444,stroke:#b91c1c,stroke-width:2px,color:#fff,font-weight:bold,rx:10px,ry:10px;
    classDef db fill:#64748b,stroke:#475569,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    
    User([Platform User / Tenant]) --> |Interacts with| App[Sh_R_Mail Platform]
    class User userNode;
    class App coreApp;
    
    subgraph AppLogic [App Logic]
        Auth[Auth & Core Logistics]
        Campaigns[Campaign Engine]
        App --> Auth
        App --> Campaigns
        class Auth coreApp;
        class Campaigns coreApp;
    end

    subgraph DualProcessingQueues [Dual Processing Queues]
        SysQueue[(System Queue)]
        TenantQueue[(Campaign Queue)]
        Auth --> |"OTP, Invites, Password Resets"| SysQueue
        Campaigns --> |"Newsletters, Bulk Promos"| TenantQueue
        class SysQueue db;
        class TenantQueue db;

        SysWorker[System Mail Worker]
        TenantWorker[Tenant Mail Worker]
        SysQueue --> SysWorker
        TenantQueue --> TenantWorker
        class SysWorker sysWorker;
        class TenantWorker tenWorker;
    end

    subgraph EmailDeliveryProviders [Email Delivery Providers]
        Gmail[Gmail SMTP]
        SES[AWS SES]
        SysWorker --> |"shrmail.app@gmail.com"| Gmail
        TenantWorker --> |"sales@tenantdomain.com"| SES
        class Gmail provider;
        class SES provider;
    end

    Inbox1([User Inbox])
    Inbox2([Subscriber Inbox])
    Gmail --> |"Guaranteed Inbox Delivery"| Inbox1
    SES --> |"Isolated Tenant Reputation"| Inbox2
    class Inbox1 userNode;
    class Inbox2 userNode;

    classDef dualBox fill:#f8fafc,stroke:#cbd5e1,stroke-width:2px,stroke-dasharray: 5 5;
    class DualProcessingQueues dualBox;
    class EmailDeliveryProviders dualBox;
```
</details>

<br>

### 2. 📱 [Chatnalyxer](https://github.com/Rahul-pamula/chatnalyxer)
An AI-powered mobile app that automatically extracts events and deadlines directly from WhatsApp messages using NLP and Multi-Modal integrations.

<details open>
<summary><b>View Architecture Flow</b></summary>

```mermaid
graph TD
    classDef mobile fill:#0984e3,stroke:#074b83,stroke-width:2px,color:#fff,font-weight:bold,rx:10px,ry:10px;
    classDef backend fill:#00b894,stroke:#00876c,stroke-width:2px,color:#fff,font-weight:bold,rx:5px,ry:5px;
    classDef ai fill:#fdcb6e,stroke:#e17055,stroke-width:2px,color:#2d3436,font-weight:bold,rx:10px,ry:10px;
    classDef db fill:#636e72,stroke:#2d3436,stroke-width:2px,color:#fff;
    
    User([Mobile User]) --> App[React Native App]
    class User mobile;
    class App mobile;
    
    subgraph CoreBackend [FastAPI Backend]
        API[API Gateway]
        Queue[Task Scheduler]
        App --> |Syncs Conversations| API
        API --> Queue
        class API backend;
        class Queue backend;
    end

    subgraph AIServices [AI Processing Layer]
        Gemini[Google Gemini API]
        AzureVision[Azure Vision API]
        AzureSpeech[Azure Cognitive Speech]
        
        API --> |Text Extraction| Gemini
        API --> |Image Parsing| AzureVision
        API --> |Voice Transcription| AzureSpeech
        class Gemini ai;
        class AzureVision ai;
        class AzureSpeech ai;
    end
    
    DB[(PostgreSQL DB)]
    API --> |Stores Deadlines| DB
    class DB db;
    
    App --> |Smart Calendar Notifications| User
```
</details>

<br>

### 3. ✂️ [Tailoring](https://github.com/Rahul-pamula/tailoring)
A highly polished, modern frontend architecture. Uses Vite, TypeScript, React, and Tailwind CSS to craft a premium user interface.

---

<div align="center">
  <i>"I build resilient systems and beautiful interfaces."</i>
</div>