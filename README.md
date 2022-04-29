# Workflow generic templates

This project contains generic workflow templates for Mono application (Angular + Spring)

# List of workflow available :


## 🔨 Build workflow

### Example Usage
```
  build:
    name: 🔨
    uses: bouygues-construction/workflow/.github/workflows/build.yml@v1-mono
    with:
      APPLICATION_NAME: eparahp 
      PROFILE_ENV: 'dev'
      GITHUB_ENVIRONMENT: development
      RELEASE_INCREMENT: false
      JDK_VERSION: 11
      SKIP_TEST: true

      
```
### Inputs
| Variable  | Example Value &nbsp;| Description &nbsp; | Type | Required | Default |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- |
| APPLICATION_NAME | Eparahp | Name of the application | String | Yes | N/A
| PROFILE_ENV | dev/pro | Profile defined in pom file | String | No | N/A
| GITHUB_ENVIRONMENT | development/production | Environment defined in github environments | String | No | development
| RELEASE_INCREMENT | true/false | Increase version for release or not? | String | No | false
| JDK_VERSION | dev/pro | JDK version | String | No | 8
| SKIP_TEST | true/false | skip maven test | String | No | false
    
## 📝 Quality gate workflow

### Example Usage
```
  quality-gate:
    name: 📝
    needs: build
    uses: bouygues-construction/workflow/.github/workflows/quality-gate.yml@v1-mono
  with:
      CODEQL_CHECKS: false
      SONAR_CHECKS: true
      CUCUMBER_CHECKS: false
      PROFILE_ENV: 'dev'
      GITHUB_ENVIRONMENT: development
      SKIP_TEST: 'false'
      JDK_VERSION: 11     
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}     
```
### Inputs
| Variable  | Example Value &nbsp;| Description &nbsp; | Type | Required | Default |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- |
| CODEQL_CHECKS | true/false | To launch CodeSql or not? | String | false | N/A
| SONAR_CHECKS | true/false | To launch Sonar Scan or not? | String | false | N/A
| CUCUMBER_CHECKS | true/false | To launch Cucumber test or not? | String | false | development
| PROFILE_ENV | dev/pro | Profile defined in pom file | String | No | N/A
| GITHUB_ENVIRONMENT | development/production | Environment defined in github environments | String | No | development
| JDK_VERSION | dev/pro | JDK version | String | No | 8
| SKIP_TEST | true/false | skip maven test | String | No | 

### Secrets
| Variable  | Example Value &nbsp;| Description &nbsp; | Type | Required | Default |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- |
| SONAR_TOKEN | ******** | Secret token to connect SonarQube | String | false | N/A

## 🚚 Publish workflow

### Example Usage
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
### Inputs
| Variable  | Example Value &nbsp;| Description &nbsp; | Type | Required | Default |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- |
| RELEASE_VERSION | ${{ needs.build.outputs.RELEASE_VERSION }} | Version returned from pervious step | String | true | N/A
| APPLICATION_NAME | Name of the application | Same name as other job | String | true | N/A
| ACR_URL | ACR URL | URL of ACR | String | true | development
| ACR_USERNAME | dev/pro | ACR account name | String | true | N/A
| IMAGE_NAME | Name of Image | Environment defined in github environments | String | true | development

### Secrets
| Variable  | Example Value &nbsp;| Description &nbsp; | Type | Required | Default |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- |
| ACR_PASSWORD | Password for ACR account | password of ACR account | String | true | N/A

