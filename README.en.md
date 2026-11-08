# my-dsh-bundle

An aggregate bundle that installs a curated set of DeepSeek Harness (DSH) web UI plugins in one command.

> Requires DSH kernel `>= 0.2.1-rc.2` (`dsh.engines.dsh`).
> All eight plugins are third-party packages by their respective authors. This bundle only aggregates and mounts them; it does not modify their source.

---

## Included

| Plugin | Version | Purpose | License |
|---|---|---|---|
| `@anysearch/anysearch-dsh` | 0.1.8 | Web search + fetch providers and tools | MIT |
| `@linxin666/dsh-client-ui-skin-center` | 0.4.8 | Skin center: theme switching, try-on, custom colors, background blur | Apache-2.0 |
| `@linxin666/dsh-client-ui-market` | 0.4.8 | Workshop: browse and install skins / pets / plugins / presets inside the GUI | BSD-3-Clause |
| `dsh-all-usage` | 1.2.1 | Usage dashboard by model, provider and workspace, with cache hit rate, balance and CSV export | MIT |
| `dsh-better-reasoning-effort` | 0.5.4 | Per-model reasoning-effort and image-input declaration, editable in the official Models page | MIT |
| `dsh-better-sidebar` | 0.28.0 | Dual workbench: file tree, editable editor, Git (read-only commit graph), task topology, side chat, bottom terminals | MIT |
| `dsh-pet` | 0.3.7 | Desktop pet that reacts to model activity | MIT |
| `dsh-pocket` | 2.12.0 | Scan-to-pair phone access to the local DSH instance | GPL-2.0 |

### Deliberately excluded

- `dsh-purge` — machine-specific, installed from a git source
- `dsh-workbuddy-connect` — bound to a WorkBuddy desktop sign-in, not distributable
- `@opencode2dsh/dsh-plugin` — bound to an OpenCode free-tier source, currently unusable

---

## Install

### Desktop (DSH Desktop, profile managed by the app)

```powershell
cd "$env:USERPROFILE\.dsh\profiles\desktop"
pnpm add https://github.com/zhuihuiguang018/my-dsh-bundle
```

Then add `my-dsh-bundle` to `dsh.profile.bundles` in `package.json` and restart DSH.

### Other profiles (Web / custom)

```sh
dsh plugin --profile web add https://github.com/zhuihuiguang018/my-dsh-bundle
```

`dsh plugin add` registers the bundle in `dsh.profile.bundles` automatically.

### Notes

- Every row id is prefixed with `my-`, so this bundle coexists with standalone installs of the same plugins without id collisions.
- Do not mount the same plugin twice (once via this bundle, once standalone) — you would get two copies of its UI.
- The `- id: web` row points the host's search/fetch backend at AnySearch. Remove those three lines if you don't want that.

---

## Uninstall

```powershell
cd "$env:USERPROFILE\.dsh\profiles\desktop"
pnpm remove my-dsh-bundle
```

Remove `my-dsh-bundle` from `dsh.profile.bundles` and restart DSH.

To disable a single plugin instead, override its row id in your profile's `cordis.patch.yml`:

```yaml
- id: my-dsh-pet
  disabled: true
```

---

## Versioning

Dependencies are pinned to exact versions so installs are reproducible. Bump the version in `package.json` and re-run `pnpm install` to upgrade.

If your kernel moves to `0.2.2+`, some plugins may need newer releases; upgrade them one by one as prompted by the compatibility errors.

---

## License

The bundle itself is MIT (see `LICENSE`). Each bundled plugin keeps its own license; see `THIRD_PARTY_NOTICES.md`. `dsh-pocket` is GPL-2.0 and is referenced only as a dependency — its source is not vendored here.
