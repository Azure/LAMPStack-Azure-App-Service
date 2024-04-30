# Deploy a LAMP App to Azure Action

## Description

This GitHub Action automates the deployment of a LAMP (Linux, Apache, MySQL, PHP) stack on an App Service (App). The action provisions the App and deploys the LAMP stack using an ARM template and parameter file.

This template uses the Azure Linux CustomScript extension to deploy a LAMP application on Ubuntu. It creates an Ubuntu VM, does a silent install of MySQL, Apache and PHP, then creates a simple PHP script. Go to /phpinfo.php to see the deployed page.

**Currently, only works for non-Microsoft tenant accounts due to Storage account policy**

[For more information on this quickstart template](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/lamp-app/)

**This documentation is for v4 of vaibbavisk20/deploy_lamp_app**

## Inputs

Please install the Azure OIDC app from the Github Marketplace to populate the below inputs as secrets in your repo

- **client-id** (required): Client ID used for Azure login.
- **tenant-id** (required): Tenant ID used for Azure login.
- **subscription-id** (required): Azure subscription ID used with the `az login`.
- **resource-group-name** (required): Resource group where Azure resources will be deployed.

Set these values as secrets on your repo where the workflow runs

- **admin-username** (required): Admin username to login to the App.
  
  Username must only contain letters, numbers, hyphens, and underscores and may not start with a hyphen or number.
  Usernames must not include reserved words.
  The value is in between 1 and 64 characters long.
  
- **admin-password** (required): Admin password to login to the App and mySql password.

  Password must have 3 of the following: 1 lower case character, 1 upper case character, 1 number, and 1 special character.
  The value must be between 12 and 72 characters long.

## Usage

Create this workflow in your repo on this path: .github/workflows/workflow_file.yml

```yaml
name: Workflow to deploy LAMP App on Azure

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:

permissions:
  id-token: write
  contents: read

jobs:
  deploy-resources-to-azure:
    runs-on: ubuntu-latest
    steps:
        - name: Checkout master
          uses: actions/checkout@v3
          
        - name: Deploy a LAMP VM to Azure action
          uses: vaibbavisk20/deploy_lamp_app@v2
          with:
            client-id: ${{ secrets.AZURE_CLIENT_ID }}
            tenant-id: ${{ secrets.AZURE_TENANT_ID }}
            subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
            resource-group-name: ${{secrets.AZURE_RG}}
            admin-username: ${{secrets.ADMIN_USERNAME}}
            admin-password: ${{secrets.ADMIN_PASSWORD}}
```
## Output

The action creates a App which can be viewed on portal.azure.com and provides information about the deployed resources.
        
