# Verification and runtime integrity

Use this reference for integration, runtime-affecting work, data-integrity
changes, or high-risk acceptance where stale state could invalidate a local
result. Ordinary bounded Git edits use the final gate in `SKILL.md`.

## Minimal completion acceptance

Advance the main task without a default testing phase. During implementation,
use only a discriminating check needed to resolve a blocking unknown or failure.
After completion, accept through the shortest authoritative path proving the
user's requested outcome and directly affected invariants, then stop.

Do not accumulate probes, fixtures, hashes, health checks, smoke runs or test
suites and call the collection one gate. Prefer existing entry paths and evidence;
new machinery requires a specific uncovered acceptance requirement. Full-suite,
whole-repository and unrelated-dependency testing is prohibited by default;
explicit user requirements and applicable mandatory project checks still apply.

Bind evidence to the adopted revision or bounded diff using existing identity
where sufficient. Evidence remains reusable for invariants unaffected by a fix,
commit, merge or nonfunctional edit. Recheck only failed or invalidated parts.
Inspect the final diff as part of existing review; negative/count checks apply
only when they resolve a concrete duplicate-owner risk, never automatically.

## Scope evidence by invalidated invariant

Final-state identity and validation breadth are separate decisions. Bind the
evidence to the exact adopted revision or deliverable, then validate only the
semantic surface and invariants that the delta can invalidate:

- payload values or content hashes, member set or order, encoding,
  partitioning, value-producing transformations, selection, or eligibility
  semantics: validate affected content, hashes, coverage, and semantics;
- owner identity, receipt, contract, manifest identity, or reference hash with
  unchanged member and payload hashes: revalidate owner existence and
  acceptance, manifest self-identity, recorded member/hash equality, schema and
  reference closure; inspect payload values only if a payload, member, or
  value-semantic dependency changed;
- value-free bindings, locators, catalogs, indexes, or control metadata with
  unchanged owner identities and hashes: validate schema, existing receipt and
  hash bindings, reference closure, resolution, and a real consumer load;
- consumers or adapters: exercise the real lookup, boundary, and resulting
  input or behavior; reopen affected value validation only when selection,
  decoding, interpretation, ordering, membership, or eligibility semantics
  changed; and
- metadata-only changes: never mmap, hash, or sample-read large unchanged
  payloads merely as a generic confidence check.

An immutable receipt remains valid for the unchanged owner and invariant it
proves. A new reference to that receipt does not make the underlying evidence
stale. Evidence is stale only after a relevant candidate, baseline, input,
semantic owner, validation implementation, or covered invariant changes.

Full-data, full-history, or whole-dependency-graph validation is prohibited by
default. Admit it only when the changed invariant truly spans that scope, no
narrower authoritative evidence can prove the requirement, and the run has an
explicit bounded resource envelope and stop condition. “Safer,” “final state,”
or “fail closed” alone are not sufficient justifications. The parent must treat
unnecessary validation as a defect alongside missing validation.

## Reuse accepted source evidence during migration

Migration validation begins with evidence, not a payload replay. The parent resolves
the accepted source receipt or report and confirm that it binds the current
artifact identity, manifest identity, exact ordered members, recorded member and
content hashes, baseline, schema, and the semantic invariants it claims to
cover. A file being present, a prior write succeeding, or a copy completing is
not proof of semantic acceptance.

When the source evidence is current and the migration leaves payload,
membership, order, encoding, partition meaning, selection, eligibility,
decoding, and value semantics unchanged, retain that receipt as the proof of
source semantics. Do not reopen, mmap, sample, or semantically revalidate the
source payload. The parent instead checks the exact source and
destination boundaries, source stability and concurrent-write exclusion,
transfer method and integrity proof, failure recovery, cutover, real consumer
loading, and old-path retention or cleanup.

Choose migration checks by what changes:

- same-filesystem move or rename: verify the exact move result, source absence
  and destination presence, preserved identity and manifest binding, excluded
  writers, consumer cutover, and recovery path;
- byte-preserving copy: compare the destination against the source identity
  recorded in the accepted receipt using an authoritative transfer checksum,
  storage proof, or equivalent content/member-hash evidence; validate manifest
  completeness, source stability, cutover, and cleanup. This proves transfer
  integrity, not upstream numeric semantics; and
- format, encoding, layout, partition, membership, order, selection,
  eligibility, decoding, or value-semantic transformation: validate the exact
  affected equivalence and semantic invariants. Expand toward full data only
  when the changed invariant truly spans it and no narrower proof is sufficient.

Missing, stale, conflicting, or identity-unbound source evidence fails closed.
It justifies only the checks needed for the uncovered invariant, not an
automatic whole-dataset replay. The final migration evidence names the source
receipt, source and destination identities, transfer-integrity proof, write
exclusion, recovery result, cutover, consumer load, and old-path disposition.
It proves migration and adoption while the unchanged source receipt continues
to prove the upstream payload semantics.

## Integration determines the acceptance surface

The parent owns the one acceptance decision and may delegate its execution.
For `must_integrate`, accept the actual adopted target; an isolated child does
not need an independent preflight gate merely to hand off its candidate. Reuse
child evidence for unchanged invariants and check only integration effects that
can invalidate it. Resolve semantic conflicts even without textual conflicts;
use existing revision/diff identity rather than inventing another proof system.

Use b23 only when the next step is a closed final action proven to have no
reliable and authorized rollback or recovery path. At the last responsible
moment, the stable b23 session reviews the exact operation package once and
returns `PASS`, `FAIL`, or `BLOCKED`. `PASS` authorizes only that identical
package; `FAIL`, `BLOCKED`, inability to inspect, or any package change prohibits
execution. Do not use b23 for preparation, reversible work, failure repair, or
milestone review, and do not run a second or post-execution b23. After execution,
the parent verifies deterministic postconditions and keeps the one verdict and
postcondition evidence in the same final bundle. For migration, this applies
only to a final cleanup or cutover action that meets the no-recovery definition.

For `research_only` or `checkpoint_only`, do not merge merely to satisfy a generic closure pattern. Validate only the deliverable that the adoption contract actually defines, and keep an unverified checkpoint explicitly unaccepted.

## Prove the running system is loading the verified result

When runtime behavior changes, the acceptance path must reach the actual loaded revision, configuration or artifact. Check loader identity where stale loading is plausible; the same behavior observation can prove loading and the requested outcome. Do not add separate health, smoke or process sweeps when they add no missing evidence.

Do not claim runtime success from source-tree state alone. If the loaded revision or configuration cannot be determined, report that limit and do not promote a local result into runtime acceptance.
