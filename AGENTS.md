# Testfoss demo agent guide

## Scope and source pin

This branch is `testfoss-demo`, based on upstream commit `a4e9cc79415213263090ee47bf380d2a33e0118e` (Mol* 5.13.1). The public fork is `https://github.com/testfoss/molstar`; upstream is `https://github.com/molstar/molstar`.

Keep the change additive: only append the README Testfoss section and add `AGENTS.md`, `durable-memory/**`, `testfoss/**`, and `.github/workflows/testfoss.yml`. Never modify upstream source, examples, lockfiles, or other workflows. Do not commit build output, `node_modules`, screenshots, or browser results. Do not open upstream issues or pull requests for this demo.

## Reproduce

Use Node 24 (24.15 or newer is required by lmscript 0.2.463); the historical lab build used Node 22. From the repository root:

```sh
./testfoss/run.sh package
(cd testfoss && npm ci && npx playwright install chromium)
./testfoss/run.sh browser
```

`package` runs `npm ci`, `npm run build`, and `npm test -- --runInBand`. The recorded upstream result at this pin is 1,522 Jest tests passed and 5 skipped across 143 suites (140 passed, 3 skipped), plus lint. The browser command serves the repository on loopback port 8111 and runs `testfoss/browser.lms` through `lmwebtest playwright`.

Browser dependencies are isolated in `testfoss/package-lock.json`: lmwebtest commit `2920befd355eacc654697820d690db074e94dce3`, `@lmctl-ai/lmctl` 0.2.463, and Playwright 1.62.1. Every browser CI path must invoke lmwebtest, not a separate direct-Playwright replacement.

The only copied lab artifact is `testfoss/local-1cbs.mvsj`, SHA-256 `c69c99a079a32c1b3984ab85ca363680084267e1f88f5a0a6ca5e3205f97ff9a`. It intentionally references the existing local `/examples/1cbs_full.bcif`; do not copy structure or trajectory fixtures into Testfoss. The stock viewer loads `/examples/trajectory/protein.pdb` and `.dcd` through its public `loadTrajectory` API.

## Limitations

Headless Chromium may report benign WebGL `ReadPixels` performance warnings. The stock viewer can also contact its default remote-state catalog at `webchem.ncbr.muni.cz`; molecular structures, trajectory data, MVS, and viewer code in this scenario are local. The lab recorded 30 dependency-audit findings at the pinned lockfile (3 low, 25 moderate, 2 high); do not apply unrelated audit upgrades. See `durable-memory/status.md` for historical versus fork-local verification.
