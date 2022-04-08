## Deploy a Shiny App to our main Shiny server

Public repositories owned by Stats4SD on Github can use a custom GitHub Action to get deployed to our Shiny server. To do this:
 
 1. Go to your github repository, and go to the Actions tab. 
 2. Find the "Deploy To Stats4SD Shiny Server Workflow" Action and click "setup this workflow"
  - If your project is recognised as mostly R code, this workflow should appear as one of the 'suggested' workflows at the top of the list.
 3. This will add a new file into your repository at `.github/workflows/shiny-deployment.yml`. Don't edit the file - just click the green "start commit" button in the top-right, then the "commit new file" button to submit.
 
 That's it. Now, your Github repo is setup to do the following:
  - **Deploy to Staging:** every time you deploy to the default / main branch, the action will run and the code will be deployed to the *staging* environment at https://shiny.stats4sdtest.online
 - **Deploy to Production:**: every time you create a new 'release' on Github, this action will run and the code will be deployed to the *live* environment at https://shiny.stats4sd.org.
 
 **Monitoring Deployment / Handling Errors**
 You can check the progress and outcome of any deployment by looking at the Actions tab on the Github repo. You will see a list of previous workflows, with a green tick (success), a red cross (fail) or an orange circle (still in progress...). You can retry failed deployments by clicking on it, then in the top-right selecting "re-run failed jobs". 
 
 Currently, errors trigger an email to support@stats4sd.org, with attached logs to help fix the issue.


## Deploy a **Private** Repository:

- The action relies on 2 organisation-level GitHub Secrets: 
	- SHINYSERVERURL = the endpoint that the action will POST to.
	- SHINYDEPLOYSECRET = the very long random(ish) string that the server uses to confirm that the POST request is valid. 

Private repos cannot use organisastion secrets. To deploy a private repo, you must create these 2 secrets within the repo itself. The secrets are available on 1Password (Search for Github Shiny Secret). 

You will also need to create a custom action, as the organisation-level action will not be available. You can copy the action from the Stats4SD `.github` repo [here](https://github.com/stats4sd/.github/blob/main/workflow-templates/shiny-deployment.yml):



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
