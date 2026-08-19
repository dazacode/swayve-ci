# swayve-ci

Build automation for a private repository.

GitHub Actions is free on public repos and billed on private ones. This repo
holds the Windows, Android and iOS build workflows for `swayve-client` so they
run on free minutes — the private repo itself carries no CI.

Every workflow checks out `swayve-client` using a fine-grained personal access
token (`SWAYVE_CLIENT_TOKEN`, repo secret) scoped to read-only Contents on that
one repository, plus write access for `release.yml`, which publishes the
finished release back onto `swayve-client` — that's where the app's own
update check and "get the newer build" link point.

Everything here is manual (`workflow_dispatch`) — a push to `swayve-client`
doesn't cross repos on its own, so a build is triggered after the fact:

```bash
gh workflow run "Build Windows" --repo dazacode/swayve-ci -f ref=main
gh workflow run "Build Linux" --repo dazacode/swayve-ci -f ref=main
gh workflow run "Build Android APK" --repo dazacode/swayve-ci -f ref=main
gh workflow run "Build iOS IPA" --repo dazacode/swayve-ci -f ref=main
gh workflow run Release --repo dazacode/swayve-ci -f version=v1.1.0
```
