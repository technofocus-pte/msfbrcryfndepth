## Usecase 1 - Build and Deploy the Contoso Chef Application with Rayfin

### Scenario

Contoso Chef is a modern recipe-sharing platform that enables food enthusiasts to discover recipes, create and manage their own culinary content, upload recipe images, and collaborate with other users through likes and comments. To accelerate application development while minimizing infrastructure complexity, Contoso Chef leverages **Rayfin on Microsoft Fabric** to provide a fully managed backend, authentication, database services, and application hosting.

As a developer, your goal is to deploy the Contoso Chef application to Microsoft Fabric, connect it to a managed Rayfin backend, and explore how Rayfin simplifies the process of building, deploying, and maintaining scalable full-stack applications. Throughout this lab, you will provision Fabric resources, deploy the application, validate Microsoft SSO authentication, create recipes, and learn how to update and redeploy application components efficiently.

### Introduction

Building modern applications often requires developers to manage multiple services, including authentication, databases, APIs, storage, and application hosting. Rayfin simplifies this process by providing a managed backend platform that integrates directly with Microsoft Fabric, enabling developers to focus on application functionality rather than infrastructure management.

In this lab, you will deploy the **Contoso Chef** sample application using Rayfin on Microsoft Fabric. You will create a Fabric workspace, configure the development environment, deploy a managed backend, launch the application, and explore its core recipe-sharing features. You will also learn how local development and production deployments can seamlessly share the same backend services, enabling rapid application development and deployment workflows. The Contoso Chef application demonstrates how Microsoft Fabric and Rayfin can be used together to build fast, secure, and resilient cloud-native applications with Microsoft Entra authentication and managed data services.

### Objectives

- Create and configure a Microsoft Fabric workspace.

- Validate the required development tools and environment.

- Clone and configure the Contoso Chef application source code.

- Install project dependencies and authenticate with Rayfin.

- Deploy a managed backend and application using Rayfin on Microsoft Fabric.

- Verify the deployment and access the application through Microsoft Entra SSO.

- Run the application locally while connecting to the deployed Fabric backend.

- Create, edit, and manage recipes within the Contoso Chef application.

- Upload media assets and interact with recipe content through comments and likes.

- Redeploy frontend and database updates using Rayfin deployment commands.

- Understand how Rayfin accelerates full-stack application development on Microsoft Fabric.


### Prerequisites

Before starting, make sure you have:

1. Node.js 20 or later installed. Check with "node -v" in a terminal. If it's missing or older, install it from nodejs.org.

1. Git installed, to clone the sample repository.

1. Access to a Microsoft Fabric workspace where you have permission to create an app (ask your Fabric admin if unsure).

1. A terminal / command-line application (PowerShell, Terminal, etc.).

 ## Prerequisites


- **GitHub Account: You are expected to have your own GitHub login credentials. If you do not have an account, please create one by visiting: +++https://github.com/signup?user_email=&source=form-home-signup+++**


## Task 0: Create a GitHub account

In this task, you create a new **Github account** with the same tenant credentials that you have used in this lab.

1. Navigate to the GitHub with this link +++https://github.com/+++ and click on **Sign up** to proceed further.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/imga1.png)

1. Now, to create a new GitHub account, enter the +++email+++, **password** and a unique **username** and click on **Continue** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/imga2.png)

1. Start the **verification** **puzzle** by following the instruction on the screen. Click on **Submit.**

1. Enter the +++verification+++ **code** you've received on your mail.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/imga3.png)

1. Now, with your credentials sign-in to GitHub and click on **Sign in.**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/imga4.png)

1. You have successfully created a new account on GitHub.


# Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and reports.

1. Open your browser, navigate to the address bar, and type or paste the following URL: +++https://app.fabric.microsoft.com/+++ then press the **Enter** button and sign in with your credentials

    | Credential | Value |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image1.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image2.png)

1. In the portal, switch to **Fabric** Mode before proceeding to create workspace.

    ![A screenshot of a computer AI-generated content may be incorrect.](./media/image3.png)

1. In the Workspaces pane, click on **+New workspace** tile

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image4.png)

1. In the **Create a workspace** pane that appears on the right side, enter the following details, and click on the **Apply** button.

    | Setting | Value |
    |---|---|
    | Name | +++Rayfin-Fabric@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | **Small dataset storage format** |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image5.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image6.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image7.png)

1. Once the workspace loads, copy the URL from the browser address bar. Remove anything after the workspace ID. The URL should look like https://app.fabric.microsoft.com/groups/*{workspace-id}*.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image8.png)


# Task 2: Clone the lab repository

1. Open your browser, navigate to the address bar, type or paste the following URL: +++https://github.com/technofocus-pte/rayfin-on-microsoft-fabric+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image9.png)

1. Click on **fork** to fork the repo. Give unique name to the repo and click on **Create repo** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image10.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image11.png)

1. In your GitHub repository, click **Code** and then select the **Copy** icon next to the repository URL to copy the clone link for use in the upcoming steps.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image12.png)


# Task 3: Validate Required Software Setup

1. In your Windows search box, type Visual Studio, then click on **Visual Studio Code**.

    ![A screenshot of a computer Description automatically > generated](./media/image13.png)

1. Launch Visual Studio Code and sign in using the **Sign In** button located in the upper-right corner of the application window.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image14.png)

1. Click on **Continue with GitHub**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image15.png)

1. On the GitHub sign-in page, enter the provided username or email address and password, then click **Sign in** to authenticate and connect GitHub with Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image16.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image17.png)

1. Click **Open** to open the selected repository and begin working in Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image18.png)

1. In Visual Studio Code, click the **More Actions (⋯)** menu, select **Terminal**, and then choose **New Terminal** to open a new integrated terminal window

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image19.png)

1. In the terminal, navigate to the **Labfiles** directory

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image20.png)

1. Run the following commands in your terminal and confirm each returns a version number:

    `node --version`
    >
    `npm --version`
    >
    `git --version`
    >
    > **+++copilot --version+++**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image21.png)


# Task 4: Get the source code

1. Clone the repository and move into the app's source folder:

    `git clone https://github.com/<youraccount>/rayfin-on-microsoft-fabric.git`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image22.png)

1. Change the directory

    `cd rayfin-on-microsoft-fabric/src`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image23.png)

    All remaining commands are run from this "src" folder.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image24.png)


# Task 5: Install dependencies

1. In the integrated terminal, run the following command to install all required project dependencies:

    `npm install`

    This downloads all the packages the app needs (React, Vite, Rayfin CLI, etc.).

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image25.png)

1. In the integrated terminal, run the following command to sign in to Rayfin and authenticate your deployment environment

    `npx rayfin login`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image26.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image27.png)

1. If your account has access to more than one Fabric tenant, pin to the right one:

    `npx rayfin login --tenant <tenant-id>`

    This opens a browser sign-in prompt — complete it with your Fabric/Entra ID credentials.


# Task 6: Deploy the backend and the app

1. After signing in, run the following command to start the deployment process:

1. In the integrated terminal, run the following command, replacing ***{workspace-id}*** with the **Microsoft Fabric workspace ID** that you saved in Task 1

    `npx rayfin up --workspace-id <workspace-id>`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image28.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image29.png)

    This single command:

    - Provisions a Rayfin item in your Fabric workspace

    - Applies the database schema

    - Builds the Vite frontend (npm run build:fabric)

    - Deploys the static site bundle

    - Writes the live URLs and publishable key into a new .env.fabric file

    This can take a few minutes the first time.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image30.png)


1. Copy the application **URL** and keep it available for use in the next tasks.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image31.png)


# Task 7: Open and verify the deployed app

1. Check the deployment status and get the hosting URL:

    `npx rayfin up status`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image32.png)

    Open the printed URL in a browser and sign in with your Microsoft Fabric account. On first sign-in, the app automatically imports a 100-recipe catalogue into the database (takes about 30 seconds, with a progress banner). This "self-seeding" is idempotent, so revisiting later won't create duplicates.

    > **Note on anonymous access:** At release, unauthenticated (anonymous)
    > access to Fabric data sources is not supported. Every visitor —
    > including the discover page and "unlisted" recipe links — must sign in
    > via Microsoft Fabric SSO.

1. Open the Microsoft Fabric portal at +++https://app.fabric.microsoft.com+++.

1. Open the Rayfin_Fabric@lab.LabInstance.Id workspace you created in Exercise 1.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image33.png)

1. Select **contoso-chef** app

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image34.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image36.png)


# Task 8: Run the app locally against the deployed backend

1. For local development with hot-reload:

    `npm run dev`

    This regenerates a .env file from .env.fabric and starts the Vite dev server at http://localhost:5173, connected to the same Fabric backend you deployed in Step 4. Sign in via the popup that appears.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image37.png)

1. Copy the local frontend URL shown in the terminal, which should be similar to http://localhost:5173, and open it in a new browser tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image38.png)

1. Select the **Sign in with Microsoft** button, sign in with the same Microsoft account you used for Fabric:


    - **Email**: +++@lab.CloudPortalCredential(User1).Username+++

    - **TAP**: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image39.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image40.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image42.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image43.png)


1. In the terminal output from the **npx rayfin up** command you ran in Task 6, Step 3, find the **static hosting URL** printed by the CLI. The URL should look similar to https://*{random-prefix}*.webapp.rayfin….com.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image44.png)

1. Open the hosting URL in a new browser tab. The app displays the same auth page as your local frontend, including the **Sign in with Microsoft** button, because both use the same Fabric backend.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image44.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image45.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image46.png)


# Task 9: Explore core features

1. Browsing the discover page (public recipes) and select My recipes

1. Then, click **My recipes** in the navigation menu and create your own recipe

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image47.png)

1. Enter the details for a new recipe of your choice. For this lab, a sample recipe is used to demonstrate the recipe creation process.

    > **Description**: Fluffy, fool-proof steamed white rice — a simple side
    > that pairs with almost anything.
    >
    > **Type**: main (or "side", if that option exists)
    >
    > **Cuisine**: Global / Asian
    >
    > **Origin country**: (optional — leave blank or pick where you learned
    > it)
    >
    > For the ingredients and steps sections further down the form:
    >
    > **Ingredients:**
    >
    > 1 cup white rice (basmati or jasmine)
    >
    > 2 cups water
    >
    > 1/2 tsp salt
    >
    > 1 tsp butter or oil (optional)
    >
    > **Steps:**


    - Rinse the rice under cold water until it runs clear, to remove excess starch.

    - Add rice, water, salt, and butter/oil to a pot; bring to a boil uncovered.

    - Once boiling, reduce heat to low, cover tightly, and simmer for 15 minutes without lifting the lid.

    - Remove from heat and let it rest, covered, for 5 minutes.

    - Fluff with a fork before serving.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image48.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image49.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image50.png)


1. Liking and commenting on recipes

1. Uploading a cover image when creating/editing a recipe. Browse to **C:\LabFiles\** on your VM, then select ***SteamredRice*** file and click on **Open** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image51.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image52.png)

1. Click **Save recipe**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image54.png)


# Task 10: Redeploying after changes

1. Deploy the updated backend API metadata and frontend:

    `npx rayfin up`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image55.png)

1. To redeploy only the frontend (static web application) after making UI changes, run the following command in the integrated terminal

    > **+++npx rayfin up staticapp deploy+++** \# Redeploy frontend only

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image56.png)

1. To apply only the database schema changes without redeploying other project components, run the following command in the integrated terminal

    > **+++npx rayfin up db apply+++** \# Apply schema changes only

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image57.png)

1. Open the application using the Hosting URL generated during deployment. After the application loads successfully, click **My recipes** in the navigation menu to access and manage your personal recipes

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image58.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image59.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image60.png)

1. Open the Microsoft Fabric portal at +++https://app.fabric.microsoft.com+++.

1. Open the Rayfin\_+++Fabric@lab.LabInstance.Id+++ workspace you created in Exercise 1.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image33.png)

1. Select **contoso-chef** app

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image34.png)

1. Click **My recipes** in the navigation menu to access and manage your personal recipes.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image61.png)


# Task 11: Create Data agent

1. Now, click on +++RayfinFabric@lab.LabInstance.Id+++ on the left-sided navigation pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image62.png)

1. In the **Fabric** home page, select **+New item.** In the Filter by item type search box, enter **+++data agent+++** and select the Data agent

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image63.png)

1. Enter **+++Rayfin_agent+++** as the Data agent name and select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image64.png)

1. In **Rayfin_agent** page, select **Add a data source**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image65.png)

1. In the OneLake catalog tab, select the **contoso-chef** SQL database and select **Add.**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image66.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image67.png)

1. Select all tables

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image68.png)

1. Enter the following text and click on the **Submit icon** as shown in the below image.

    +++How many recipes are available in the Contoso Chef application?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image69.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image70.png)

1. Enter the following text and click on the **Submit** icon as shown in the below image.

    +++What are the newest recipes added to the platform?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image71.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image72.png)

    +++Which cuisine is most popular among users?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image74.png)

1. Select **Publish**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image75.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image76.png)

1. After publishing, verify the success message and select **View publishing details** to review the agent deployment.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image77.png)


# Task 12: Clean up resources

1. Select your workspace, the +++Rayfin_Fabric@lab.LabInstance.Id+++ from the left-hand navigation menu. It opens the workspace item view.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image78.png)

1. Select the ... option under the workspace name and select **Workspace settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image79.png)

1. Navigate to the bottom of the General tab and select **Remove this workspace**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image80.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2001/media/image82.png)
