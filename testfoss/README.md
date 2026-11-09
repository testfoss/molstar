# Mol* Testfoss demo

Source pin: `a4e9cc79415213263090ee47bf380d2a33e0118e` (5.13.1). This is an additive demo; no upstream viewer or example is patched.

## Build and test

From the repository root:

```sh
./testfoss/run.sh package
```

This runs `npm ci`, `npm run build`, and `npm test -- --runInBand`. The recorded lab gate at this pin passed lint and 1,522 Jest tests (5 skipped).

## Browser viewer and trajectory

```sh
(cd testfoss && npm ci && npx playwright install chromium)
./testfoss/run.sh browser
```

The runner serves the repository at <http://127.0.0.1:8111/> and invokes `lmwebtest playwright` through `browser.lms`. Set `TESTFOSS_CHROME_EXECUTABLE=/path/to/chrome` to use an installed Chrome instead of Playwright-managed Chromium. The 1CBS URL is:

<http://127.0.0.1:8111/build/viewer/index.html?mvs-url=%2Ftestfoss%2Flocal-1cbs.mvsj&mvs-format=mvsj>

The scenario verifies polymer/ligand components, ligand hide/show, and local fixture/structure requests. It then loads the stock `/examples/trajectory/protein.pdb` and `.dcd`, advances from Model 1 / 10 to Model 2 / 10, starts and stops trajectory animation, and checks page/console errors. Disposable screenshots go to `testfoss/artifacts/`.

`local-1cbs.mvsj` is the only copied lab fixture. Its `/examples/1cbs_full.bcif` reference and the trajectory pair deliberately use existing upstream data rather than duplicate fixtures. Headless WebGL performance warnings are benign; the stock viewer may contact its default remote-state catalog even though all molecular demo data is local.

Syntax-only checks:

```sh
bash -n testfoss/run.sh
node -e 'JSON.parse(require("fs").readFileSync("testfoss/local-1cbs.mvsj", "utf8"))'
(cd testfoss && npm ci && npm run check)
```

See `../durable-memory/status.md` for exact tool pins and verification provenance.
