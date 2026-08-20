# **Usecase 2 - Build ZAVA Interior Designing App with intelligent insights using Fabric IQ,Data Agents, and Rayfin​**

### Scenario

ZAVA Interiors is a growing interior design and home improvement company that wants to modernize how it manages customer projects, designer assignments, work orders, and collaboration across teams. The company plans to build an intelligent Interior Designing application using Microsoft Fabric, Rayfin, GitHub Copilot, and Fabric Intelligence capabilities.

As a Fabric Developer, you are tasked with rapidly building and deploying a full-stack business application using Rayfin's managed backend services. The application will enable interior designers and project managers to manage projects, collaborate through comments, analyze operational data, and leverage AI-powered insights using Fabric Data Agents and Fabric Intelligence.

### Introduction

Microsoft Fabric provides a unified platform for building intelligent business applications by combining data, AI, analytics, and application services. Rayfin further accelerates development by providing managed backend capabilities such as authentication, data APIs, storage, and deployment.

In this lab, you will build the **ZAVA Interior Designing App**, provision a managed backend in Microsoft Fabric, deploy the application, extend it using GitHub Copilot, and create intelligent analytics experiences using Semantic Models and Fabric Data Agents. By integrating operational data with AI-powered querying, the application enables users to gain business insights through natural language interactions without writing SQL or complex analytics code

### Objectives

- Create and configure a Microsoft Fabric workspace.

- Bootstrap a new application using the Rayfin application template.

- Explore Rayfin architecture, configuration, and data models.

- Provision and deploy a managed backend using Microsoft Fabric.

- Run, test, and validate the application locally and in Fabric.

- Use GitHub Copilot CLI to generate and implement new application features.

- Extend the application with schema-backed entities and business functionality.

- Seed application data for analytics and reporting scenarios.

- Build a Semantic Model using data stored in Microsoft Fabric.

- Configure and publish a Fabric Data Agent.

- Use natural language queries to generate intelligent business insights.

- Understand how Fabric Intelligence, Data Agents, and Rayfin work together to accelerate AI-powered application development


    1. Navigate to the GitHub with this link +++https://github.com/+++ and click on **Sign up** to proceed further.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/imga1.png)

1. Now, to create a new GitHub account, enter the +++email+++, **password** and a unique **username** and click on **Continue** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/imga2.png)

1. Start the **verification** **puzzle** by following the instruction on the screen. Click on **Submit.**

1. Enter the +++verification+++ **code** you've received on your mail.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/imga3.png)

1. Now, with your credentials sign-in to GitHub and click on **Sign in.**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/imga4.png)

1. You have successfully created a new account on GitHub.


# **Exercise 1: Set Up Your Environment**

## Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and reports.

1. Open your browser, navigate to the address bar, and type or paste the following URL: +++https://app.fabric.microsoft.com/+++ then press the **Enter** button and sign in with your credentials

    | Credential | Value |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image1.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image2.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image3.png)

1. In the portal, switch to **Fabric** Mode before proceeding to create workspace.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image4.png)

1. In the Workspaces pane, click on **+New workspace** tile

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image5.png)

1. In the **Create a workspace** pane that appears on the right side, enter the following details, and click on the **Apply** button.

    | Setting | Value |
    |---|---|
    | Name | +++Fabric-Apps@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | **Small dataset storage format** |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image6.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image7.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image8.png)

1. Once the workspace loads, copy the URL from the browser address bar. Remove anything after the workspace ID. The URL should look like https://app.fabric.microsoft.com/groups/*{workspace-id}*.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image9.png)


## Task 2: Validate Required Software Setup

1. In your Windows search box, type Visual Studio, then click on **Visual Studio Code**.

    ![A screenshot of a computer Description automatically > generated](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image10.png)

1. Launch Visual Studio Code and sign in using the **Sign In** button located in the upper-right corner of the application window.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image11.png)

1. Click on **Continue with GitHub**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image12.png)

1. On the GitHub sign-in page, enter the provided username or email address and password, then click **Sign in** to authenticate and connect GitHub with Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image13.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image14.png)

1. Click **Open** to open the selected repository and begin working in Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image15.png)

1. In Visual Studio Code, click the **More Actions (⋯)** menu, select **Terminal**, and then choose **New Terminal** to open a new integrated terminal window

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image16.png)

1. In the terminal, navigate to the **Labfiles** directory

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image17.png)

1. Run the following commands in your terminal and confirm each returns a version number:


    - `node --version`

    - `npm --version`

    - `git --version`

    - `copilot --version`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image18.png)


# **Exercise 2: Bootstrap the App from a Template**

## **Task 1: Clone the lab repository**

1. Open your browser, navigate to the address bar, type or paste the following URL: +++https://github.com/technofocus-pte/ship-ai-apps-fast-with-a-managed-backend-in-microsoft-fabric.git+++

1. Click on **fork** to fork the repo. Give unique name to the repo and click on **Create repo** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image19.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image20.png)

1. In your terminal, from your working folder (e.g. C:\Labfiles), clone the lab repository:

    `git clone https://github.com/<your repo name>/ship-ai-apps-fast-with-a-managed-backend-in-microsoft-fabric.git`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image21.png)

1. Change the directory

    `cd ship-ai-apps-fast-with-a-managed-backend-in-microsoft-fabric`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image22.png)

## **Task 2: Bootstrap a new Rayfin project from the Field Services template**

1. Type (do not run yet) the following command, leaving *{workspace-uri}* in place for now. Adjust the --template path if your clone is elsewhere:

    `npm create -y @microsoft/rayfin@latest -- --project-name field-services-app --template " ./template/field-services-app" --workspace-uri <workspace-uri>`

    > Example:

    npm create -y @microsoft/rayfin@latest -- --project-name field-services-app --template "./template/field-services-app" --workspace-uri https://app.fabric.microsoft.com/groups/dae5eeaf-f597-4686-b15d-6c82e4d99725

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image24.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image25.png)

1. Create a new **field-services-app** folder in your current directory and copy the template files into it


    - Wire the project to your Fabric workspace using the --workspace-uri you provided

    - Run npm install in the new project folder (this can take a couple of minutes on first run)


## Task 3: Explore the generated project and make your first commit

In this task, you will inspect the generated project, initialize a Git repository, and make your first commit. This is important for GitHub Copilot CLI in later exercises, as it will show you clean diffs and what it will be changing when you ask it to generate code.

1. In the **Visual Studio Code** editor, click on **File**, then navigate and click on **Open Folder**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image26.png)

1. Open the **field-services-app** folder in Visual Studio Code by running the following command in the terminal:

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image27.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image28.png)

1. In Visual Studio Code, click the **More Actions (⋯)** menu, select **Terminal**, and then choose **New Terminal** to open a new integrated terminal window

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image29.png)

1. In the pop-up dialog in Visual Studio Code that appears asking if you trust the authors, select **Yes, I trust the authors**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image30.png)

1. In the Visual Studio Code Explorer, expand the **field-services-app** folder that was created by the bootstrap command.

    ![Visual Studio Code Explorer](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image31.png)

1. Review the project structure. It should look something like this:


    - **src/**: React + TypeScript frontend

    - **rayfin/**: Rayfin backend configuration (*rayfin.yml*, data entities)

    - **data/**: Seed data and **the original prompt + dataset used to generate this template** (worth a look if you're curious how it was built)

    - **package.json**: Dependencies and scripts for the project, including **build** and **dev**.


1. In the Visual Studio Code terminal, run this command to initialize a new Git repository:

    `git init`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image32.png)

1. Next, add all the files to the staging area with this command:

    `git add .`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image34.png)

1. Finally, make your first commit with this command:

    `git commit -m "Initial commit - bootstrap from template"`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image35.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image36.png)


# Exercise 3: Explore the App Template

In this exercise, you will quickly inspect the app you created in Exercise 2.

You will look at:

- the main project folders

- the Rayfin configuration file

- the Rayfin data model

- the frontend files you may use later


>[!Tip] Skim the files. You do not need to understand every line yet.

## Task 1: Explore the project structure

1. In Visual Studio Code, expand the **field-services-app** folder.

1. Notice these files and folders:

    | Folder / File | Why it matters |
    |---|---|
    | **rayfin/rayfin.yml** | Turns Rayfin managed services on or off. |
    | **rayfin/data/** | Defines the backend data model. |
    | **src/** | Contains the React + TypeScript frontend. |
    | **src/services/** | Connects the frontend to Rayfin auth and data. |
    | **src/pages/** | Contains the main app pages. |
    | **src/components/ui/** | Contains reusable UI components. |
    | **data/** | Contains the original app spec and seed dataset. |
    | **package.json** | Lists scripts such as **dev**, **rayfin:dev**, **rayfin:db**, **dev:fabric**, **build**, and **test**. |
    | **AGENTS.md** | Gives guidance for AI coding agents. |

1. We will review the Rayfin configuration and data model in more detail in the next tasks, review the rest of the files lightly for now.

1. Ignore the generated configuration files for now. You will come back to the important files later.


## Task 2: Explore the Rayfin YAML configuration

The **rayfin/rayfin.yml** file tells Rayfin which services this app uses.

1. Open **rayfin/rayfin.yml**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image37.png)

1. Find the **services** section.

1. Notice these services:

    | Service | What it does in this app |
    |---|---|
    | **auth** | Two providers are enabled: **password** (used for local development) and **fabric** (Microsoft Entra SSO, used when deployed to Microsoft Fabric). |
    | **data** | Creates a managed SQL database for the app. |
    | **storage** | Is turned off in this template, but can be enabled for blob storage. |
    | **staticHosting** | Hosts the built frontend from *dist/* after deployment. |

1. Do not edit this file yet. For now, just learn where the settings live.


## Task 3: Explore the Data Model

The **rayfin/data/** folder defines the database tables for this app.

1. Open **rayfin/data/**.

1. You should see these files:


    - **schema.ts**

    - **ServicePro.ts**

    - **WorkOrder.ts**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image38.png)


1. Open **schema.ts** and review the imports and the **schema** array. Remember this rule: every entity must be imported and added to the **schema** array.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image39.png)

1. Open **ServicePro.ts** and review the **ServicePro** class and notice these decorators:


    - **@entity()** creates a database table.

    - **@role('authenticated', '\*')** allows signed-in users to create, read, update, and delete rows.

    - **@uuid()**, **@text(...)**, and **@date()** define columns.

    - **user_id** stores the signed-in user's identity from auth.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image40.png)

    >[!Note] **user_id** is **@text()** because auth provider user IDs are strings. It is not a relationship to another Rayfin entity.


1. Open **WorkOrder.ts** and review the **WorkOrder** class and notice that it has similar decorators to **ServicePro**, but also has some new ones:


    - **@set(...)** limits *status* to known values, such as **pending** and **completed**.

    - **@one(() =\> ServicePro, { optional: true })** links a work order to a service pro.

    - **servicePro_id** stores the selected service pro ID for the app to read and write.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image41.png)


1. Remember this rule: these TypeScript entity classes are the source of truth for the database schema.


## Task 4 (Optional): Explore the Frontend Code

You do not need to understand the full frontend in this exercise.

1. Open **src/**.

1. If you have time, look at these files:


    - **services/ServiceContainer.ts**: singleton that bootstraps the Rayfin client and auto-selects the right auth provider (password locally, Fabric Entra in production).

    - **services/rayfin/RayfinFieldService.ts**: CRUD operations for *ServicePro* and *WorkOrder* using the typed Rayfin data API.


1. Keep these ideas in mind:


    - The frontend uses types generated from the Rayfin schema.

    - If you change a field in **rayfin/data/**, TypeScript can help find frontend code that needs updating.

    - The app does not use a hand-written REST client.


# Exercise 4: Run the App Locally

In this exercise, you will run the Field Services app locally.

You will:

- **Provision a Fabric backend** for the app with one command

- Start the **frontend dev server** locally Then you will sign in, create a Service Pro profile, complete a work order, and test the manager view.


## Task 1: Provision the Fabric backend

The **rayfin up** command provisions a managed backend (database, auth, data API) in your Fabric workspace and wires your local frontend to talk to it.

1. Use the Visual Studio Code terminal from the previous exercise. It should already be in the **field-services-app** folder.

1. Provision the Fabric backend by running:

    `npx rayfin up --encryption-fallback-enabled`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image42.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image43.png)

1. Watch the terminal for progress on each step. The first run takes a couple of minutes.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image44.png)

1. Copy the application URL and keep it available for use in the next tasks.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image45.png)

    >[!Tip] The CLI saves the deployment details to **rayfin/.deployments.json** so subsequent **rayfin up** runs update the same deployment instead of creating a new one.


## Task 2: Start the frontend

The frontend is the React app that users interact with.

1. In the same terminal, start the Vite dev server:

    `npm run dev`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image46.png)

    Because **rayfin up** already wrote the backend URL and publishable key into **.env.local**, Vite picks them up automatically. Your locally-served frontend talks to the freshly provisioned Fabric backend with no extra configuration.

1. Copy the local frontend URL shown in the terminal, which should be similar to http://localhost:5173, and open it in a new browser tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image47.png)

1. Confirm that the app sign-in page opens.

1. Confirm that the app sign-in page opens.


## Task 3: Sign up as a Service Pro

The authentication page includes a **"Sign in with Microsoft"** button, and the backend uses Microsoft Entra (Fabric SSO) for sign-in.

1. Select the **Sign in with Microsoft** button. Since you already have an active SSO session from Exercise 1, you should be signed in automatically without needing to enter credentials again. Otherwise, sign in with the same Microsoft account you used for Fabric:


    - **Email**: +++@lab.CloudPortalCredential(User1).Username+++

    - **TAP**: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image48.png)


1. After successful sign-in, a Microsoft Fabric dialog will pop up asking you to allow the app to use your Microsoft Fabric credentials. Select **Accept** to continue.

1. After successful authentication, you should land in the Service Pro First Access view where you can create your profile.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image49.png)

1. Create your Service Pro profile by entering your name and relevant skills.

1. For the skills, enter a few relevant skills such as:

    `painting, hanging, drilling`

1. Complete the profile creation by selecting **Create Profile**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image50.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image51.png)

1. After your profile is created, you should see the **Jobs** view with the seeded work order for you to test with.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image52.png)

1. Accept the work order by selecting the **Accept job** button on the work order card.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image53.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image54.png)

1. Mark the work order complete by selecting the **Mark done** button on the work order card.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image55.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image56.png)


## Task 4: Explore the manager view

The same app also includes a manager view.

1. Open a new browser tab and navigate to the manager URL:

    +++http://localhost:5173/manager/+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image57.png)

1. Review the manager view and create a new work order by completing the form and selecting **Create order**.

    > Name: +++Adam+++
    >
    > Address:+++1234 Example Street 3 Suite 100 Springfield,IL 627045
    > United States+++
    >
    > Task : +++Hang large painting in living room+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image58.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image59.png)

1. Assign the work order to your Service Pro profile.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image60.png)

1. Select **Jobs** in the top navigation bar.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image61.png)

1. Confirm that the assigned work order appears for the Service Pro.

1. Back in the Visual Studio Code terminal, stop the Vite dev server by pressing **Ctrl+C**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image62.png)


# Exercise 5: Verify the Production Deployment on Microsoft Fabric

In Exercise 4, you ran **npx rayfin up** to provision the Rayfin backend in Microsoft Fabric. That command also built the frontend by running **npm run build:fabric**, uploaded the compiled app to Microsoft Fabric's managed static hosting, and deployed the schema.

In this exercise, you will verify the live deployment created by **rayfin up**. There is no separate production publishing step for this lab: each **rayfin up** updates the deployment associated with your selected Microsoft Fabric workspace.

You will:

- Open the hosted app URL generated by **rayfin up**

- Sign in with Microsoft Fabric

- Confirm that the hosted app uses the same backend, database, and schema as the app you tested locally

- Optionally inspect the deployed app and SQL Database items in the Microsoft Fabric portal


## Task 1: Open the live app

1. In the terminal output from the **npx rayfin up** command you ran in Exercise 4\>Task 4, find the **static hosting URL** printed by the CLI. The URL should look similar to https://*{random-prefix}*.webapp.rayfin….com.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image63.png)

    ![Rayfin up command output](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image64.png)

    >[!Tip] If you missed the hosting URL, run **npx rayfin up** status from the **field-services-app** folder to print the current deployment details.

1. Open the hosting URL in a new browser tab. The app displays the same auth page as your local frontend, including the **Sign in with Microsoft** button, because both use the same Fabric backend.

1. Select **Sign in with Microsoft** just like you did in Exercise 4.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image65.png)

1. After authentication completes, confirm that you land in the Service Pro view.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image66.png)

1. Confirm that you can see the same profile and work orders you created in the previous exercise. This verifies that the hosted app is using the same database as the app you tested locally.

1. Navigate to /manager/ and create a couple of new work orders.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image67.png)

    At this point, you have verified that the app is live. Any user with access to your Microsoft Fabric workspace can open the hosted app and use the deployed backend.

    Tip

    **Need separate dev and production environments?** Run npx rayfin up against a second Microsoft Fabric workspace to create a separate deployment with its own backend, database, and frontend. Then switch between deployments anytime with npx rayfin up switch. Each deployment is tracked independently in **rayfin/.deployments.json**.


## Task 2: Inspect the deployment in Fabric

Let's take a look at the deployed app and database in the Microsoft Fabric portal.

1. Open the Microsoft Fabric portal at +++https://app.fabric.microsoft.com+++.

1. Open the +++Fabric-Apps-@lab.LabInstance.Id+++ workspace you created in Exercise 1.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image68.png)

1. Confirm that the workspace contains a **Fabric data app** item and a **SQL Database** item. The **ServicePro** and **WorkOrder** tables are stored in the SQL Database item.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image69.png)

1. Select the Fabric SQL Database item to open it, and in the left explorer, expand the **field-services-app** database and then expand **dbo \> Tables** to see the **ServicePro** and **WorkOrder** tables.

1. Select each table to see the data contained within it.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image70.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image71.png)

    ![Fabric SQL Database tables](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image72.png)


# Exercise 6: Add a Feature with Copilot CLI

In the previous exercises, you deployed the Field Services app to Microsoft Fabric and confirmed that the hosted app uses the same Rayfin backend as your local development experience. In this exercise, you will use GitHub Copilot CLI to implement a schema-backed comments feature for work orders.

The comments feature gives each work order a conversation thread where service pros and managers can discuss job details over time. Implementing this feature requires changes across the Rayfin data model, permissions, typed data access, and React user interface.

By completing this exercise, you will:

- Review the project-specific agent instructions that guide GitHub Copilot CLI.

- Launch GitHub Copilot CLI from the Field Services app project.

- Generate a Rayfin-backed **Comment** entity and comments user interface.

- Review the generated schema, permission, data access, and frontend changes.

- Apply the schema update to the Microsoft Fabric backend.

- Redeploy the app and validate the comments workflow locally and in the hosted app.


## Task 1: Review the project agent instructions

The app template includes agent instructions that help GitHub Copilot CLI generate Rayfin code that follows the project's conventions.

The Rayfin agent skill provides domain guidance for entities, decorators, permissions, schema updates, and typed client usage. The project-level **AGENTS.md** file adds repository-specific context, including architecture notes, important files, and implementation conventions.

1. In Visual Studio Code, Open **AGENTS.md** at the project root.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image73.png)

1. Review the guidance so you understand the context GitHub Copilot CLI will use while editing this project.

1. Open **package.json** and confirm that **@microsoft/rayfin-mcp** is listed in the development dependencies.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image74.png)

    >[!Tip] The template installs the Rayfin agent skill automatically. If you start from a blank project in the future, install the agent files by running npx rayfin init ai-files install from the project folder.


## Task 2: Launch GitHub Copilot CLI

Start GitHub Copilot CLI from the app project folder so it can read the project files, agent instructions, and Rayfin configuration.

1. Open a terminal in Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image75.png)

1. Launch GitHub Copilot CLI:

    `copilot –yolo`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image76.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image77.png)

    >[!Tip] The --yolo option automatically approves tool calls such as file edits and terminal commands. Use it only in a controlled lab workspace that does not contain secrets, production data, or changes you cannot easily revert.

1. In the GitHub Copilot CLI prompt, type the following command and press Enter to sign in:

    `/login`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image78.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image79.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image80.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image81.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image82.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image83.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image84.png)

1. In the GitHub Copilot CLI prompt, open the model picker by typing the following command and enter:

    `/model`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image85.png)

1. Select **GPT-5.4** by using the arrow keys and enter, and choose **low** reasoning effort.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image86.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image87.png)

    >[!Tip] The model selection applies to the current GitHub Copilot CLI session. If you exit and start GitHub Copilot CLI again, repeat this step.


## Task 3: Generate the comments feature

Use GitHub Copilot CLI to generate the comments feature without applying the backend schema change yet.

1. In the GitHub Copilot CLI prompt, paste the following implementation request:

    ```
    Create a Comment entity for the comments feature. The entity should:
    1. Be located at rayfin/data/workOrderComment.ts
    2. Have these fields: id (uuid), content (text, min:1 max:1000), userId (text), createdAt (date), workOrderId (uuid)
    3. Include a one-to-one relationship to WorkOrder via the workOrderId field
    4. Use @entity() and @role('authenticated', 'read', 'create') decorators
    5. Register the Comment entity in rayfin/data/schema.ts (import it, add to FieldServiceSchema type, add to schema array)
    6. Ensure all other code that uses client.data.Comment has the correct imports

    Generate the code only; do not apply the backend schema change.
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image88.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image89.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image90.png)
    
    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image91.png)

1. If you face any issue where Copilot only shows the code instead of creating the files, create the files manually

1. In the Explorer, right-click rayfin/data/ and select **New File**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image92.png)

1. Name it +++WorkOrderComment.ts.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image93.png)

1. Copy this into it:

    ```
    import {
      date,
      entity,
      one,
      role,
      text,
      uuid,
    } from '@microsoft/rayfin-core';

    import { WorkOrder } from './WorkOrder.js';

    @entity()
    @role('authenticated', '*')
    export class Comment {
      @uuid() id!: string;
      @text({ min: 1, max: 1000 }) content!: string;
      @text() userId!: string;
      @uuid() workOrderId!: string;
      @one(() => WorkOrder, { optional: true }) workOrder?: WorkOrder;
      @date() createdAt!: Date;
    }
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image94.png)
    
    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image95.png)

1. Make sure schema.ts has:

    ```
    import { ServicePro } from './ServicePro.js';
    import { WorkOrder } from './WorkOrder.js';
    import { Comment } from './WorkOrderComment.js';

    export type FieldServiceSchema = {
      ServicePro: ServicePro;
      WorkOrder: WorkOrder;
      Comment: Comment;
    };

    export const schema = [ServicePro, WorkOrder, Comment];
    ```


## Task 4: Review the generated implementation

Before applying backend changes, review the files generated by GitHub Copilot CLI.

1. Open the Visual Studio Code Source Control view.

    ![Visual Studio Code Source Control](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image96.png)

1. Review each changed file.

1. Confirm that the implementation includes a comment entity, schema registration, permission logic, data access updates, and user interface updates.

1. If you prefer the terminal, run **git status** or **git diff** from the **field-services-app** folder to inspect the same changes.

    Expected changes include:

    - A new file under `rayfin/data/`, such as `WorkOrderComment.ts`
    - An update to `rayfin/data/schema.ts`
    - New or updated files under `src/components/`, `src/pages/`, `src/hooks/`, or `src/services/`
    - A comments user interface that is collapsed by default

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image97.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image98.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image99.png)


    The implementation should rely on Rayfin decorators, schema generation, permission policies, and the typed client. You should not need to create a hand-written REST endpoint, migration script, or authorization middleware for this feature.


## Task 5: Apply the schema update and redeploy

The implementation request asked GitHub Copilot CLI to generate code without applying backend changes. To test the feature, apply the new schema to the Microsoft Fabric backend and redeploy the app.

1. Exit GitHub Copilot CLI by typing +++/exit+++ in the prompt.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image100.png)

1. In a terminal in the **field-services-app** folder, apply the database schema update:

    > **+++npx rayfin up db apply+++**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image101.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image102.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image103.png)

1. Confirm that the command completes successfully and creates the new **WorkOrderComments** table.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image104.png)

    >[!Note] If the command warns about destructive changes, stop and review the listed operations before continuing. Adding a new entity should not require dropping or renaming existing columns.

1. In the Explorer pane, navigate to ***src* \> *services* \> *rayfin* and open the *RayfinFieldService.ts*** file. Review or update the highlighted workOrder line – 421. Update the statement as required for the lab exercise.

    `await client.data.WorkOrder.update({ id: workOrderId }, { note: nextNote, updatedAt: new Date() });`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image105.png)

1. Deploy the updated backend API metadata and frontend:

    `npx rayfin up --encryption-fallback-enabled`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image106.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image107.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image108.png)

    >[!Note] The full rayfin up flow can also apply pending database migrations. In this lab, you applied the database change first so the schema update is visible as a separate step.


## Task 6: Validate the comments workflow

Test the comments feature from both the local development app and the hosted app.

1. Start the local Vite dev server if it's not already running:

    `npm run dev`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image109.png)

1. Open the local app at the Vite dev server URL, such as <http://localhost:5173>.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image110.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image111.png)

1. Open a work order assigned to your Service Pro profile.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image112.png)

1. Expand the new **Comments** section.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image113.png)

1. Add a comment to the work order.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image114.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image115.png)

1. Navigate to the manager view at **/manager/.**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image116.png)

1. Open the same work order and confirm that the manager can see the comment thread.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image117.png)

1. Add a reply from the manager view.

1. Return to the Service Pro view and confirm that the reply appears in the same thread.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image118.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image119.png)

1. Open the live hosting URL from Exercise 5 and repeat a quick smoke test to confirm that the deployed app also includes the comments feature.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image120.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image121.png)


# Exercise 7: Seed Data for Analysis

In Exercise 6, you implemented and deployed a schema-backed comments feature. In this exercise, you will seed the backend with a richer dataset for the analysis-focused steps that follow.

Seeding data provides a realistic set of service pros, work orders, and statuses so the next analysis-focused exercises have meaningful data to query.

By completing this exercise, you will:

- Use the built-in admin experience to seed the database.

- Confirm that the app now includes a larger, more realistic dataset.

- Optionally add additional comments to improve downstream analysis scenarios.


## Task 1: Open the admin page and seed the database

The template includes an authenticated admin page at **admin** that can generate a larger dataset from src/data/field-service-seed.json.

1. In the hosted app, navigate directly to admin by appending it to the hosting URL.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image122.png)

1. Confirm that the admin page is accessible while you are signed in.

1. Select **Seed data**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image123.png)

1. Wait for the operation to complete.

    >[!Important] If you the data seeding process gives you an error (time out or fetch), you can still continue with the next tasks.

1. Confirm that the completion message reports the number of generated service pros and work orders.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image124.png)

    >[!Tip] The admin page can also reset the data back to a minimal sample dataset. Use reset only if you need to return to a small baseline.


## Task 2: Validate the seeded dataset in the app

After the seed operation completes, confirm that the larger dataset is visible in the main app experiences.

1. Return to the Service Pro view.

1. Confirm that you now see a broader set of work orders across multiple statuses.

1. Open the manager view at /manager/.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image125.png)

1. Confirm that the manager view now shows a larger set of service pros and work orders.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image126.png)

1. Open a few records and verify that seeded data includes varied skills, locations, and operational states.


# Exercise 8: Explore Data with Fabric Intelligence

In Exercise 7, you seeded the database with a realistic production dataset. In this exercise, you will use Microsoft Fabric intelligence capabilities to query that data using natural language.

The Microsoft Fabric SQL Database provisioned by Rayfin is a first-class citizen in your Fabric workspace. You will build a Power BI semantic model over it, publish it to Fabric, and create a **Fabric data agent** that lets you ask questions like "Which Service Pro has the most completed jobs?" without writing a single line of SQL.

By completing this exercise, you will:

- Build and publish a semantic model over the service pro, work order, and comments tables.

- Create a Fabric data agent backed by the semantic model.

- Query the agent with natural-language questions about your production data.

- Optionally call the agent from a Fabric notebook using the Fabric Data Agent SDK.


## Task 1: Build a Semantic model

A semantic model gives the data agent a clean, well-described view of your data — including table relationships, friendly column names, and descriptions so it can translate natural-language questions into accurate queries.

1. Navigate to the Microsoft Fabric portal and open the workspace you created for this lab.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image127.png)

1. Select **+ New item** from the top menu and in the dialog, search and select **Semantic model**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image128.png)

1. In the semantic model creation experience, select **OneLake catalog** as the data source type.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image129.png)

1. Select the **SQL Database** item and select **Connect**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image130.png)

1. Provide a name for the semantic model, in this case, +++FieldServices+++.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image131.png)

1. Select the **ServicePros**, **WorkOrders**, and **WorkOrderComments** tables to include in the model and select **Confirm** to create the model.

    >[!Tip] If you don't see the **WordOrderComments** table when creating a semantic model please wait a few minutes, refresh your page and create the semantic model again.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image132.png)

    >[!Note] This process will take a few minutes to complete.

1. Define the relationship between the **ServicePros** and **WorkOrders** tables by dragging the **servicePro_id** column from **WorkOrders** onto the **id** column in **ServicePros**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image133.png)

1. Power BI Service will automatically determine the cardinality and cross-filtering direction. Select **Save** in the New relationship dialog.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image134.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image135.png)

1. Define the relationship between the **WorkOrders** and **WorkOrderComments** tables by dragging the **id** column from **WorkOrders** onto the **workOrder_id** column in **WorkOrderComments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image136.png)

1. Again, confirm the relationship settings in the New relationship dialog and select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image137.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image138.png)

1. Rename key columns to clear, readable labels. For each of the three tables rename the following columns:

1. To rename a column, select it in the **Data** view, then expand each table to view its columns. Right-click the column and select **Rename**.

    1)  ServicePros:


    - **id** → ServiceProId

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image139.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image140.png)

    2)  WorkOrders:

    - **id** → WorkOrderId

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image141.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image142.png)

    - **scheduledAt** → ScheduledDate

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image143.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image144.png)


1. Select each table and add descriptions to the tables from the **Properties** pane. The descriptions are as follows:

    - **ServicePros:** Service professionals who perform jobs at customer locations. Each has a set of skills and may be assigned to work orders.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image145.png)

    - **WorkOrders:** Work orders for field service jobs. Each has a status, may be assigned to a Service Pro, and has a scheduled date.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image146.png)

    - **WorkOrderComments:** Comments on work orders. Each comment is associated with a single work order and includes content, the authoring user, and a timestamp.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image147.png)

    >[!Tip] Adding descriptions is optional but highly recommended, as the data agent uses them to understand your data and answer questions accurately. Spend a few minutes here, it pays back tenfold in agent answer quality.


## Task 2: Create a Fabric data agent

1. From the left navigation bar, select +++Fabric-Apps@lab.LabInstance.Id.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image148.png)

1. Switch to your workspace on the left navigation pane and select **+ New item** from the top menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image149.png)

1. In the dialog, search for and select **Data agent**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image150.png)

1. Name the agent +++field-services-agent+++.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image151.png)

1. In the **Explorer** panel, select **Add Data \> Data Source**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image152.png)

1. In the **Add data source** dialog, select the **Semantic model** you created in Task 1 and select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image153.png)

1. In the explorer panel, select the tables you want the agent to have access to. Check the boxes next to **ServicePros**, **WorkOrders**, and **WorkOrderComments**.

1. Select **Agent Iinstructions** in the toolbar and add the following domain context, deleting any placeholder text:

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image154.png)

    `This is a field-services work-order management app. Service Pros perform jobs at customer addresses. WorkOrders have a status (pending, assigned, in_progress, completed, needs_followup, cancelled) and may be assigned to one ServicePro. Use Scheduled date for time-based questions about when jobs happen.`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image155.png)

1. Close the Agent Instructions editor.


## Task 3: Query the agent with natural-language questions

Use the agent's chat interface to ask questions about your seeded production data.

1. Check the boxes next to **ServicePros**, **WorkOrders**, and **WorkOrderComments**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image156.png)

1. In the agent chat pane, ask the following questions one at a time and review the results:

    How many work orders do we have in total?

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image157.png)

1. For each response, expand the steps completed to see the generated DAX query and the returned answer.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image158.png)
    
    +++How many work orders are assigned to each Service Pro?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image159.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image160.png)

    +++List all work orders scheduled for the next 7 days.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image161.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image162.png)

    +++Which Service Pros have plumbing in their skills and have no in-progress jobs?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image163.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image164.png)

1. Publish the data agent by selecting **Publish** from the top toolbar. Once published, the agent can be accessed from other Fabric experiences such as notebooks and the standalone Copilot experience.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image165.png)

1. In the publish dialog, provide the following description for the agent:

    This agent answers questions about field service work orders, including assigned service professionals, job statuses, and scheduled dates.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image166.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image167.png)

    >[!Tip] The description is important as it helps other agents and copilot experiences understand the agent's purpose and capabilities. A clear description improves the chances of the agent being recommended for relevant questions.

1. Select the **Standalone Copilot** from the left navigation pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image168.png)

1. In the Copilot text input, ask a question that the agent should be able to answer, such as:

    +++Which Service Pro has the most completed jobs?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image169.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image170.png)


## Task 4: Clean up resources

1. Select your workspace, the +++Fabric_Apps@lab.LabInstance.Id+++ from the left-hand navigation menu. It opens the workspace item view.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image171.png)

1. Select the ... option under the workspace name and select **Workspace settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image172.png)

1. Navigate to the bottom of the General tab and select **Remove this workspace**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image173.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image174.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image175.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%2002/media/image176.png)


### Summary

In this lab, you successfully developed and deployed the ZAVA Interior Designing application using Microsoft Fabric and Rayfin. You learned how to rapidly create a modern business application with managed authentication, database services, and deployment capabilities. Using GitHub Copilot, you enhanced the application with additional functionality while minimizing manual development effort.

You also explored Microsoft Fabric Intelligence capabilities by creating Semantic Models and Fabric Data Agents that enable natural language analysis of operational data. The completed solution demonstrates how organizations can build intelligent, data-driven applications that combine transactional workflows, AI-assisted development, and conversational analytics within a unified Microsoft Fabric environment
