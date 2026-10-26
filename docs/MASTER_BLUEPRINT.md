# Skills for Good — OpenSea Implementation Blueprint

**Status:** Proposed project, not yet deployed or verified in a wallet.  
**Pilot:** 5 free, one-hour learning vouchers; 5 adult community participants; 5 volunteer sessions.  
**Network:** Polygon PoS, provided it is offered in your OpenSea Studio account.  
**NFT standard:** ERC-1155, using **Open Collection** in OpenSea Studio.  
**As of:** 26 October 2026.

## 1. Purpose and success criteria

Skills for Good uses transferable blockchain vouchers to enable participants to redeem real educational services—not to sell digital pictures. Each voucher entitles its current holder to one 60-minute learning or mentoring session, subject to published eligibility, scheduling and expiration rules.

Pilot success criteria:
- A smart-contract collection deployed on the chosen blockchain.
- A voucher item minted with total supply **5**.
- Five units distributed to five participant wallets (one each).
- Participants can request, confirm and redeem their vouchers.
- Each redemption is recorded with a confirmed on-chain transfer to a designated redemption wallet.
- Five completed sessions are confirmed in an off-chain ledger; session completion is not automatically proven by the blockchain.

## 2. Architecture

`OpenSea Studio -> ERC-1155 contract -> organizer wallet (5 vouchers) -> participant wallets (1 voucher each) -> redemption wallet (redeemed voucher) -> volunteer session -> private impact log`

**OpenSea's role:** deployment, minting, public item page and NFT transfers.  
**Project team's role:** recruitment, capacity planning, eligibility, communication, booking, voucher redemption verification, volunteer delivery, safeguarding and impact reporting.

OpenSea Studio does **not** create a booking platform, verify tutoring occurred, or automatically prevent double redemption. A voucher sent to the redemption wallet must remain there and never be sent out again, and redemption transaction hashes must be marked used.

## 3. Required preparation

1. Use an official [MetaMask wallet](https://metamask.io/) connected to [OpenSea](https://opensea.io/). Keep seed phrases private.
2. Have sufficient gas currency for deployment, mint and transfers; on Polygon PoS, this is normally **POL**. Prices vary and should be checked before authorizing transactions.
3. Make two *separate* wallet accounts: `ISSUER_WALLET` for holding unissued vouchers and `REDEMPTION_WALLET` for collecting used vouchers. Use labels to prevent accidental transfer.
4. Arrange five real 60-minute volunteer sessions **before distributing vouchers**.
5. Decide the following and publish them publicly: who can redeem; supported topics; online/in-person format; where service is available; booking availability; exact expiration date; rescheduling and cancellation rules; whether resale or transfer is permitted; what happens if a volunteer cancels.
6. Prepare one simple square voucher icon or video. OpenSea Studio still requires media; the icon is incidental, not the product.
7. Create a project landing page (or simple publicly accessible page) with the redemption instructions. Do not post student names, contact details or personal histories on-chain.

## 4. Create the collection in OpenSea Studio

Go to [OpenSea Studio](https://opensea.io/studio), connect the issuer wallet, select **Create new -> Open Collection -> Create Collection**.

| Field | Proposed value |
| --- | --- |
| Collection logo | Simple Skills for Good mark |
| Contract name | `Skills for Good — Learning Bank` |
| Token symbol | `SFG` |
| Network | `Polygon` if available |
| Collection type | Open Collection / ERC-1155 |

Carefully review immutable details and the displayed gas estimate. Select **Publish Contract** and confirm in MetaMask. Save the deployed contract address and transaction hash.

## 5. Mint the voucher NFT

Within your collection go to **Media & Metadata**. Upload your basic voucher icon. Enter:

| Field | Proposed value |
| --- | --- |
| Item name | `Skills for Good — 1 Hour Learning Voucher` |
| Supply | `5` |
| Description | See below |
| External link | Your published project / redemption page |
| Trait: Project | `Skills for Good` |
| Trait: Utility | `Free learning session` |
| Trait: Duration | `60 minutes` |
| Trait: Redemption | `One-time transfer to project wallet` |

**Copy-ready NFT description:**

> Skills for Good — Community Learning Voucher. This NFT is a voucher for one free 60-minute educational or mentoring session delivered by a participating volunteer, subject to published eligibility, scheduling and availability rules. The current holder may request a session via the official project website. After receiving a booking confirmation, the holder redeems the voucher by transferring one NFT unit to the official redemption wallet. A voucher is valid for one redemption only. This NFT is not an investment, ownership in an organization, or a promise of financial returns. Read the official terms before accepting or transferring it.

Select **Mint**, review the network and gas cost, confirm in MetaMask. There should now be **one token type with five copies** under ERC-1155. This is not five independent ERC-721 tokens.

## 6. Publish the project information

Create a minimal project page before distributing NFTs with:
- Project name, mission, trusted organizer contact and volunteering partners.
- The exact public **contract address and blockchain** so participants can identify genuine NFTs.
- Topics: Financial Literacy 101; Spreadsheet Basics; CV & Interview Skills; Intro to Python.
- Duration: 60 minutes per NFT.
- Eligible participants and service delivery methods.
- Full expiration date and cancellations policy.
- **Official redemption wallet address** (do not publish a private key).
- A simple **request form** with private contact method, wallet address, selected topic and availability.
- A note that a request is **not** a confirmed booking.
- Instructions to transfer only after the team confirms a session.
- Explanation of blockchain visibility, wallet and gas costs, privacy, and no guaranteed NFT resale value.

A website can be built later with Claude. For the pilot, a simple page plus form and spreadsheet can suffice.

## 7. Distribute a voucher to each participant

1. Ask each participant privately for their **public** wallet address, not their recovery phrase.
2. On OpenSea, open **Profile -> NFTs** and select the voucher item (or open the voucher's item page).
3. Use the transfer / **Send** action. Choose **Add Wallet** if the address is new.
4. Enter the recipient address, choose **quantity 1** where shown, and click **Review Transfer**.
5. Confirm the transaction in your wallet, then record the transaction hash.
6. Repeat for all five participants.
7. Check each receiving address holds one NFT unit before treating it as issued. Some NFTs may not appear immediately in the wallet UI; use the on-chain explorer for authoritative confirmation.

Transfers are irreversible. Test the flow with one participant first. Confirm the interface supports the ERC-1155 quantity selection you need before making a batch of transfers.

## 8. Redeem a voucher and deliver education

**Simple, no-code operational design:**

1. Participant opens official project page and submits booking form with wallet address and topic.
2. Organizer verifies the submitted address currently holds at least **1 unit** of the correct token contract and token ID on the correct blockchain.
3. Organizer confirms an available volunteer and session, and privately sends booking confirmation.
4. Participant opens the original NFT page on OpenSea, clicks **Send**, and transfers **1 unit** to the published `REDEMPTION_WALLET`.
5. Organizer confirms the on-chain transfer went **from the participant wallet to the designated redemption wallet**, with correct contract, token ID and quantity.
6. Organizer records the transaction hash as **redeemed**, preventing it from being accepted twice, and leaves the redeemed voucher in the collection wallet.
7. Volunteer delivers the session.
8. Organizer records **completion** and impact (e.g., 60 minutes delivered). Never treat transfer alone as proof the lesson occurred.

**Cost consideration:** the participant usually needs a small gas balance to transfer the voucher on-chain. To avoid excluding participants without gas funds, the project may provide a small POL gas subsidy or do a supervised demonstration with funded practice wallets.

**Design limitation:** these ERC-1155 vouchers are normally transferable; do not claim they are non-transferable qualifications or identity credentials. If strict anti-resale or automated burning/redeeming is essential, commission and test a purpose-built smart contract and redemption application.

## 9. Off-chain redemption register

Keep this in a private spreadsheet, not in public NFT metadata.

| Record | Description |
| --- | --- |
| Voucher collection address | Your deployed contract address |
| Token ID | ID of the one-hour voucher |
| Participant wallet | Public address only |
| Distribution TX | Confirmed distribution transaction hash |
| Topic | Requested service |
| Booking status | Requested / Confirmed / Cancelled |
| Redemption TX | Confirmed transfer to redemption wallet |
| Redemption status | Unused / Redeemed |
| Session status | Not delivered / Delivered / Rescheduled |
| Minutes delivered | Typically 60; record actual minutes |
| Private contact record | Stored separately with restricted access |

Avoid putting recipients' names or personally identifying stories directly on a blockchain.

## 10. Project impact dashboard

Report at least:
- Vouchers minted: 5.
- Vouchers distributed: 0–5.
- Verified redemptions: 0–5.
- Sessions delivered: 0–5, verified separately.
- Total training minutes delivered.
- A voluntary, privacy-respecting learner feedback measure.

Do not label redeemed vouchers as completed sessions before the volunteer confirms delivery.

## 11. Pilot testing checklist

- [ ] Real volunteers and five time slots agreed.
- [ ] User-facing terms, eligibility and expiry published.
- [ ] Issuer and redemption wallets created and clearly labeled.
- [ ] OpenSea contract deployed; address recorded.
- [ ] One voucher token minted with supply exactly five.
- [ ] One test unit transferred and receipt confirmed.
- [ ] One participant can access the official booking page.
- [ ] Owner and token contract verified on chain.
- [ ] One redemption transfer completed to the correct wallet.
- [ ] Redemption transaction recorded and not reusable.
- [ ] One real learning session delivered and separately marked complete.
- [ ] Gas burden and wallet usability tested before launch to a wider group.

## 12. Future upgrades (not part of basic OpenSea Studio)

- Real-time wallet connection and signature-based login.
- Booking system integrating volunteer calendars.
- Automated verification of voucher balance and redemption status.
- Custom contract for burn-on-redeem or non-transferable service entitlements.
- Sponsored gas / account abstraction for participants.
- Anonymized social-impact dashboard with audit-friendly session evidence.
- Partnerships with NGOs, community centres and university outreach teams.

## 13. Official sources

- [OpenSea — How do I use OpenSea Studio?](https://support.opensea.io/en/articles/8867080-how-do-i-use-opensea-studio)
- [OpenSea — How do I create an NFT?](https://support.opensea.io/en/articles/8867023-how-do-i-create-an-nft)
- [OpenSea — How can I transfer an NFT using OpenSea?](https://support.opensea.io/en/articles/8866959-how-can-i-transfer-an-nft-using-opensea)
- [OpenSea — Who pays gas fees?](https://support.opensea.io/en/articles/8867014-who-pays-the-gas-fees-on-opensea)
- [OpenSea — Creator FAQ](https://support.opensea.io/en/articles/8867028-creator-faq)
- [MetaMask — ERC-1155 support](https://support.metamask.io/manage-crypto/nfts/does-metamask-support-erc-1155)
- [Polygon — Developer docs (POL for gas)](https://docs.polygon.technology/tools/dApp-development/common-tools/remix/)

> **Verification notice:** Official sources confirm Studio's creation and transfer workflows, but I cannot verify which networks and fields are displayed inside your connected wallet until you inspect them. No on-chain collection, wallet, site or volunteer capacity has been created by this document.


---

# Part II — Website and NFT Integration Blueprint

## 14. This project's deployable website

The actual Skills for Good teaching website is included in this repository. It is **not** a Canva mockup, OpenSea-hosted page, or a Next.js app. It is a static HTML/CSS/JavaScript website designed to run directly on GitHub Pages. The site includes:

- An explanatory project landing page and public first-lesson preview.
- Six bilingual (English/Arabic) personal-finance lesson sections.
- Interactive budgeting, savings-growth and borrowing calculators, scenario exercises, and a 10-question assessment.
- A local printable course-completion acknowledgement (not accredited certification).
- An instructor demo mode to demonstrate lessons *before* any NFT exists.
- Read-only MetaMask integration that checks the ERC-1155 `balanceOf(wallet, tokenId)` on Polygon PoS (chain ID `137`) using the contract address configured in `site-config.js`.
- `nft-voucher.png` as optional media when minting on OpenSea.

The **website can be live before the NFT is minted**. Copy its actual GitHub Pages public URL into the NFT's **External Link** field on OpenSea Studio. After minting, copy your *actual* Polygon ERC-1155 contract address and token ID into `site-config.js` in the repository root, and commit that update. Do **not** invent contract addresses or token IDs.

### New GitHub repository (standalone — do not modify existing teaching repositories)

Repository owner (connected account): `Ahmad0Maaitah`  
Requested new repository name: `skills-for-good`  
Public URL *after repository creation, file upload and Pages activation*: `https://ahmad0maaitah.github.io/skills-for-good/`  
Repository URL *after creation*: `https://github.com/Ahmad0Maaitah/skills-for-good`

**Status: the URLs above are targets, not a claim of deployment.** A brand-new repository must actually be created, files committed and GitHub Pages configured before those links become live.

To publish: create the new public repository, put `index.html`, `app.js`, `styles.css`, `site-config.js`, `favicon.svg`, `.nojekyll`, `nft-voucher.png`, `README.md` and the `docs/` folder at repository root, then select **Settings → Pages → Deploy from a branch → main → /(root)**. Finally open the Pages URL and test all lessons, the language selector and quiz.

Useful official reference: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

### Demo-versus-production security rule

This version teaches the NFT use case. **It is not secure commercial token-gated access.** The full lesson content is in public JavaScript, instructor demo is open, and progress is saved in the local browser. Real private access, persistent progress, student data, verified course achievements and redemption automation require a backend, signed wallet login, server-side chain verification, and a database. Do not describe the browser-demo gate as tamper-proof or claim the site issues accredited certificates.

### Suggested class sequence

1. Open the real GitHub Pages URL and demonstrate the educational service in instructor-demo mode.
2. Show students the public introduction lesson, bilingual switch, exercises, calculators and marked quiz.
3. Create the collection and voucher on OpenSea Studio. Add the live site URL to the NFT External Link field.
4. Update `site-config.js` with verified on-chain contract address and token ID.
5. Transfer one NFT voucher to a funded practice wallet and check that NFT ownership unlocks access.
6. Explain how service-redemption transfers differ from actually delivering a lesson.

---

# Part III — Copy-and-paste AI prompts for lesson and website creation

The following are **two distinct prompts** for ChatGPT or Claude. Prompt 1 expands the educational content. Prompt 2 extends the delivered website. Both respect the existing GitHub Pages/static-code starting point and OpenSea-minted NFT collection.

## 15. PROMPT 1 — Create or improve the actual lessons

> You are an expert financial-literacy educator, bilingual English/Arabic instructional designer, and front-end JavaScript engineer. Work on the **existing Skills for Good website repository** that I provide, rather than building an unrelated demo. Before editing, inspect `index.html`, `app.js`, `styles.css`, `site-config.js` and `README.md`. Preserve working pages, wallet verification, language switching and GitHub Pages support.
>
> Improve the existing **Personal Finance Foundations** 60-minute learning journey for complete beginners in six sections: (1) money and financial choices — 5 minutes; (2) budgeting and needs/wants — 15 minutes; (3) saving and financial goals — 10 minutes; (4) borrowing, interest and APR — 15 minutes; (5) fraud, phishing and digital-wallet safety — 10 minutes; (6) recap and ten-question quiz — 5 minutes. Provide complete English and Arabic lesson scripts, plain-language explanations, step-by-step worked calculations and culturally neutral fictional examples.
>
> Make the budgeting calculator, compound-growth calculator and loan-payment calculator mathematically correct, labelled as educational scenarios, responsive and accessible. Write useful instant feedback for each interactive scenario, including why an answer is wrong. Provide 10 varied quiz questions with four plausible options, automatic marking, a 70% practice pass criterion, explanations, retry and a printable **non-accredited** completion record. No paid services, API keys or teacher-only secrets in the client.
>
> Cite reputable primary references for financial topics (for example MoneyHelper, the UK FCA, UK National Cyber Security Centre and relevant regulator guidance); verify links. Do not give personalised financial advice, do not invent citations, and flag numerical examples as hypothetical. Maintain right-to-left layout for Arabic and keyboard/screen-reader usability.
>
> **Deliver changes as actual source files** in the existing static site, report exactly what you changed, and supply a manual test matrix. If a real backend is needed for a request, explain that boundary and do not fake persistent completion records or identity verification. Do not change the Polygon/ ERC-1155 configuration unless I provide verified contract information.

## 16. PROMPT 2 — Create, improve and deploy the website

> Act as a senior front-end engineer, Web3 engineer, educational UX designer and software tester. **Continue the attached Skills for Good website** rather than discarding or replacing it. It is a working static GitHub Pages project with `index.html`, `styles.css`, `app.js`, `site-config.js`, `favicon.svg`, `nft-voucher.png`, and `README.md`. My intended separate repository is `Ahmad0Maaitah/skills-for-good`. Check the actual repository exists before claiming deployment.
>
> Deliver a professional, fast and accessible English/Arabic website for **Skills for Good — Community Learning Bank**. Keep the public project explanation, instructor demo, six real educational sections, calculators, practice quizzes, printable acknowledgment, and responsive layout. Add clear How It Works, FAQ, terms, privacy and contact information (use transparent placeholders where policy facts have not been approved). Mark future courses **Coming soon** unless fully authored.
>
> Use the actual website to demonstrate how an NFT provides a real educational service: visitors can view a preview, connect MetaMask, and if holding the correct ERC-1155 token on the correct blockchain, unlock the lesson. Set Polygon PoS chain ID `137` **only if that is where I eventually mint**. Use `balanceOf(wallet, tokenId)` for the front-end demonstration. Never invent a smart contract address, token ID, redemption wallet or successful chain transaction; leave placeholders until supplied. Display clear states for no wallet, wrong chain, wallet rejection, network failure and NFT not owned. Never request a wallet seed phrase or token-transfer approval to view lessons.
>
> **Respect security boundaries:** the current GitHub Pages architecture is an educational demonstration, not secure paid-content protection. Tell me explicitly that static source is publicly readable, browser storage is not certified records, and wallet-connect alone is not robust identity authentication. For production access, propose a separate backend using signed nonce-based wallet authentication, server-side NFT verification, stored enrolment and transaction-verified one-time redemption, with appropriate privacy and security controls. Do not falsely claim this infrastructure is already deployed.
>
> Provide complete working source files, maintainable code, relevant source citations, automated/static checks when available and reproducible manual QA. Include GitHub Pages instructions using a **new standalone repository**, a correct public URL after deployment, and a deployment status check. Explain how to paste the real published URL into the **OpenSea NFT External Link** field and then enter contract address and token ID in `site-config.js`. Do not edit any of my unrelated repos.

---

# Part IV — Original detailed full-stack upgrade prompt

If you later want a **production LMS rather than the already-built GitHub Pages classroom demo**, use the more ambitious prompt below. It includes proper backend login, database, bookings and on-chain redemption validation. This is a **future upgrade**, not something already delivered by the static website.

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


---

## Implementation references

- GitHub: https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository
- GitHub Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
- ERC-1155 standard: https://eips.ethereum.org/EIPS/eip-1155
- EIP-4361 wallet sign-in: https://eips.ethereum.org/EIPS/eip-4361
- OpenSea Studio: https://support.opensea.io/en/articles/8867080-how-do-i-use-opensea-studio
- OpenSea minting: https://support.opensea.io/en/articles/8867023-how-do-i-create-an-nft