# Architecture Debate: Tuttle Store Stack

**Topic 1: Medusa v2 Readiness**
*   **Skeptic:** "Medusa v2 is still in beta/early release. Using it for a production store requiring high stability is risky. The module API has changed drastically."
*   **Proponent:** "v2 brings the *native* Regions and Subscription support we listed as requirements. Building those in v1 would be custom tech debt. We need the modularity for the 'Shipping Proxy' (NFR15)."
*   **Verdict:** **Stick with Medusa v2.** The architectural benefits (Modules) outweigh the beta risk for a new build.

**Topic 2: Air-Gapped Verification (The Mechanism)**
*   **WebUSB:** "Too invasive, requires drivers, browser incompatibility." -> **Discarded.**
*   **QR Code (Keystone style):** "High friction but zero-trust. Best for 'Air-Gapped' claim." -> **Approved.**
*   **File Upload:** "Easier but user might upload the wrong file." -> **Backup option.**
*   **Verdict:** **QR Code Handshake is Primary.** PWA must implement a QR scanner (e.g., `react-qr-reader`).

**Topic 3: Next.js 16 (React 19) for Security**
*   **Skeptic:** "RSC (React Server Components) by default might accidentally render sensitive data on the server that we wanted client-side only (Zero-Knowledge)."
*   **Proponent:** "We can enforce `"use client"` for all Vault/Crypto components. Next.js 16's performance is non-negotiable for the 'Aurora Day' UX."
*   **Verdict:** **Next.js 16 Approved**, but with a **Strict Mandate**: All "Vault" and "Crypto" logic must be in isolated client-side packages.

**Proposed Updates to PRD:**
1.  **Tech Constraint:** Explicitly ban Server-Side Rendering (SSR) for the "Vault" and "Crypto" modules. They must be Client-Side Only (CSR).
2.  **Library Choice:** Specify `react-qr-reader` (or equivalent) for the PWA Verification tool.

**Do you accept these architectural refinements?**
(Y/N/Select)