# getting started

the pawprnt flatpak repo is a shared repository for pawprnt apps. add the remote once, install any app.

## install a remote

```
flatpak remote-add --user pawprnt https://pawprnt.github.io/flatpak-repo/repo
```

## install an app

```
flatpak install --user pawprnt io.github.pawprnt.forager
```

## run an app

```
flatpak run io.github.pawprnt.forager
```

## available apps

- [forager](install.md#forager) — steam-style game launcher for your local library
