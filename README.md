# Caprover Deploy

A GitHub action to build a docker image, upload it to GitHub Packages and then deploy it to a CapRover server.

# Usage

Follow steps 1 through 8 listed here https://caprover.com/docs/ci-cd-integration.html
Once you have done these, have a Dockerfile in your repo and have the secrets set up, create the action to deploy it using this.

## Inputs

### `token`

**Required** `GITHUB_TOKEN`, needed to upload built image to packages. Provide using `${{ secrets.GITHUB_TOKEN }}`

### `server`

**Required** Captain URL for your CapRover server. Ex. https://captain.example.com.

### `password`

**Required** Admin password for your CapRover server. Please put this in a Repo/Org environment secret and use it like this, `${{ secrets.CAPROVER_PASSWORD }}`.

### `app`

**Required** Application name to deploy this repo to on the CapRover server. Application must have been already created.

## Example usage

```yaml
uses: JasirZaeem/caprover-deploy@v1.1
with:
  token: ${{ secrets.CAPROVER_PASSWORD }}
  server: "https://captain.example.com"
  password: ${{ secrets.CAPROVER_PASSWORD }}
  app: "application"
```

## Example workflow

```yaml
name: 'Deploy to Caprover'

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  caprover-deploy:
    runs-on: ubuntu-latest
    steps:
      # Checking out to the correct repo is required  
      - name: Checkout Repository
        uses: actions/checkout@v2
      - name: Deploy to Caprover
        uses: JasirZaeem/caprover-deploy@v1.1
        with:
          token: ${{secrets.GITHUB_TOKEN}}
          server: "https://captain.example.com"
          password: ${{secrets.CAPROVER_PASSWORD}}
          app: "application"
```