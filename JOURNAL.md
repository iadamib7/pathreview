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