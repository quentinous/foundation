# 5 Whys: Why Medusa v2?

**1. Why use Medusa v2?**
Because we need a headless commerce backend that is customizable.

**2. Why do we need it to be so customizable?**
Because we have unique "Sovereign" requirements: Shipping Proxy, Crypto Payments, and Zero-Knowledge architecture that standard platforms (Shopify) block or make impossible.

**3. Why not build a custom backend (Go/Node)?**
Because building an *entire* OMS (Order Management System), Inventory, and Admin panel from scratch is a massive effort that diverts focus from our core value prop (Privacy/Hardware).

**4. Why specifically Medusa v2 (Beta) and not v1 (Stable)?**
Because v2 introduces the **Module Architecture**. In v1, hacking the "Shipping Proxy" would require forking core services. In v2, we can swap the Fulfillment Module with our own "Proxy Module" cleanly.

**5. Why is Module swapping critical now?**
Because "Antifragility" (Shark Tank goal) requires us to be able to swap Payment, Fulfillment, and Auth providers *instantly* without rewriting the core. Medusa v2 is the only JS framework designed for this level of modularity.

**Root Cause:**
We chose Medusa v2 because its **Modular Architecture** is the only viable path to achieve **Antifragility** (Swappable Infrastructure) while avoiding the technical debt of a custom build.

**Conclusion:**
The choice is valid and aligns with the strategic "State-Resistant" goal. No PRD change needed, but the *rationale* should be documented in the Architecture.

**Action:**
Add a "Technology Justification" note to the Project Scope or Architecture section to permanently record this decision.

**Do you want to add this justification?**
(Y/N/Select)