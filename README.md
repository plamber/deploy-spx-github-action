# Deploy SPFx WebPart using GitHub actions and Microsoft 365 CLI

This repository shows how you can deploy an SPFx WebPart to a SharPoint Online site collection using GitHub actions and the [Microsoft 365 CLI](https://pnp.github.io/cli-microsoft365/).

## STEP 1 - Prepare the SharePoint project
First of all I had to prepare my enviornment and create a simple SPFx WebPart. to do this I performed following activities.

### 1.1 - Setup your SharePoint Framework development environment
You can follow [this guide](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-development-environment) to setup your SharePoint Framework development environment. The SPFx project requires node 10.x to run. You can consider [this blogpost](https://www.nubo.eu/Install-Multiple-Node-Versions-On-Windows/) if you plan to use multiple node packages on your machine.

### 1.2 - Create your SPFx WebPart using Typescript and React
This repository is using a basic SPFx WebPart with Typescript and React. I created the package with standard settings using the command `yo @microsoft/sharepoint`.

### 1.3 Verified if bundle and ship works properly
After these changes I am able to perform following actions:
- bundle the project: `gulp bundle --ship`
- package the solution: `gulp package-solution --ship`

## STEP 2 - Prepare the GitHub action to test, bundle, and package the SPFx project
We will use [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions) in this repository to test, build and package the SPFx project. For this step I created a sample action under `.github\workflows\build-spfx.yml` to test, bundle, and package the SPFx solution. The operation will be executed on each push to this project.