# 🚀 Automated Lead Qualification & CRM Pipeline Agent

An enterprise-grade, serverless lead qualification and CRM automation pipeline built using **n8n**, **Webhooks**, **Google Apps Script**, and **Google Sheets**. This agent automatically filters incoming leads, validates data to prevent errors, and implements a smart **Upsert (Append or Update)** logic to maintain clean, duplicate-free records.

---

## 📌 What is this Agent? (Overview)
This project is an automated backend intelligence system designed to capture potential client inquiries, filter them based on predefined qualification criteria (such as project budget), and securely sync them with Google Sheets. It acts as an autonomous sales assistant that operates 24/7 without manual intervention.

---

## 🌍 Where Can This Be Used? (Real-World Use Cases)

1. **Freelance & Software Development Agencies:**
   * Automatically filter incoming "Contact Us" or project inquiry forms on your agency website, separating low-budget leads from high-value clients.
2. **E-Commerce & Wholesale Stores:**
   * Route bulk or wholesale buyers to VIP CRM lists while handling retail inquiries separately.
3. **Real Estate & Property Firms:**
   * Qualify property buyers instantly based on their stated investment budget.
4. **EdTech & Online Course Creators:**
   * Filter serious workshop or course applicants from casual form submissions.

---

## 💡 How Does It Solve Real-World Problems & Save Time?

* **Eliminates Manual Filtering:** Sales teams and developers no longer need to manually check spreadsheets or emails to see which leads are worth pursuing.
* **Strict Input Validation:** Blocks incomplete or empty submissions (such as missing emails or budgets) right at the entry layer, ensuring zero bad data enters your database.
* **Smart Upsert Logic (No Duplicate Entries):** Uses the user's **Email** as a unique matching key. If a user submits the form again with a higher budget, the system updates their existing row instead of creating messy duplicates.
* **Instant Automation:** Operates in real-time the moment a form is submitted.

---

## 🔌 Integration Channels (Beyond Google Forms)
While this agent was tested using **Google Forms**, because it runs on a secure **Webhook**, you can plug it into almost any data source:
* **Custom React / HTML Websites:** Send JSON payloads directly via JavaScript `fetch()` POST requests.
* **WordPress / Elementor / Webflow Forms:** Use native webhook integrations to forward form submissions to n8n.
* **Meta (Facebook/Instagram) Lead Ads:** Capture ad leads in real-time.
* **Telegram & WhatsApp Bots:** Process conversational lead generation data.

---

## 🛠️ Tech Stack
* **Workflow Automation:** n8n (Self-hosted)
* **Backend Integration:** Google Apps Script (`UrlFetchApp`)
* **Frontend Collection:** Google Forms (`name`, `email`, `budget`, `country`)
* **Database / CRM:** Google Sheets API (Service Account Authentication)

---

## ⚙️ Step-by-Step Setup Guide

### Step 1: Import n8n Workflow
1. Download the `Leads Qualification Agent.json` file from this repository.
2. Import the JSON into your **n8n** instance.
3. Configure your Google Sheets credentials and link your target spreadsheet.
4. Activate the workflow to generate your production Webhook URL.

### Step 2: ## 🔌 Flexibility & Integration Channels (Any Form, Any Platform)
Because this agent is powered by a **Secure Webhook Endpoint**, it is not limited to Google Forms. You can connect it to **any** lead generation source:

1. **Custom React / Next.js / HTML Websites:** Send JSON payloads directly using a JavaScript `fetch()` POST request.
2. **WordPress, Elementor, or Webflow Forms:** Use native webhook actions to forward submissions.
3. **Google Forms:** Connected via a lightweight Google Apps Script.
4. **Meta (Facebook/Instagram) Lead Ads or Chatbots (Telegram/WhatsApp).**

---

### 🛠️ Dynamic Fields Handling (Not Restricted to Name/Email)
The system is built to dynamically parse incoming JSON keys. While our default test implementation used fields like `name`, `email`, `budget`, and `country`, **you can pass any custom keys** (e.g., `company_size`, `phone`, `project_scope`) as long as your n8n workflow and database columns match those keys.

 function onSubmit(e) {
  var formResponse = e.response;
  var itemResponses = formResponse.getItemResponses();
  var leadData = {};
  
  // Dynamically map whatever fields the user created in their form
  for (var i = 0; i < itemResponses.length; i++) {
    var itemResponse = itemResponses[i];
    var title = itemResponse.getItem().getTitle().toLowerCase().trim().replace(/\s+/g, '_');
    var answer = itemResponse.getResponse();
    
  Automatically convert numbers if it looks like a budget or amount
    if (title.includes("budget") || title.includes("price") || !isNaN(answer)) {
      leadData[title] = Number(answer);
    } else {
      leadData[title] = answer;
    }
  }
  
  var webhookUrl = "YOUR_N8N_PRODUCTION_WEBHOOK_URL_HERE"; 
  
  var options = {
    "method": "post",
    "contentType": "application/json",
    "payload": JSON.stringify(leadData)
  };
  
  try {
    UrlFetchApp.fetch(webhookUrl, options);
  } catch (error) {
    Logger.log("Error sending data to n8n: " + error);
  }
}

Replace "YOUR_N8N_PRODUCTION_WEBHOOK_URL_HERE" with your active n8n Webhook URL.

   Set up an Apps Script Trigger: Click on the Triggers (Clock icon), add a new trigger for function onSubmit, set event source to From form, and event type to On form submit.

👨‍💻 Author

Muhammad Danish

Full-Stack Web Developer & AI Automation Engineer

  GitHub: Dani-Baba
