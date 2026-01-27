# SCAMPER: Innovating Canary & Referral

**1. SUBSTITUTE (The Referral Code)**
*   *Idea:* Substitute "Random Code" with "Master Seed Public Key Fragment".
*   *Result:* Too complex. Stick to simple codes but allow **Custom Aliases** (e.g., "TUTTLE-NEO").

**2. COMBINE (Canary + Blockchain)**
*   *Idea:* Combine the Warrant Canary with a **Bitcoin Transaction**.
*   *Innovation:* **"The Dead Man's Switch Transaction"**.
    *   Tuttle Store sets up a monthly recurring transaction.
    *   If the transaction *stops* (or sends to a specific "Duress Address"), the system automatically flags the Canary as RED on the Dashboard.
    *   *Why:* Harder to fake than a PDF. It's on-chain proof of liveness.

**3. ADAPT (Referral -> Resistance)**
*   *Idea:* Adapt the "Referral Program" into a **"Cell Structure"**.
*   *Innovation:* **"The Resistance Cell"**.
    *   A "Parrain" (Referrer) can create a "Cell" (Group).
    *   Members of the cell share a "Cell Status" widget.
    *   If the Parrain's account is compromised (or they trigger a "Panic Button"), the whole cell is alerted.

**4. REVERSE (Canary Logic)**
*   *Idea:* Instead of "I am safe", publish "I have been compromised".
*   *Result:* Standard Canary logic ("I am safe") is better because silence = danger.

**Proposed Innovations for PRD:**
1.  **FR23 (Canary) Upgrade:** Add **"On-Chain Liveness"** (Bitcoin OP_RETURN or simple Tx) as a secondary validation signal alongside the PGP PDF.
2.  **FR21 (Referral) Upgrade:** Rebrand as **"Resistance Cells"**. Allow Parrains to broadcast a "Cell Alert" (Panic) to their referees in case of local compromise.

**Do you want to add these innovations?**
(Y/N/Select)