---
description: >-
  Seal ETH or USDC as a gift that opens on a date you choose. The recipient
  opens it with a link, and claiming is free.
---

# Heirloom (sealed gifts)

Heirloom seals value for someone until a date that matters: a Bar or Bat
Mitzvah, a wedding, a birth, an 18th birthday. The gift is held by the same
audited timelock contract the rest of the app uses. Until the opening date
nobody can take it out, including you.

The thing you send is a gift. Heirloom is the product that seals it.

## Sending a sealed gift

Open **Heirloom** in the top menu (or the **Send a sealed gift** button on
the Timelock tab) and fill one card:

1. **Occasion**: Bar/Bat Mitzvah, Wedding, New baby, Birthday, Graduation,
   Hanukkah, Christmas, Eid, or Someday. The occasion sets the design of
   the printable sheets and suggests a message. It also proposes a fitting
   unlock horizon (five years for most occasions, eighteen for a new baby).
2. **To**: three ways to address it.
   - **Their wallet**: paste the recipient's wallet address.
   - **Just their email**: type the recipient's email. The gift is sealed
     to a wallet that only that email can open (see "Email gifts" below).
     Sealing asks for one extra signature, the wallet lookup, before the
     transaction.
   - **A printed card**: the app generates a fresh wallet for the gift
     and you hand its key over on paper. The paper is the gift.
3. **Amount**: ETH, or USDC for stable value. Both are escrowed on
   sealing; the choice changes what the recipient receives, not how the
   seal works.
4. **Unlocks**: a preset horizon or an exact date. The date is final and
   can never be shortened.
5. **Message (optional)**: stays off the blockchain. It travels inside the
   claim link, prints on the paper's cover (first two lines), and shows on
   the gift page when the link is opened.

Sealing sends one transaction (two for USDC: an exact-amount approval,
then the seal). The gift cannot be recalled or changed after it confirms,
not even by you.

{% hint style="info" %}
Some wallets show a caution while our escrow contract is still young. This
is a reputation flag on a new contract, not a finding. Rather than
ignoring any warning, verify: the contract address your wallet shows must
match the published list in the public repository
(`contract-addresses.json`).
{% endhint %}

## The claim link is the gift

Sealing produces one link, shaped like
`app.10102.io/heirloom/1/42#g=...`. The part after `#` carries the names
and your message. Browsers never send that part to any server, ours
included, so the personal words stay between the two of you.

- Anyone with the link can SEE the gift: who it is from, what it holds,
  when it opens.
- Only the recipient's wallet can TAKE it, and taking it is free (see
  claiming below).
- Share it by copy, WhatsApp, email, or the printed card's QR code.

The seal panel can also email the link for you, with a timing choice:

- **Right away**: "a gift is waiting" today, plus a reminder on opening
  day.
- **On a day you pick**: nothing today; the announcement goes out on the
  day you choose (the birthday, the wedding), and the opening-day
  reminder still follows.
- **On opening day**: full surprise; one email when the gift opens.

You sign one free message to authorize it (the signing prompt shows only
a fingerprint, never the email). The address is stored encrypted and
deleted once its job is done. The same email panel lives on the gift's
detail page under Timelocks, so you can add or correct an email any time
after sealing.

The names, message, and occasion are also kept on the device that sealed
the gift (never on a server), so the gift's detail page on that device can
rebuild the full link and the printed card later.

## The printables

- **The key sheet** (paper-key gifts): one paper holds the whole gift. The
  occasion cover faces front and carries a big scannable code at the
  heart of the design: scanning it opens the gift page, with the names,
  the message, and the countdown. "How to open your gift" faces back, and
  the secret key is sealed inside the fold, clearly marked THE KEY, NOT A
  WEB LINK. Print single-sided and fold on the printed dashed lines.
  Whoever holds this paper controls the gift: never photograph it open.
  The cover is safe to show; the inside is not.
- **The gift card** (wallet-address gifts): the digital greeting for gifts
  sent to an existing wallet. Occasion design, who it is for, the opening
  date, your message in quotes, and a QR of the claim link. Safe to share
  and photograph; it contains no key material and never shows the amount.
  Paper-key gifts don't need it: their sheet already carries everything.
- **The gift record**: a factual document for the giver, downloadable
  from the gift's detail page. It leads with the fair market value on the
  sealing date, then dates, amounts, shortened wallet addresses, the
  escrow contract, and the sealing transaction as a scannable code and
  link for direct verification on the public ledger. It is not tax advice
  and says so; hand it to your advisor.

## Claiming (free for the recipient)

On opening day the recipient opens the link (or scans the paper's front
again) and claims:

- **With the paper alone**: the gift page asks for the key folded inside
  the sheet. Scan it with the camera or type it in; the key signs the
  claim on the recipient's own device and never leaves it. No wallet app,
  no account, nothing to install.
- **With their email** (email gifts): the gift page asks for the email
  the gift was sent to, then for the six-digit code that arrives in that
  inbox. Typing the code is the whole login. The wallet behind the email
  signs the claim and 10102 submits it.
- **With a wallet**: connect the wallet the gift was sealed for and sign
  one free message.
- **Straight to your own wallet**: on the claim page you can paste the
  address of a wallet you already use, and the gift lands there in the
  same free transaction. The choice is part of the message the gift's
  key signs, so nobody else can change where it goes. We recommend it for
  paper and email gifts: the printed or email key is then used exactly
  once and never holds the funds. Leave the field empty and the gift goes
  to the gift's own wallet, as before.
- Whichever way, claiming costs the recipient nothing: 10102 submits the
  transaction and pays the network fee, and the contract pays out only to
  the on-chain recipient or to the wallet that recipient named in the
  signed claim.
- If the sponsored path is ever unavailable, a wallet holder can submit
  the withdrawal self-paid, and a paper holder can import the key into a
  wallet app (MetaMask: "Import account") and do the same.
- Before opening day, the gift page shows a live countdown to the second,
  can add the opening day to Google Calendar or any calendar app, and can
  be shared or copied with one tap. Nothing is sent to our servers.
- After claiming, the page becomes a small keepsake: sealed date, opened
  date, and an invitation to seal one forward.

## Email gifts

"Just their email" exists for the recipient who has no wallet and should
not need one: a grandparent, a child, a friend who has never touched
crypto. Here is exactly what happens, so you can decide if it fits.

- At sealing time we ask Turnkey, an embedded-wallet provider, for a
  wallet tied to that email. If none exists, one is created. The gift is
  sealed to that wallet's address, on-chain, like any other gift.
- The wallet's private key lives inside Turnkey's secure hardware
  enclaves. Nobody, including 10102, can read it. It signs only after the
  email's owner proves it is them with the code, and 10102 holds no
  credential that could move the funds.
- On opening day the recipient enters the email and the code, and the
  wallet signs the claim. The claim page asks where the gift should land:
  paste the address of a wallet the recipient already uses and the funds
  go there directly, in the same free transaction. Left empty, the funds
  stay in the email wallet, and the recipient can log in again on the
  gift page any time. For gifts claimed to the email wallet before this
  option existed, write to info@10102.io and we walk the recipient
  through moving them.
- **The honest trade**: this is a step away from pure self-custody. The
  recipient depends on their email account and on Turnkey's service to
  reach the key, where the paper gift and the wallet gift depend on
  nothing but the key holder. If that trade is not right for your
  recipient, use the printed card.
- Double-check the spelling. The gift can only be opened with the exact
  email you typed; a typo seals it to a wallet nobody will ever log into,
  and it cannot be recalled.

## Honest limits

- **Irrevocable**: once sealed, no one can cancel, shorten, or redirect a
  gift. Triple-check the recipient address, or email, and the date.
- **Paper is a bearer instrument**: for paper-key gifts, losing the key
  sheet before opening day loses the gift, and anyone who photographs the
  open sheet can take it. The sheet says this on the sheet.
- **The link shows the gift**: treat the claim link like a greeting card,
  not like a password. It cannot move funds, but it does reveal names,
  message, and amounts to whoever holds it.
- **Sponsored claims are capped**: gas sponsorship is bounded by daily
  caps to keep the feature abuse-proof. If the day's budget is exhausted,
  claiming still works self-paid, or free again the next day.
