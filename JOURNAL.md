## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/150

**Issue title:** Tech detector counts vendored and build-output files, skewing language detection

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The technology detector currently counts files inside directories such as `node_modules/` and `build/` when deciding a repository’s primary programming language. These files are usually third-party dependencies or generated build artifacts rather than code written by the repository owner. As a result, a mostly Python project can incorrectly be classified as JavaScript when vendored JavaScript files outnumber the real source files. A successful fix would filter out these paths before counting languages so the detector reflects the repository’s actual source code.

**Issue selection reasoning:**
I chose this issue because it is a Tier 1 bug with a clear reproduction example, a narrow scope, and existing failing tests. The relevant implementation and test files are identified in the issue, so I can trace the problem without needing to understand the entire codebase. The expected behavior is also specific: files inside `node_modules/` and `build/` should not affect primary-language detection. This makes the issue realistic for my current experience while still requiring me to understand and test an existing module.

**Branch name:** fix/150-ignore-vendored-build-files

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger



## Week 8 — Reproduction & solution planning


**Reproduction commit link:** https://github.com/iadamib7/pathreview/commit/2df3497

**Reproduction summary:**
I reproduced Issue #150 by running the existing `test_node_modules_excluded` and `test_build_directory_excluded` tests in `tests/unit/test_tech_detector.py`. Both tests failed because `TechDetector` counted JavaScript files inside `node_modules/` and `build/`, causing it to report JavaScript instead of the expected Python primary language.


**PLAN.md link:** https://github.com/iadamib7/pathreview/blob/fix/150-ignore-vendored-build-files/PLAN.md

**Walkthrough video (recommended):** Not recorded

**Blockers or open questions:**
I still need to confirm how file paths are normalized inside `TechDetector` and whether nested or Windows-style paths require additional handling.



## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I implemented the fix for Issue #150 in `agent/tools/tech_detector.py`. The detector now normalizes file paths and excludes files located inside vendored or generated directories before performing language detection. The existing `test_node_modules_excluded` and `test_build_directory_excluded` regression tests now pass, and all 27 tests in `tests/unit/test_tech_detector.py` pass.

**Next steps:**
I will open a draft pull request, request peer or mentor feedback, review the repository contribution checklist, and document the repository-wide pre-existing test failures. After addressing any relevant feedback, I will mark the pull request ready for review and complete Check-in 2.

**Blockers:**
The full unit-test suite currently has 51 pre-existing failures in unrelated modules. My focused TechDetector tests pass, and my changes do not touch the failing modules.

**Tests added or updated:**
Updated the TechDetector test coverage by adding regression tests for Windows-style paths, deeply nested vendored/build directories, and filenames that resemble skipped directory names. Verified that all TechDetector unit tests pass.