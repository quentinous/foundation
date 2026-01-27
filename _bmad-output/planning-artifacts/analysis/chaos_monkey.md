# Chaos Monkey: Resilience Testing

**Scenario 1: The Lost Master Seed**
*   **Event:** User loses their physical paper backup of the Seed. They are logged in on the Dashboard.
*   **Current Behavior:** They can see their account, but can't generate new keys or sign messages.
*   **The Fail:** If they log out, they are locked out forever.
*   **Mitigation:** **"Emergency Export"**. While session is active, allow user to *re-export* the Master Seed (protected by a PIN/Password if set, or just re-display it) or trigger a "Key Rotation" to a new Seed immediately.

**Scenario 2: The Shipping Proxy Crash**
*   **Event:** The ephemeral Redis DB in the Shipping Proxy fills up or the service crashes during Black Friday.
*   **Current Behavior:** Medusa tries to ship, API fails. Orders get stuck in "Processing".
*   **The Fail:** If Medusa retries indefinitely without the Proxy, it might fallback to a default (unsafe) shipping method if misconfigured?
*   **Mitigation:** **"Fail-Safe Circuit Breaker"**. If Proxy is down, Medusa must **HALT** all label generation. It must *never* bypass the proxy. Queue orders until Proxy is back.

**Scenario 3: The Zombie Canary**
*   **Event:** Admin is on holiday and forgets to sign the monthly Canary. It expires by 1 hour.
*   **Current Behavior:** Dashboard Trust Monitor turns RED.
*   **The Fail:** Users panic. Mass exodus. "Tuttle is compromised!"
*   **Mitigation:** **"Grace Period & Pre-Alert"**.
    *   System emails Admin 7 days, 3 days, 24h before expiry.
    *   Dashboard shows "Amber/Warning" for 24h post-expiry before turning "Red/Compromised".

**Proposed PRD Refinements:**
1.  **FR13 (Identity) Update:** Add **"Seed Re-Export / Rotation"** capability for active sessions.
2.  **NFR15 (Proxy) Update:** Explicitly mandate a **"Fail-Closed"** architecture (never bypass proxy).
3.  **FR23 (Canary) Update:** Add **"Admin Alerting"** and a 24h **"Grace Period"** (Amber Status) logic.

**Do you want to add these resilience requirements?**
(Y/N/Select)