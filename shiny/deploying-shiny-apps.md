## Deploy a Shiny App to our main Shiny server

Public repos owned by Stats4SD on Github can use a custom GitHub Action to get deployed to our Shiny server. The default action is configured as follows:

- When a new commit is pushed to the `main` branch, the repo is deployed to the staging environment on the shiny server. (https://shiny.stats4sdtest.online)
- When a new release is published, that release is deployed to the live environment on the shiny server (https://shiny.stats4sd.org/)

Comments:

- The action relies on 2 organisation-level GitHub Secrets: 
	- SHINYSERVERURL = the endpoint that the action will POST to.
	- SHINYDEPLOYSECRET = the very long random(ish) string that the server uses to confirm that the POST request is valid. 

> Private repos cannot use organisastion secrets. To deploy a private repo, you must create these 2 secrets within the repo itself. The secrets are available on 1Password (Search for Github Shiny Secret). 
> You will also need to create a custom action, as the organisation-level action will not be available. You can copy the action from the Stats4SD `.github` repo [here](https://github.com/stats4sd/.github/blob/main/workflow-templates/shiny-deployment.yml):
> 

## Deploy a Shiny App to a different server

The server-side component of the deployment process is [this NodeJS application ](https://github.com/stats4sd/shiny-deploy). This can be deployed on any server that needs to run Shiny apps that get updated regularly. 

To setup a Shiny app to deploy to a different server: 
- you will need confirm the secrets match the NodeJS application and server you are targeting.

You may need to deploy to a "staging" and "live" environments that are in different locations. To do this, you should modify the secrets to include 2 separate urls:
- SHINYSERVERURL_STAGING
- SHINYSERVERURL_LIVE
- SHINYDEPLOYSECRET

Next, you should create 2 separate Github Actions - one for each environment. Copy the example actions from the .github repo:
- [Deploy 'main' branch to staging](https://github.com/stats4sd/.github/blob/main/workflow-templates/shiny-deploy-staging-example)
- [Deploy releases to live](https://github.com/stats4sd/.github/blob/main/workflow-templates/shiny-deploy-live-example)

# Using .env files to keep sensitive information out of your code
Sensitive strings, like passwords, API keys, usernames etc  should not be pushed to Github, even within private repos. Instead, create a `.env` file in your project root to store all your sensitive data as environment variables. Use the `dotenv` library, combined with `Sys.getenv()` to pull in the variables when needed.  You should include the `.env` file in your `.gitignore` so it does not get commited. 

To add your .env variables to the server, for now please talk to the Shiny Server admin (Dave, currently...). If this becomes a very common activity, we'll write up a set of instructions to allow others to easily add .env files to apps deployed onto our main Shiny server.
