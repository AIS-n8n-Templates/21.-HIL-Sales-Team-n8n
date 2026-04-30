# Boosting Sales Productivity through Human-in-Loop AI Email Automation

![n8n](https://img.shields.io/badge/Automation-n8n-orange?style=for-the-badge)
![Airtable](https://img.shields.io/badge/Database-Airtable-blue?style=for-the-badge)
![OpenAI](https://img.shields.io/badge/Model-GPT--4--O-green?style=for-the-badge)
![Gmail](https://img.shields.io/badge/Integration-Gmail-red?style=for-the-badge)
![Gemini](https://img.shields.io/badge/Classifier-Gemini--2.5--Flash-yellow?style=for-the-badge)
![HumanInLoop](https://img.shields.io/badge/System-Human--in--Loop-black?style=for-the-badge)

---

## 🌟 **Project Overview**

This project introduces a **Human-in-Loop Sales Response System** designed to automate lead engagement while maintaining human quality assurance.  
It empowers sales teams to respond faster, personalize communication, and improve decision-making — ensuring every client interaction feels both intelligent and human.

---

## ❗ **Problems We Solve**

Traditional sales workflows often face:
- Delayed responses to new leads due to manual drafting.
- Inconsistent communication tone across sales representatives.
- Missed opportunities caused by slow follow-ups.
- Lack of centralized feedback between human reviewers and automation tools.

---

## 💡 **How We Help**

This workflow bridges automation and human expertise:
- **AI-driven lead responses** drafted instantly using GPT-4-O.  
- **Human review loop** ensures every message aligns with tone, context, and client expectations.  
- **Smart revisions** automatically adjust drafts based on reviewer feedback using the Revision Agent.  
- **Integrated Airtable and Gmail tools** centralize leads and streamline communication within one automated system.

---

## 📈 **Productivity Gains**

With this system in place:
- Response time per lead is reduced from hours to minutes.  
- Sales reps save up to **70% of manual drafting time**.  
- Conversion rates improve through faster, contextual responses.  
- Human oversight ensures trust, consistency, and professionalism.

---

## 🧭 **Workflow Diagram**

![Workflow Diagram](https://github.com/SachinSavkare/HIL-Sales-Team-n8n/blob/main/21.%20HIL%20Sales%20Team.JPG)

---

## 🧩 **Mermaid Overview (High-Level Flow)**

```mermaid
flowchart TD
  A[Airtable Trigger] --> B[Lead Response Agent - GPT-4-O]
  B --> C[Set Email Field]
  C --> D[Gmail - Human Review Request]
  D --> E[Check Feedback - Gemini 2.5 Flash]
  E -->|Approved| F[Gmail - Send to Lead]
  E -->|Declined| G[Revision Agent - GPT-4-O]
  G --> C
````

---

[![Watch the Project](https://raw.githubusercontent.com/AIS-n8n-Templates/21.-HIL-Sales-Team-n8n/main/Thumbnail.png)](https://www.youtube.com/watch?v=tjl8CK088a4)

---

## ⚙️ **Node-by-Node Configuration**

### **Step 1: Airtable Trigger**

* **Purpose:** Detect new lead entries in Airtable.
* **Credential:** `Airtable_AIS_08`
* **Trigger Field:** `Created`
* **Fields:** name, email, intent, budget, companyName, projectDescription, timeline

---

### **Step 2: AI Agent — Lead Response Agent**

* **Purpose:** Generate a concise, professional sales email.
* **Model:** `OpenAI GPT-4-O`
* **System Message:**

  ```
  You are an expert sales person for an agency that delivers AI solutions. 
  Respond to incoming leads by addressing their needs in a professional, concise manner.
  Your goal is to convince them to book a second call. Retrieve a similar past project 
  from the Project Database to demonstrate capability. 
  Sign off as Jim, Dunder AI.
  ```
* **Tool Used:** Project Database (Airtable → Projects table)

---

### **Step 3: Edit Field — Set Email Field**

* **Purpose:** Store generated email content for routing and revisions.
* **Mapping:** `email = {{ $json.output.email }}`

---

### **Step 4: Gmail — Get Feedback**

* **Purpose:** Send draft email to human reviewer and wait for feedback.
* **Reviewer:** `sachinsavkare08@gmail.com`
* **Subject Template:**

  ```
  APPROVAL REQUIRED for NEW LEAD: {{ $('Airtable Trigger').item.json.fields.name }}
  ```
* **Response Type:** Free Text

---

### **Step 5: Text Classifier — Check Feedback**

* **Purpose:** Classify reviewer feedback into Approved or Declined.
* **Model:** `Gemini 2.5 Flash`
* **Categories:**

  * **Approved:** “Looks good”, “Approved”, “Send it.”
  * **Declined:** “Needs some changes”, “Make it shorter”, “Reword this.”

---

### **Step 5A: Gmail — Send Approved Email**

* **Purpose:** Deliver final approved email to the lead.
* **Recipient:** `{{ $('Airtable Trigger').item.json.fields.email }}`
* **Append n8n Attribution:** OFF

---

### **Step 5B: AI Agent — Revision Agent**

* **Purpose:** Revise email based on feedback and re-submit for review.
* **Model:** `OpenAI GPT-4-O`
* **System Message:**

  ```
  You are an expert email writer. 
  Revise the provided email based on human feedback while keeping tone professional.
  Sign off as Jim, Dunder AI.
  ```

---

### **Step 6: Project Database (Tool)**

* **Purpose:** Retrieve similar past projects to build credibility in AI email responses.
* **Credential:** `Airtable_AIS_08`
* **Operation:** Search → Return All → Model-driven filtering

---

## 📥 **Free Workflow Template**

* **Download:** [n8n workflow JSON](https://github.com/SachinSavkare/HIL-Sales-Team-n8n/blob/main/21.%20Human%20in%20Loop%20Sales%20Team.json)

---

## ✍️ **Author**

**Sachin Savkare**
📧 `sachinsavkare08@gmail.com`

---
