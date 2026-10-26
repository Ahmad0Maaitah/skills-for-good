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