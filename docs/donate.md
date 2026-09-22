# Donate — thanks for helpful content

A **donate** is optional USDC sent because the content helped. It is **not** a license.

**Join:** [contributing.md](contributing.md) · **Machine:** [donate-v1.json](../schemas/donate-v1.json) · **Access ladder:** [agent-access-model.md](agent-access-model.md)

---

## What it is

Someone previewed a SkillClip, a policy, a recipe, or a named offer and found it useful. They may send USDC to the producer.

| Donate | License |
| --- | --- |
| Thanks | Rights to full bytes + commercial use |
| Any amount | Fixed `licenseFee` |
| 100% to creator | Creator / referrer / treasury split |
| No receipt that unlocks bytes | On-chain `ClipLicensed` receipt |

Preview stays free. Donating does **not** skip LICENSE. Agents must not auto-donate.

---

## This week (contracts not deployed)

If the producer published a `donate.address` (sidecar or `[SkillOffer]`), send **USDC** there. Chain is whatever they listed (`chain_id`, default Hoodi `167013` when they mint).

No address listed → nothing to send. Do not invent a protocol tip jar.

---

## When Hoodi `LicenseStream` is live

`donateClip(tokenId, amount)` pulls USDC from the donor and transfers **all of it** to the SkillClip creator. It does not write a license record.

```bash
# amount is USDC 6 decimals, e.g. 100000 = 0.1 USDC
cast send $USDC "approve(address,uint256)" $LICENSE_STREAM $AMOUNT --rpc-url $HOODI_RPC --private-key $PK
cast send $LICENSE_STREAM "donateClip(uint256,uint256)" $TOKEN_ID $AMOUNT --rpc-url $HOODI_RPC --private-key $PK
```

---

## How producers opt in

Sidecar (optional):

```json
"donate": {
  "enabled": true,
  "currency": "USDC",
  "address": "0xREPLACE_if_different_from_creator_address",
  "chain_id": 167013,
  "note": "Thanks only — does not grant a license"
}
```

If `donate.enabled` is true and `address` is omitted, pay `creator_address`.

SkillOffer field: `donate_usdc: 0x...` (optional).
