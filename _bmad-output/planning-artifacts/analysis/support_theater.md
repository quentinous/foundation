# Customer Support Theater: The "Lost" Cash

**The Scene:**
*   **User_X:** "I sent €150 to your PO Box in Geneva! My tracking says delivered yesterday. Why is my order cancelled?? SCAM!"
*   **Support Bot (AI):** "I understand you are frustrated about order #1234. It was cancelled due to non-payment."
*   **User_X:** "I PAID! Talk to a human!"

**The Failure:**
1.  **Workflow Gap:** The "Cash-by-Mail" / "Western Union" workflow has a *Manual Confirmation* step that is prone to human delay.
2.  **System Gap:** The auto-cancellation timer (Medusa default: 24h?) is too short for physical mail/transfers.
3.  **Support Gap:** The AI chatbot doesn't know the *real-time status* of the PO Box checks.

**The Fixes:**
1.  **Extended Timeout:** Orders with "Manual Payment" method must have a **7-day expiry** (not 24h).
2.  **"Payment Declared" State:** User must be able to click "I have sent the payment" in the Dashboard. This creates a **"Payment Pending Review"** state that *pauses* the cancellation timer.
3.  **Proof of Payment Upload:** Allow user to upload a photo of the Western Union receipt / Tracking number to the Dashboard. This gives Support immediate evidence to override the cancellation.

**Proposed PRD Refinements:**
1.  **FR29 (Alternative Payments) Update:**
    *   Mandate **7-day expiry** for manual payments.
    *   Add **"Payment Declared"** button in Dashboard (pauses timer).
    *   Add **"Proof Upload"** capability (Receipt photo).

**Do you want to add these support workflows?**
(Y/N/Select)