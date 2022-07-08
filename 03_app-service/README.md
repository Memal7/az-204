# Create Azure App Service web apps

## Explore Azure App Service
Azure App Service is an HTTP-based service for hosting web applications, REST APIs, and mobile apps. You can develop in your favorite language like .NET, .NET Core, Java, Ruby, Node.js, PHP, or Python, and then run and scale with ease on both Windows and Linux-based environments.

## Azure App Service key features
- **Built in auto-scale support:** Scale up/down or scale in/out depending on the usage of app (Not supported in Free & Shared tiers!)
- **CI/CD support:** Provides many CI/CD platforms such as Azure DevOps, GitHub, GitLab, Bitbucket, or local Git repo
- **Deployment slots:** This feature enables, that you can build, test, and run your apps in non-prod environement (slot) before pushing to production. You can have many slots for apps, e.g. dev deployment slot, test deployment slot, etc. For instance of your app create a staging deployment slot to push your code and test in Azure. If the test succeed, swap the staging deployment slot with the production. All this few cliks. Deployment slots are only available in the Standard and Premium tiers
  
- **App Service on Linux:** Host web apps natively on Linux for supported application stacks. It can also run custom Linux containers.

---

## What's Azure App Service Plan?
- An app in App Service runs always in an App Service Plan
- An App Service Plan defines a set of compute resources for a app to run
- One or more apps can be configured to on run on same computing resources (or on same App Service Plan)
- Each App Service Plan defines:
  - Region
  - Number of VM instances
  - Size of VM instances (Small, Medium, Large)
  - Pricing tier (Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated)
  - The pricing tier of App Service Plan determines what App Service features you get and how much you pay for the plan. Here are the pricing tiers:
    - **Shared compute:** Free and Shared share resource pools of your apps with the apps of other customers. This tier is a base tier that run on the same Azure VMs as other apps. This tier is recommended just for dev and test puposes
  
    - **Dedicated compute:** The basic, Standard, Premium, PremiumV2, PrmiumV3 tiers run apps on dedicated Azure VMs. Only apps in the same App Service Plan share the same compute resources. The higer the tier, the more VM instaces are available for you to scale-out.

---

## How to add, remove, and isolate more capabilities or more features to my app?
- The App Service Plan can be scaled up/down at any time. Just change it's pricing tier
- If your app is in the same App Service Plan with other apps and you want to improve the it's performance by isolating the compute resources. Move the app into a separate App Service plan
- Putting multiple apps on the sam App Service Plan saves money, but you have to be aware about the capacity of your existing App Service Plan and the expected load
- When to isolate your app into a new App Service Plan?:
  - When the app resource is intensive
  - When you  want to scale the app independently from others in existing plan
  - When the app needs resource in a diffrent geographical region

---

## How to deploy an App Service?
- **Automated way:**
  - **Azure DevOps:** Push and build your code in Azure DevOps, run the tests, generate a release from the code, and finally, push your code to an Azure Web App
  - **GitHub:** Azure supports automated deployment directly from GitHub. When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub will be automatically deployed for you
  - **Bitbucket:** Similar to GitHub, you can configure an automated deployment with Bitbucket
- **Manuell way:**
  - **Git:** pushing to the remote repo will deploy your app
  - **CLI:** _webapp up_ is a feature of the _az_ command-line interface that packages your app and deploys it. Unlike other deployment methods, _az webapp up_ can create a new App Service web app for you. if you haven't already created one
  - **ZIP:** Use _curl_ or a similar HTTP utility to send a ZIP of your application files to App Service
  - **FTP/S:** a traditional way of pushing your code to many hosting environments, including App Service
- **Deployment slots:**
  - Whenever possible, use deployment slots when deploying a new production build
  - When using a Standard App Service Plan tier or better, you can deploy your app to a staging deployment slot and then swap your staging and production slots.

---

## Authentication and authorization flows of an app in App Service
- **Authentication flow:** App service supports many providers with SDKs and without SDKs
  - **Without provider SDK (server-directed flow or server flow):**
    - The app delegates federated sign-in to App Service
    - This is typically the case with browser apps, which can present the provider's login page to the user.
    - The server code manages the sign-in process
  - **With provider SDK (client-directed flow or client flow):**
    - The application signs the user in to the provider manually and then submits the authentication token to App Service for validation
    - This is typically the case with browser-less apps, which can't present the provider's sign-in page to the user
    - The application code manages the sign-in process
    
    | Provider                      | Sign-in point                | How-to guidance
    | :---                          | :---                         | :---
	| Microsoft Identity Platform   | /.auth/login/aad	           | App Service Microsoft Identity Platform login
	| Facebook	                    | /.auth/login/facebook        | App Service Facebook login
	| Google	                    | /.auth/login/google          | App Service Google login
	| Twitter	                    | /.auth/login/twitter	       | App Service Twitter login
	| Any OpenID Connectt provider  | /.auth/login/<provider name> | App Service OpenID Connect login

  - **Authorization flow:** On azure Portal you can configure App Service with a number bahviiors when the incoming request isn't authenticated.
    - **Allow unauthenticated request:**
      - This option defers authorization of unauthenticated traffic to your application code
      - For authenticated requests, App Service also passes along authentication information in the HTTP headers.
      - This option provides more flexibility in handling anonymous requests. It lets you present multiple sign-in providers to your users.
    - **Require authentication:**
      - This option will reject any unauthenticated traffic to your application
      - This rejection can be a redirect action to one of the configured identity providers
      - In these cases, a browser client is redirected to _/.auth/login/<provider>_ for the provider you choose
      - If the anonymous request comes from a native mobile app, the returned response is an _HTTP 401 Unauthorized_
      - You can also configure the rejection to be an _HTTP 401 Unauthorized_ or _HTTP 403 Forbidden_ for all requests






