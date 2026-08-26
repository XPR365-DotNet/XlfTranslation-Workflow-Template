# XLF Translation Workflow Templates

This repository contains reusable GitHub Actions workflows for the Translate App pipeline.

## Workflows

- **translate-app-download-artifacts.yml**: Discovers apps to translate and downloads BC artifacts, the AL compiler, and dependency symbols
- **translate-app-compile.yml**: Compiles AL apps and generates `.g.xlf` translation files
- **translate-app-translate.yml**: Translates XLF files via the translation service
- **translate-app-pr-management.yml**: Commits translations directly to the triggering branch (falling back to a PR if the branch is protected) and manages CI/CD dispatch

These four workflows are meant to run as sequential jobs (`needs:`) in a single pipeline, in
the order listed above. The self-hosted runners are a pool, not one dedicated machine, so
nothing on the local filesystem is assumed to survive between jobs - each job hands its output
to the next via `actions/upload-artifact` / `actions/download-artifact`:

| Artifact | Produced by | Consumed by | Contents |
| --- | --- | --- | --- |
| `translate-build-inputs` | Download Artifacts | Compile | `.apps-order.json`, `.alpackages/**` (symbols), `.altools/**` (AL compiler) |
| `translate-generated-xlf` | Compile | Translate | `.apps-order.json`, `**/Translations/*.g.xlf` |
| `TranslationFiles` | Translate | PR Management | `**/Translations/*.xlf` |

`.apps-order.json` records each app's folder as a path **relative to the repo root** (not an
absolute path), since it is read back by jobs running on a different runner with a different
checkout path. Don't hand-edit it.

## Usage

Add a workflow to your repository that calls these four jobs in sequence - see
[`examples/translate-app.yml`](examples/translate-app.yml) for a full, ready-to-use example
(triggers, permissions, and job wiring included). The core pattern:

```yaml
jobs:
  DownloadArtifacts:
    uses: XPR365-DotNet/XlfTranslation-Workflow-Template/.github/workflows/translate-app-download-artifacts.yml@main
    secrets: inherit

  Compile:
    needs: DownloadArtifacts
    uses: XPR365-DotNet/XlfTranslation-Workflow-Template/.github/workflows/translate-app-compile.yml@main

  Translate:
    needs: Compile
    uses: XPR365-DotNet/XlfTranslation-Workflow-Template/.github/workflows/translate-app-translate.yml@main
    secrets: inherit

  PRManagement:
    needs: Translate
    uses: XPR365-DotNet/XlfTranslation-Workflow-Template/.github/workflows/translate-app-pr-management.yml@main
    with:
      startCicd: ${{ github.event_name == 'workflow_dispatch' && inputs.startCicd || false }}
    secrets: inherit
```

Required secrets in the calling repository: `GHTOKENWORKFLOW` (PAT or GitHub App definition,
used to download cross-org dependency apps and to push the translation commit) and
`XLF_TRANSLATION_FUNCTION_KEY` (the translation service's host/master key).

## Keeping the orchestrator in sync

The four `uses: ...@main` jobs above always resolve to this repo's latest `main`, so they
auto-update on every run. The orchestrator file itself (`examples/translate-app.yml`) is
different: it's a plain copy that lives in each consumer repo, so triggers/permissions/job
wiring changes made here don't reach it automatically. [`examples/update-translate-app.yml`](examples/update-translate-app.yml)
is a companion scheduled workflow (daily at 22:00 UTC) you can also copy into your repo - it
diffs your local orchestrator against this repo's `examples/translate-app.yml` and opens a PR
(or commits directly, if the branch isn't protected) whenever they've drifted.

## Reference Strategy

Using @main ensures all repositories automatically get the latest fixes and improvements deployed here.
