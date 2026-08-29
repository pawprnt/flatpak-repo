# adding an app

1. create `packaging/flatpak/` in your repo with:
   - `io.github.pawprnt.<app>.yml` — flatpak manifest
   - `io.github.pawprnt.<app>.metainfo.xml` — appstream metadata
   - `io.github.pawprnt.<app>.desktop` — desktop entry
   - `io.github.pawprnt.<app>.svg` — app icon

2. add a `flatpak.yml` workflow to `.github/workflows/`:

```yaml
name: Build Flatpak
on:
  push:
    tags: ['v*']
  workflow_dispatch:

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/andyholmes/flatter/kde:6.9
      options: --privileged
    steps:
      - uses: actions/checkout@v4

      - name: Install runtime
        run: |
          flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
          flatpak install --user --noninteractive -y flathub org.kde.Platform//6.8 org.kde.Sdk//6.8

      - name: Clone flatpak-repo
        run: |
          git clone https://x-access-token:${{ secrets.FLATPAK_GITHUB_TOKEN }}@github.com/pawprnt/flatpak-repo.git /tmp/flatpak-repo

      - name: Build
        run: |
          flatpak-builder --user --force-clean --disable-rofiles-fuse --repo=/tmp/flatpak-repo/repo _build packaging/flatpak/io.github.pawprnt.<app>.yml

      - name: Update repo
        run: |
          flatpak build-update-repo --generate-static-deltas --prune /tmp/flatpak-repo/repo

      - name: Push
        run: |
          cd /tmp/flatpak-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git commit -m "update <app>" || exit 0
          git push
```

3. the workflow builds into the shared ostree repo at `/tmp/flatpak-repo/repo` and pushes.
4. after the first build, users can find your app with `flatpak update` or by re-adding the remote.
