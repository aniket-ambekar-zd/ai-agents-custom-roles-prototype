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
| 3 · Saved role + Assign | Effective-permissions summary (including the named AI agents in scope), Assign role modal |
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

## Added Aug 12 2026 — "Manage selected AI agents"

**Ownership and scope** (subsequent release) now carries a third scoping option: a multi-select
allowlist of specific AI agents, alongside *own only* and *brand*. Selected agents render as
chips under the picker and are listed by name and ID in the effective-permissions summary on
screen 3.

**Why it's here.** This is the replacement home for *Restrict AI agent access* — one of the 12
Admin-only features in the Admin Role Deprecation initiative. Restricting a customer admin to
specific agents exists today but is configurable only by the internal Zendesk Admin role, so it
can't be handed to the customer admin directly without letting them lift their own restriction.
Moving it into Custom Roles makes the configurer the account's Support admin or owner — a higher
authority than the role being restricted, which removes the circularity rather than mitigating it.

The dashboard version is being removed under Admin Role Deprecation with an accepted capability
gap until this control ships. Existing restrictions are expected to stay enforced through the gap
— **this depends on deprecating the configuration surface while retaining enforcement, and needs
engineering confirmation.**

**Why it's cheaper than brand scoping.** Brand scoping is blocked because brand isn't an
access-control attribute on an AI agent. Here the AI agent *is* the resource, so no new attribute
has to be exposed — it's an `id $in [...]` condition. The open question is where the permitted set
is stored and how it reaches policy evaluation (policy-statement condition on the role, vs. passed
per request like `user.group.id`).

**Two design questions the UI deliberately surfaces:**
1. **Allowlist staleness** — an agent created after the role is configured belongs to no list and
   is silently invisible to that role. Correct as least-privilege, but the admin creating the agent
   usually isn't the admin owning the role. Needs a decision: silent exclusion, warning at agent
   creation, or an "include new agents" opt-in.
2. **Picker scale** — a checkbox list works for the 8 sample agents; accounts with hundreds need
   search-and-page or a different pattern.

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
