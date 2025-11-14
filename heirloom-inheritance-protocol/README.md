# ARG25 Project Submission Template

Welcome to Invisible Garden- ARG25.

Each participant or team will maintain this README throughout the program.  
You'll update your progress weekly **in the same PR**, so mentors and reviewers can track your journey end-to

## Project Title : ### Heirloom Inheritance Protocol"

_A short, descriptive name of your project._
— A tool that allows people who wish to preserve cultural assets or secret knowledge to securely and permanently pass them down to others across generations.

## Team

- Team/Individual Name: Legacy Protocol
- GitHub Handles @cruujon, @DaroMacs
- Devfolio Handles: cruujon, daro_macs

## Problems in the Field and Product Value

### Field Situation

Currently, those who want to pass down their knowledge or skills have no choice but to share secret information directly—either orally or on paper.

There is no verifiable way to record _who passed it to whom_, which makes the extinction of such knowledge a real risk.

Traditional craftsmanship is disappearing globally, not only because knowledge is lost, but because its transmission is invisible to institutions and future generations. By anchoring these acts of transmission on-chain — with auditable records and public funding mechamism— we make cultural succession visible. This creates traceable evidence that can be used by local governments, museums, and preservation programs to recognize, fund, and protect these practices in a transparent and community-controlled way.

### Therefore, We Propose

By recording _who (wallet)_ has passed their knowledge to _whom (wallet)_ on the blockchain, we can preserve the lineage and history of these successions permanently.

At the same time, by using **client-side encryption** and **distributed storage** (e.g., IPFS), we enable private and secure inheritance of valuable information across generations—without making the contents public.

- Secret information can be inherited securely from one wallet address to another.
- As a result, traditional cultural assets and valuable private knowledge can be preserved and carried forward through time.

---

## Product Purpose (Purpose)

To make the process of inheriting personal knowledge and skills permanently traceable as a trustworthy record.

---

## Scope

✔ Recording the history of inheritance on-chain

✔ Encrypted storage of secret data off-chain

✔ Wallet-to-wallet handoff via **signed, encrypted payloads** (no fixed messaging stack required)

---

## Target Users

Individuals who wish to pass down their valuable private knowledge to the next generation **without making it public**.

Examples:

- A restaurant owner who possesses a secret recipe (a trade secret) but has no successor.
- A craftsman who holds local traditional techniques or special know-how that cannot be publicly shared.

---

## Functional Requirements

### Essential Features

- **Encrypted handoff**: grant a specific wallet address the ability to decrypt the secret and (optionally) pass it onward.
  - Mechanism: client-side symmetric encryption (e.g., AES-GCM) + **key wrap** to the successor’s public key (e.g., X25519/ECDH → AES-GCM key wrap).
  - Delivery: export a **signed JSON/QR/bundle** that the successor can receive via any secure channel (download link, QR scan, file transfer).
- **Distributed storage**: store the encrypted content on IPFS/Arweave; only a **CID/hash** is ever referenced on-chain.
- **Blockchain-based record**: record _who transferred inheritance rights to whom_ (lineage events) as immutable on-chain events.
  - Optional: represent current right-holder as an NFT for wallet visibility.

---

## MVP Success Criteria (Definition of Completion)

### Short-Term (Within the Hackathon) – Mandatory

- Record at least one **inheritance event** from one wallet to another on-chain and show the Tx hash.
- Produce and deliver a **signed, encrypted payload** (download or QR) from originator to successor, and **successfully decrypt** it on the successor’s device.
- A UI that clearly shows “inheritance completed” and renders the on-chain lineage (A → B) with timestamps.

### If Possible (Optional)

- Show an NFT in the successor’s wallet that represents the inherited right.

---

## Architecture

### Blockchain

- **Arbitrum** (testnet) → public ledger for inheritance records.
- **Registry / (optional) Rights NFT** smart contracts → represent and guarantee inheritance rights; emit `SecretRegistered` / `Inherited` events.

### Delivery Layer (no XMTP)

- **Signed, encrypted payloads** handed off via any channel (download URL with short expiry, QR code, secure file transfer, etc.).
- Payload includes: `{ secretId, cid, wrappedKey, senderSignature, createdAt }`.

### Storage

- **IPFS (Pinning service)** for encrypted content; on-chain stores only `cidHash`.
- (Optional) **Arweave** for long-term persistence.

---

## <<<<<<< HEAD

=======

> > > > > > > b927b64 (Update Legacy Protocol README content)

## Constraints

### Limitations (Out of Scope for This MVP)

- Strict, audited file-encryption UX (use well-known primitives but keep UX simple).
- Policy-based automatic re-encryption (e.g., PRE / Lit) — only mention as a future track.

### Assumptions

- A wallet address represents the intended individual successor.
- Secret data is encrypted client-side and never visible to the application backend or the blockchain.

---

## Minimal Contract Surface (for reference)

- `registerSecret(bytes32 cidHash, bytes meta) returns (uint256 secretId)`
- `inherit(uint256 secretId, address to)`
- `event SecretRegistered(uint256 indexed secretId, address indexed owner, bytes32 cidHash, uint256 time)`
- `event Inherited(uint256 indexed secretId, address indexed from, address indexed to, uint256 time)`

## User Flow

## User Flow (MVP)

### Flow 1 — Register a Secret (Originator)

1. **Encrypt** (client-side): generate `symKey (AES-GCM)`, then `ciphertext = Encrypt(symKey, plaintext)`.
2. **Store**: upload `ciphertext` to IPFS → get `cid`; compute `cidHash = keccak256(cid)`.
3. **Commit**: call `Registry.registerSecret(cidHash, meta)` → event `SecretRegistered(secretId, owner, cidHash, time)`.

### Flow 2 — Handoff & Lineage Record (Originator → Successor)

1. **Key wrap**: derive shared secret with successor’s public key (X25519/ECDH) and **wrap `symKey`** (AES-GCM).
2. **Bundle**: create a signed payload `{ secretId, cid, wrappedKey, senderSignature }`.
3. **Deliver**: show **QR** or provide a **download button** (the file can be transferred over any channel).
4. **On-chain lineage**: call `Registry.inherit(secretId, to=Successor)` → event `Inherited(secretId, from, to, time)`.

### Flow 3 — Receive & Decrypt (Successor)

1. **Import**: the successor loads the bundle (scan QR or upload the file).
2. **Unwrap**: decrypt `wrappedKey` with successor’s private key → recover `symKey`.
3. **Fetch & Decrypt**: pull `ciphertext` by `cid` from IPFS, `Decrypt(symKey, ciphertext)` locally.
4. **UI**: show “Decrypted ✅” and render the lineage `A → B` from events.

_What are the specific outcomes you aim to achieve by the end of ARG25?_

get to the grants and focus on consistent building and find a PMF

## Weekly Progress

### Week 1 (ends Oct 31)

**Goals:** team up & bouncing idea off

**Progress Summary:**
teamed up with @DaroMacs , @masaun

### Week 2 (ends Nov 7)

**Goals:** fix the whole product design and make a rough decision tech stack

**Progress Summary:**
Frontend MVP

Scaffold the Next.js + Wagmi + viem application and styling components.

Integrate wallet connection, transaction confirmation, and basic lineage visualization.

Simulate IPFS integration by allowing users to input placeholder CIDs.

Record one complete flow on testnet (registration → inheritance → lineage display).

Documentation

Add architecture diagram and stack summary to /docs/ARCHITECTURE.md.

Update README with contract address, tech stack, and project usage instructions.

Stack Overview

Blockchain: Arbitrum Sepolia (EVM-compatible)

Smart Contracts: Solidity, Arbitrum Stylus (for testing & deployment)

Frontend: Next.js 14, TypeScript, Wagmi, viem, shadcn/ui

Storage: IPFS (placeholder CID references for now)

Tooling: pnpm, dotenv, eslint/prettier

System Architecture (MVP)

<img width="379" height="408" alt="image" src="https://github.com/user-attachments/assets/2867107c-252e-4e49-91b5-1c2b46b94335" />

**Progress Summary:**
we fixed core tech stack and whole architecture to implement at invisible Garden. we already start buiding actual MVP

## Week 3 (ends Nov 14)

**Goals:**  
Complete the MVP development and deploy all components.

**Progress Summary:**

- Smart contract deployed to Arbitrum Sepolia.
- IPFS integration with actual encrypted blobs is live.
- Full end-to-end inheritance flow implemented:
  encrypt → upload → register → claim → decrypt.
- Users can now experience the full MVP on the live deployment.

# Next Steps (After Invisible Garden)

## Short Term

- Deploy to mainnet and expand across multiple L2s.
- Upgrade encryption model (e.g., migrate from PBKDF2 → ECDH-based key agreement).
- Integrate with EAS so other protocols can reuse inheritance lineage permissionlessly.

## Medium Term

- **AI Integration**

  - Automatically estimate cultural/economic importance scores for each inheritance.
  - Auto-tag inherited data for better discoverability.
  - Match inheritors and successors algorithmically.

- **Funding Mechanisms**
  - Integrate Gitcoin stack for donation and grant-based preservation funding.
  - Run funding rounds for cultural assets.
  - Collaborate with local governments and cultural institutions to test real-world deployments.

---

---

# Final Wrap-Up

### Deliverables

- Fully functional MVP
- On-chain contract
- Live frontend with complete user flows
- Encrypted inheritance mechanism
- Lineage visibility and basic UI

### Technical Outcomes

- Verified viability of client-side AES-256-GCM encryption + PBKDF2 key derivation tied to successor wallet address.
- Implemented a minimal yet secure pipeline combining IPFS, Ethereum smart contracts, and browser crypto.
- Identified areas for improvement (key rotation, ECDH upgrade, multi-layered permissions).

### Repository / MVP / DEMO

- **Repository:** https://github.com/Heirloom-Inheritance-Protocol
- **MVP page:** https://heirloom-inheritance-protocol.vercel.app
- **Slides:** https://www.figma.com/make/rSGqrMpI7cr1QmmQGiirqD/Create-Presentation-Material

---

# Learnings

During ARG25, we deepened our understanding of:

- Core Arbitrum Stylus concepts from the lecture series, including how Rust-based contracts interact with the Arbitrum toolchain.
- Security best practices around client-side encryption, emphasizing key derivation, storage minimization, and safe handling of encrypted payloads.
- Designing IPFS upload flows that keep the encrypted data decoupled from on-chain references while preserving traceability.
- Structuring inheritance journeys in the UI so wallet interactions, encryption steps, and status updates remain transparent to non-technical users.
- Coordinating smart contract events with frontend state to build reliable lineage timelines across deployments and testnets.

---

## Next Steps

- Craft an investor-facing narrative: package traction metrics, produce a concise pitch deck, and rehearse a scripted demo that highlights lineage tracking, encryption safeguards, and cultural impact.
- Line up showcase opportunities: schedule live walk-throughs for targeted angels, heritage-focused foundations, and web3 funds; capture demo recordings to share asynchronously.
- Build an organizational funding module: let verified organizations pledge capital to specific inheritance chains, surface on-chain proofs of contribution, and expose APIs for matching grants.
- Add treasury controls for funders: support multi-sig or role-gated wallets, transparency dashboards, and automated disbursement rules tied to verified lineage milestones.
