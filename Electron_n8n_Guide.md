# Electron + n8n Integration Guide

This guide shows how to connect an **Electron desktop app (EXE)** with **n8n** using a simple webhook.

---

## 🚀 Overview
1. Create a Webhook workflow in n8n.  
2. Make Electron send a POST request to the webhook.  
3. Let n8n process the data (store, forward, or respond).  

---

## 🔧 Step 1: Create Webhook in n8n
1. Open **n8n editor**.  
2. Create a **new workflow**.  
3. Add a **Webhook Node**:  
   - Method: `POST`  
   - Path: `electron-test`  
   - URL will look like:  
     ```
     http://localhost:5678/webhook/electron-test
     ```  
4. Add another node (e.g., **Respond to Webhook** or **Google Sheets → Append Row**).  
5. Connect nodes.  
6. **Activate** the workflow.  

---

## 🔧 Step 2: Electron App Sends Request

In your Electron project (`main.js`):

```javascript
const { app, BrowserWindow } = require("electron");
const fetch = require("node-fetch"); // install with: npm install node-fetch

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: { nodeIntegration: true }
  });

  win.loadFile("index.html");

  // Send request to n8n webhook
  fetch("http://localhost:5678/webhook/electron-test", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ message: "Hello from Electron!" })
  })
    .then(res => res.json())
    .then(data => console.log("Response from n8n:", data))
    .catch(err => console.error("Error:", err));
}

app.whenReady().then(createWindow);
```

---

## 🔧 Step 3: Test
1. Run your **n8n workflow** → make sure it’s active.  
2. Run your Electron app → `npm start`.  
3. Electron sends POST request → n8n receives it.  
4. n8n can then:  
   - Store it in **Google Sheets**  
   - Forward via **Email/Telegram**  
   - Or just **respond with JSON**  

---

✅ You now have a working **Electron EXE ↔ n8n integration** 🎉  
