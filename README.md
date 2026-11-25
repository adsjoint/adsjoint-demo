# 💬 Adsjoint Demo

A minimal demo showing how to integrate the **@adsjoint/sdk** into an LLM-based chat app.

> 🧩 Adsjoint helps LLM apps monetize through context-aware ads — automatically placed in chat with smart delivery.

---
## 🖼️ Live Example — What Inline Ads Look Like
Here’s an example of what the Adsjoint inline ad looks like inside a chat interface:
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/fa937877-7e3e-479f-a74a-54e2d946a068" />
<br><br>
## 🤝 Adsjoint Early Partner Program
An early-stage research initiative exploring monetization infrastructure for LLM-native applications.
### 🎁 Early Partner Benefits
1. Preferential revenue share after GA
2. Top priority for the first real advertisers once campaigns begin
3. Influence product decisions (ad UI, frequency rules, categories)

### 🛠️ What you get today
1. Demo ads for travel and games (test mode)
2. Context-aware inline ads insertion
3. Production campaign event tracking (requires backend setup; test events are console-only).
<br><br>
## 🚀 Quick Start
### 0️⃣ Get Access
Contact **admin@adsjoint.ai** to request your publisher credentials and SDK package access. No separate Adsjoint API key is required.
### 1️⃣ Clone the Repo
```bash
git clone https://github.com/adsjoint/adsjoint-demo.git
cd adsjoint-demo
```
### 2️⃣ Add Your .npmrc File
Create a new .npmrc file and open it in your default text editor, paste the following content inside the file:
```bash
@adsjoint:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
```
🪪 Replace YOUR_GITHUB_TOKEN with your GitHub Personal Access Token (classic)
(must include read:packages permission).
This allows npm to authenticate with GitHub Packages and download the private SDK.
### 3️⃣ SDK Integration
#### 1. Installation
```bash
npm login --registry=https://npm.pkg.github.com --scope=@adsjoint
npm install @adsjoint/sdk
```
#### 2. Initialization with Your Publisher Credentials
In src/App.jsx (or wherever you set up your chat logic), initialize the Adsjoint SDK with your assigned credentials:
```bash
initAdsjoint({
  publisherName: "demo_llm_app",    // unique name of your LLM app (e.g., "ChatGPT", "MyAwesomeBot")
  mode: "test",                     // use "production" (or "prod") for real campaigns
  publisherId: "01234567890"        // your publisher ID (string), provided by Adsjoint
});
```
💡 These credentials identify your app to the Adsjoint platform and ensure impressions/clicks are properly tracked.
#### 3. Inline Ads Placement
In src/App.jsx (or wherever you set up your chat logic), insert the Adsjoint ad component where your AI messages are displayed
```bash
<InlineAdBanner message={msg} />
```
### 4️⃣ Run
```bash
  npm start
```
In this demo, AI replies are simulated; no LLM API key is needed. In test mode,
send `ADSJOINT_DEMO_TRAVEL` or `ADSJOINT_DEMO_GAME` (exact, case-sensitive markers)
to trigger a demo ad. Ordinary words such as "travel" and "game" do not trigger demo ads.
Frequency limits still apply; reload the page to reset them.

The SDK defaults to `mode: "production"` (`"prod"` also works). Production never
shows demo ads and requires a configured campaign backend. Without backend setup
or an eligible campaign, no ad is displayed. Test events do not consume campaign budgets.

### Minimal Usage Example
```js
import { initAdsjoint, initAdLogic, InlineAdBanner } from "@adsjoint/sdk";

initAdsjoint({
  publisherName: "demo_llm_app",
  publisherId: "01234567890",
  mode: "test"
});

initAdLogic(true);

export default function App() {
  const [messages, setMessages] = useState([]);

  return (
    <div>
      {messages.map((msg, i) => (
        <div key={i}>
          <MessageBubble msg={msg} />
          <InlineAdBanner message={msg} />
        </div>
      ))}
    </div>
  );
}
```
## 💬 Support
📧 Email: admin@adsjoint.ai \
🌐 Website: https://adsjoint.ai (WIP)
