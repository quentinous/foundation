# Pre-mortem Analysis: Tuttle Store (2027)

**Scenario:** It is January 2027. Tuttle Store launched, gained traction, but has now collapsed. The community feels betrayed, and the "State-Resistant" brand is dead.

**Why did we fail?**

1.  **The "Shipping Leak" Scandal (Data Privacy Failure)**
    *   **Cause:** A user's physical address (from a hardware order) was leaked and correlated with their VPN activity.
    *   **Why:** Despite "No-IP Logging", the *shipping label generation API* (third-party carrier integration) stored metadata we couldn't purge. We deleted our DB, but DHL/FedEx kept the API logs linking "Order ID" to "Customer Name".
    *   **Mitigation:** We need strict isolation of carrier data integration.

2.  **The "Firmware Update" Brick (Operations Failure)**
    *   **Cause:** A mandatory firmware update for the Tuttle Key bricked 15% of devices.
    *   **Why:** The update mechanism relied on a central server which was DDoS'd during the rollout. Users couldn't verify the signature, forced the update, and corrupted the bootloader.
    *   **Mitigation:** Decentralized firmware distribution (IPFS/Torrent) and dual-boot safety partitions on the hardware.

3.  **Payment Processor "Shadow Ban" (Resilience Failure)**
    *   **Cause:** Stripe didn't just block us; they flagged our domain as "High Risk". This cascaded to our backup fiat provider (who uses the same Visa/Mastercard risk lists).
    *   **Why:** We relied on "Payment Failover" between two traditional providers. We didn't have a truly *uncensorable* fiat on-ramp (like Cash-by-Mail or Voucher Resellers).
    *   **Mitigation:** Add a "Voucher Code" payment method redeemable for credits, sold via third-party authorized resellers.

**Proposed Mitigations for PRD:**

1.  **Carrier Data Isolation (NFR Update):** "Shipping label generation must occur via a proxy service that strips internal Order IDs before communicating with Carrier APIs. Carrier labels must be generated using ephemeral references."
2.  **Robust Firmware Updates (FR Update):** "Firmware updates must support peer-to-peer distribution (IPFS) and the hardware must support A/B partition updates (Safe Fallback) to prevent bricking."
3.  **Reseller Vouchers (Payment Update):** "Add support for 'Tuttle Vouchers' (Gift Cards) to allow users to purchase credits from third-party resellers if direct payment channels are blocked."

**Do you want to add these mitigations to the PRD?**
(Y/N/Select)