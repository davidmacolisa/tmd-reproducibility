# TMD Reproducibility Mirror

**Purpose:** MP.34 Phase E public mirror. Hosts the reproducibility package for every published TMD episode. Stable URLs referenced in every episode's YouTube description (BC-34.7) and in every derivative-content footer.

**Structure:**

```
_PACKAGE-HOST/
├── README.md                      ← this file
├── _headers                       ← HTTP caching + CORS rules
└── episodes/
    └── EP###_v{package_version}/  ← one directory per published episode
        ├── manifest.json          ← top-level metadata (MP.34 §5)
        ├── README.md              ← human-readable summary + version log
        ├── archive_snapshots.json ← Wayback + Archive.today permalinks per source
        ├── versions.json          ← software/font/token pins (MP.34 BC-34.5)
        ├── assets_hashes.json     ← SHA-256 of every rendered asset (BC-34.6)
        ├── consultation_log.json  ← full fetch/archive audit trail (MP.1 + BC-34.1)
        ├── verification_report.json  ← G(ξ)/E(ε)/S(σ)/C(γ)/N(ν)/E(η)/V(ν)/X(ρ) + Phase 3.5 gate results (v6.0.1)
        └── derivation_records/    ← data-derivation steps for Type-11 claims (BC-34.4)
```

## v6.0.1 — Package-on-Retraction handling (MP.34 §8.20.3)

When an episode is retracted under MP.20 Level 4 (TMD_R §4.3A), the package is **PRESERVED** — not deleted. The retraction is made visible through nine obligations defined in TMD_R §8.20.3:

1. **Package preservation.** Directory remains at `episodes/EP###_v{package_version}/`.
2. **Retraction manifest field.** `manifest.json` gains a top-level `retraction` object:
   ```json
   "retraction": {
     "retracted_at":        "<ISO-8601>",
     "level":               4,
     "grounds":             "source-fraud | premise-fabricated | causal-collapse | nel-top-level | legal-defamation",
     "crr_entry_id":        "<CORR-EP###-###>",
     "retraction_notice_url": "<YouTube URL of Level-4 retraction notice>",
     "supersedes":          "<URL of re-investigation episode, or null>",
     "rerender_status":     "LOCKED_POST_RETRACTION"
   }
   ```
3. **README.md banner.** Prepend `> **[RETRACTED on <date>]** — See retraction notice at <URL>. This Package is preserved for audit and reproducibility obligations only. The investigation's findings are NOT currently endorsed by the channel.`
4. **Archive URLs remain live.** `archive_snapshots.json` preserves Wayback + Archive.today permalinks — an independent verifier must still be able to reproduce the original verification chain, including (where applicable) identifying where it failed.
5. **verification_report.json preserved.** Original Phase-8 report is preserved unmodified; a `retraction_post_mortem` object may be appended when the retraction was triggered by a verification-process gap.
6. **Re-render lock.** `versions.json.rerender_status = "LOCKED_POST_RETRACTION"`. The annual re-render test (MP.34 Phase F.3) skips the retracted package.
7. **Package URL survival.** URL remains referenced in the (unlisted) original video description and in any surviving derivative content.
8. **MP.28 side-effect.** Upstream ECR entries that cite the retracted episode are removed from the Connection Registry by the retraction workflow — the package itself does not manage MP.28 directly, but the manifest's `retraction` field is the signal.
9. **Θn+1 learning.** The retraction event and originating process gap feed the next cycle's Phase 1 process-improvement inputs.

**Retraction is irreversible.** If new evidence later vindicates an original claim, a NEW investigation produces a NEW package; the retracted package's `retraction` field is NOT removed.

## Schema authority

All JSON files in `episodes/EP###_v*/` validate against `_INFRASTRUCTURE/I-1_schemas.json`. The v6.0.1 additions (`verification_report`, `mp34_package_manifest` with `retraction` field, `correction_register_entry` with Level 4) are the schema surfaces for this mirror.

## Git

This directory is a submodule under the channel workspace. Commits are additive: a package v1.0 remains accessible at its URL when v1.1 is published alongside. Nothing is deleted (BC-34.10 — no silent edits).

---

*v1.0.1 — 2026-04-24 (v6.0.1 cascade Session 4). Prior: v1.0 (pre-launch scaffold).*
