# aeth-devkit VS Code extension

`aeth.aeth-devkit`. Installed by `devkit setup-project` from this repository's GitHub
releases (`vN`, asset `aeth-devkit-vscode-N.vsix`); never published to the marketplace. It
shows each Docker change setup-project proposes as a native diff with per-hunk Accept and
Reject, opens a multi-diff review for `--dry-run`, and carries the
`Add to runtime-evaluated-base-classes` command. The consent protocol it speaks with devkit
is versioned (`protocol` in every request; a mismatch retires the reviewer for that run);
devkit's side is `crates/aeth-devkit-setup/src/vscode/` in `AetherBreaker/aeth-devkit`.

## Develop

```sh
npm ci
npm run typecheck   # esbuild strips types without checking them; this is the only type check
npm test
npm run build       # dist/extension.js
```

The `VS Code extension (dev host)` launch configuration runs the extension from this
checkout. The repository is devkit-managed: `uv sync` and `poe setup-project` keep the shared
configuration current; the Python tooling that merge brings in is inert here.

## Release

Builds are numbered. Push an annotated tag `vN` with the next integer `N` and the release
workflow builds, typechecks, tests, packages `aeth-devkit-vscode-N.vsix` (manifest `N.0.0`)
and publishes GitHub release `vN` with it:

```sh
git tag -a v3 -m "VS Code extension build 3" && git push origin v3
```

`devkit setup-project` installs the newest published release (not tag) `N` that meets its
`MIN_EXTENSION_VERSION` and carries the vsix; a draft or prerelease is ignored, and a tag whose
run failed installs nothing until `gh run rerun <run-id>` succeeds. Bump that constant in
devkit when a protocol change makes older builds unusable. Build 1 was released from
`aeth-devkit` as `vscode-extension-v1`; numbering continues from there.
