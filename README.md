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

1. **Settings → Pages → Build and deployment → Source: GitHub Actions.**
2. For the *instant* update on a manual change, create a fine-grained Personal Access Token with
   **Contents: Read and write** on this repository, and add it in the **module** repository
   (`communicationhub`) as the secret `MANUAL_SITE_TOKEN`. Without it, the daily schedule still keeps
   the site up to date.

Spanish is not published yet: the module's Spanish translation is incomplete, so its screenshots would be inconsistent.
