# Electron App Guide (Build to .exe)

This guide shows you how to create a simple Electron app that launches your web app (`https://google.com/`) and build it into a Windows `.exe` file.

---

## 📦 Step 1. Setup Project

```bash
# Create project folder
mkdir electron_exe && cd electron_exe

# Initialize package.json
npm init -y

# Install Electron & Builder
npm install --save-dev electron electron-builder
```

---

## 📄 Step 2. Configure `package.json`

Open `package.json` and update it like this:

```json
{
  "name": "google",
  "version": "1.0.0",
  "description": "Electron desktop launcher for https://google.com/",
  "main": "main.js",
  "author": "Your Name",
  "license": "MIT",
  "scripts": {
    "start": "electron .",
    "build": "electron-builder"
  },
  "devDependencies": {
    "electron": "^32.1.0",
    "electron-builder": "^25.0.0"
  },
  "build": {
    "appId": "com.google",
    "productName": "google",
    "win": {
      "target": "nsis",
      "icon": "icon.ico"
    }
  }
}
```

- `appId` → unique ID (reverse domain style).
- `productName` → installer name.
- `icon.ico` → place your app icon in project root.

---

## 📝 Step 3. Create `main.js`

```js
const { app, BrowserWindow } = require("electron");

function createWindow() {
  const win = new BrowserWindow({
    width: 1000,
    height: 800,
    webPreferences: {
      nodeIntegration: false
    },
    icon: __dirname + "/icon.ico"
  });

  // Load your web app
  win.loadURL("https://google.com/");
}

app.whenReady().then(createWindow);
```

---

## ▶️ Step 4. Run App

```bash
npm start
```

This opens your app in an Electron window.

---

## 📦 Step 5. Build .exe

```bash
npm run build
```

Electron Builder will generate your `.exe` installer in:

```
dist/
```

Example: `dist/google Setup 1.0.0.exe`

---

## 🎨 Step 6. Change Icon

1. Create or convert your logo into `icon.ico` (256x256 recommended).
2. Place `icon.ico` in project root.
3. Rebuild (`npm run build`) and the `.exe` will use your custom icon.

---

✅ Done! You now have a Windows `.exe` that launches your web app in Electron.
