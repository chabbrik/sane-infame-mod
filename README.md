# Sane Infamy

A focused diplomacy rebalance mod for Victoria 3 (1.12 / Sphere of Influence). Targets specific areas where vanilla infamy mechanics are miscalibrated or broken, without reworking the underlying system.

## Design Philosophy

Vanilla Victoria 3 has several infamy problems:

- **Passive decay is too slow**, punishing countries that fought one justified war and then stopped.
- **Unrecognized nations were doubly discounted** — a script-level 0.5× multiplier stacked on top of the rank-level discount already in country_ranks, making colonial wars almost free.
- **Great powers and minor states have nearly the same diplomatic influence**, compressing a gap that should be meaningful.
- **Loyal subject multipliers were hardcoded** rather than conditional on actual loyalty, making the distinction between a genuinely loyal subject and one that merely hasn't revolted yet meaningless.
- **A vanilla bug** causes the Ban Slavery diplomatic play to produce no effect when resolved peacefully.

This mod addresses each of these specifically. It does not touch military mechanics, economic systems, or anything outside diplomacy and war goals.

## Changes

### Infamy Decay & Peace Refund

- **Passive decay rate**: 5.0 → 6.0 per year (+20%). Countries that stop after one war recover at a reasonable pace rather than being locked out of diplomacy for years.
- **War goal peace refund**: 50% → 75%. When a belligerent capitulates or makes peace without enforcing a staged war goal, 75% of that infamy is refunded. Rewards restraint in peace negotiations.

### Diplomatic Play Bug Fix

- **Ban Slavery peaceful resolution**: Fixed a vanilla bug where the target backing down (conceding without war) failed to activate `law_slavery_banned`. The play now correctly enacts the law on peaceful resolution, matching what happens when enforced through war.

### Country Influence

Recognized country ranks had too little separation between great powers and insignificant states. Increased `country_influence_add` for all recognized ranks:

| Rank | Vanilla | Sane Infamy |
|---|---|---|
| Great Power | 1,000 | 1,300 (+30%) |
| Major Power | 750 | 1,000 (+33%) |
| Minor Power | 600 | 750 (+25%) |
| Insignificant Power | 500 | 600 (+20%) |
| Unrecognized (all) | unchanged | unchanged |

All infamy scaling, prestige thresholds, and other rank properties are unchanged.

### War Goals

- **`annex_country`**: Restricted to subjects only. Annexing independent nations now requires subjugating them first. Loyal subject infamy multiplier made conditional on liberty desire (< 25) rather than unconditional.
- **`regime_change`**: Base infamy reduced from 5 to 0.5, with population-based scaling so regime-changing a major power still carries real cost. Net effect: very cheap for weak states, meaningful for large ones.
- **`return_state`**: Now also valid when the target state's owner is a member of the attacker's power bloc, supporting internal territorial adjustments.
- **`ban_slavery`**: Added population-based infamy scaling (previously absent). Loyal subject multiplier made conditional on liberty desire.
- **Unrecognized discount removed** across all war goals that had it (`annex_country`, `force_nationalization`, `open_market`, `reduce_autonomy`, `regime_change`, `return_state`, `take_treaty_port`, `transfer_subject`). The rank-level discount in country_ranks is retained as the sole mechanism.
- **Loyal subject multiplier standardised** across `take_treaty_port`, `transfer_subject`, `force_nationalization`: changed from unconditional hardcoded values to vanilla defines, conditional on liberty desire below threshold.
- **`restore_union`**: Removed unintended infamy multipliers producing incorrect results for union restoration plays.

### Treaty Articles

Eleven treaty articles reworked (`transfer_money`, `transfer_state`, `foreign_investment_rights`, `military_access`, `prohibit_trade_with_global_market`, `acquire_monopoly_for_company`, `law_commitment`, `no_tariffs`, `transit_rights`, `trade_privilege`, `no_subventions`):
- AI acceptance scoring improved for more rational AI behaviour.
- Infamy penalties added for breaking binding treaties early, proportional to remaining binding period.
- Cost and relation progress values adjusted for internal consistency.

## Compatibility

Targets vanilla 1.12 / Sphere of Influence. Compatible with most mods that do not replace the same war goal, diplomatic play, or country rank files.

**Known conflicts**: any mod that fully replaces `common/country_ranks/00_country_ranks.txt` or `common/war_goal_types/` files this mod edits.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for a full diff against vanilla.
