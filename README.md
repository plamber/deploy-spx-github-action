# Deploy an SPFx WebPart using GitHub actions and Microsoft 365 CLI

This repository shows how to deploy an SPFx WebPart to a SharePoint Online site collection using GitHub actions and the [Microsoft 365 CLI](https://pnp.github.io/cli-microsoft365/).
In this example, I wanted to cover the scenario where the solution is deployed on a dedicated [site collection app catalog](https://docs.microsoft.com/en-us/sharepoint/dev/general-development/site-collection-app-catalog).

## STEP 1 - Prepare the SharePoint project
First of all, I had to prepare my environment and create a simple SPFx WebPart. to do this I performed the following activities.

### 1.1 - Setup your SharePoint Framework development environment
You can follow [this guide](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-development-environment) to set up your SharePoint Framework development environment. The SPFx project requires node 10.x to run. You can consider [this blog post](https://www.nubo.eu/Install-Multiple-Node-Versions-On-Windows/) if you plan to use multiple node packages on your machine.

### 1.2 - Create your SPFx WebPart using Typescript and React
This repository is using a basic SPFx WebPart with Typescript and React. I created the package with standard settings using the command `yo @microsoft/sharepoint`.

### 1.3 Verify bundle and ship
Verify if the bundle and ship command run properly.
- bundle the project: `gulp bundle --ship`
- package the solution: `gulp package-solution --ship`

## STEP 2 - Prepare the GitHub action to bundle and package the SPFx project
We will use [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions) in this repository to bundle and package the SPFx project. For this step, I created a sample action under `.github\workflows\build-spfx.yml` to perform this operation. It is always executed when you push changes to the repository.

Once you pushed the changes you should see the workflow running on GitHub under `Actions`.

## STEP 3 - Prepare your SharePoint Online target environment
This example will push the SPFx Webpart in this project to a SharePoint Online site collection. This will be done by using the *CLI for Microsoft 365*. The team already created the required GitHub actions to perform operations with the CLI. We just need to collect the necessary values to operate. We will need:
- SharePoint Online Site Collection
- Username and Password of a user with Site Collection Admin rights on that site

## STEP 4 - Grant consent to the CLI for Microsoft 365
The CLI for Microsoft 365 will upload the SPFx package to the SharePoint Online site by using the user we will pick for the deployment. All operations will be executed without interactions in the GitHub action. 

We have to ensure that the account has consented to the CLI to perform actions on his behalf. The simplest way to accomplish this is by installing the CLI on a machine using [this article](https://pnp.github.io/cli-microsoft365/). Afterward, perform an `m365 login` command and log in using the account that will perform the upgrade operation. Once the login operation is successful, you can logout again using `m365 logout`.

## STEP 5 - Create a site collection app catalog for the site in question using the CLI for Microsoft 365
We want to publish the solution to an individual site and not the tenant catalog. To do this, I am going to create an app catalog for the target site collection. We do this using the CLI for Microsoft 365.

```bash
$site = "putYourSharePointOnlineSiteURLHere"
m365 login
m365 spo site appcatalog add --url $site
Write-output "App Catalog Created on " $site
```

## STEP 6 - Prepare the GitHub action to bundle, package, and deploy the SPFx project
We will use [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions) in this repository to bundle and package the SPFx project. For this step, I created a sample action under `.github\workflows\build-spfx-deploy.yml` to perform this operation. It is always executed when you push changes to the repository.

The action will use three secret parameters. In the GitHub repository I am going to add these secrets:
- `spoURL` - Put here the target SharePoint Online site collection URL
- `user` - Put here the username that will operate. He must have site collection admin rights and must have consented the CLI to perform operations on his behalf
- `password` - Put here the password of the user performing the operation

Once you pushed the changes you should see the workflow running on GitHub under `Actions`.
