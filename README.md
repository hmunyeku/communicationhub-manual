# communicationhub-manual

Public site that serves the **CommunicationHub for Dolibarr** user & administrator manual.

- French and English, online and as a PDF.
- Published to GitHub Pages.

## How it is built

This repository holds **no manual content** — only the deploy workflow. The manual lives in the module
repository [hmunyeku/communicationhub](https://github.com/hmunyeku/communicationhub) under `docs/`
(`communicationhub-user-manual.<lang>.html`, `docs/manual/img/<lang>/`, the PDFs and the fonts).

The workflow [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) checks out that repository,
runs its `scripts/dev/manual/publish_site.py` to assemble the static site, and deploys it to this
repository's GitHub Pages. It runs:

- **instantly** when the module repository changes the manual (it sends a `manual-updated` repository dispatch);
- **once a day** as a safety net;
- **on demand** from the Actions tab (*Run workflow*).

Nothing is committed here by the build — the site is produced and deployed straight from the Pages artifact.

## One-time setup

The module repository `communicationhub` is **private**, so the build needs a token that can read it.

1. Create a **fine-grained Personal Access Token** (GitHub → Settings → Developer settings → Fine-grained tokens)
   with access to **both** `hmunyeku/communicationhub` and `hmunyeku/communicationhub-manual`, permission
   **Contents: Read-only** on the first and **Contents: Read and write** on the second.
2. In **this** repository (`communicationhub-manual`): Settings → Secrets and variables → Actions → add the token as
   **`MANUAL_REPO_TOKEN`** (the build uses it to read the private module repository).
3. In the **module** repository (`communicationhub`): add the same token as **`MANUAL_SITE_TOKEN`** — this is what
   lets a manual change trigger an *instant* rebuild here. Without it, the daily schedule still keeps the site current.
4. Settings → Pages is already set to **Source: GitHub Actions** (done).

> Alternatively, if `communicationhub` is made public, step 1–3 are unnecessary: delete the `token:` line in the
> workflow and the build reads the manual with the built-in token. The daily schedule and *Run workflow* then suffice.

Spanish is not published yet: the module's Spanish translation is incomplete, so its screenshots would be inconsistent.
