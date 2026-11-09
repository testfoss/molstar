# Testfoss decisions

## Preserve upstream

Use source pin `a4e9cc79415213263090ee47bf380d2a33e0118e`. Do not port lab logs, screenshots, patches, or generated viewer assets. The sole copied artifact is the portable `local-1cbs.mvsj`; its existing `/examples/1cbs_full.bcif` reference is intentional and keeps the fixture self-contained relative to the repository server.

## Exercise the stock viewer

Serve the locally built `build/viewer/index.html`. Load 1CBS with its `mvs-url` query, then use `molstar.Viewer.instances[0].loadTrajectory` for the existing PDB/DCD pair. No custom viewer HTML or upstream source patch is needed.

## Use lmwebtest for interactions

`testfoss/browser.lms` embeds `_lmcode_ cli="lmwebtest playwright"` and calls `run_lmcode`. It checks scene components, ligand hide/show, Model 1 to Model 2, animation start/stop, resource URLs, and page/console errors. Screenshots are supporting disposable evidence, not the assertions themselves.

## Keep the full practical CI gate

Run build, lint, and the full serial Jest suite in Actions because the lab completed all 1,522 passing tests in about one minute after installation. The browser check runs in the same job after that build, avoiding generated-artifact upload or deployment.
