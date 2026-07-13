# Welcome to AnonVote

<div align="center">

![AnonVote Banner](https://img.shields.io/badge/Private%20Voting-On%20Stellar-0066CC?style=flat-square&logo=stellar)

**Private decision infrastructure for organizations**

> Separates voter identity from ballots at every layer — cryptographically, not by policy.

[Documentation](#) • [Getting Started](#getting-started) • [GitHub Issues](https://github.com/AnonVote/core/issues) • [Discord](#)

</div>

---

## 🔐 What is AnonVote?

AnonVote is a privacy-preserving voting platform where:

- **No voter is linked to their ballot** – Cryptographically enforced, not by policy
- **Results are blockchain-verified** – Anchor to Stellar for independent verification
- **No accounts required** – Voters only need a link and their identifier
- **Multiple voting schemes** – Standard, weighted, ranked-choice, delegated voting
- **Fully open source** – MIT licensed, self-hostable

---

## ✨ Core Features

### 🛡️ Privacy-First

- Voter identifiers hashed before storage
- Tokens cryptographically unlinked from votes
- Votes encrypted with AES-256-GCM
- No voter PII on-chain

### ⛓️ Blockchain Anchored

- Immutable audit trail on Stellar
- Smart contract verification via Soroban
- Independent result verification
- No need to trust AnonVote's servers

### 🎯 Flexible Voting

- **Standard** – One vote per token
- **Weighted** – Tokens with vote weight > 1
- **Ranked-Choice** – Preference ranking
- **Delegated** – Vote delegation chains

### 🚀 Developer Friendly

- TypeScript throughout
- REST API with clear contracts
- Shared crypto library (`@anonvote/crypto`)
- Extensively documented codebase

---

## 🏗️ Architecture at a Glance

```
┌─────────────────────────────────────────┐
│    React Frontend (Vite)                │
│  Ballot creation, voting, results view  │
└─────────────────┬───────────────────────┘
                  │ REST API
                  ▼
┌─────────────────────────────────────────┐
│    Express Backend (Node.js)            │
│  • Ballot management                    │
│  • Token issuance                       │
│  • Vote encryption & tallying           │
│  • Audit logging                        │
└─────────────────┬───────────────────────┘
                  │
          ┌───────┴────────┐
          ▼                ▼
    ┌──────────────┐  ┌────────────────┐
    │  PostgreSQL  │  │ Stellar Chain  │
    │              │  │                │
    │ • Ballots    │  │ • Audit Trail  │
    │ • Votes      │  │ • Verification │
    │ • Tokens     │  │ • Governance   │
    │ • Results    │  │ (Soroban)      │
    └──────────────┘  └────────────────┘
```

---

## 🚀 Getting Started

### Quick Start (5 minutes)

```bash
# Clone the main repository
git clone https://github.com/AnonVote/core.git
cd core

# Backend setup
cd backend
npm install
cp .env.example .env
npm run prisma:migrate
npm run dev

# Frontend setup (new terminal)
cd ../frontend
npm install
npm run dev

# Visit http://localhost:5173
```

### Requirements

- Node.js 18+
- PostgreSQL 14+
- (Optional) Rust + Stellar CLI for smart contract work

### Documentation

📖 See [core/README.md](https://github.com/AnonVote/core) for full setup instructions

---

## 📦 Our Repositories

| Repo                                                   | Purpose                               | Tech                               |
| ------------------------------------------------------ | ------------------------------------- | ---------------------------------- |
| **[core](https://github.com/AnonVote/core)**           | Main application (backend + frontend) | Express, React, Prisma, PostgreSQL |
| **[js](https://github.com/AnonVote/js)**               | Cryptographic primitives & types      | TypeScript, Node.js crypto         |
| **[contracts](https://github.com/AnonVote/contracts)** | Soroban smart contracts               | Rust, Stellar Soroban SDK          |
| **[protocol](https://github.com/AnonVote/protocol)**   | Whitepaper & specifications           | Markdown, diagrams                 |

---

## 🔍 How Voting Works

### 1️⃣ Ballot Creation

Admin creates a ballot with options, deadline, and eligibility list

### 2️⃣ Token Issuance

- Voter enters identifier (email, employee ID, etc.)
- System verifies eligibility
- One-time random token generated
- Token sent via email; original never stored in DB

### 3️⃣ Vote Casting

- Voter uses token to submit vote choice
- Vote encrypted with AES-256-GCM
- Encrypted payload stored; vote content never visible
- Event recorded on Stellar

### 4️⃣ Tallying & Results

- Admin closes ballot at deadline
- System decrypts votes and counts them
- Result published to PostgreSQL + Stellar + Soroban
- Consistency verified: tokens_issued == votes_cast

### 5️⃣ Public Verification

Anyone can verify the result by checking the Stellar transaction ID or querying the Soroban contract

---

## 🔒 Privacy & Security

### Privacy Model

```
Voter Identifier → SHA-256 hash → stored only as hash
                                    ↓
                    Generate cryptographic token
                                    ↓
Token → SHA-256 hash → stored only as hash
        (original token → given to voter, discarded)
                                    ↓
Vote Option → AES-256-GCM encrypt → stored encrypted
              (decrypted only at tally)
```

**Result:** No link between voter identity, token, and ballot choice exists anywhere.

### Security Features

✅ JWT authentication for organizations  
✅ Rate limiting on token requests  
✅ Duplicate attempt tracking (audit events only)  
✅ Vote consistency verification  
✅ M-of-N governance for critical operations  
✅ 48-hour timelock on smart contract upgrades  
✅ Immutable audit trail on Stellar

---

## 💡 Use Cases

- **Corporate governance** – Board elections, shareholder votes
- **Academic institutions** – Student government, faculty voting
- **Nonprofits** – Member voting, annual elections
- **DAOs** – Decentralized autonomous organizations
- **Public polls** – Surveys with privacy guarantees
- **Team decisions** – Agile retrospectives, sprint planning

---

## 🛠️ Tech Stack

### Frontend

- React 18, Vite, TypeScript
- Tailwind CSS, Radix UI
- Axios for HTTP

### Backend

- Express.js 4.18, Node.js
- TypeScript
- Prisma ORM, PostgreSQL
- JWT authentication
- stellar-sdk v12

### Smart Contracts

- Rust, Soroban SDK
- WebAssembly (wasm32-unknown-unknown)
- Stellar blockchain
- M-of-N governance

### Crypto

- SHA-256 (identifier/token hashing)
- AES-256-GCM (vote encryption)
- CSPRNG (token generation)

---

## 🤝 Contributing

We'd love your help! Areas for contribution:

- **Frontend** – React components, UX/UI improvements
- **Backend** – API enhancements, performance optimization
- **Smart Contracts** – Rust/Soroban contract work
- **Crypto** – `@anonvote/crypto` enhancements
- **Documentation** – Guides, tutorials, specs
- **Testing** – Unit tests, integration tests, property-based tests
- **Security** – Audits, vulnerability reports

See [CONTRIBUTING.md](https://github.com/AnonVote/core/blob/main/CONTRIBUTING.md) for guidelines.

---

## 🔐 Security

AnonVote is designed for transparency and auditability:

- **Open source** – All code publicly available
- **No black boxes** – Every component auditable
- **Formal privacy model** – Published whitepaper
- **Blockchain verified** – Independent verification possible

### Report a Vulnerability

Please email **security@anonvote.app** (do not post publicly)

---

## 📚 Documentation

- **[Core Docs](https://github.com/AnonVote/core/blob/main/README.md)** – Setup, API reference, deployment
- **[Crypto Docs](https://github.com/AnonVote/js/blob/main/README.md)** – Cryptographic primitives
- **[Contract Docs](https://github.com/AnonVote/contracts/blob/main/README.md)** – Smart contract guide
- **[Whitepaper](https://github.com/AnonVote/protocol/blob/main/WHITEPAPER.md)** – Formal specification

---

## 📊 Project Status

| Component           | Status              | Notes                        |
| ------------------- | ------------------- | ---------------------------- |
| Backend API         | ✅ Production Ready | Fully tested, deployed       |
| Frontend            | ✅ Production Ready | Responsive, accessible       |
| Stellar Integration | ✅ Active           | manageData audit trail       |
| Soroban Contract    | ✅ Ready            | Written, awaiting deployment |
| Testnet Deployment  | ✅ Available        | Public testnet instance      |
| Mainnet Deployment  | 🔄 In Progress      | Pending security review      |

---

## ❓ FAQ

**Q: Do voters need an account?**  
A: No. Voters only need the ballot link and their identifier (email, employee ID, etc.).

**Q: Is my vote really private?**  
A: Yes. Cryptographically. Your identifier → hashed. Token → hashed. Vote → encrypted. No joins exist.

**Q: Can the admin see how I voted?**  
A: No. Votes are encrypted. Admins see only aggregate results.

**Q: How do I verify the results?**  
A: Check the Stellar transaction ID on [Stellar Expert](https://stellar.expert). Query the Soroban contract. Independent verification is the whole point.

**Q: Can I self-host AnonVote?**  
A: Yes! It's open source. See [DEPLOYMENT.md](https://github.com/AnonVote/core/blob/main/DEPLOYMENT.md).

**Q: Is it free?**  
A: Yes, open source MIT licensed. Only costs are Stellar testnet (free) or mainnet (tiny XLM fees).

---

## 🌟 Built on Stellar

AnonVote uses the **Stellar blockchain** for:

- ⛓️ Immutable audit trails
- 🔐 Decentralized verification
- 💰 Low-cost transactions
- 🚀 High throughput
- 🌐 Global network

[Learn more about Stellar](https://stellar.org)

---

## 📞 Contact & Community

- **Discord** – [Join our community](#)
- **Email** – hello@anonvote.app
- **Security** – security@anonvote.app
- **Issues** – [GitHub Issues](https://github.com/AnonVote/core/issues)
- **Discussions** – [GitHub Discussions](https://github.com/AnonVote/core/discussions)

---

## 📄 License

AnonVote is **MIT licensed** – free to use, modify, and distribute.

[View License](LICENSE)

---

<div align="center">

### Made with ❤️ by the AnonVote community

**Privacy is a right, not a policy.**

[⭐ Star us on GitHub](https://github.com/AnonVote) | [🚀 Deploy Now](#) | [📖 Read Docs](https://github.com/AnonVote/core)

</div>
