# Al-bahr Admin — Agent instructions

## App version

`const APP_VERSION` at the top of `index.html` is the single source of truth for the visible admin-app version. It is rendered:

- as a tiny `v<x.y.z>` stamp fixed above the bottom navigation on every tab,
- on the login screen under the sign-in card,
- inside the System settings section (`[["Version", "Broast Albahr Admin v" + …]]`).

### Auto-bump policy — do this on every commit that changes `index.html`

Before committing any code change to `index.html`, bump `APP_VERSION` in place. Follow semver:

- **patch** (`x.y.Z`) — every non-trivial fix, small tweak, copy change, wiring update. This is the default bump.
- **minor** (`x.Y.0`) — a new user-visible feature or a new admin section/tab.
- **major** (`X.0.0`) — breaking change to the Firestore schema or a rewrite of a full flow.

Never skip the bump. Never batch — one commit that changes `index.html` = one version bump. Do not require the user to remind you.

Docs-only commits (README, CLAUDE.md, comments-only) do not bump the version.

Current baseline established: `8.2.0`.

## Branching

Work on `claude/printing-customer-data-sync-e4obsw` unless the user tells you otherwise. When the user asks to push to `main`, do a fast-forward merge from the feature branch and push `main`.
