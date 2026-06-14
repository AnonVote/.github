<div align="center">

<h1>AnonVote</h1>

<p><strong>Private decision infrastructure for organizations on Stellar.</strong></p>

<p>
  Run secure, anonymous votes where participation is confidential,<br/>
  results are verifiable, and records are tamper-proof on the blockchain.
</p>

<p>
  <a href="https://github.com/AnonVote/core"><img src="https://img.shields.io/badge/core-backend%20%2B%20frontend-0d1117?style=flat-square&logo=github" alt="core" /></a>
  <a href="https://github.com/AnonVote/js"><img src="https://img.shields.io/badge/%40anonvote%2Fcrypto-npm%20package-cb3837?style=flat-square&logo=npm" alt="js" /></a>
  <a href="https://github.com/AnonVote/contracts"><img src="https://img.shields.io/badge/contracts-Soroban%20%2F%20Stellar-6f42c1?style=flat-square&logo=rust" alt="contracts" /></a>
  <a href="https://github.com/AnonVote/protocol"><img src="https://img.shields.io/badge/protocol-whitepaper%20%2B%20specs-0284c7?style=flat-square&logo=gitbook" alt="protocol" /></a>
  <img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT" />
</p>

</div>

---

## What is AnonVote?

AnonVote solves a fundamental problem with digital voting: most tools expose voter identity, store results on a server only you control, and provide no independent way to verify outcomes.

AnonVote separates identity from the ballot at every layer — cryptographically, not by policy. Every vote is anchored to the Stellar blockchain so results can be verified by anyone, without trusting AnonVote's servers.

| Property             | How it's enforced                                                       |
| -------------------- | ----------------------------------------------------------------------- |
| One person, one vote | Cryptographic token system, not policy                                  |
| Voter anonymity      | Structural unlinkability — no DB join between identity and token tables |
| Result integrity     | AES-256-GCM encrypted votes anchored to Stellar                         |
| Public auditability  | Stellar transaction IDs surfaced on every result page                   |

---

## Repositories

### [`core`](https://github.com/AnonVote/core)

The main application — Express backend, React frontend, Prisma + PostgreSQL, and Stellar integration. This is where the product lives.

### [`js`](https://github.com/AnonVote/js)

The `@anonvote/crypto` npm package. Zero-dependency cryptographic primitives and shared TypeScript types used across the ecosystem: `hashIdentifier`, `generateToken`, `hashToken`, `encryptVote`, `decryptVote`.

### [`contracts`](https://github.com/AnonVote/contracts)

Soroban smart contracts written in Rust. Records immutable ballot, token, vote, and result events on-chain with public read access. Includes a TypeScript service stub ready to wire into `core`.

### [`protocol`](https://github.com/AnonVote/protocol)

Whitepaper, cryptographic specs, token flow diagrams, and API surface documentation. The reference for auditors, contributors, and anyone evaluating AnonVote's security properties.

---

## How it works

```
Eligible Voter List
       │
       ▼
 Identity Manager ──► Anonymous Token  (one-time, 32-byte CSPRNG)
       │
       ▼
  Vote Submission ──► Encrypted Vote   (AES-256-GCM)
       │
       ▼
 Stellar Blockchain ──► Immutable Audit Trail
       │
       ▼
  Result Engine ──► Public Verified Results
```

Even with full database access it is computationally infeasible to link a vote back to an individual voter. The privacy model is structural.

---

## Tech stack

| Layer           | Technology                          |
| --------------- | ----------------------------------- |
| Frontend        | React 18, Vite, TailwindCSS         |
| Backend         | Node.js 20, Express, TypeScript     |
| Database        | PostgreSQL 15 + Prisma ORM          |
| Blockchain      | Stellar SDK v12 (Testnet / Mainnet) |
| Smart contracts | Soroban SDK 21 (Rust)               |
| Crypto          | AES-256-GCM, SHA-256, CSPRNG tokens |
| Auth            | JWT via HTTP-only cookies, bcrypt   |
| Email           | Resend                              |

---

## Contributing

Pull requests are welcome across all repos. For significant changes, open an issue in the relevant repository first.

- Bug reports and feature requests → [`core`](https://github.com/AnonVote/core/issues)
- Crypto / type questions → [`js`](https://github.com/AnonVote/js/issues)
- Contract changes → [`contracts`](https://github.com/AnonVote/contracts/issues)
- Protocol / spec discussion → [`protocol`](https://github.com/AnonVote/protocol/issues)

---

<div align="center">
  <sub>Built with ♥ on Stellar · MIT License</sub>
</div>
