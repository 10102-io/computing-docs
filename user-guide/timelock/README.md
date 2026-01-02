# Timelock

A time-based security layer to protect self-custody EOA wallets from wrench attacks, coercion, and other forms of compromise or premature access.

The feature comes with 3 flexible configurations for different use cases:

## Timelock

This will lock the funds until a specified date. The owner then can claim back the funds.

## Soft Timelock

This will lock the funds for an unlimited amount of time, and require a pre-configured waiting period after the owner initiate an unlock, safeguarding funds even if private keys are compromised. Once the waiting period is over, the owner can claim back the funds.

## Timelocked Gift

Timelocked Gift allows scheduled fund delivery to another wallet - ideal for scheduled payments, trust-fund style disbursements, or planned giving. When the locked period is over, the designated recipient wallet can claim the funds.
