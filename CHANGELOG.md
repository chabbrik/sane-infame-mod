# Sane Infamy — Changelog

All changes are relative to vanilla Victoria 3 (1.12).

---

## v1.2

### Infamy Defines (`common/defines/00_sane_infamy_defines.txt`)
- `BASE_YEARLY_INFAMY_DECAY_RATE` 5.0 → 6.0. Passive infamy decay is 20% faster per year. Rewards countries that stop after one war rather than chaining conflicts.
- `WAR_GOAL_INFAMY_PEACE_REFUND` -0.5 → -0.75. When a belligerent capitulates or makes peace without enforcing a war goal, 75% of its staged infamy is refunded (up from 50%). Rewards restraint in peace negotiations.

### Diplomatic Play Bug Fix (`common/diplomatic_plays/00_ban_slavery.txt`)
- Vanilla bug: when a Ban Slavery diplomatic play resolves peacefully (target backs down without war), `law_slavery_banned` was never activated — the demand produced no effect. Fixed by adding `on_demand_accepted` to activate the law when the target concedes. War-path enforcement was already handled correctly by the engine.

### Country Ranks (`common/country_ranks/00_country_ranks.txt`)
Increased `country_influence_add` for recognized ranks. Vanilla compressed the gap too much between a great power and an insignificant state. Infamy scaling, prestige thresholds, and all other rank properties are unchanged from vanilla.

| Rank | Vanilla | v1.2 |
|---|---|---|
| Great Power | 1000 | 1300 (+30%) |
| Major Power | 750 | 1000 (+33%) |
| Minor Power | 600 | 750 (+25%) |
| Insignificant Power | 500 | 600 (+20%) |
| Unrecognized (all) | unchanged | unchanged |

---

## v1.1

### War Goals

**`annex_country`** — Vanilla allowed annexing any country with `always = no` in possible (i.e., the goal existed but was unusable by design, presumably reserved for scripted events). Reworked:
- `possible` now requires the target to be a subject of the attacker. Annexing independent countries without first subjugating them is removed.
- Loyal subject infamy multiplier: vanilla had an unconditional hardcoded `0.1×` multiplier. Changed to use the vanilla define `WAR_GOAL_INFAMY_LOW_LIBERTY_DESIRE_SUBJECT_MULT` (0.25×), conditional on the subject's liberty desire being below `WAR_GOAL_INFAMY_LOW_LIBERTY_DESIRE_THRESHOLD` (25). A truly loyal subject is still cheap to annex; one that merely hasn't revolted yet is not.

**`regime_change`** — Base infamy reduced from 5 to 0.5, minimum infamy from 5 to 0.5. Added population-based scaling so regime-changing a major power is still meaningful. Net effect: small or weak states are very cheap to regime-change; large states scale appropriately.

**`return_state`** — Valid condition extended. Vanilla required the attacker to already hold a claim on the target state. Now also allows the war goal if the target state's owner is a member of the same power bloc as the attacker. Supports historical cases where power blocs negotiate internal territorial adjustments.

**`ban_slavery`** — Added population-based infamy scaling (previously absent). Loyal subject infamy multiplier made conditional on liberty desire (same fix as `annex_country`).

**Unrecognized discount removed** across all war goals that had it (`annex_country`, `force_nationalization`, `open_market`, `reduce_autonomy`, `regime_change`, `return_state`, `take_treaty_port`, `transfer_subject`). Vanilla applied a flat 0.5× multiplier to infamy when attacking unrecognized countries at the war-goal script level, on top of the rank-level `infamy_target_scaling` already present in country ranks. This double-discounting made colonial wars almost free. The rank-level discount (country_ranks `infamy_target_scaling`) is retained; the redundant script-level discount is removed.

**`restore_union`** — Removed the loyal/rebellious subject blocks and `country_infamy_generation_against_unrecognized_mult` multiplier, which were producing unintended infamy interactions for union restoration plays.

**Loyal subject multiplier** standardised across `take_treaty_port`, `transfer_subject`, `force_nationalization`: changed from unconditional hardcoded values to the vanilla defines (conditional on liberty desire below threshold).

### Treaty Articles

Eleven treaty articles reworked (`transfer_money`, `transfer_state`, `foreign_investment_rights`, `military_access`, `prohibit_trade_with_global_market`, `acquire_monopoly_for_company`, `law_commitment`, `no_tariffs`, `transit_rights`, `trade_privilege`, `no_subventions`):
- AI acceptance scoring improved to produce more rational AI treaty behaviour.
- Infamy penalties added for breaking binding treaties early (`on_break`), proportional to remaining binding period.
- Cost and relation progress values adjusted for internal consistency.
