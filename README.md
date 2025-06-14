# 📘 Expense Tracker Automation using n8n + Google Sheets
[https://mani-expense-tracker.netlify.app/]

Track your daily expenses using a beautiful web form and log them directly into Google Sheets — **no code**, **no paid APIs**, just smart automation with **n8n**.

---

## ✅ What You’ll Build (Phase 1)

A form that:

- Collects expense data (date, category, description, amount)  
- Sends it to n8n via Webhook  
- Appends the data to a Google Sheet named `ExpenseTracker → Sheet1`

---

## 🛠 Tools Used

| Tool           | Purpose                         | Free Tier |
|----------------|----------------------------------|-----------|
| n8n Cloud      | Automation workflows             | ✅ Yes     |
| Google Sheets  | Data storage for expenses        | ✅ Yes     |
| HTML           | Frontend expense form            | ✅ Yes     |

---

## 🔧 Setup Instructions

### 📄 Step 1: Create Google Sheet

1. Open [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet named `ExpenseTracker`
3. Rename the first sheet to `Sheet1`
4. In Row 1, add the following headers:

Date | Category | Description | Amount



---

### ⚙️ Step 2: Create an n8n Workflow

1. Go to [n8n.io](https://n8n.io) → Sign up or log in
2. From your dashboard, click **"New Workflow"**

---

### 🌐 Step 3: Add a Webhook Node

1. Click the ➕ button and search for **Webhook**
2. Set the following:
   - **HTTP Method**: `POST`
   - **Path**: `expense-form`
3. Click **Execute Node** to enter test mode
4. Note the **Test Webhook URL**

---

### 📝 Step 4: Create the Form (HTML)

Paste this into an `.html` file (you’ll update the URL in Step 6):

```html
<form action="https://YOUR-WEBHOOK-URL" method="POST">
  <input type="date" name="date" required>
  <select name="category" required>
    <option value="Food">Food</option>
    <option value="Travel">Travel</option>
    <option value="Shopping">Shopping</option>
    <option value="Bills">Bills</option>
  </select>
  <textarea name="description" placeholder="Optional..."></textarea>
  <input type="number" name="amount" step="0.01" required>
  <button type="submit">Submit</button>
</form>

```

🔄 Submit the form while in test mode to pass sample data to the webhook.

### 📊 Step 5: Add a Google Sheets Node
Click ➕ → search for Google Sheets

Choose Action: Append Row

Connect your Google Account

Set the following fields:

Setting	Value
Spreadsheet	ExpenseTracker
Sheet	Sheet1
Mapping Mode	Map Each Column Manually
```
Date	{{$json["body"]["date"]}}
Category	{{$json["body"]["category"]}}
Description	{{$json["body"]["description"]}}
Amount	{{$json["body"]["amount"]}}
```
### 🚀 Step 6: Switch to Production Webhook
Open the Webhook node

Copy the Production URL (not the test one)

Update your HTML form to:
```
<form action="https://YOUR-N8N-WORKSPACE.n8n.cloud/webhook/expense-form" method="POST">
```
  
### ✅ Step 7: Activate the Workflow
Click the green Activate button at the top-right of the workflow

Your workflow is now live and listening forever 🎉

### ✅ Result
Every time you fill the form:

A new row is added to your Google Sheet

It includes: Date, Category, Description, Amount
