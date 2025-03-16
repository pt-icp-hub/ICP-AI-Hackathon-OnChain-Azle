# 🤖 OnChain AI with Azle (TypeScript)

This repository provides an example of how to build an **on-chain AI agent** on the Internet Computer (IC) using **TypeScript** and **Azle**, integrating seamlessly with the **LLM Canister** developed by the DFINITY Foundation. This project demonstrates how to deploy a full-stack **TypeScript-based** canister that interacts with the LLM Canister to generate AI-driven responses on-chain.

## ✨ Overview

- **Leverages the LLM Canister**: The DFINITY Foundation’s LLM Canister makes it easy to integrate AI-powered text generation into IC projects.
- **Azle-Based**: This project utilizes **Azle**, a TypeScript framework for building canisters, making it accessible for JavaScript/TypeScript developers.
- **Full-Stack Solution**: Includes both a **backend** (Azle canister) and a **frontend** (React-based UI) to interact with the AI agent.
- **Expandable**: Developers can modify prompts, interactions, and logic to suit their specific AI use cases.

---

## 🚀 Getting Started

### 1. Prerequisites

1. **DFX SDK**: Install the [DFX SDK](https://internetcomputer.org/docs/current/developer-docs/build/install-upgrade-remove).
2. **Node.js (Version 22.14.0)**: This project requires Node.js version **22.14.0**, as specified in `mise.toml`. You can install this specific version using:
   - [NVM](https://github.com/nvm-sh/nvm):
     ```bash
     nvm install 22.14.0
     nvm use 22.14.0
     ```
   - [Mise](https://mise.jdx.dev/):
     ```bash
     mise use -g node@22.14.0
     ```
   - Or manually download and install [Node.js 22.14.0](https://nodejs.org/download/release/v22.14.0/).
3. **Ollama (Optional for Local Testing)**: Install [Ollama](https://ollama.com/) for running AI models locally.

### 2. Clone & Install

```bash
git clone https://github.com/pt-icp-hub/ICP-AI-Hackathon-OnChain-Azle.git
cd ICP-AI-Hackathon-OnChain-Azle

# Install Azle backend dependencies
npm install

# Install frontend dependencies
cd src/frontend
npm install
cd ../..
```

---

### 3. Local Testing with Ollama (Optional)

If you want to test the AI agent locally before deploying it to the Internet Computer, you can use **Ollama**:

1. **Run Ollama Server**  
   ```bash
   ollama serve
   # Expected to start listening on port 11434
   ```
2. **Download a Model** (e.g., llama3.1:8b)  
   ```bash
   ollama run llama3.1:8b
   # This will download the model; you can terminate once the model loads.
   ```

> **Note**: You can modify the backend code to point requests to **Ollama** locally instead of the **LLM Canister** for debugging.

---

### 4. Deploy to the Internet Computer

1. **Start the Local Replica**  
   ```bash
   dfx start --clean
   ```
2. **Deploy Canisters**  
   ```bash
   dfx deploy
   ```

---

## 🏗 Project Structure

```
ICP-AI-Hackathon-OnChain-Azle/
├── src
│   ├── backend
│   │   ├── index.ts        # Core Azle backend interacting with the LLM Canister
│   │   ├── llm.ts          # Utility functions for LLM interactions
│   ├── frontend
│   │   ├── assets
│   │   ├── dist
│   │   ├── src
│   │   ├── index.html
│   │   └── package.json    # Frontend dependencies
├── dfx.json               # DFX configuration
├── mise.toml              # Specifies Node.js version 22.14.0
├── package.json           # Backend dependencies
├── tsconfig.json          # TypeScript configuration
└── README.md              # You are here
```

- **`index.ts`**: Contains the backend logic to interact with the LLM Canister via Azle.
- **`llm.ts`**: Utility module for handling interactions with the LLM Canister.
- **`frontend`**: Basic web interface to send prompts and display AI responses.
- **`dfx.json`**: Configures canisters for local testing and deployment.
- **`mise.toml`**: Specifies the required Node.js version for compatibility.
- **`tsconfig.json`**: TypeScript configuration settings.

---

## 🛠 Core Functionality

The agent utilizes **Azle** to interface with the **LLM Canister**. The backend consists of two main files:

1. **`llm.ts`**: Handles low-level interactions with the LLM Canister, converting messages to IDL format and making requests.
2. **`index.ts`**: Defines the Azle canister and exposes AI functions via **update calls**.

### LLM Integration in `index.ts`
```typescript
import { IDL, update } from "azle";
import { chat_message as ChatMessageIDL } from "azle/canisters/llm/idl";
import * as llm from "@dfinity/llm";

export default class {
  @update([IDL.Text], IDL.Text)
  async prompt(prompt: string): Promise<string> {
    return await llm.prompt(llm.Model.Llama3_1_8B, prompt);
  }

  @update([IDL.Vec(ChatMessageIDL)], IDL.Text)
  async chat(messages: llm.ChatMessage[]): Promise<string> {
    return await llm.chat(llm.Model.Llama3_1_8B, messages);
  }
}
```

### Message Formatting and Canister Calls in `llm.ts`
- Converts messages to IDL format before sending them to the LLM Canister.
- Uses `call` to interact with the LLM Canister (`w36hm-eqaaa-aaaal-qr76a-cai`).
- Provides helper functions to handle chat message serialization and API calls.

---

## 🤝 Contributing

We welcome contributions! Feel free to open issues or pull requests to improve this template. Join the [IC Developer Community](https://forum.dfinity.org/) for discussions, updates, and collaboration.

---

## 📚 Learn More

- [LLM Canister Introduction](https://forum.dfinity.org/t/introducing-the-llm-canister-deploy-ai-agents-with-a-few-lines-of-code/41424)
- [Azle Documentation](https://demergent-labs.github.io/azle/)
- [Internet Computer Docs](https://internetcomputer.org/docs/home)
- [Node.js Releases](https://nodejs.org/en/download/releases)

---

**Happy hacking!** 🚀 Build your own on-chain AI agent and explore the limitless potential of AI on the Internet Computer!
