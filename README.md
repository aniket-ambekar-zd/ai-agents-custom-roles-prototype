# AI Agents Permissions in Custom Roles — Prototype

Clickable prototype of the AI agents permissions section in Zendesk Admin Center Custom Roles.

**Live:** https://aniket-ambekar-zd.github.io/ai-agents-custom-roles-prototype/

Built Aug 2026 by Account Provisioning to make the PRD requirements concrete ahead of the
Permissions sync.

> ⚠️ This is a prototype, not a product. No backend, no real permissions, no real accounts.

---

## What it covers

Four screens, mirroring the existing Admin Center flow:

| Screen | PRD requirements |
|---|---|
| 1 · Roles list | Context only — existing `People > Team > Roles` page |
| 2 · Create role | **ACC1–ACC4** (None / Full / Limited), **API1–API3, API5** (API integrations View Only vs. Full Access), **ADM1–ADM3** (placement, discoverability, default = None), **GRN1–GRN3** (subsequent-release granular controls) |
| 3 · Saved role + Assign | Effective-permissions summary, Assign role modal |
| 4 · Team member | **ADM5–ADM7** — Support role shows the custom role; AI agent product role is disabled with "Defined by Support role" |

Two toggles in the banner:

- **First release / + Subsequent** — shows v1 scope vs. the UC2 expansion (Analytics, Settings, Content, Ownership)
- **Screen 1–4** — jump between flows

## "Under the hood" panel

Right-edge tab opens a live panel showing what each admin-facing control actually produces
as `resource:scope` permissions, including which resources carry conditions, and a running
count of controls vs. resources vs. permission pairs.

This is the point of the prototype. It makes visible that **one control is never one
permission** — "API integrations: Full access" is five permissions on one resource; "Content:
Full access" is twenty across five resources.

Permission and scope names are **illustrative**. No AI Agents resources are defined in the
permissions platform today — the vocabulary is still ours to propose. See
`../context/decision-api-investigation.md`.

## Open questions surfaced in the UI

The prototype deliberately shows unresolved items rather than hiding them:

1. **Propagation delay** — enforcement evaluates a cached policy bundle, not the database.
   Changes take up to a minute. Not yet a PRD requirement. (PRD TECH1 says 5 seconds and is
   likely unachievable as written.)
2. **Content grouping** — one control over five entity types. Cheap to split later, but only
   if each entity is its own resource from day one.
3. **Migration behaviour** — what happens to a user who already holds an AI agent product
   role when they move into a custom role that carries AI agent permissions? Undecided, and
   it affects accounts that already have custom roles.
4. **Enforcement ownership** — the Permissions platform only answers "is this allowed?".
   Hiding credential fields, blocking writes and filtering lists is AI Agents product work.

## Source

- PRD: `../docs/prd-account-provisioning.md`
- Engineering investigation: `../context/decision-api-investigation.md`
- Screenshots this was built against: `../context/CR_*.png`
