# Agent Context: computing-docs

GitBook space for docs.10102.io. Sibling of `computing`, `computing-sc`, `computing-admin`, `computing-subgraph`; read `computing/AGENTS.md` for the system-wide context.

## Style rules (founder preferences; do not regress)

- **No em dashes, at all.** The em dash character (U+2014) is banned from prose, headings, frontmatter descriptions, tables, and mermaid labels (founder preference, tightened 2026-08-07 from "no pairs" to "none"). Use a colon for bullet labels (`- **Term**: definition`), and commas, colons, semicolons, parentheses, or separate sentences in prose. Check by grepping for the U+2014 character before committing: zero matches expected, in this file too.
- Plain, confident, honest register. State limits out loud ("Honest limits", "the worst case is…"). No marketing fluff.
- Facts must match what is deployed on mainnet. When in doubt, verify against the live contracts / frontend code before writing. Don't document UI that doesn't exist yet; say "rolling out" or "via Etherscan today".
- Mermaid diagrams render natively in GitBook code blocks; use them for flows worth a picture.
- GitBook syntax: `{% hint style="info|warning|success" %}` blocks, frontmatter `description:`, `SUMMARY.md` is the nav; every new page needs an entry.

## Git hygiene

- **One commit per day** (same convention as the other repos): amend/squash same-day work rather than stacking commits.
- Subject style: `docs: item one, item two` (lowercase type, `, ` separators, no scope parentheses).
