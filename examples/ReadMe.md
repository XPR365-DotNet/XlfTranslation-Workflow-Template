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
