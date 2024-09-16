---
updated: 11 Sep 2024
authors:
  - John-David Tan
---

# Webapp

The following details how to set up the webapp on your local machine.

## Dependencies needed

| Dependency          | Version                                                                                        | Remarks                                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Python              | [>=3.10](https://python.org/)                                                                  | Recommended version would be 3.11 as that is what the Azure instance runs. |
| NodeJS              | [^18.19.1 / ^20.11.1 / ^22.0.0](https://nodejs.org/en/download/package-manager)                | Recommended version would be 22 as it has the longest active support.      |
| Angular CLI         | [18.0.0](https://angular.dev/tools/cli)                                                        | Recommended version would be 18.0.0 as it was instantiated as such.        |
| Azure Developer CLI | [Latest Possible](https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd) | To use the latest version, update whenever prompted.                       |

## Setup Local Development

### 1. Create an Azure account

Head to the [Azure Portal](https://portal.azure.com/) and create an account.

Once you have an account, an administrator will need to invite you.

Once invited, ensure you have set the current directory to `Default Directory` with domain `trialgenaiprojectoutlook.onmicrosoft.com`.

Check if you are able to view the subscription `Synapxe Data Analytics & AI` and resource group `rg-hhgai-dev-eastus-001`.

### 2. Create Local Azure Environment

Follow the instructions [here](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd) to install the Azure Developer CLI. Select the appropriate installation method for your environment. i.e For Windows Powershell:

`powershell -ex AllSigned -c "Invoke-RestMethod 'https://aka.ms/install-azd.ps1' | Invoke-Expression"`

Verify your installation by running `azd version`.

Upon successful installation, run the following command to setup the azure configuration:

`azd auth login`

Upon successful login, run the following command to get the environment variables:

`azd env refresh -e hhgai-dev-eastus-001`

### 3. Install Dependencies

Proceed to the following websites and follow the instructions to install the dependencies:

- [Python](https://www.python.org/downloads/)
- [NodeJS](https://nodejs.org/en/download/)
- [Angular CLI](https://angular.dev/tools/cli)

### 4. Run Shell Script

Depending on your OS, run either `start.sh` or `start.ps1` for linux-based or Windows-based systems respectively.

On successful completion, you should be able to view the webapp at `http://127.0.0.1:50505`.
