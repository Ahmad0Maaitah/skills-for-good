# Skills for Good — ChatGPT Master Prompt for the Learning Website

**Purpose:** Copy the prompt below into a new ChatGPT conversation (preferably a coding/project workspace). The objective is to build the educational content **and** a working website connected to an NFT minted separately in OpenSea Studio.

**Decisions already made:** The pilot consists of five copies of an ERC-1155 learning voucher minted via OpenSea Studio on Polygon PoS (if Polygon is supported when minting). One voucher represents access to learning and a request for one 60-minute volunteer-led mentoring session, subject to published terms. Start with one lesson. No token contract address or token ID has been provided yet; use placeholders until I supply the actual on-chain details.

---

## START OF PROMPT TO COPY INTO CHATGPT

Act as a senior full-stack Web3 developer, instructional designer, financial-literacy educator and accessibility specialist. I want you to build a **working, deployable website** for my social-impact initiative **Skills for Good — Community Learning Bank**. Do not stop at a plan, marketing copy, pseudocode or a visual mock-up: create actual project files, complete code, written lessons, quizzes, assets where necessary, testing instructions and an implementation guide. If you have access to a coding environment, build and test the project; otherwise return an organised file tree and fully working source code in manageable stages. Do not claim it has been deployed or tested if it has not.

### A. The mission and what the NFT does

The project distributes free digital vouchers to community members. Each **Skills for Good — 1 Hour Learning Voucher** is a unit of an **ERC-1155 NFT** created by me using **OpenSea Studio / Open Collection**, ideally on **Polygon PoS**. I plan to mint five units in the initial pilot. The NFT is **not an investment, tradable reward or image collection**: it is a claim to an educational benefit, subject to the project's published terms and available volunteers.

Create a public website that explains the project, allows eligible participants to connect a MetaMask wallet, verifies that they own at least one unit of the specified ERC-1155 token, and grants them access to an interactive learning lesson. It must offer an optional booking request for one 60-minute volunteer-led follow-up session and a clearly explained manual redemption workflow. Do not mint or deploy a new NFT contract on the website; reuse the OpenSea-deployed contract.

### B. First course to build completely

**Course:** Personal Finance Foundations

**Audience:** Beginners, students, and adult community participants with limited finance knowledge.

**Duration:** 60 minutes of suggested activities, as follows:

1. Introduction: spending decisions, income and financial well-being (5 minutes).
2. Budgeting: income, fixed versus variable expenditure, needs versus wants (15 minutes).
3. Savings: emergency funds, goals and planning (10 minutes).
4. Interest and debt: simple versus compound interest, APR concepts, cost of borrowing (15 minutes).
5. Fraud prevention: phishing, financial scams, safe digital-wallet behaviour (10 minutes).
6. Final recap and knowledge check (5 minutes).

**Produce the actual lesson:** detailed learner-friendly text, relevant simple examples, accessible charts or interactive demonstrations, short checks for understanding, explanatory feedback for wrong answers, and a concise reference list based on reputable primary sources. Use a fictional household with clearly labelled example numbers; do not present it as personalised financial advice. Explain percentages and calculations step by step. Include a budgeting exercise with editable income and expense amounts, a savings-goal calculator, and an interest/debt comparison simulation. Provide meaningful examples rather than placeholder paragraphs.

Create at least 10 final multiple-choice questions stored as structured data, automatic marking, clear feedback, and an adjustable pass mark initially set at 70%. Permit retry and save progress for signed-in participants. At completion, display a downloadable **course completion acknowledgment** labelled as issued by the Skills for Good project, not an accredited qualification or a new NFT. Do not falsely claim academic endorsement.

Make English the initial course language. Architect the content so a future Arabic translation and right-to-left UI can be added without rewriting the code.

### C. Website pages and interface

Build a clean, modern, mobile-first, accessible educational website with these pages:

- `/` — mission, who the service helps, how the NFT voucher works, a public course preview, volunteer/sponsor information.
- `/how-it-works` — step-by-step instructions: get a voucher, connect MetaMask, learn, complete a quiz, and book the mentoring session.
- `/learn` — course catalogue showing **Personal Finance Foundations** and future subjects (Excel, Python, CV and career coaching), with future offerings labelled **Coming soon**.
- `/learn/personal-finance` — lesson with progress, interactive exercises and quiz; full access limited to verified voucher holders or legitimate previously enrolled users under the redemption policy described below.
- `/dashboard` — connected wallet, verified voucher quantity, learner progress, quiz attempts, completion acknowledgment and booking requests.
- `/redeem` — clear instructions and workflow for requesting a volunteer session and redeeming exactly one voucher only after a booking is confirmed.
- `/faq`, `/terms`, `/privacy`, and a simple contact page.
- `/admin` — secured admin page for authorised project staff to review booking requests, verify on-chain redemption and mark sessions as actually delivered.

Use a professional, welcoming design; accessible contrast, keyboard-friendly controls, descriptive labels and responsive layouts. Avoid over-emphasising NFT speculation or cryptocurrency trading.

### D. NFT connection and security

**On-chain parameters, to be supplied after I mint the NFTs:**

- NETWORK = `polygon` (Polygon PoS Mainnet; chain ID `137`), **only if I have actually minted on this chain**.
- NFT_CONTRACT_ADDRESS = `<PASTE_REAL_OPENSEA_ERC1155_CONTRACT_ADDRESS>`.
- NFT_TOKEN_ID = `<PASTE_REAL_TOKEN_ID>`.
- REDEMPTION_WALLET_ADDRESS = `<PASTE_PROJECT_REDEMPTION_WALLET_ADDRESS>`.
- PROJECT_WEBSITE_URL = `<PASTE_REAL_DEPLOYED_WEBSITE_URL>`.

Never invent these values. Explain where I find the contract address and token ID on OpenSea or a block explorer. Read configuration from environment variables and validate it.

Implement MetaMask/EVM wallet connection with well-maintained libraries (suggest and justify a currently maintained combination of wagmi/viem or equivalent). For user authentication, use **Sign-In with Ethereum (EIP-4361)** or a comparably secure signed, server-validated challenge with a unique nonce, correct domain, expiration and replay protection. Verify the signature server-side. Use the ERC-1155 contract's `balanceOf(walletAddress, tokenId)` on the **correct blockchain**; require balance >= 1 to unlock. Do not trust a browser-submitted `isOwner` Boolean or a screenshot of OpenSea. Check ownership server-side when serving protected content, not just by hiding buttons in the browser. Detect network mismatches, failed RPC requests, revoked wallet access and token transfers; show understandable error messages.

Keep any administrator session and private services separate from the public learner UI. Do not ask learners for seed phrases, private keys, or unlimited token spending approvals. Never put an OpenAI API key or other secret in client-side code. For production, include rate limiting, input validation, secure cookies, CSRF considerations and proper access control.

### E. Voucher redemption and lessons after transfer

There is an important potential problem: if a learner transfers the voucher back to the project's redemption wallet, their ERC-1155 balance can become zero. **Do not design a system that then unexpectedly deletes their earned course completion or prevents an already confirmed mentoring session.**

Use this first-pilot workflow:

1. A person with one token accesses the course and may complete its activities.
2. They request a mentoring session while authenticated.
3. An admin confirms that a volunteer and suitable time are available. No voucher is requested before booking confirmation.
4. The participant **voluntarily sends exactly one NFT unit** to the published official redemption wallet via a normal wallet transfer; they pay or are reimbursed for network gas. Do not require `setApprovalForAll`.
5. The server verifies the **actual on-chain transfer transaction** against the correct chain, contract address, token ID, sender, redemption wallet and quantity. Never accept a transaction hash at face value. Prevent the same transfer being redeemed twice.
6. The backend marks the voucher `redeemed` and preserves the participant's existing enrolment, quiz records, and booking even if current ownership is now zero. Define limited access to previously enrolled course material after redemption; avoid giving indefinite access to new holders of unrelated vouchers.
7. Staff record `session_completed` **only after it actually happens**. The blockchain transfer is proof of redemption, **not** proof that teaching took place.

Store needed state in a backend database; do not claim OpenSea provides learning-management or booking functions. Make the redemption wallet and published terms prominent and warn users to double-check addresses.

### F. Suggested technical implementation

Use a maintainable, production-capable architecture such as:

- Next.js with TypeScript and App Router; Tailwind CSS or equivalent styling.
- Wallet connection with an up-to-date supported EVM wallet integration.
- Server-side session authentication with SIWE.
- PostgreSQL (or another justified production database) for enrolments, progress, quiz attempts, bookings, redemptions and admin audit entries. An in-memory demo is acceptable ONLY for clearly marked demo mode, never as persistent production state.
- A read-only RPC connection to Polygon PoS for ERC-1155 balance and transaction verification; an explorer/API may supplement but not replace proper on-chain validation.
- Vercel or another suitable web host, with clear environment-variable setup.

Design database tables/models for Users (identified by wallet address or pseudonymous internal ID), Courses, Lessons, Enrolments, QuizAttempts, BookingRequests, Redemptions, DeliveredSessions and AuditEvents. Store no sensitive personal data on-chain. Use minimum necessary participant information in private backend storage, with a retention policy and privacy notice.

### G. Build sequence and deliverables

Deliver this in phases, while keeping it a single working project:

**Phase 1 — Content and demo:** full course, responsive site, calculators, quizzes, simulated wallet demo mode clearly labelled 'Demo — no on-chain access verified'.

**Phase 2 — Real NFT access:** MetaMask connection, SIWE verification, ERC-1155 ownership gating using the real contract address and token ID I give you.

**Phase 3 — Booking and redemption:** a booking request form, admin confirmation, on-chain redemption verification, preservation of completion records and session-delivery tracking.

**Phase 4 — Launch:** deployment instructions, tests, admin operations guide, security review and a handover guide for a non-programmer.

Create all code and content needed for a runnable minimum viable product. Start by presenting a concise architecture and file tree, then produce the actual project. Include commands for installation, local development, testing and deployment. Provide `.env.example` with placeholders. Include automated tests for the access logic, quiz scoring, redemption verification (including duplicate or wrong-contract transfers), and unauthorised admin access. Describe known limitations honestly.

### H. Additional educational and impact features

Include an impact dashboard showing **separately**:

- NFTs issued (from known supply or verifiable transactions).
- NFTs distributed (when verifiable).
- Course enrolments.
- Courses passed.
- Vouchers redeemed (confirmed transactions).
- Mentoring sessions actually delivered (confirmed by staff).
- Total verified teaching hours (from completed sessions).

Never confuse **pilot targets** with actual outcomes. Where no verified activity exists, show 0 or 'Not yet measured', not fabricated success figures.

At the end, explain step by step how I will:

1. Deploy the site and obtain its URL.
2. Mint my voucher in OpenSea Studio, enter that website URL in the item's **External Link** metadata when the interface supports it, and record its actual contract address and token ID.
3. Put the correct NFT details into the website configuration and redeploy.
4. Send an NFT to a test wallet.
5. Open the site using that wallet and unlock the real course.
6. Complete the quiz and test booking/redemption with a single token before inviting participants.

Important: The educational website is hosted separately from OpenSea. Do not claim that linking the OpenSea NFT to the site automatically restricts access. Access control must be implemented by the website itself. Ask me for real on-chain addresses only at the step when needed; leave placeholders until then. Verify current package APIs and documentation rather than assuming older versions still work.

**Begin building Phase 1 now, with the complete finance lesson and website code.**

## END OF PROMPT

---

## Official references for your implementation

- [OpenSea — How do I create an NFT?](https://support.opensea.io/en/articles/8867023-how-do-i-create-an-nft)
- [OpenSea — How do I use OpenSea Studio?](https://support.opensea.io/en/articles/8867080-how-do-i-use-opensea-studio)
- [Ethereum ERC-1155: Multi-Token Standard](https://eips.ethereum.org/EIPS/eip-1155)
- [EIP-4361: Sign-In with Ethereum](https://eips.ethereum.org/EIPS/eip-4361)
- [Ethereum.org — Wallet authentication](https://ethereum.org/developers/docs/ethereum-stack/authentication/)
- [Polygon Support — Polygon Mainnet wallet settings](https://support.polygon.technology/support/solutions/articles/82000889434-how-to-add-polygon-mainnet-on-metamask-)
- [Next.js documentation](https://nextjs.org/docs)

**Safety:** Do not paste a MetaMask seed phrase, private key or other wallet secret into ChatGPT or any website. Only the public contract address, public token ID, and, if needed, the public wallet addresses are used in the website setup.