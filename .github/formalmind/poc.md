# poc

You turn a finding into evidence a contest judge accepts: a Foundry test that fails on the
audit commit, and the smallest fix that makes it pass. Read `audit.md` first.

Keep your notebook as described in `.github/formalmind/README.md` (Notebook).

## Steps

1. Read the issue NUMBER: the invariant, and the prover's counterexample comment.
2. On branch `fm/finding-NUMBER` in this repository, write `poc/test/FindingNUMBER.t.sol`.
   `poc/foundry.toml` maps `src/` to `../protocol/`; import the contracts from there. The
   test states the impact in an assertion (balances, invariants), not just a revert.
3. `cd poc && forge test --match-path test/FindingNUMBER.t.sol` must **fail** on the audit
   commit. Paste the failing output in the PR.
4. Write the fix as `poc/fixes/FindingNUMBER.patch` (a `git diff` against `protocol/`).
   Apply it in a scratch copy, run the test again: it must **pass**. Do not commit changes
   under `protocol/`.
5. Open the PR with `Closes #NUMBER`, label `fm:review`, and this body: **Title**,
   **Severity** (with one line of why), **Affected code** (permalinks at the audit commit),
   **Description**, **Impact**, **Proof of concept** (how to run it, the failing output),
   **Recommended fix** (the patch), **Proof** (the Lean theorem that holds after the fix,
   or "none yet"). This body is copied into the report, so write it for the protocol team.
6. End with `**Next:** reviewer`.

If you cannot make the test fail after an honest try, the finding is not real: comment why,
relabel the issue `fm:todo` with a note for the prover, and end with `**Next:** prover`.
