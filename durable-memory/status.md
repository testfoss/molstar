# Testfoss status

## Exact pins and commands

- Source: `a4e9cc79415213263090ee47bf380d2a33e0118e` (`a4e9cc7`, Mol* 5.13.1), branch `testfoss-demo`.
- Node: 24 for fork commands/Actions (24.15+ for lmscript); the lab used Node 22. Playwright: 1.62.1.
- lmwebtest: `2920befd355eacc654697820d690db074e94dce3`; lmscript: `@lmctl-ai/lmctl` 0.2.463.
- Build/test: `npm ci`, `npm run build`, `npm test -- --runInBand`.
- Browser: serve root on `127.0.0.1:8111`; run `lmscript testfoss/browser.lms` with local lmwebtest on PATH.

## Recorded lab evidence

At the same source pin, the isolated molecular lab completed the source build, lint, and Jest: 140 suites passed, 3 skipped; 1,522 tests passed, 5 skipped. The stock source-built viewer displayed local 1CBS polymer cartoon, Gaussian surface, ligand ball-and-stick, and spacefill. Ligand hide/show worked. The existing PDB/DCD trajectory had 10 frames; Next Model reached Model 2 / 10 and Animate Trajectory advanced through Models 1–3 before Stop. All molecular assets were local. These are lab results, not a claim of a completed fork Actions run.

The fixture SHA-256 is `c69c99a079a32c1b3984ab85ca363680084267e1f88f5a0a6ca5e3205f97ff9a`. Existing structure hashes in the lab record are 1CBS `5deef84d41846a2e409bd0348983f671a726ad68bfc2ea683842425c0e7f5691`, PDB `f6a359ba4d54ddfa41b4f9abc3e260e258022d6319df085b88b79a2bf322b9b5`, and DCD `b2d65ed83192c2a9c1fce306824a89b898e544f78119dcabf9968d744c7e5a6b`.

## Limitations and fork verification

Headless WebGL `ReadPixels` warnings are benign. The stock remote-state catalog may make an external request even though all demo molecular content is local. The pinned dependency tree had 30 audit findings in the lab; no broad upgrade is part of this demo.

Shell/JSON/YAML syntax checks and real lmwebtest schema discovery plus `lmscript --check browser.lms` passed. The new scenario then passed 18/18 checks through `lmwebtest playwright` against the existing same-pin lab-built viewer. A loopback path mapping served the new Testfoss fixture while all other assets came from the lab build. The run used installed Chrome 157 via `TESTFOSS_CHROME_EXECUTABLE` and verified local MVS/BinaryCIF/PDB/DCD requests, polymer/ligand presence, ligand hide/show, Model 1 to Model 2, animation play/stop, and zero page/console errors.

GitHub Actions run 42645348582 subsequently passed the full build and 1,522-test Jest gate, followed by the corrected lmwebtest viewer and trajectory scenario. Never commit generated `build/`, `lib/`, `node_modules/`, or `testfoss/artifacts/` output.
