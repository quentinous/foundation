# Stakeholder Round Table: The "Client-Side Only" Conflict

**Participants:**
*   **Amelia (Dev/Security):** "We cannot SSR the Vault. If the Master Seed touches the server memory even for a millisecond during rendering, we fail NFR1. It must be `useEffect` only, running after hydration."
*   **Sally (UX Designer):** "But `useEffect` means a flash of empty content or a loading spinner on every page load! We promised 'Aurora Day' fluidity (NFR6). A jarring layout shift destroys the premium feel."

**The Negotiation:**
*   **Amelia:** "We can't compromise on Zero-Knowledge. It's the core product."
*   **Sally:** "We can compromise on the *visual* handling. We don't need to show a spinner. We can show a 'Skeleton State' that looks like the encrypted vault, then animate the decryption once the JS loads."

**The Solution:**
1.  **Architecture:** The "Vault" component is wrapped in a `NoSsr` HOC (Next.js dynamic import with `ssr: false`).
2.  **UX:** The `fallback` for the `NoSsr` component is a **High-Fidelity Skeleton** that matches the design perfectly, avoiding layout shift.
3.  **Animation:** When the client-side logic hydrates and decrypts the vault (unlock), we play a subtle "Unlock Animation" (part of the Ritual Design) instead of a snap-change.

**Proposed PRD Refinement:**
*   **NFR6 Update:** Explicitly mandate **"Skeleton-First Loading"** for all Client-Side Only components to prevent layout shifts (CLS) despite the strict security architecture.

**Do you accept this refinement?**
(Y/N/Select)