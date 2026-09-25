# BerryBrowser

BerryBrowser is the browser extension used by [OneBerryWiki](https://github.com/cmictAI0629/oneberrywiki)'s
"Browser connection" feature. It is a fork of [Tencent/BrowserSkill](https://github.com/Tencent/BrowserSkill)
(MIT License, copyright Tencent; see [LICENSE](LICENSE)).

## Differences from upstream

- Remote endpoints may use `ws://` on any host, so IP + HTTP deployments can pair without a trusted
  certificate (`wss://` is still used when the gateway serves HTTPS).
- User-visible branding: name, icon and UI strings say BerryBrowser. Internal identifiers
  (`bsk`, package names, storage keys) are unchanged to keep upstream merges simple.

## Branches and versions

- `main` tracks upstream `Tencent/BrowserSkill` and carries no local changes.
- `berry` (default) = an upstream release + the changes above.
- Versions are `<upstream version>-berry.<N>`, e.g. `0.3.1-berry.1`. The manifest `version` stays equal
  to upstream (the OneBerryWiki server checks it); `version_name` carries the suffix
  (`BERRY_RELEASE` in `apps/extension/wxt.config.ts`).

## Updating from upstream

```bash
git fetch upstream --tags
git checkout berry
git merge <upstream release commit or tag>   # e.g. ext-v0.3.2
# reset BERRY_RELEASE to "berry.1", fix conflicts, then:
git tag v0.3.2-berry.1 && git push origin berry v0.3.2-berry.1
```

Pushing a `v*-berry.*` tag runs `.github/workflows/berry-release.yml`, which builds the extension and
publishes `BerryBrowser-<version>.zip` on the Releases page. Then update `scripts/browserskill-release.json`
in OneBerryWiki to the new commit so the server bundles the same version.

## Install

Download the zip from the OneBerryWiki "Browser connection" page (recommended; it always matches the
server) or from [Releases](https://github.com/cmictAI0629/BerryBrowser/releases), unzip it, open
`chrome://extensions`, enable Developer mode and choose **Load unpacked**.
