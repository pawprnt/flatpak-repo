# pawprnt flatpak repo

a shared flatpak repository for pawprnt apps. add the remote once, install any app.

## install

```
flatpak remote-add --user pawprnt https://pawprnt.github.io/flatpak-repo/repo
flatpak install --user pawprnt io.github.pawprnt.forager
```

browse available apps at https://pawprnt.github.io/flatpak-repo/

## available apps

| app | app id | description |
|-----|--------|-------------|
| forager | io.github.pawprnt.forager | steam-style game launcher for your local library |

## adding your own app

1. create a `packaging/flatpak/` directory in your repo with a manifest (`io.github.pawprnt.<app>.yml`), metainfo, desktop file, and icon.
2. add a `flatpak.yml` workflow that builds with `flatpak-builder --repo=/tmp/flatpak-repo/repo` and pushes to this repo.
3. the ostree repo at the root handles multiple apps — each build adds its refs alongside existing ones.

## wiki

the forager wiki lives in `wiki/` with pages on install, configuration, features, library layout, and more.

start at [wiki/index.md](wiki/index.md)

## how it works

this repo is an ostree repository served via github pages. each app's ci workflow builds a flatpak and pushes the result here. the `.flatpakrepo` file at the root is what flatpak uses to discover available apps.
