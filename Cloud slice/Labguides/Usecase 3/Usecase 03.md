# Use Case 3: Implement a Public Transportation Data Application with Rayfin in Fabric Apps

*Stream live vehicle positions into Microsoft Fabric Real-Time Intelligence, model them in a semantic model, and publish a secure real-time tracking app with Rayfin.*

| **Item**           | **Details**                                                                                                  |
|--------------------|--------------------------------------------------------------------------------------------------------------|
| Platform           | Microsoft Fabric - Real-Time Intelligence, Power BI semantic models, Fabric apps (preview) with Rayfin       |
| Level              | Intermediate - Advanced                                                                                      |
| Estimated duration | 60 minutes                                                                                                   |
| Approach           | Fabric portal + Azure portal + GitHub + VS Code + PowerShell terminal                                        |
| What you build     | **Helsinki Public Transport Realtime Tracker** - a live map and fleet dashboard of public transport vehicles |

**Introduction**

In this lab, you build and deploy a real-time public transportation monitoring application on **Microsoft Fabric**. The solution combines several Fabric workloads:

- **Eventstream** ingests the live vehicle feed.

- **Eventhouse (KQL database)** stores and transforms the events in real time.

- A **notebook** produces the events on a schedule.

- A **Power BI semantic model** exposes the data for analytical queries (DAX).

- **Fabric apps** with **Rayfin** host the web application, with Microsoft Entra sign-in.

You deploy the whole back end with a set of Python scripts, register a Microsoft Entra application so that the browser can query the semantic model on behalf of the signed-in user, and then publish the app to Fabric with a single Rayfin command.

**Scenario**

**Helsinki Region Transport (HSL)** publishes a live GTFS-Realtime feed with the position of every bus, tram, train, and ferry in the region. A city operations team wants a dashboard that shows where each vehicle is right now, how many vehicles are moving, their average speed, and the routes in service, without managing servers, streaming clusters, or a separate identity system.

The team chooses **Microsoft Fabric** for real-time ingestion and analytics and **Rayfin** to host the web app inside their Fabric workspace. In this lab, you are a data engineer on that team. You deploy the real-time back end, build the semantic model, secure the app with Microsoft Entra ID, and publish the **Helsinki Public Transport Realtime Tracker**.

**Objectives**

After completing this lab, you will be able to:

- Set up a Microsoft Fabric workspace and the development environment

- Deploy an Eventhouse, KQL schema, Eventstream, notebook, and schedule with Python deployment scripts

- Publish a semantic model on top of the KQL database and bind its credentials to end-user OAuth2 (SSO)

- Register a Microsoft Entra single-page application and grant it delegated Power BI permissions

- Run the Rayfin app locally and deploy it to Fabric with **npx rayfin up**

- Validate the end-to-end solution, from the live feed to the map in the app

**Architecture**

| **Component**                           | **Role in the lab**                                                                                                             |
|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| GitHub fork of TF-Rayfin                | Source code: deployment scripts and the app in **templates/helsinki-public-transport**                                          |
| Eventhouse HSL_EH (KQL database)        | Stores **raw_events**, **vehicle_positions**, **trip_updates**, **alerts**, and the **last_vehicle_position** materialized view |
| Eventstream ES_HSL_Events               | Routes events from the custom source to the Eventhouse                                                                          |
| Notebook NB_Helsinki_Realtime_Tracker   | Reads the HSL feed and sends events to the Eventstream; runs hourly                                                             |
| Semantic model HSL_KQL_SM               | DAX layer over the KQL database, queried by the app                                                                             |
| Entra app helsinki-public-transport-spa | Lets the browser call the Power BI API (**Dataset.Read.All**) on behalf of the user                                             |
| Fabric App helsinki-public-transport    | Rayfin backend, Entra sign-in, and the hosted web app                                                                           |

**Prerequisites**

- **Node.js 20 or later**, **Git**, **Python 3.10 or later**, and the **Azure CLI** installed on the lab VM.

- A **GitHub account** to fork the repository. If you don't have one, create it at +++https://github.com/signup+++.

- Access to a Microsoft Fabric workspace on Fabric capacity where you can create items.

- Permission to create app registrations in Microsoft Entra ID.

**Note:** Fabric apps is a preview feature. If a label or message differs slightly from this guide, follow what your portal and CLI show.

**Important:** Several commands in this lab print IDs, keys, and tokens. Don't share screenshots of them, and don't commit **.env** files to source control.

## Exercise 1: Set up the environment

### Task 1: Create a Fabric workspace

1.  Open your browser, go to +++https://app.fabric.microsoft.com/+++, and sign in with your credentials.

| **Username** | **+++@lab.CloudPortalCredential(User1).Username+++** |
|----|----|
| **Temporary Access Pass** | **+++@lab.CloudPortalCredential(User1).AccessToken+++** |

![](./media/image1.png)

![](./media/image2.png)

2.  If the portal opens in **Power BI**, select the experience switcher at the bottom left, and then select **Fabric**.

![](./media/image3.png)

3.  On the Fabric home page, select **New workspace**.

![](./media/image4.png)

![](./media/image5.png)

4.  In the **Create a workspace** pane, enter +++Rayfin-Fabric@lab.LabInstance.Id+++ as the **Name**, and then expand **Advanced**.

![](./media/image6.png)

5.  Under **License mode**, select **Fabric**, verify the capacity under **Details**, and then select **Apply**.

![](./media/image7.png)

6.  When the workspace opens, copy the **workspace ID** from the browser address bar and save it in Notepad. It is the GUID after **/groups/** in the URL **https://app.fabric.microsoft.com/groups/\<workspace-id\>/...**

![](./media/image8.png)

### Task 2: Fork the lab repository

1.  In a new browser tab, go to +++https://github.com/technofocus-pte/TF-Rayfin+++ and sign in to GitHub.

2.  Select **Fork**. Keep the **Repository name** **TF-Rayfin**, and then select **Create fork**.

![](./media/image9.png)

3.  In your fork, select **Code**, verify that the **HTTPS** tab is selected, and then select the **Copy** icon to copy the repository URL. Save it in Notepad.

![](./media/image10.png)

### Task 3: Validate the required software

1.  In the Windows search box, type +++Visual Studio Code+++, and then select **Visual Studio Code**.

2.  Select the **More Actions (...)** menu, select **Terminal**, and then select **New Terminal**.

![](./media/image11.png)

3.  In the terminal, go to the lab files folder:

    +++cd LabFiles+++

![](./media/image12.png)

4.  Run the following commands and verify that each returns a version number:

    +++node --version+++

    +++npm --version+++

    +++git --version+++

    +++python --version+++

    +++az --version+++

![](./media/image13.png)

**Note:** The screenshot also shows **copilot --version**. GitHub Copilot isn't required for this lab.

### Task 4: Get the source code and sign in to Azure

1.  Clone your fork. Replace **\<your-account\>** with your GitHub username, or paste the URL you copied in Task 2:

    +++git clone https://github.com/<your-account>/TF-Rayfin.git+++

![](./media/image14.png)

2.  Go to the app folder:

    +++cd TF-Rayfin/templates/helsinki-public-transport+++

![](./media/image15.png)

3.  Sign in to Azure:

    +++az login+++

![](./media/image16.png)

4.  In the **Sign in** window, select **Work or school account**, and then select **Continue**.

![](./media/image17.png)

5.  Enter your lab username and select **Next**, enter the Temporary Access Pass and select **Sign in**, and then select **Yes** on the **Sign in to all apps and websites on this device?** page.

![](./media/image18.png)

![](./media/image19.png)

![](./media/image20.png)

6.  In the terminal, type **1** to select the lab subscription and press **Enter**.

![](./media/image21.png)

7.  Get your tenant ID and save it in Notepad:

    +++az account show --query tenantId -o tsv+++

![](./media/image22.png)

8.  Sign in again, pinned to that tenant. Replace **\<tenant-id\>** with the value from the previous step. Select your account in the **Sign in** window, and then type **1** to select the subscription.

    +++az login --tenant <tenant-id>+++

![](./media/image23.png)

![](./media/image24.png)

9.  Set the environment variables used by the deployment scripts. Replace the placeholders with your tenant ID and the workspace ID from Task 1:

    +++$env:FABRIC_TENANT_ID = "<tenant-id>"+++

    +++$env:FABRIC_WORKSPACE_ID = "<workspace-id>"+++

![](./media/image25.png)

**Important:** These variables exist only in the current terminal. If you open a new terminal, set them again before you run the scripts.

## Exercise 2: Deploy the real-time back end

The **fabric/deploy** folder contains numbered Python scripts. Run them in order; each one creates one part of the back end and prints the IDs that the next steps use.

| **Script**             | **What it creates**                                                 |
|------------------------|---------------------------------------------------------------------|
| 01_eventhouse.py       | Eventhouse **HSL_EH** and its KQL database                          |
| 02_kql_schema.py       | KQL tables, parse functions, materialized view, and update policies |
| 03_eventstream.py      | Eventstream **ES_HSL_Events** (source, stream, destination)         |
| 04_notebook.py         | Producer notebook **NB_Helsinki_Realtime_Tracker**                  |
| 05_semantic_model.py   | Semantic model **HSL_KQL_SM** over the KQL database                 |
| 06_bind_credentials.py | End-user OAuth2 (SSO) credentials for the semantic model            |
| 07_schedule.py         | Hourly schedule for the notebook                                    |
| 09_verify.py           | End-to-end health check                                             |

### Task 1: Create the Eventhouse and the KQL schema

1.  Go to the deployment folder and create the Eventhouse:

    +++cd fabric/deploy+++

    +++python 01_eventhouse.py+++

![](./media/image26.png)

2.  Verify that the output shows **created eventhouse 'HSL_EH'** with the **kql_database_id** and **kusto_cluster** values. The script creates the Eventhouse, which also creates a KQL database with the same name, and reads back its query URI and database ID.

3.  Apply the KQL schema:

    +++python 02_kql_schema.py+++

4.  Verify that all 11 commands show **ok** and the output ends with **schema applied**.

![](./media/image27.png)

**Note:** The script sends each command in **fabric/eventhouse/DatabaseSchema.kql** to the Kusto management endpoint. It creates the **raw_events**, **vehicle_positions**, **trip_updates**, and **alerts** tables, three parse functions, the **last_vehicle_position** materialized view, and the update policies that load **raw_events** into the typed tables. All commands are idempotent, so you can run the script again safely.

### Task 2: Create the Eventstream and the notebook

1.  Create the Eventstream:

    +++python 03_eventstream.py+++

2.  Verify that the output shows **created eventstream 'ES_HSL_Events'** with the **eventstream_id** and **source_id** values.

![](./media/image28.png)

**Note:** The script creates the source, stream, and destination nodes and points the destination at the Eventhouse. The destination may show **Creating** at first; it changes to **Running** after a few minutes.

3.  Publish the producer notebook:

    +++python 04_notebook.py+++

4.  Verify that the output shows **created notebook 'NB_Helsinki_Realtime_Tracker'** and its ID.

![](./media/image29.png)

**Note:** The notebook resolves the Eventstream connection string at run time through the Fabric REST API, so it never stores a secret.

### Task 3: Create the semantic model and bind its credentials

1.  Publish the semantic model:

    +++python 05_semantic_model.py+++

2.  Verify that the output shows **created semantic model 'HSL_KQL_SM'**. Copy the GUID after **Set VITE_PBI_DATASET_ID ... to:** and save it in Notepad as the **dataset ID**.

![](./media/image30.png)

![](./media/image31.png)

**Note:** The script publishes the TMDL definition in **fabric/semantic-model/** and points every partition to your Kusto cluster and database.

3.  Bind the semantic model credentials:

    +++python 06_bind_credentials.py+++

4.  Verify that the output ends with **final credentialType: OAuth2**.

![](./media/image32.png)

**Important:** Don't skip this step. The script takes over the dataset, seeds its data source with a Kusto token, and then switches it to end-user OAuth2 (SSO). Without it, DAX queries fail with **HTTP 400 DatasetExecuteQueriesError**, and the app shows only zeros.

5.  Schedule the notebook to run every hour:

    +++python 07_schedule.py+++

6.  Verify that the output shows **schedule: HTTP 201** with **"type": "Cron"** and **"interval": 60**.

![](./media/image33.png)

## Exercise 3: Configure security and authentication

The app queries the semantic model directly from the browser through the Power BI REST API. For that, it needs a Microsoft Entra application with delegated **Dataset.Read.All** permission.

### Task 1: Register the Microsoft Entra application

1.  Create the app registration:

    +++az ad app create --display-name "helsinki-public-transport-spa" --sign-in-audience AzureADMyOrg+++

![](./media/image34.png)

2.  In the JSON output, find the **appId** value. This is the **client ID** of the application.

![](./media/image35.png)

![](./media/image36.png)

3.  Store the client ID in a variable and display it. Save the value in Notepad:

    +++$appId = az ad app list --display-name "helsinki-public-transport-spa" --query "[0].appId" -o tsv+++

    +++echo $appId+++

![](./media/image37.png)

### Task 2: Grant the Power BI permission

1.  Open the Azure portal at +++https://portal.azure.com+++. In the search box, enter +++Microsoft Entra ID+++, and then select **Microsoft Entra ID**.

![](./media/image38.png)

2.  Under **Manage**, select **App registrations**. On the **Owned applications** tab, select **helsinki-public-transport-spa**.

![](./media/image39.png)

3.  Under **Manage**, select **API permissions**.

![](./media/image40.png)

4.  Select **Add a permission**.

![](./media/image41.png)

5.  In the **Request API permissions** pane, select the **APIs my organization uses** tab.

![](./media/image42.png)

6.  Enter +++Power BI Service+++ in the search box, and then select **Power BI Service**.

![](./media/image43.png)

7.  Select **Delegated permissions**.

![](./media/image44.png)

8.  Expand **Dataset**, select **Dataset.Read.All**, and then select **Add permissions**.

![](./media/image45.png)

9.  Verify that **Power BI Service - Dataset.Read.All (Delegated)** appears under **Configured permissions**.

![](./media/image46.png)

**Note:** **Admin consent required** shows **No**, so each user consents the first time they connect live data in the app (Exercise 4, Task 3).

### Task 3: Configure the production environment file

1.  In the VS Code terminal, go back to the app folder:
```
cd ..\..
$datasetId = "<add dataset-id>"
echo $env:FABRIC_WORKSPACE_ID
```

2.  Create the **.env.production.local** file. Replace the placeholders with the **dataset ID** (Exercise 2, Task 3), the **client ID** (Task 1), and your **tenant ID**:

```powershell
@"
VITE_PBI_DATASET_ID=<dataset-id>
VITE_PBI_CLIENT_ID=<client-id>
VITE_PBI_TENANT_ID=<tenant-id>
"@ | Set-Content -Path .env.production.local -Encoding utf8
```

![](./media/image47.png)

**Note:** Vite builds these values into the production app. If any value is missing or wrong, the app can't query the semantic model.

3.  Point the app's semantic-model connector at your workspace and semantic model. In **rayfin/rayfin.yml**, the **transitModel** connector ships with placeholder IDs (00000000-…). Replace \<dataset-id\> with the dataset ID from Exercise 2, Task 3, and run:
```
(Get-Content rayfin\rayfin.yml) `
  -replace 'workspaceId: 00000000-0000-0000-0000-000000000000', "workspaceId: $env:FABRIC_WORKSPACE_ID" `
  -replace 'itemId: 00000000-0000-0000-0000-000000000000', "itemId: $datasetId" |
  Set-Content rayfin\rayfin.yml

Select-String -Path rayfin\rayfin.yml -Pattern "workspaceId|itemId"
```
**Important:** Don't skip this step. Without it, **npm run dev** and **npx rayfin up** fail with **Semantic-model connector 'FabricSemanticModel' (key 'transitModel') declares a missing or invalid Config.ItemId**. If \$env:FABRIC_WORKSPACE_ID is empty, set it again first (Exercise 1, Task 4).

### Task 4: Verify the back end

1.  From the app folder (**helsinki-public-transport**), run the verification script:

    +++python fabric/deploy/09_verify.py+++

2.  Verify that the output shows **PASS** for **eventstream**, **kusto**, and **dax**. The script checks, in order: (1) every Eventstream node is **Running**, (2) the Kusto row counts and data freshness, and (3) the same DAX query that the app sends.

![](./media/image48.png)

**Tip:** If the all-time position count is large but the live vehicle view is empty, ingestion stopped more than about two hours ago. That is a pipeline or capacity issue, not an authentication issue. Run the notebook manually from the Fabric portal and run the check again.

3.  In the Fabric portal, open your workspace. Verify that it contains the Eventstream **ES_HSL_Events**, the Eventhouse and KQL database **HSL_EH**, the semantic model **HSL_KQL_SM**, and the notebook **NB_Helsinki_Realtime_Tracker**.

![](./media/image49.png)

## Exercise 4: Run and deploy the Rayfin application

### Task 1: Install dependencies and run locally

1.  In the app folder, install the project dependencies:

    +++npm install+++

![](./media/image50.png)

**Note:** If **npm install** stops with **EIDLETIMEOUT** or **EPERM**, the lab network timed out. Run +++npm config set fetch-timeout 600000+++ and +++npm config set fetch-retries 5+++, delete the node_modules folder with +++Remove-Item -Recurse -Force node_modules+++, and then run +++npm install+++ again.

2.  Verify that the Rayfin CLI is version **1.36.2** or later:

    +++npx rayfin --version+++

**Important:** Fabric static hosting requires Rayfin CLI 1.35.0-alpha.1413 or later. If the version is older, sync your fork with technofocus-pte/TF-Rayfin on GitHub (**Sync fork** \> **Update branch**), run +++git pull+++, and then run +++npm install+++ again.

3.  Allow Rayfin to store its sign-in token on the lab VM. The VM has no OS keychain, so without this setting the CLI stops with **OS keychain is not available and plaintext token storage is not enabled**:

    +++$env:RAYFIN_ENCRYPTION_FALLBACK_ENABLED = "true"+++

    +++[Environment]::SetEnvironmentVariable("RAYFIN_ENCRYPTION_FALLBACK_ENABLED","true","User")+++

4.  Start the app in development mode:

    +++npm run dev+++

5.  When prompted **Enter a Fabric workspace name to deploy to**, enter your workspace name, for example +++Rayfin-Fabric@lab.LabInstance.Id+++, and press **Enter**.

**Note:** If the CLI asks **Who should be able to open your deployed app?**, select **Only users with permissions to view the Fabric item (recommended)**. Anonymous access to Fabric data isn't supported.

![](./media/image51.png)

**Note:** The first **npm run dev** creates the Rayfin backend item in your workspace (without static hosting) so that the local app has a backend to connect to.

6.  Wait until the output shows **Project "helsinki-public-transport" is now deployed to Fabric!** and **Local: http://localhost:5173/**.

![](./media/image52.png)

7.  Hold **Ctrl** and select **http://localhost:5173/** to open the app.

![](./media/image53.png)

8.  Select **Sign in with Fabric** and sign in with your lab account.

![](./media/image54.png)

9.  The tracker opens. The vehicle counters show **0** and a banner shows **Connect live data**, because the app isn't yet registered as a redirect URI for the Entra application. You fix this in the next tasks.

![](./media/image55.png)

### Task 2: Deploy the app to Fabric

1.  In the VS Code terminal, press **Ctrl + C** to stop the dev server. Deploy the app. Replace the placeholders with your workspace ID and tenant ID:

    +++npx rayfin up --workspace-id <workspace-id> --tenant <tenant-id> -y+++

![](./media/image56.png)

2.  Wait for the deployment to finish. Copy the URL after **Your app is live at:** (it ends in **.webapp.fabricapps.net**) and save it in Notepad as the **hosting URL**.

![](./media/image57.png)

### Task 3: Add the hosting URL as a redirect URI

1.  In the Azure portal, go to **Microsoft Entra ID \> App registrations**, and select **helsinki-public-transport-spa**.

2.  Under **Manage**, select **Authentication (Preview)**, and then select **Add Redirect URI**.

![](./media/image58.png)

3.  Under **Web applications**, select **Single-page application**.

![](./media/image59.png)

4.  In the **Redirect URI** box, enter your **hosting URL**, and then select **Configure**.

![](./media/image60.png)

5.  Verify the notification **Successfully updated helsinki-public-transport-spa**.

![](./media/image61.png)

**Tip:** To use **Connect live data** in the local app too, add +++http://localhost:5173+++ as a second single-page application redirect URI.

### Task 4: Rebuild and redeploy

1.  In the VS Code terminal, verify that **.env.production.local** contains your three values. If the file is missing or the values are wrong, create it again in the app folder (Exercise 3, Task 3):

    +++Get-Content .env.production.local+++

![](./media/image62.png)

2.  Rebuild and redeploy the app:

    +++npx rayfin up --workspace-id <workspace-id> --tenant <tenant-id> -y+++

![](./media/image63.png)

3.  Verify that the output shows **Redeployment detected - reusing Rayfin item** and ends with **Your app is live at:** and the hosting URL.

![](./media/image64.png)

## Exercise 5: Validate the end-to-end solution

### Task 1: Open the hosted app and connect live data

1.  Hold **Ctrl** and select the hosting URL in the terminal. If VS Code asks **Do you want Code to open the external website?**, select **Open**.

![](./media/image65.png)

2.  Select **Sign in with Fabric** and sign in with your lab account.

![](./media/image66.png)

3.  The tracker opens with zeros and a **Connect live data** banner.

![](./media/image67.png)

4.  Select **Connect live data**.

![](./media/image68.png)

5.  On the **Permissions requested** page for **helsinki-public-transport-spa** (**View all datasets** and **Sign you in and read your profile**), select **Accept**.

![](./media/image69.png)

6.  Verify that the status changes to **LIVE**. The counters now show the number of **Vehicles**, **Moving** vehicles, **Avg speed**, **Routes**, and **Positions / h**; the map shows the vehicles, and the **Fleet** list shows the fastest vehicles.

![](./media/image70.png)

![](./media/image71.png)

**Note:** The map tiles show **API KEY REQUIRED** watermarks because the sample uses map tiles without an API key. The vehicle data isn't affected.

### Task 2: Open the app in the Fabric portal

1.  In the Fabric portal, open your workspace and select the **helsinki-public-transport** App item. Notice its **SQL database** and **SQL analytics endpoint**.

![](./media/image72.png)

2.  The app opens inside the Fabric portal. The banner explains that live data needs a sign-in that can't be done inside the portal frame. Select **Open in a new tab** to use the app with live data.

![](./media/image73.png)

3.  In the workspace navigation pane, expand **helsinki-public-transport** and select its **SQL database**.

![](./media/image74.png)

4.  Review the database tables (**Users**, **VehicleComments**) that Rayfin created for the app.

![](./media/image75.png)

**Important:** This database is a child item of a Fabric App. Don't edit its schema in the portal, because that can break the app.

## Exercise 6: Clean up resources

1.  In the Fabric portal, open your workspace **Rayfin-Fabric\<number\>**, and then select **Workspace settings**.

![](./media/image76.png)

2.  On the **General** tab, scroll to **Delete workspace**, and then select **Remove this workspace**.

![](./media/image77.png)

3.  In the **Delete workspace?** dialog, select **Delete**, and verify the notification **Workspace deleted**.

![](./media/image78.png)

![](./media/image79.png)

**Note:** The screenshots show a different workspace name. Delete the workspace you created in this lab.

4.  In the Azure portal, delete the **helsinki-public-transport-spa** app registration, or run:

    +++az ad app delete --id $appId+++

**Summary**

In this lab, you built a real-time public transportation application on Microsoft Fabric. You created a workspace and deployed the back end with Python scripts: an Eventhouse with KQL tables, functions, a materialized view, and update policies; an Eventstream; an hourly producer notebook; and a semantic model bound to end-user OAuth2 credentials. You registered a Microsoft Entra single-page application with delegated **Dataset.Read.All** permission, configured the production environment, and verified the pipeline end to end. Finally, you ran the Rayfin app locally, published it to Fabric with **npx rayfin up**, added its hosting URL as a redirect URI, and connected live data to see Helsinki's vehicles moving on the map in real time.
