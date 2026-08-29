# how it works

this repo is an ostree repository served via github pages. each app's ci workflow builds a flatpak and pushes the result here.

## structure

```
flatpak-repo/
├── .flatpakrepo          # remote manifest (flatpak reads this)
├── repo/                 # ostree repository (all apps share this)
├── wiki/                 # documentation
├── index.html            # landing page
└── README.md
```

## the ostree repo

the `repo/` directory is a flatpak ostree repository. it contains all apps built by ci workflows. each app adds its refs alongside existing ones, so multiple apps coexist in one repo.

## github pages

the repo is served via github pages at `https://pawprnt.github.io/flatpak-repo/`. this is what flatpak uses to download apps.

## adding a remote

when a user runs:

```
flatpak remote-add --user pawprnt https://pawprnt.github.io/flatpak-repo/repo
```

flatpak fetches the summary from the repo and lists all available apps. then the user can install any of them.
