# Usecase 01 - Develop a CRUD-enabled Todo application with Rayfin in Fabric Apps

## Introduction

This use case demonstrates how to develop and deploy a CRUD (Create, Read, Update, Delete) enabled To-Do application using **Rayfin** within **Microsoft Fabric Apps**. The exercise provides hands-on experience in creating a Microsoft Fabric workspace, deploying a prebuilt To-Do App template, configuring the development environment, running the application locally, and publishing it to Fabric. Participants learn how Fabric Apps simplifies full-stack application development by providing integrated backend services, authentication, data storage, and deployment capabilities.

### Objective

- Create and configure a Microsoft Fabric workspace for application
  development.

- Deploy a To-Do App using the Rayfin-powered App template available in
  Fabric Apps.

- Set up a local development environment using Visual Studio Code,
  Node.js, and Rayfin tooling.

- Perform CRUD operations on to-do items through the application
  interface.

- Test the application locally and validate functionality.
- Publish the application to Microsoft Fabric using Rayfin deployment
  commands.

- Verify data persistence by confirming that to-do records are stored in
  the underlying Fabric SQL database.

- Understand the end-to-end application lifecycle within the Microsoft
  Fabric ecosystem.


### Prerequisites

Before starting, make sure you have:

1. Node.js 20 or later installed. Check with "node -v" in a terminal. If it's missing or older, install it from nodejs.org.

1. Git installed, to clone the sample repository.

1. Access to a Microsoft Fabric workspace where you have permission to create an app (ask your Fabric admin if unsure).

1. A terminal / command-line application (PowerShell, Terminal, etc.).


### Task 1: Create a Fabric workspace

In this task, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and reports.

1. Open your browser, navigate to the address bar, and type or paste the following URL: +++https://app.fabric.microsoft.com/+++ then press the **Enter** button and sign in with your credentials

    | Credential | Value |
    |---|---|
    | Username | +++@lab.CloudPortalCredential(User1).Username+++ |
    | Password | +++@lab.CloudPortalCredential(User1).Password+++ |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image1.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image2.png)

1. In the portal, switch to **Fabric** Mode before proceeding to create workspace.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image3.png)

1. In the Workspaces pane, click on **+New workspace** tile

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image4.png)

1. In the **Create a workspace** pane that appears on the right side, enter the following details, and click on the **Apply** button.

    | Setting | Value |
    |---|---|
    | Name | +++Rayfin-Fabric-Todoapp@lab.LabInstance.Id+++ |
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | **Small dataset storage format** |

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image5.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image6.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image7.png)

1. Once the workspace loads, copy the URL from the browser address bar. Remove anything after the workspace ID. The URL should look like: https://app.fabric.microsoft.com/groups/*{workspace-id}*.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image8.png)


### Task 2: Create a Fabric App

1. Create a new lakehouse by clicking on the **+New item** button in the navigation bar.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image9.png)

1. In the New item dialog, enter +++app+++ in the search box, and then select **App (preview)** from the search results. Enter +++To do_app+++ for the app name.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image10.png)

1. On the Pick a template to get started page, select the **To-Do App** template.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image11.png)

1. After you select the *To-Do App* template, the app deployment starts automatically. Wait approximately 2 to 3 minutes for the deployment to complete

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image12.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image13.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image14.png)

1. In the Getting started section, under Set up your project, select the **copy icon** to copy the scaffold command to your clipboard.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image15.png)


### Task 3: Deploy the backend and the app

1. In File Explorer, navigate to **C:\LabFiles**, create a new folder named +++Todo-app+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image16.png)

1. In your Windows search box, type Visual Studio, then click on **Visual Studio Code**.

    ![A screenshot of a computer Description automatically generated](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image17.png)

1. In Visual Studio Code, select **File \> Open Folder**, and then browse to and open the **C:\LabFiles\Todo-app** folder.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image18.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image19.png)

1. When the Workspace Trust dialog appears, select **Yes, I trust the authors** to open the folder and enable all features in Visual Studio Code.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image20.png)

1. In Visual Studio Code, click the **More Actions (⋯)** menu, select **Terminal**, and then choose **New Terminal** to open a new integrated terminal window

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image21.png)

1. In the Visual Studio Code terminal, paste the copied scaffold command, and then press enter to create the To-Do app project in the Todo-app folder.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image22.png)

1. Enter +++Y+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image23.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image24.png)

1. Wait for the project scaffolding process to complete. When the Project created successfully! message appears in the terminal, the To-Do app project is ready for development.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image25.png)

1. In the Visual Studio Code terminal, type `cd to-do-app` and press Enter to navigate to the newly created project directory.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image26.png)

1. Edit your app code directly. Run it locally against your Fabric backend.

    `npm run dev`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image27.png)

1. Copy the local frontend URL shown in the terminal, which should be similar to http://localhost:5173, and open it in a new browser tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image28.png)

1. Select the **Sign in with Microsoft** button, sign in with the same Microsoft account you used for Fabric:

    - **Username**: +++@lab.CloudPortalCredential(User1).Username+++
    - **TAP**: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image29.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image30.png)


1. In the Todo App, enter `Create Fabric Workspace` in the input box, and then select **Add** to create a new to-do item.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image31.png)

    `Create Fabric App`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image32.png)

1. In the To-Do list, select the circle next to Create Fabric Workspace to mark the task as completed.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image33.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image34.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image35.png)

1. Back in the Visual Studio Code terminal, stop the Vite dev server by pressing **Ctrl+C**.

1. When you're ready, deploy your updates to Fabric.

    `npx rayfin up`

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image36.png)

1. In the Visual Studio Code terminal output, locate the published application URL, press Ctrl and select the URL (https://happy-pearl-cd18684b37-westus2.webapp.fabricapps.net) to open the deployed application in your default web browser.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image37.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image38.png)

1. Select the **Sign in with Microsoft** button, sign in with the same Microsoft account you used for Fabric:

    - **Username**: +++@lab.CloudPortalCredential(User1).Username+++
    - **TAP**: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image39.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image40.png)


1. Open the Microsoft Fabric portal at +++https://app.fabric.microsoft.com+++.

    Open the **Rayfin\_<Fabric@lab.LabInstance.Id>** workspace you created in Task 1

1. Select **To do_app**

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image41.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image42.png)

1. In the SQL database explorer, expand **To do app \> dbo \> Tables**, and then select the Todos table to verify that the to-do items are stored successfully and that the Create Fabric Workspace task is marked as completed.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image43.png)

1. In the Fabric portal, select the *Rayfin-**Fabric-Todoapp@lab.LabInstance.Id*** workspace from the navigation pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image44.png)

1. Select the ... option under the workspace name and select **Workspace settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image45.png)

1. Navigate to the bottom of the General tab and select **Remove this workspace**.

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image46.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image47.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/msfbrcryfndepth/refs/heads/main/Cloud%20slice/Labguides/Usecase%201/media/image48.png)
