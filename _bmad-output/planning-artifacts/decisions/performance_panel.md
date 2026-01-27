# Performance Profiler Panel: Client-Side Crypto vs UX

**The Problem:**
*   **Frontend Specialist:** "Decrypting the Vault client-side is CPU intensive. If we do this on the main thread while hydrating Next.js and animating 'Aurora' particles, the UI will freeze. TBT (Total Blocking Time) will skyrocket, killing our Lighthouse score."
*   **DevOps:** "Plus, loading the full `openpgp.js` or `sodium-native` WASM bundle is heavy (>500KB)."

**The Analysis:**
1.  **Bundle Size:** We can't ship full crypto libs to the landing page.
2.  **Thread Blocking:** Decryption *must* happen off the main thread.

**The Solution:**
1.  **Web Workers:** All Vault decryption/encryption must run in a dedicated Web Worker. This keeps the UI thread free for animations (Aurora).
2.  **Lazy Loading:** Crypto libraries must be lazy-loaded *only* when the user clicks "Unlock Vault" or enters the Dashboard. They should not block the initial LCP (Largest Contentful Paint).
3.  **WASM Optimization:** Use Rust-compiled WASM for crypto operations (faster than pure JS).

**Proposed PRD Refinements:**
1.  **NFR Performance Update:** Explicitly mandate **"Web Worker Offloading"** for all cryptographic operations to prevent Main Thread blocking.
2.  **Tech Constraint:** Mandate **"Lazy-Loaded Crypto Modules"** to protect the initial load performance (Lighthouse).

**Do you want to add these performance mandates?**
(Y/N/Select)