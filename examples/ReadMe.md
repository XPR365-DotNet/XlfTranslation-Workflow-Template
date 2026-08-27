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

## Remarks
* The runtime in app.json must be available and correct for the BC version. This will be used for finding the right version of the AL compiler.
* in the .AL-Go/setting.json file, the dependencies must provide the path where it can be found.
* External dependencies will fail, because we can't download them.
