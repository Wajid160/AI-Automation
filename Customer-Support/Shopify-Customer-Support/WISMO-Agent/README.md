# AI Automation – Shopify Customer Support

## 📦 Shopify WISMO (Where Is My Order) AI Agent

### Overview

* This project implements a **production-ready AI-powered WISMO customer support agent** for Shopify stores.
* The agent automatically reads incoming customer emails, identifies order-related queries, retrieves order information from Shopify, and replies with accurate, policy-compliant responses.
* Built using **agentic AI principles** with strict tool-calling and hallucination guardrails.

---

## 🎯 Problem Statement

* WISMO ("Where is my order?") emails account for **40–60%** of Shopify customer support volume.
* Manual handling leads to:

  * Slow response times
  * High operational costs
  * Poor customer experience

---

## ✅ Solution

* An autonomous AI agent that:

  * Listens for new, unread support emails
  * Extracts **Order ID** or **Customer Email** from the message
  * Queries Shopify only when valid parameters exist
  * Responds with real order data or requests missing information
  * Never hallucinates order details

---

## 🧠 Agent Capabilities

* Order ID detection (e.g. `#1023`, `1023`)
* Email-based order lookup
* Shopify order status & fulfillment retrieval
* Graceful fallback for missing information
* Professional, brand-safe email responses

---

## 🏗️ Architecture

* **Trigger:** Gmail (Unread customer emails)
* **Reasoning Layer:** Agentic AI (LangChain-style agent)
* **Tools:**

  * Shopify Orders API
* **Output:** Automated email reply via Gmail

> Designed with strict execution rules to prevent unauthorized tool usage or hallucinated responses.

---

## 🛠️ Tech Stack

* n8n (Workflow Orchestration)
* Shopify API
* Gmail API
* Google Gemini (LLM)
* LangChain Agent Node (n8n)

---

## 📂 Repository Structure

```
AI-Automation/
└── Customer-Support/
    └── Shopify-Customer-Support/
        └── WISMO-Agent/
            ├── workflow/
            │   └── wismo-agent.n8n.json
            ├── prompts/
            │   └── system-prompt.md
            ├── architecture/
            │   └── wismo-agent-diagram.png
            ├── setup-guide.md
            └── README.md
```

---

## ⚙️ Setup (High-Level)

* Configure Gmail Trigger with support inbox
* Connect Shopify store via access token
* Import the n8n workflow JSON
* Attach LLM credentials (Gemini)
* Activate workflow

> Detailed step-by-step instructions available in `setup-guide.md`.

---

## 📊 Example Outcome

* Customer sends: "Where is my order #1003?"
* Agent replies instantly with:

  * Order status
  * Fulfillment state
  * Tracking info (if available)

---

## 🚧 Limitations & Notes

* Uses test / dummy Shopify data
* Credentials are removed from workflow export
* Intended for educational, portfolio, and client-demo use

---

## 🚀 Future Enhancements

* Refund status agent
* Cancellation & modification agent
* Main orchestrator agent with specialist tools
* Multi-channel support (Chat, WhatsApp)

---

## 👤 Author

**Wajid Javed**
AI Automation & Agentic Systems Developer

---

## 📄 License

MIT License
