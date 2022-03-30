# RTU Gateway

This project contains generic templates to launch workflows

**List of workflow available :**


🔨 Build workflow

  build:
    name: 🔨
    uses: bouygues-construction/rtu-workflow/.github/workflows/build.yml@test-reusable-workflows
    with:
      APPLICATION_NAME: rtu-gateway
    secrets:
      SONAR_TOKEN: ${{ secrets.RTU_SONAR_TOKEN }}
    
📝 Quality gate workflow

  quality-gate:
    name: 📝
    needs: build
    uses: bouygues-construction/rtu-workflow/.github/workflows/quality-gate.yml@test-reusable-workflows
    secrets:
      SONAR_TOKEN: ${{ secrets.RTU_SONAR_TOKEN }}

🚚 Publish workflow

  publish:
    name: 🚚
    needs: [build, quality-gate]
    uses: bouygues-construction/rtu-workflow/.github/workflows/publish.yml@test-reusable-workflows
    with:
      RELEASE_VERSION: ${{ needs.build.outputs.RELEASE_VERSION }}
      APPLICATION_NAME: rtu-gateway
      ACR_URL: https://acrbcnzakstemp.azurecr.io/
      ACR_USERNAME: acrbcnzakstemp
      IMAGE_NAME: acrbcnzakstemp.azurecr.io/acrbcnzakstemp/rtu-gateway
    secrets:
      ACR_PASSWORD: ${{ secrets.RTU_ACR_PASSWORD }}