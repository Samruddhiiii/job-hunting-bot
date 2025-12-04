<img width="1826" height="885" alt="Screenshot 2025-12-03 011824" src="https://github.com/user-attachments/assets/b602aeec-eea0-4877-b736-86543b9a7c05" />

#  Automate Your Entire Job Hunt Using n8n + AI 
Tired of searching for jobs manually, opening every posting, matching your resume, and writing cover letters every day?

**Let’s automate everything.**
This guide shows how to build a full **AI-powered Job Application Assistant** using:

* **n8n (open-source automation tool)**
* **Google Sheets**
* **Google Drive**
* **Telegram Bot**
* **Google Gemini AI**
* **Your resume**

Once set up, this workflow will:

###  Search LinkedIn daily at 5 PM

###  Match each job to your resume using AI

###  Generate custom cover letters

###  Store everything in Google Sheets

###  Send Telegram notifications for high-scoring jobs

### All automatically — *while you sleep.* 

---

# 📌 What This Workflow Does

| Feature                   | Description                                          |
| ------------------------- | ---------------------------------------------------- |
|  Automated Job Search   | Scrapes LinkedIn daily for your keywords & filters   |
|  AI Resume Matching     | Scores jobs based on your PDF resume                 |
|  Auto Cover Letters     | AI generates a customized cover letter for each role |
|  Google Sheets Database | Stores all job listings, scoring & cover letters     |
|  Telegram Alerts        | Sends you jobs that score ≥ 50                       |
|  Fully Automated        | You set it once — it runs forever                    |

---

# Requirements

You need:

* An **n8n instance**
* A **Google account** (Drive + Sheets)
* A **Telegram account**
* A **Google Gemini API key**
* Your **resume in PDF format**

---

# 🧱 Part 1 — Create the Workflow

## 1. Create a New Workflow

* Name it: **AI Job Application Assistant**
* Save it.

## 2. Add **Schedule Trigger**

Runs daily at 5 PM.

* Interval → Days → 1
* Trigger at Hour → **17**
* Execute the node to test.

---

# 📄 Part 2 — Resume Setup

## 3. Upload Resume to Google Drive

* Upload PDF → Right click → Get Link
* Extract File ID from URL:

```
https://drive.google.com/file/d/FILE_ID/view
```

## 4. Add Google Drive Credentials (OAuth2)

## 5. Add “Download File” Node

* Operation: **Download**
* URL: your sharing link

## 6. Add “Extract From File” Node

* Operation: **PDF**
* Binary Property: `data`

This gives your resume text.

---

# 📝 Part 3 — Job Search Preferences (Google Sheets)

## 7. Create a Spreadsheet With Two Sheets

### Sheet 1 — **Filter**

| Keyword  | Location | Experience Level              | Remote         | Job Type  | Easy Apply |
| -------- | -------- | ----------------------------- | -------------- | --------- | ---------- |
| React JS | Gurugram | Entry level, Mid-Senior level | Remote, Hybrid | Full-time | TRUE       |

### Sheet 2 — **Result**

Columns:

```
Title | Company | Location | link | description | score | Cover Letter
```

## 8. Add Google Sheets Credentials

## 9. Add “Get Rows” Node (Filter Sheet)

Reads all preferences.

---

# 🔗 Part 4 — Build LinkedIn Job Search URL

## 10. Add Code Node — “Create search URL”

This code dynamically builds a LinkedIn Job Search URL using your preferences.

*(The full code from your previous message goes here — unchanged)*

Outputs:

```
{ url: "https://linkedin.com/jobs/search/?...." }
```

---

# 🌐 Part 5 — Fetch Jobs From LinkedIn

## 11. Add “HTTP Request” Node

* GET → URL = `{{ $json.url }}`

## 12. Add “HTML Extract”

Extracts job listing URLs.

## 13. Add “Split Out”

Converts list into individual items.

---

# 🔁 Part 6 — Process Jobs One By One

## 14. Add “Loop Over Items”

## 15. Add “Wait” (8 seconds delay)

To avoid LinkedIn rate limits.

## 16. Add “Fetch Job Page”

Fetches each job detail page.

---

# 📦 Part 7 — Parse Job Attributes

## 17. Add “HTML Extract” — Parse Job Info

Extract:

* title
* company
* location
* description
* job ID

## 18. Add “Modify Job Attributes”

Clean and format fields.

---

# 🧠 Part 8 — AI Resume Matching Using Gemini

## 19. Get API Key

From **Google AI Studio**.

## 20. Add Credential: **Google Gemini API**

## 21. Add “Google Gemini Chat Model” Node

## 22. Add “AI Agent” Node

Paste the full prompt (from your previous message).

Connect it to Gemini model.

This generates:

* **score**
* **custom cover letter**

---

# 🧹 Part 9 — Parse AI Output

## 23. Add “Edit Fields (Set)” Node

Remove markdown code blocks:

````
$json.output.replaceAll(/```(?:json)?/g, "")
````

---

# 📊 Part 10 — Save to Google Sheets

## 24. Add “Append or Update Row”

* Maps all fields
* Matching Column: **link**
  (Prevents duplicates)

---

# 🎯 Part 11 — Filter High-Matching Jobs

## 25. Add "IF" Node — Score Filter

Condition:

```
score >= 50
```

True → Send notification
False → Continue loop

---

# 📲 Part 12 — Telegram Notifications

## 26. Create Telegram Bot with **@BotFather**

Get:

* **Bot Token**
* **Chat ID**

## 27. Add Telegram Credential

## 28. Add “Send Text Message” Node

Message template includes job info + cover letter link.

---

# 🔁 Part 13 — Close The Loop

## 29. Connect “True” & “False” to Loop Over Items

Job processing continues.

## 30. Loop → Wait

Flow is complete.

---

# 🧪 Part 14 — Test & Deploy

## 31. Execute Workflow

Verify:

* Sheets updated
* Telegram alerts sent
* Cover letters generated

## 32. Activate Workflow

Now it runs every day automatically.

## 33. Adjust Search & Filters

Tune:

* Keywords
* Score threshold
* Timing

---

# 🎉 Final Result

You have built a fully automated:

* Job finder
* Resume matcher
* Cover-letter writer
* Daily notifier
* Job-tracking system

All with **zero manual work**.
