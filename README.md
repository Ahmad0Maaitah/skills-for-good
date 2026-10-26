# Skills for Good — Interactive NFT Learning Website

**Working classroom-ready static prototype · English / Arabic · Polygon ERC-1155 support**

This website turns a *Skills for Good — One Hour of Learning* NFT into a demonstrable utility voucher. The front end contains an interactive personal-finance course that learners can preview, an instructor demonstration mode, and optional wallet-based ERC-1155 ownership verification.

## What is included

- Fully responsive landing page with a social-good project explanation.
- **Six authored teaching modules (English and Arabic)** covering money basics, budgeting, saving and compound growth, borrowing, scams, and a final exam.
- Three working calculators: monthly budget, hypothetical compound growth, and amortising-loan repayment.
- Interactive scenario checks and **10 automatically scored multiple-choice questions**.
- Printable classroom **completion record** for results at or above 70% (not an accredited certificate).
- MetaMask browser-wallet connection with ERC-1155 `balanceOf(address,uint256)` verification on Polygon PoS mainnet (`chainId` 137).
- Instructor demo mode for teaching before any NFTs are minted.
- Optional browser-local instructor settings for contract address and token ID.
- OpenSea-ready sample media file: `nft-voucher.png` (1200 × 1200 pixels).

## Try it immediately

Open `index.html` in a desktop browser. Click **Try instructor demo** to unlock all six sections with no wallet or blockchain costs. Switch to **العربية** in the header to demonstrate the same lessons in Arabic. Your lesson progress is saved in this browser only.

The website requires no API key and has no paid dependencies. For testing browser wallets, **publish it under HTTPS** or use a localhost development server because wallet injection and network policies vary by browser.

## Publish as a real website on GitHub Pages

1. Sign in to GitHub and create a new **public repository**, e.g. `skills-for-good`.
2. Unzip the downloaded package and **upload all the files together into the repository root**. Do not upload only the ZIP.
3. Open **Settings → Pages → Build and deployment → Source: Deploy from a branch**.
4. Choose **`main`**, folder **`/(root)`**, and save.
5. After GitHub Pages finishes publishing, open the URL shown under **Settings → Pages → Visit site**. For a conventional project repository, the URL format will usually be `https://YOUR_GITHUB_USERNAME.github.io/skills-for-good/`.
6. Test the teacher demo, Arabic language switch, calculators, final quiz, and printable record on the published site.

Official documentation: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

## Create the related NFT with OpenSea Studio

1. Open **https://opensea.io/studio** and connect your wallet.
2. Select **Open Collection → Create Collection**. Choose **Polygon PoS** if available; confirm network and fees before deploying.
3. Contract name: `Skills for Good — Learning Bank`; token symbol: `SFG`.
4. Upload `nft-voucher.png` as your NFT's media.
5. NFT name: `Skills for Good — One Hour of Learning`; supply: `5` for the classroom pilot.
6. **External link:** use your now-public **GitHub Pages** website URL.
7. Add this description:

> One Skills for Good learning voucher, designed to provide access to the Personal Finance Foundations educational course and—subject to published eligibility, availability and redemption rules—one hour of volunteer-led learning or mentoring. This NFT is a utility voucher, not an investment or a promise of financial returns. Service delivery is confirmed separately from any blockchain transfer.

8. Complete the mint in your wallet. Copy the **ERC-1155 collection contract address** and the exact **token ID** of the voucher from the NFT's details or block explorer. They are not interchangeable.

Official OpenSea NFT creation guide: https://support.opensea.io/en/articles/8867023-how-do-i-create-an-nft

## Link your minted NFT to the website

Open `site-config.js` and replace the two **empty strings**:

```js
window.SFG_CONFIG = Object.freeze({
  chainId: 137,
  contractAddress: "0xYOUR_REAL_COLLECTION_CONTRACT_ADDRESS",
  tokenId: "YOUR_REAL_TOKEN_ID",
  openSeaUrl: "https://opensea.io/studio"
});
```

Commit and republish the updated file to GitHub Pages. With MetaMask on Polygon, a visitor can click **Connect wallet** to request their wallet address and check their exact ERC-1155 balance on chain. If the balance is at least 1, the demo unlocks the remaining modules. **The first introduction module is deliberately public.**

Teacher convenience: the **Teacher NFT setup** button in the footer can temporarily override these two fields in your own browser. This does **not** change the published configuration for students.

How ownership is checked: `eth_call` to the NFT contract with the ERC-1155 `balanceOf(address,uint256)` signature. The wallet is asked to switch to Polygon (chain ID 137) if necessary. Only read-only blockchain calls are made; the website does not request approval to transfer NFTs and does not request seed phrases or wallet secrets.

## A 10-minute classroom demonstration

1. Open the live URL and show the social mission and NFT utility model.
2. Click **Try instructor demo**. Open the budgeting lesson and change the income and expenses.
3. Open the savings lesson and compare growth with different monthly deposits.
4. Open the borrowing lesson and show how interest and repayment length change monthly costs.
5. Open the scam lesson and answer the safe-choice question.
6. Complete the ten-question test; demonstrate the practice completion record.
7. Switch to Arabic and show how content and quiz translate.
8. Explain that OpenSea mints the token while **this website supplies the actual educational utility**.
9. Once minted, connect a wallet holding the real NFT and verify live token ownership.

## Important limitations / what is not included

- **Not yet publicly deployed by this package.** You must publish it to an HTTPS host to obtain a real link for OpenSea. The locally opened site is not an internet URL.
- **No NFT exists yet.** The website is initially in teaching demo mode until a real contract address and token ID are configured.
- **Browser-only client-side gating is not secure against technically skilled visitors.** All static lesson content is downloadable and the instructor demo can be enabled by anyone. For true private content or monetised access, add a backend with challenge/response wallet authentication (e.g. SIWE), session management and server-side NFT verification.
- **Not a full redemption system.** NFT-to-service redemption, booking, volunteer management, and proof of completed mentoring would require backend implementation and policies. Do not transfer NFTs to a redemption wallet without publishing explicit service terms and confirming appointments.
- **No learner database or formal certification.** Progress and quiz answers live only in each user's browser storage and can be edited/reset. The printed learning record is not verified evidence of attendance or any accredited qualification.
- **No collection of personal data.** No registration form, addresses, name storage in a database, or analytics. Optional name entered on the print record remains local.
- **No financial recommendations.** Financial numbers are simplified illustrations, not predictions or offers.
- Future course cards are intentionally marked **Coming soon**; only Personal Finance Foundations is complete.
- The project figures (5 vouchers, 5 learners, 5 hours) are **pilot targets**, never reported achievements.
- Google Fonts are requested remotely; local fallbacks are provided if they cannot load.

## File guide

| File | Purpose |
|---|---|
| `index.html` | Site content, navigation, learning hub layout |
| `styles.css` | Responsive design + mobile and print layouts |
| `app.js` | Lessons, translations, calculations, assessment, wallet read-only checks |
| `site-config.js` | Two NFT settings to update after minting |
| `nft-voucher.png` | Optional PNG NFT media for uploading to OpenSea Studio |
| `favicon.svg` | Brand favicon |
| `.nojekyll` | Prevents Jekyll processing on GitHub Pages |

## Trusted technical sources

- OpenSea: https://support.opensea.io/en/articles/8867023-how-do-i-create-an-nft
- ERC-1155 standard: https://eips.ethereum.org/EIPS/eip-1155
- OpenZeppelin ERC-1155 balanceOf: https://docs.openzeppelin.com/contracts/5.x/erc1155
- MetaMask developer docs: https://docs.metamask.io/quickstart/
- GitHub Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

For questions about this prototype, retain this README alongside the source code in your repository.


## Complete blueprint and AI development prompts

The **full OpenSea-to-website-to-lesson blueprint** and both ready-to-paste ChatGPT/Claude prompts are in [docs/MASTER_BLUEPRINT.md](docs/MASTER_BLUEPRINT.md). The original standalone full-stack prompt is at [docs/CHATGPT_WEBSITE_PROMPT.md](docs/CHATGPT_WEBSITE_PROMPT.md).

**Separate repository rule:** publish this project only in a dedicated `skills-for-good` GitHub repository, not as a folder within another course or personal website repo. The currently documented repository URL and Pages URL are *intended addresses* until a repository is created and deployed.