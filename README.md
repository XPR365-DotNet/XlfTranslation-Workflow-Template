# XLF Translation Workflow Templates

This repository contains reusable GitHub Actions workflows for the Translate App pipeline.

## Workflows

- **translate-app-download-artifacts.yml**: Downloads BC artifacts and discovers apps to translate
- **translate-app-compile.yml**: Compiles AL apps and generates translation files
- **translate-app-translate.yml**: Translates XLF files via translation service
- **translate-app-pr-management.yml**: Creates PRs with translations and manages CI/CD dispatch

## Usage

Use these workflows in your repository by referencing them in your Translate App workflow:

```yaml
jobs:
  DownloadArtifacts:
    uses: XPR365-DotNet/XlfTranslation-Workflow-Template/.github/workflows/translate-app-download-artifacts.yml@main
    secrets: inherit
```

## Reference Strategy

Using @main ensures all repositories automatically get the latest fixes and improvements deployed here.
