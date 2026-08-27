## Copy the following files to .github/workflows:
* [translate-app.yml](translate-app.yml)
* [update-translate-app.yml](update-translate-app.yml)

## Update .github/AL-Go-Sttings.json
If not exist, add the setting: 

```
"CICDPushBranches": []
```

## Update .github/workflows/CICD.yaml
Remove the following:

```
push:
  paths-ignore:
    - '**.md'
    - '.github/workflows/*.yaml'
    - '!.github/workflows/CICD.yaml'
  branches: [ 'main', 'release/*', 'feature/*' ]
```

## Optional you can create a TranslationInfo.json file in the directory .AL-Go
### TranslationInfo.json Structure

```json
{
  "translateTo": "nl-BE,nl-NL,fr-FR,fr-BE",
  "customTranslations": {
    "fr": [
      {
        "source": "Customer No.",
        "target": "N° Client"
      }
    ],
    "fr-BE": [
      {
        "source": "Customer No.",
        "target": "N° Client"
      }
    ]
  }
}
```

**Parameters:**
- `translateTo`: 
  - `*` - Translate to all supported Business Central languages
  - Comma-separated culture codes (e.g., "nl-BE,fr-FR")
  - Empty/omitted - Uses defaults: nl-BE, nl-NL, fr-FR, fr-BE, en-US, en-GB, de-DE

- `customTranslations`: Optional. Language-specific or culture-specific custom translations that override API results.

### Supported Business Central Languages

- `nl-BE` - Dutch (Belgium)
- `nl-NL` - Dutch (Netherlands)
- `fr-FR` - French (France)
- `fr-BE` - French (Belgium)
- `en-US` - English (United States)
- `en-GB` - English (Great Britain)
- `de-DE` - German (Germany)
- `da-DK` - Danish (Denmark)
- `sv-SE` - Swedish (Sweden)
- `no-NO` - Norwegian (Norway)
- `cs-CZ` - Czech (Czechia)
- `fi-FI` - Finnish (Finland)
- `it-IT` - Italian (Italy)
- `es-ES` - Spanish (Spain)
- `pt-BR` - Portuguese (Brazil)
- `ja-JP` - Japanese (Japan)
- And more...

## Remarks
* The runtime in app.json must be available and correct for the BC version. This will be used for finding the right version of the AL compiler.
* in the .AL-Go/setting.json file, the dependencies must provide the path where it can be found.
* External dependencies will fail, because we can't download them.
