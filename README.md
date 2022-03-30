# Workflow generic templates

This project contains generic templates to launch workflows

**List of workflow available :**


🔨 Build workflow

```
  build:
    name: 🔨
    uses: bouygues-construction/rtu-workflow/.github/workflows/build.yml@test-reusable-workflows
    with:
      APPLICATION_NAME: *****
    secrets:
      SONAR_TOKEN: ${{ secrets.***** }}
```
    
📝 Quality gate workflow

```
  quality-gate:
    name: 📝
    needs: build
    uses: bouygues-construction/rtu-workflow/.github/workflows/quality-gate.yml@test-reusable-workflows
    secrets:
      SONAR_TOKEN: ${{ secrets.***** }}
```

🚚 Publish workflow

```
  publish:
    name: 🚚
    needs: [build, quality-gate]
    uses: bouygues-construction/rtu-workflow/.github/workflows/publish.yml@test-reusable-workflows
    with:
      RELEASE_VERSION: ${{ needs.build.outputs.RELEASE_VERSION }} # do not modify this
      APPLICATION_NAME: *****
      ACR_URL: https://*****.azurecr.io/
      ACR_USERNAME: *****
      IMAGE_NAME: *****.azurecr.io/*****
    secrets:
      ACR_PASSWORD: ${{ secrets.***** }}
```