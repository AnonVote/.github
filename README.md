# AnonVote

**Private decision infrastructure for organizations on Stellar.**

AnonVote is a privacy-preserving voting platform that separates voter identity from ballots at every layer — cryptographically, not by policy. Results are anchored to the Stellar blockchain so anyone can verify outcomes without trusting AnonVote's servers.

---

## Why AnonVote?

- **Structural Privacy** – Voter identifiers are hashed before storage. Tokens are cryptographically unlinked from votes. No database join between who voted and what they voted.
- **Cryptographic Guarantees** – Not policies. Every privacy guarantee is enforced by mathematics, not trust.
- **Blockchain Verified** – Every ballot, token issuance, and vote is recorded on Stellar. Independent verification without trusting AnonVote's servers.
- **Multiple Voting Schemes** – Standard, weighted, ranked-choice, and delegated voting.
- **Open Source** – MIT licensed. Self-host for free. Transparent codebase.

---

## Key Features

✓ **Anonymous Voting** – No link between voter identity and ballot choice  
✓ **Cryptographic Separation** – SHA-256 hashing + AES-256-GCM encryption  
✓ **Blockchain Anchoring** – Stellar manageData + Soroban smart contracts  
✓ **Flexible Voting** – Weighted, ranked-choice, delegated voting modes  
✓ **Audit Trail** – Immutable event log on-chain for independent verification  
✓ **Email Token Delivery** – One-time tokens sent via email  
✓ **Result Verification** – Vote consistency checks (tokens_issued == votes_cast)  
✓ **No Setup Required** – Voters don't need accounts or passwords

---

## Voting Modes Supported

| Mode              | Description                                      |
| ----------------- | ------------------------------------------------ |
| **Standard**      | One token, one vote per voter                    |
| **Weighted**      | Tokens can have vote weight > 1                  |
| **Ranked-Choice** | Voters rank options in preference order          |
| **Delegated**     | Voters can delegate votes to other token holders |

---

## Tech Stack

### Frontend

- **React 18** + Vite
- **TypeScript**
- **Tailwind CSS**
- **Radix UI Components**
- **Axios** for HTTP

### Backend

- **Express.js** 4.18
- **Node.js** + TypeScript
- **Prisma ORM** 5.10
- **PostgreSQL** database
- **JWT** authentication
- **stellar-sdk** v12

### Smart Contracts

- **Rust** + Soroban SDK
- **WebAssembly** (wasm32-unknown-unknown)
- **Stellar** blockchain
- **M-of-N governance** for critical operations

### Crypto

- **SHA-256** for identifier/token hashing
- **AES-256-GCM** for vote encryption
- **CSPRNG** for token generation

---

## Architecture

```
┌─────────────────────────────────────┐
│     React Frontend (Vite)           │
│  Ballot creation, voting, results   │
└────────────┬────────────────────────┘
             │ REST API
             ▼
┌─────────────────────────────────────┐
│   Express Backend (TypeScript)      │
│ • BallotEngine                      │
│ • IdentityManager (token issuance)  │
│ • PrivacyEngine (vote encryption)   │
│ • ResultEngine (tally & publish)    │
└────────────┬────────────────────────┘
             │
     ┌───────┴───────┐
     ▼               ▼
┌─────────────┐  ┌──────────────────┐
│ PostgreSQL  │  │ Stellar Blockchain
│             │  │
│ • Ballots   │  │ ┌──────────────────┐
│ • Votes     │  │ │ Soroban Contract │
│ • Tokens    │  │ │                  │
│ • Results   │  │ │ • Audit trail    │
│ • AuditLog  │  │ │ • M-of-N ops     │
│             │  │ │ • Verification   │
└─────────────┘  └──────────────────┘
```

---

## How It Works

### 1. Organization Creates Ballot

- Admin creates a ballot with options, deadline, eligible voters
- Ballot is recorded on Stellar blockchain

### 2. Voter Gets Token

- Voter enters their identifier (email, employee ID, etc.)
- System checks eligibility (identifier hash matches list)
- One-time token is generated (32 bytes, cryptographically random)
- Token is sent via email; **original never stored**
- Only token hash is persisted in database

### 3. Voter Casts Vote

- Voter uses token to submit their vote choice
- Vote is encrypted with AES-256-GCM
- Encrypted payload stored; raw vote option never persisted
- Vote recorded on Stellar blockchain

### 4. Admin Closes & Tallies

- Admin closes ballot at deadline
- System decrypts all votes and counts them
- Result is published to PostgreSQL + Stellar + Soroban contract
- Consistency check: tokens_issued == votes_cast

### 5. Public Verification

- Anyone can query Stellar blockchain for audit trail
- Anyone can verify result hash on Soroban contract
- No need to trust AnonVote's servers

---

## Repositories

| Repo                                                   | Purpose                                                          |
| ------------------------------------------------------ | ---------------------------------------------------------------- |
| **[core](https://github.com/AnonVote/core)**           | Express backend + React frontend + Prisma + PostgreSQL           |
| **[js](https://github.com/AnonVote/js)**               | `@anonvote/crypto` — shared crypto primitives + TypeScript types |
| **[contracts](https://github.com/AnonVote/contracts)** | Soroban smart contracts (Rust) + service wrappers (TypeScript)   |
| **[docs](https://github.com/AnonVote/docs)**           | Whitepaper, specs, security analysis, token flow diagrams        |

---

## Privacy & Security

### Privacy Guarantees

**Voter Identity Separation:**

```
Identifier → SHA-256(trimmed & lowercased) → identifierHash
Only identifierHash stored in DB; original never saved
```

**Token Unlinkability:**

```
generateToken() → 256-bit CSPRNG → raw token (given to voter)
raw token → SHA-256() → tokenHash (stored in DB only)
No way to link token to voter identity
```

**Vote Encryption:**

```
Vote option → AES-256-GCM(key) → ciphertext + iv + authTag
Raw vote never stored; only encrypted payload persisted
Decryption only during tally
```

**On-Chain:**

- No voter identifiers
- No token values
- No vote content
- Only counts and hashes

### Security Features

✓ **JWT Authentication** – Org-level access control  
✓ **Rate Limiting** – Prevent brute-force token requests  
✓ **Duplicate Detection** – Track failed attempts without storing identifier  
✓ **Consistency Verification** – Tokens issued == votes cast  
✓ **M-of-N Governance** – Critical ops require distinct approvals on-chain  
✓ **48-hour Upgrade Timelock** – Soroban contract prevents sudden changes  
✓ **Audit Trail** – Immutable event log on Stellar

---

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+
- Rust (for contract development)
- Stellar CLI (for blockchain interaction)

### Local Development

```bash
# Clone repositories
git clone https://github.com/AnonVote/core.git
git clone https://github.com/AnonVote/js.git
git clone https://github.com/AnonVote/contracts.git

# Setup backend
cd core/backend
npm install
cp .env.example .env
npm run prisma:migrate
npm run dev

# Setup frontend
cd ../frontend
npm install
npm run dev

# Visit http://localhost:5173
```

### Environment Variables

Required in `backend/.env`:

```
DATABASE_URL=postgresql://user:pass@localhost:5432/anonvote
JWT_SECRET=<64-char-min-random-string>
STELLAR_SECRET_KEY=<your-stellar-keypair-secret>
STELLAR_NETWORK=testnet
BALLOT_ENCRYPTION_KEY=<64-char-hex-string>
RESEND_API_KEY=<your-resend-email-api-key>
SOROBAN_CONTRACT_ID=<deployed-contract-id>
```

---

## Deployment

### Testnet

```bash
# Deploy contracts
cd contracts
cargo build --target wasm32-unknown-unknown --release
stellar contract deploy --wasm target/wasm32-unknown-unknown/release/anonvote.wasm --network testnet

# Set SOROBAN_CONTRACT_ID in backend/.env
# Deploy backend (Heroku, Railway, etc.)
# Deploy frontend (Vercel, Netlify, etc.)
```

### Mainnet

Same process, but use `--network public` and real Stellar keypairs with XLM funds.

---

## Contributing

We welcome contributions! Areas for help:

- **Frontend** – React components, UX improvements
- **Backend** – API enhancements, performance optimization
- **Smart Contracts** – Rust contract improvements, security audits
- **Crypto** – `@anonvote/crypto` package enhancements
- **Documentation** – Specs, deployment guides, tutorials
- **Testing** – Unit tests, integration tests, property-based tests
- **Security** – Vulnerability reports (security@anonvote.app)

See [CONTRIBUTING.md](https://github.com/AnonVote/core/blob/main/CONTRIBUTING.md) for details.

---

## Security & Audits

AnonVote is open source and transparent. No security audits yet, but the codebase is designed for auditability:

- Cryptographic primitives isolated in `@anonvote/crypto`
- Smart contract written in Rust with formal verification capabilities
- Extensive test coverage planned
- Public audit-friendly architecture

**To Report a Vulnerability:** security@anonvote.app

---

## FAQ

**Q: Do voters need an account?**  
A: No. Voters only need the ballot link and their identifier (email, employee ID, etc.). No registration, no password.

**Q: How is voter privacy guaranteed?**  
A: Structurally. Identifier → hash. Token → hash (raw never stored). Vote → encrypted. No database join between identity and ballot.

**Q: What if a voter loses their token?**  
A: They can request a new one if they haven't voted yet. Once voted, no new token is issued.

**Q: How are results verified?**  
A: Every event is on Stellar. Transaction IDs are public. Anyone can verify using Stellar Expert or the Soroban contract read calls.

**Q: Can the admin see how people voted?**  
A: No. Votes are encrypted. Admins see aggregate results only — never individual votes.

**Q: Is AnonVote free?**  
A: Yes. It's open source MIT licensed. Only cost is Stellar testnet (free) or mainnet (tiny XLM fees).

**Q: Can I self-host AnonVote?**  
A: Yes. See [DEPLOYMENT.md](https://github.com/AnonVote/core/blob/main/DEPLOYMENT.md) for full instructions.

---

## License

[MIT License](LICENSE) – Free to use, modify, and distribute.

---

## Support

- **Documentation** → https://docs.anonvote.app
- **Discord** → https://discord.gg/anonvote
- **GitHub Issues** → Report bugs or request features
- **Email** → hello@anonvote.app

---

## Built on Stellar

AnonVote leverages the Stellar blockchain for immutable audit trails and decentralized governance. Stellar provides:

- Fast, low-cost transactions
- High throughput
- Native multi-signature support
- Smart contract capability via Soroban
- Network of validators (no single point of failure)

[Learn more about Stellar](https://stellar.org)

---

**Made with ❤️ by the AnonVote community. Privacy is a right, not a policy.**
