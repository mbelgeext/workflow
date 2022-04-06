# Workflow generic templates

This project contains generic templates to launch workflows

**List of workflow available :**


🔨 Build workflow

```
  build:
    name: 🔨
    uses: bouygues-construction/workflow/.github/workflows/build.yml@v1-back
    with:
      APPLICATION_NAME: *****
      PROFILE_ENV: *****
      #GITHUB_ENVIRONMENT: *****
      #RELEASE_INCREMENT: true
```


📝 Quality gate workflow

```
  quality-gate:
    name: 📝
    needs: build
    uses: bouygues-construction/workflow/.github/workflows/quality-gate.yml@v1-back
    with:
      PROFILE_ENV: *****
      #SONAR_CHECKS: true
      #CODEQL_CHECKS: true
      #CUCUMBER_CHECKS: true
      #GITHUB_ENVIRONMENT: *****
    secrets:
      SONAR_TOKEN: ${{ secrets.***** }}
```

🚚 Publish workflow

```
  publish:
    name: 🚚
    needs: [build, quality-gate]
    uses: bouygues-construction/workflow/.github/workflows/publish.yml@v1-back
    with:
      RELEASE_VERSION: ${{ needs.build.outputs.RELEASE_VERSION }} #do not modify this
      APPLICATION_NAME: *****
      ACR_URL: https://*****.azurecr.io/
      ACR_USERNAME: *****
      IMAGE_NAME: *****.azurecr.io/*****
      #GITHUB_ENVIRONMENT: *****
    secrets:
      ACR_PASSWORD: ${{ secrets.***** }}
```

🚀 Launch CD workflow

```
  launch-cd:
      name: 🚀
      needs: [build, publish]
      uses: bouygues-construction/workflow/.github/workflows/launch-cd.yml@v1-back
      with:
        RELEASE_VERSION: ${{ needs.build.outputs.RELEASE_VERSION }}
        AZURE_PROJECT_URL: https://dev.azure.com/BYCNITOPS/*****
        AZURE_PIPELINE_NAME: *****
        APPLICATION_NAME: *****
        PROFILE_ENV: dev
        IMAGE_NAME: acrbcnzakstemp.azurecr.io/*****
        #GITHUB_ENVIRONMENT: *****
      secrets:
        AZURE_PIPELINE_TOKEN: ${{ secrets.AZURE_PIPLINE_TOKEN }}
```
