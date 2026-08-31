# Agent & developer access model

**Version 0.1 · August 2026**

**Principle:** Generate value first. Ask for money last.

Related: [artifact-plan.md](artifact-plan.md) · [architecture.md](architecture.md) · [competitive/luel-buyer-access.md](competitive/luel-buyer-access.md)

---

## Problem

Closed data marketplaces (Luel **Request a dataset**, custom collection scoping, sales follow-up) optimize for **enterprise procurement**, not **robot developer velocity** or **autonomous agent commerce**.

SHOW is an open protocol: robotic developers and agents should reach **useful information in seconds**, with payment only when they need **full rights + full bytes** for production.

---

## Access ladder (always in this order)

```
DISCOVER → PREVIEW → EVALUATE → LICENSE → DEPLOY
   free      free      free        pay       pay
```

| Stage | What the agent gets | Auth | Payment |
| --- | --- | --- | --- |
| **DISCOVER** | Taxonomy JSON, opportunity feed, catalog index, scores, sidecar metadata, `SKILL.md` | None | Free |
| **PREVIEW** | Gold example clips, low-res or watermarked bytes, 3 s trim, sidecar fields | None | Free |
| **EVALUATE** | Compare clips by `skill_id`, score, duration, environment_class; stack micro-primitives | None | Free |
| **LICENSE** | Full-resolution bytes, commercial use flag, on-chain license receipt | Wallet | USDC / x402 |
| **DEPLOY** | TaskEscrow attach, lineage registration, residual routing | Wallet | Job GMV |

**Rule:** Nothing in DISCOVER / PREVIEW / EVALUATE may require API keys, work email, sales calls, or “request access” forms.

---

## What is always public (Phase 0+)

Ship in repo and pin on Tack — no login. **Published via [public surface](public-surface.md)** (not this private repo):

| Asset | Path / surface | Agent use |
| --- | --- | --- |
| Skill taxonomy | `schemas/skill-taxonomy-atomic.json` | Match task → atomic skills |
| Opportunity feed | `schemas/opportunity-feed-v1.json` | Supply signal + bounty hints |
| Sidecar schema | `schemas/skillclip-v1.schema.json` (P0) | Validate before license |
| Catalog index | `catalog/sprint-1/index.json` (P0) | List minted clips |
| Eval scores | On-chain events + JSON mirror | Shop by lift |
| Bootstrap | `SKILL.md`, `llms.txt` | One-shot agent onboarding |
| Example clips | Tack CIDs linked from catalog | Prompt engineering |
| Contract ABIs | `contracts/abis/` | License script |

---

## What requires payment (last step)

| Asset | Gate | Why pay exists |
| --- | --- | --- |
| Full-resolution clip bytes | x402 or `LicenseStream` USDC pull | Creator + protocol economics |
| Commercial redistribution | License tier flag | Rights scope |
| Private modalities (depth, IMU) | Separate x402 quote | Capture cost premium |
| TaskEscrow settlement | Job GMV | Operator + lineage residuals |

**Preview must be enough** to answer: *“Will this clip work in my model context?”* — before any wallet connect.

---

## Developer UX targets

| Metric | Luel (observed) | SHOW target |
| --- | --- | --- |
| Time to first useful metadata | Hours–days (form → sales) | **<10 s** (`curl` taxonomy) |
| Time to preview a clip | Request sample clips | **<30 s** (public CID or embed) |
| Time to license | Contract negotiation | **<60 s** (wallet + one tx) |
| Agent-autonomous path | No | **Yes** — `SKILL.md` → script |

### Phase 0 copy-paste paths (no app required)

```bash
# Discover — zero auth (public surface URL when live; not raw GitHub while repo is private)
curl -s https://show.protocol/schemas/skill-taxonomy-atomic.json

# Catalog (when live)
curl -s https://show.protocol/catalog/sprint-1/index.json

# License — wallet last
# See SKILL.md → cast send LicenseStream ...
```

---

## Agent services (target)

HTTP + MCP tools — **read tools unauthenticated**, **write tools wallet-signed**:

| Tool | Auth | Payment |
| --- | --- | --- |
| `search_clips(skill_id, min_score)` | None | Free |
| `get_clip_metadata(token_id)` | None | Free |
| `get_preview_bytes(token_id)` | None | Free |
| `get_sidecar(token_id)` | None | Free |
| `license_clip(token_id)` | Wallet | USDC |
| `get_full_bytes(token_id, license_proof)` | Bearer / tx proof | Included in license |
| `post_bounty(skill_id)` | Wallet | Escrow deposit |

---

## Contrast with “request a dataset”

| | Luel-style | SHOW |
| --- | --- | --- |
| Entry | Web form → human | Public JSON + `SKILL.md` |
| Unit | Dataset / campaign | **Atomic SkillClip** |
| Preview | By request | **Always on** |
| Price | Quote after scoping | **On-chain** list price or x402 header |
| Agent | Blocked at form | **First-class** |
| Compliance | Delivered as PDF pack | Sidecar + C2PA digest + license event |

SHOW still supports **bulk license** (agent licenses 50 atomics in one session) — but each clip remains individually discoverable and purchasable without a services engagement.

---

## Strategic constraints (locked)

1. **No sales gate for read path** — ever, for catalog metadata and previews.  
2. **Wallet only at LICENSE** — not at browse, not at evaluate.  
3. **AEO before app** — if it isn’t in git + `SKILL.md`, Phase 0 isn’t done.  
4. **Enterprise bulk** — optional wrapper (invoice, MSA) **on top of** open API, not instead of it.

---

## Phase 0 checklist

| # | Item | Status |
| --- | --- | --- |
| A0.1 | This access model doc | Done |
| A0.2 | Public taxonomy + opportunity feed in repo | Done |
| A0.3 | `catalog/sprint-1/index.json` with preview CIDs | Not started |
| A0.4 | `SKILL.md` with license one-liner | Queued (P0.6) |
| A0.5 | One gold preview clip pinned (no wallet) | Not started |
| A0.6 | License script (pay last) | Queued (P1.6) |

---

## Changelog

| Date | Notes |
| --- | --- |
| 2026-08-30 | Value-first access ladder; counter to Luel request-a-dataset flow |
