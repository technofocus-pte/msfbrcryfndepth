# Usecase 01: Develop a CRUD-enabled Todo application with Rayfin in Fabric Apps

*Create a Fabric App item, scaffold the project, add a Todo data model, build a to-do screen by hand, and publish it to Microsoft Fabric - without GitHub Copilot or any AI coding agent.*

| **Item**           | **Details**                                                                                                  |
|--------------------|--------------------------------------------------------------------------------------------------------------|
| Platform           | Microsoft Fabric - Fabric apps (preview), Rayfin CLI 1.36.x                                                  |
| Level              | Beginner - Intermediate                                                                                      |
| Estimated duration | 90 - 120 minutes                                                                                             |
| Approach           | Manual: Fabric portal + VS Code + PowerShell terminal. No GitHub Copilot.                                    |
| What you build     | **Zava Training Team Tracker** - a to-do app with Microsoft Entra sign-in and per-user data                   |
| Source             | Microsoft Learn: [Create your first Fabric apps project](https://learn.microsoft.com/fabric/apps/create-app) |

**Scenario**

The **Zava** training team tracks its lab preparation work (setting up environments, reviewing projects, publishing content) in spreadsheets and chat messages. Tasks get lost, and nobody can see what is still open.

The team wants a simple, secure **task tracker** that every team member can open in the browser, sign in to with their work account, and use to manage *their own* tasks. Because the team already uses Microsoft Fabric, they want the app, its data, and its sign-in to live in their Fabric workspace instead of on a separate web server and database.

In this lab, you are a **Zava** developer. You create a Fabric App item, scaffold a Rayfin project, define a secured **Todo** data model in which each user sees only their own rows, build the **Zava Training Team Tracker** screen, and publish it to Fabric.

**Introduction**

**Fabric apps** (preview) let TypeScript developers build full-stack applications that run on Microsoft Fabric. Each app is an **App** item in a Fabric workspace and is powered by **Rayfin**, which provides a managed SQL database, Microsoft Entra sign-in, an auto-generated data API, and hosting for the web frontend. You describe the backend in a configuration file and TypeScript classes, and the Rayfin CLI deploys it to Fabric.

The Microsoft Learn article offers a Copilot-assisted path. In this lab, you do the same work **by hand**: you edit every file yourself in VS Code and run every command yourself in the terminal, so you understand exactly what each piece does.

**Objectives**

After completing this lab, you will be able to:

- Create a Fabric workspace and an **App (preview)** item in the Fabric portal

- Scaffold a Rayfin project with the CLI and explain its folder structure

- Deploy a Fabric app with **npx rayfin up** and run it locally

- Enable the data service and define a secured entity with decorators and row-level access policies

- Register the entity in the data and shared packages and deploy the table to Fabric

- Build a React screen that reads and writes data through the typed Rayfin client

- Publish the finished app and verify it in Fabric

**Important:** The portal command scaffolds a **starter shell**: Microsoft Entra sign-in, a Welcome page ("Your app is taking shape"), and an **empty** data model with the data service turned off. There is no to-do list yet. You build it yourself in Exercises 6 and 7.

**What you will do**

| **Exercise**                          | **Outcome**                                                                        |
|---------------------------------------|------------------------------------------------------------------------------------|
| 1\. Prepare the environment           | Node.js, VS Code, and the lab folder ready                                         |
| 2\. Create the workspace and App item | A workspace and an App item in Fabric, and the CLI command to scaffold the project |
| 3\. Scaffold and explore the project  | Local project created and its folders understood                                   |
| 4\. Deploy the starter app            | Backend and starter page published to Fabric                                       |
| 5\. Run the app locally               | Starter page running at http://localhost:5173                                      |
| 6\. Add the Todo data model           | Data service enabled and a Todo table deployed to Fabric                           |
| 7\. Build the to-do screen            | Welcome page replaced by a working task tracker                                    |
| 8\. Publish and verify                | Task tracker live on its Fabric hosting URL                                        |

**Architecture**

| **Layer**       | **Location in the project**  | **Role**                                                                 |
|-----------------|------------------------------|--------------------------------------------------------------------------|
| Configuration   | rayfin/rayfin.yml            | Turns services on or off: auth, data, static hosting, storage, functions |
| Data model      | packages/data/src            | TypeScript classes with decorators that become SQL tables in Fabric      |
| Shared contract | packages/shared/src/index.ts | Record types that give the frontend type-safe access to each entity      |
| Frontend        | packages/frontend/src        | React + Vite web app that calls the data API through getRayfinClient()   |
| Fabric          | Your workspace               | App item, SQL database, Entra sign-in, and the hosted URL                |

**Note:** Fabric apps is a preview feature. If a label or message differs slightly from this guide, follow what your portal and CLI show.

**Important:** The file **rayfin/.env** holds your publishable key, tenant ID, and workspace ID. Don't share screenshots of it, and don't commit it to source control.

## Exercise 1: Prepare the environment

### Task 1: Verify Node.js and create the lab folder

1.  If Node.js isn't installed, install the **LTS** version (20 or later) from +++https://nodejs.org+++.

2.  In the Windows search box, type +++Visual Studio Code+++, and then select **Visual Studio Code**.

![](./media/image1.png)

3.  In Visual Studio Code, select the **More Actions (...)** menu, select **Terminal**, and then select **New Terminal**.

![](./media/image2.png)

4.  Run the following commands to check the versions:

    +++node --version+++

    +++npm --version+++

5.  Verify that the Node.js version is **v20.x** or later.

6.  Create the lab folder and go to it:

    +++mkdir C:\LabFiles\FabricApps+++

    +++cd C:\LabFiles\FabricApps+++

![](./media/image3.png)

**Note:** Keep this terminal open. You use it in Exercise 3.

## Exercise 2: Create the workspace and App item

*Estimated time: 10 minutes*

### Task 1: Create a Fabric workspace

1.  Open your browser, go to +++https://app.fabric.microsoft.com/+++, and sign in with your credentials.

| **Username** | **+++@lab.CloudPortalCredential(User1).Username+++** |
|----|----|
| **Password** | **+++@lab.CloudPortalCredential(User1).Password+++** |

![](./media/image4.png)

![](./media/image5.png)

2.  If the portal opens in **Power BI**, select the experience switcher at the bottom left, and then select **Fabric**.

![](./media/image6.png)

3.  Select **+ New workspace**.

![](./media/image7.png)

4.  In the **Create a workspace** pane, enter +++Rayfin-Fabric-Todoapp@lab.LabInstance.Id+++ as the **Name**, and then expand **Advanced**.

![](./media/image8.png)

5.  Under **License mode**, select **Fabric**, verify the capacity under **Details**, and then select **Apply**.

![](./media/image9.png)

6.  The workspace opens.

![](./media/image10.png)

7.  Copy the URL from the browser address bar and save it in Notepad. Remove everything after the workspace ID. The URL should look like **https://app.fabric.microsoft.com/groups/\<workspace-id\>**.

![](./media/image11.png)

### Task 2: Create a Fabric App item

1.  In the workspace, select **+ New item**.

![](./media/image12.png)

2.  In the **New item** pane, enter +++app+++ in the search box, and then select **App (preview)**.

![](./media/image13.png)

**Note:** If **App (preview)** isn't listed, ask your administrator to enable the **Fabric apps (preview)** tenant setting, and then wait a few minutes.

3.  In the **New App** dialog, enter +++To do_App+++ as the **Name**, keep your workspace as the **Location**, and select **Create**.

![](./media/image14.png)

4.  The App item opens and deploys its template. Wait until the deployment finishes. This takes a few minutes.

![](./media/image15.png)

5.  On the **Overview** page, under **Getting started**, find step **2 Set up your project**. Select the **Copy** icon to copy the scaffold command, and save it in Notepad.

![](./media/image16.png)

6.  Review the other steps: **Edit the app** (**cd \<your-project-directory\>** and **npm run dev**) and **Publish your changes** (**npx rayfin up**). You run these commands in the next exercises.

![](./media/image17.png)

**Important:** Use the command exactly as copied from **your** portal. It contains your app name, the template name, and your workspace name, for example: **npm create @microsoft/rayfin@latest -- "To do_App" --template blankapp --workspace "Rayfin-Fabric-Todoapp56478"**.

## Exercise 3: Scaffold and explore the project

### Task 1: Scaffold the project

1.  In the VS Code terminal, make sure you are in **C:\LabFiles\FabricApps**, and then paste and run the command you copied from the portal.

![](./media/image18.png)

2.  If npm asks **Need to install the following packages: @microsoft/create-rayfin ... Ok to proceed? (y)**, type **y** and press **Enter**.

![](./media/image19.png)

3.  If a browser opens, sign in with your lab account. Wait until the CLI shows **Project created successfully!**

![](./media/image20.png)

4.  Go to the project folder and install the dependencies:

    +++cd to-do-app+++

    +++npm install+++

![](./media/image21.png)

**Note:** Warnings about moderate vulnerabilities or install scripts are expected for this sample. You don't need to run **npm audit fix**.

5.  In VS Code, select **File \> Open Folder**, open **C:\LabFiles\FabricApps\to-do-app**, and review the project in the **Explorer**.

![](./media/image22.png)

6.  Select **More Actions (...) \> Terminal \> New Terminal** to open a terminal in the **to-do-app** folder.

![](./media/image23.png)

7.  If prompted **Do you trust the authors of the files in this folder?**, select **Trust Folder & Continue**.

![](./media/image24.png)

**Note:** Run all the remaining commands in this terminal, from the **to-do-app** folder.

### Task 2: Understand the project layout

```
to-do-app/
├── .agents/                 # example kits for AI agents (reference only)
├── packages/
│   ├── data/src/index.ts    # data schema registration (empty at start)
│   ├── shared/src/index.ts  # UniversalAppSchema record contracts
│   └── frontend/src/        # React app: Welcome.tsx, hooks/, lib/
├── rayfin/
│   ├── rayfin.yml           # backend configuration
│   ├── .env                 # deployment values (do not share)
│   └── .deployments.json    # deployment history
├── scripts/
└── package.json
```

| **File**                                   | **What it does**                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| rayfin/rayfin.yml                          | Configures the services. **data** starts with **enabled: false**; **auth** uses Microsoft Entra (**fabric: enabled: true**). |
| packages/data/src/index.ts                 | Exports **schema = \[\]**. Every entity must be registered here.                                                             |
| packages/shared/src/index.ts               | Defines **UniversalAppSchema**, which types **client.data.\<Entity\>** in the frontend.                                      |
| packages/frontend/src/Welcome.tsx          | The starter page ("Your app is taking shape"). You replace it in Exercise 7.                                                 |
| packages/frontend/src/lib/rayfin-client.ts | Exports **getRayfinClient()**, which returns the typed data client.                                                          |
| .agents/skills/data-modeling/kit/          | Reference examples (Item.ts, schema.ts). Read them, but don't edit them.                                                     |

**Note:** Both **packages/data/src** and **packages/shared/src** contain a file named **index.ts**. Before you edit either one, check the **breadcrumb** at the top of the VS Code editor to make sure you have the right file open.

## Exercise 4: Deploy the starter app

*Estimated time: 10 minutes*

1.  Sign in to Fabric from the CLI (skip this step if you already signed in while scaffolding):

    +++npx rayfin login+++

![](./media/image25.png)

2.  In the browser, select your lab account.

![](./media/image26.png)

3.  Verify that the terminal shows **Signed in successfully**.

![](./media/image27.png)

4.  Preview the deployment without changing anything:

    +++npx rayfin up --dry-run+++

![](./media/image28.png)

5.  Review the **Planned operations**, for example **Create or reuse Rayfin item "to-do-app" (AppBackend)** and **POST runtime settings (auth=true, data=false)**.

6.  Deploy the starter app:

    +++npx rayfin up+++

7.  Wait until the terminal shows **Project "to-do-app" is now deployed to Fabric!** and **Your app is live at: https://\<your-app\>.webapp.fabricapps.net**. Copy the URL to Notepad.

![](./media/image29.png)

8.  Check the deployment status:

    +++npx rayfin up status+++

![](./media/image30.png)

9.  Open **rayfin/.env**. It now contains values such as **RAYFIN_PUBLIC_ITEM_ID** and **RAYFIN_PUBLIC_WORKSPACE_ID**, which shows that the deployment worked.

10. Select the hosting URL in the terminal (**Ctrl + click**). If VS Code asks **Do you want Code to open the external website?**, select **Open**.

![](./media/image31.png)

11. On the **Sign in to continue** page, select **Sign in** and sign in with your lab account.

![](./media/image32.png)

12. Verify that the starter page **Your app is taking shape** appears.

![](./media/image33.png)

**Note:** The hosting URL is also added to **rayfin/rayfin.yml** under **allowedRedirectUris** (it ends in **.webapp.fabricapps.net**).

## Exercise 5: Run the app locally

*Estimated time: 5 minutes*

1.  Start the development server:

    +++npm run dev+++

![](./media/image34.png)

2.  Hold **Ctrl** and select **http://localhost:5173/** in the terminal. Sign in if prompted.

![](./media/image35.png)

3.  Verify that the starter Welcome page appears.

![](./media/image36.png)

4.  In the terminal, press **Ctrl + C** to stop the server. If asked **Terminate batch job (Y/N)?**, type **Y**.

**Note:** You can ignore the **DeprecationWarning** and the **\[vite:react-swc\] We recommend switching...** messages. They are harmless.

## Exercise 6: Add the Todo data model

*Estimated time: 25 minutes*

To add data to a Rayfin app, you enable the data service, declare the entity as a decorated TypeScript class, export it, add it to **UniversalAppSchema**, and register it in **schema**. Every entity also needs explicit access control.

### Task 1: Enable the data service in rayfin.yml

1.  In the **Explorer**, open **rayfin \> rayfin.yml**.

2.  Change the **data:** block so that it reads exactly like this. Set **enabled** to **true** and **add** the line **dialect: mssql**:

```yaml
  data:
    enabled: true
    dialect: mssql
    path: packages/data
    buildCommand: npm run build
```

![](./media/image37.png)

3.  Check the indentation: **data:** must line up with **auth:** and **staticHosting:** (two spaces in), and the four lines under it are indented by four spaces. Use spaces, not tabs.

4.  Save the file (**Ctrl + S**).

For reference, the complete file should look like this. Your hosting URL and any values that the CLI added will be different:

```yaml
id: to-do-app
name: To do_App
version: 1.0.0
services:
  auth:
    enabled: true
    fabric:
      enabled: true
      externalEntraExchange: true
    password:
      enabled: false
    allowedRedirectUris:
      - http://localhost:5173
      - http://127.0.0.1:5173
      - https://<your-app>.webapp.fabricapps.net
  data:
    enabled: true
    dialect: mssql
    path: packages/data
    buildCommand: npm run build
  staticHosting:
    enabled: true
    path: packages/frontend
    folder: dist
    buildCommand: npm run build:fabric
    indexDocument: index.html
    assetAccess: protected
    embedded:
      only: false
  storage:
    enabled: false
  functions:
    enabled: false
```

**Important:** Without **dialect: mssql**, the deployment fails with **Dialect is required when Data module is enabled**. If **data:** is indented too far, it fails with **Map keys must be unique**.

### Task 2: Create the Todo entity

1.  In the **Explorer**, expand **packages \> data**, right-click **src**, select **New File...**, and name the file +++Todo.ts+++.

![](./media/image38.png)

![](./media/image39.png)

2.  Paste the following code and save the file:

```typescript
import { entity, role, uuid, text, boolean, date } from '@microsoft/rayfin-core';

/**
 * A to-do item. Each signed-in user sees only their own items.
 */
@entity()
@role('authenticated', '*', {
  policy: (claims, item) => claims.sub.eq(item.owner_id),
})
export class Todo {
  @uuid() id!: string;
  @text({ min: 1, max: 200 }) title!: string;
  @text({ optional: true, max: 2000 }) notes?: string;
  @boolean({ default: false }) done!: boolean;
  @date() createdAt!: Date;
  @text({ max: 200 }) owner_id!: string;
}
```

![](./media/image40.png)

**Note:** The screenshot shows an earlier version of the class. Use the code above.

3.  Review what each part does:

| **Part**                                 | **Meaning**                                                                                                        |
|------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| @entity()                                | Turns the class into a database table                                                                              |
| @role('authenticated', '\*', { policy }) | Only signed-in users can access the table, and only rows where **owner_id** matches their user ID (**claims.sub**) |
| max on every @text                       | Required. Without it, the column becomes NVARCHAR(MAX) and the API fails with **Internal server error**.           |
| { optional: true }                       | Makes the column nullable. The TypeScript **?** on its own doesn't.                                                |

### Task 3: Register the entity in the data package

1.  Open **packages \> data \> src \> index.ts**. Check that the breadcrumb shows **packages \> data \> src \> index.ts**.

2.  Replace the whole file with the following code and save it:

```typescript
//----------------------------------------------------------------------
// <copyright company="Microsoft Corporation">
//   Copyright (c) Microsoft Corporation. All rights reserved.
//   Licensed under the MIT license.
// </copyright>
//----------------------------------------------------------------------

/**
 * The app's Rayfin data schema registration.
 */
import { Todo } from './Todo.js';

export type { UniversalAppSchema } from '@rayfin-app/shared';
export { Todo };

export const schema = [Todo];
```

![](./media/image41.png)

**Note:** Keep the **.js** extension in **./Todo.js**, even though the file is named **Todo.ts**. The project uses ES modules and needs it.

### Task 4: Add the record contract to the shared package

1.  Expand **packages \> shared \> src** and open **index.ts**. Check that the breadcrumb shows **packages \> shared \> src \> index.ts**.

2.  Replace the whole file with the following code and save it:

```typescript
/**
 * Entity map shared by the data registration package and typed browser client.
 *
 * Every entity registered in `packages/data/src/index.ts` (the `schema` array)
 * must have a matching record contract here, so the frontend client
 * (`(await getRayfinClient()).data.Todo`) is fully typed.
 */

/**
 * Record contract for the Todo entity.
 * Keep these fields in step with `packages/data/src/Todo.ts`.
 */
export interface TodoRecord {
  id: string;
  title: string;
  notes?: string;
  done: boolean;
  createdAt: Date;
  owner_id: string;
}

/**
 * Map of entity name -> record type used by the Rayfin client.
 */
export type UniversalAppSchema = {
  Todo: TodoRecord;
};
```

![](./media/image42.png)

3.  Select **File \> Save All**. No editor tab should show a white dot (an unsaved change).

### Task 5: Build the packages

1.  Run the following commands:

    +++npm run build -w @rayfin-app/shared+++

    +++npm run build -w @rayfin-app/data+++

![](./media/image43.png)

2.  Verify that both commands finish with **tsc -b** and no errors. A **dist** folder appears in each package.

![](./media/image44.png)

### Task 6: Deploy the Todo table to Fabric

1.  Preview the deployment:

    +++npx rayfin up --dry-run+++

![](./media/image45.png)

2.  Verify that the planned operations include **POST runtime settings (auth=true, data=true)** and **Generate and apply DAB configuration**.

3.  Deploy:

    +++npx rayfin up+++

4.  Confirm the deployment:

    +++npx rayfin up status+++

**Note:** DAB is Data API builder. Rayfin uses it to generate the data API for your entities.

## Exercise 7: Build the to-do screen

*Estimated time: 20 minutes*

In this exercise, you replace the starter Welcome page with a task tracker that lists, adds, completes, and deletes to-do items. The new component keeps the name **Welcome**, so the rest of the app (**App.tsx**, **Root.tsx**) works without changes.

### Task 1: Back up the starter page

1.  In the terminal, run:

    +++Copy-Item packages\frontend\src\Welcome.tsx packages\frontend\Welcome.backup.tsx+++

![](./media/image46.png)

### Task 2: Replace Welcome.tsx

1.  Open **packages \> frontend \> src \> Welcome.tsx**.

2.  Select everything (**Ctrl + A**), paste the following code, and save the file:

```tsx
import { useEffect, useState } from 'react';
import { getRayfinClient } from './lib/rayfin-client';
import { useAuth } from './hooks/auth.context';

type Todo = {
  id: string;
  title: string;
  notes?: string;
  done: boolean;
  createdAt: string | Date;
  owner_id: string;
};

/** Reads the signed-in user's id from the session. */
function getUserId(session: any): string | undefined {
  return (
    session?.user?.id ??
    session?.user?.sub ??
    session?.claims?.sub ??
    session?.sub ??
    session?.userId
  );
}

function toList(result: any): Todo[] {
  if (Array.isArray(result)) return result;
  return result?.items ?? result?.data ?? [];
}

export function Welcome() {
  const { session } = useAuth() as any;
  const userId = getUserId(session);

  const [todos, setTodos] = useState<Todo[]>([]);
  const [title, setTitle] = useState('');
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  async function loadTodos() {
    try {
      setLoading(true);
      setError(null);
      const client: any = await getRayfinClient();
      const result = await client.data.Todo
        .select(['id', 'title', 'notes', 'done', 'createdAt', 'owner_id'])
        .orderBy({ createdAt: 'desc' })
        .execute();
      setTodos(toList(result));
    } catch (e: any) {
      console.error('Load failed', e);
      setError(e?.message ?? 'Could not load to-do items.');
    } finally {
      setLoading(false);
    }
  }

  useEffect(() => {
    loadTodos();
  }, []);

  async function addTodo(e: React.FormEvent) {
    e.preventDefault();
    const text = title.trim();
    if (!text) return;
    if (!userId) {
      console.log('Session object:', session);
      setError('Signed-in user id not found. Open F12 > Console and check the "Session object".');
      return;
    }
    try {
      const client: any = await getRayfinClient();
      await client.data.Todo.create({
        title: text,
        done: false,
        createdAt: new Date(),
        owner_id: userId,
      });
      setTitle('');
      await loadTodos();
    } catch (e: any) {
      console.error('Create failed', e);
      setError(e?.message ?? 'Could not add the item.');
    }
  }

  async function toggleDone(todo: Todo) {
    try {
      const client: any = await getRayfinClient();
      await client.data.Todo.update({ id: todo.id }, { done: !todo.done });
      await loadTodos();
    } catch (e: any) {
      console.error('Update failed', e);
      setError(e?.message ?? 'Could not update the item.');
    }
  }

  async function deleteTodo(todo: Todo) {
    try {
      const client: any = await getRayfinClient();
      await client.data.Todo.delete({ id: todo.id });
      await loadTodos();
    } catch (e: any) {
      console.error('Delete failed', e);
      setError(e?.message ?? 'Could not delete the item.');
    }
  }

  const remaining = todos.filter((t) => !t.done).length;

  return (
    <main style={styles.page}>
      <section style={styles.card}>
        <h1 style={styles.h1}>Zava Training Team Tracker</h1>
        <p style={styles.sub}>
          Zava Training Team &middot; {remaining} of {todos.length} open
        </p>

        <form onSubmit={addTodo} style={styles.form}>
          <input
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            placeholder="What needs to be done?"
            maxLength={200}
            style={styles.input}
          />
          <button type="submit" style={styles.addBtn}>Add</button>
        </form>

        {error && <div style={styles.error}>{error}</div>}

        {loading ? (
          <p style={styles.muted}>Loading...</p>
        ) : todos.length === 0 ? (
          <p style={styles.muted}>No items yet. Add your first task above.</p>
        ) : (
          <ul style={styles.list}>
            {todos.map((t) => (
              <li key={t.id} style={styles.item}>
                <label style={styles.label}>
                  <input type="checkbox" checked={t.done} onChange={() => toggleDone(t)} />
                  <span style={t.done ? styles.doneText : undefined}>{t.title}</span>
                </label>
                <button onClick={() => deleteTodo(t)} style={styles.delBtn}>Delete</button>
              </li>
            ))}
          </ul>
        )}
      </section>
    </main>
  );
}

export default Welcome;

const styles: Record<string, React.CSSProperties> = {
  page: { minHeight: '100vh', display: 'flex', justifyContent: 'center', padding: '48px 16px', background: '#f5f7fa', fontFamily: 'Segoe UI, sans-serif' },
  card: { width: '100%', maxWidth: 560, background: '#fff', borderRadius: 12, padding: 28, boxShadow: '0 2px 12px rgba(0,0,0,0.08)', height: 'fit-content' },
  h1: { margin: 0, fontSize: 26, color: '#0b5394' },
  sub: { marginTop: 6, color: '#666' },
  form: { display: 'flex', gap: 8, margin: '20px 0' },
  input: { flex: 1, padding: '10px 12px', fontSize: 15, border: '1px solid #ccc', borderRadius: 8 },
  addBtn: { padding: '10px 18px', background: '#0b5394', color: '#fff', border: 'none', borderRadius: 8, cursor: 'pointer' },
  error: { background: '#fdecea', color: '#a00', padding: 10, borderRadius: 8, marginBottom: 12 },
  muted: { color: '#888' },
  list: { listStyle: 'none', padding: 0, margin: 0 },
  item: { display: 'flex', justifyContent: 'space-between', alignItems: 'center', padding: '10px 4px', borderBottom: '1px solid #eee' },
  label: { display: 'flex', gap: 10, alignItems: 'center', cursor: 'pointer' },
  doneText: { textDecoration: 'line-through', color: '#999' },
  delBtn: { background: 'transparent', border: '1px solid #ddd', borderRadius: 6, padding: '4px 10px', cursor: 'pointer', color: '#a00' },
};
```

![](./media/image47.png)

3.  Review how the code works:

| **Code**                                | **Purpose**                                                                                                  |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------|
| getRayfinClient()                       | Returns the typed Rayfin client. **client.data.Todo** is your table.                                         |
| useAuth()                               | Provides the signed-in session. The user ID is saved in **owner_id**, so the access policy allows the write. |
| .select(\[...\]).orderBy(...).execute() | Reads the user's tasks, newest first                                                                         |
| .create({...})                          | Adds a task                                                                                                  |
| .update({ id }, { done })               | Marks a task as done or not done                                                                             |
| .delete({ id })                         | Deletes a task                                                                                               |

**Tip:** If **useAuth** is underlined in red, find where the hook is exported by running +++Select-String -Path packages\frontend\src\hooks\*.ts, packages\frontend\src\hooks\*.tsx -Pattern "^export"+++. Then change line 3 to import it from that file, for example **./hooks/use-auth**.

### Task 3: Test the app locally

1.  Run +++npm run dev+++ and open **http://localhost:5173**.

![](./media/image48.png)

2.  Verify that the page shows **Zava Training Team Tracker** with the message **No items yet. Add your first task above.** If no red error box appears, the app is connected to the Todo table.

![](./media/image49.png)

3.  Enter +++Complete Fabric Apps lab setup+++ and select **Add**.

![](./media/image50.png)

4.  Add +++Review Rayfin project structure+++.

![](./media/image51.png)

5.  Select the check box next to **Complete Fabric Apps lab setup**.

![](./media/image52.png)

6.  Verify that the task is crossed out and the counter shows **1 of 2 open**.

![](./media/image53.png)

7.  Press **F5** to reload the page. The tasks are still there, because they are stored in Fabric.

8.  In the terminal, press **Ctrl + C** to stop the server.

## Exercise 8: Publish and verify in Fabric

### Task 1: Publish the app

1.  Publish the frontend and the backend:

    +++npx rayfin up+++

2.  Verify that the output ends with **Your app is live at: https://\<your-app\>.webapp.fabricapps.net**.

![](./media/image54.png)

3.  Open the hosting URL in the browser and sign in with your lab account if prompted. Verify that **Zava Training Team Tracker** appears and that the tasks you created locally are listed.

![](./media/image55.png)

![](./media/image56.png)

**Note:** The local app and the hosted app use the same Fabric backend, so they show the same data.

4.  Add +++Publish app to Fabric workspace+++ and select **Add**. The counter shows **2 of 3 open**.

![](./media/image57.png)

![](./media/image58.png)

### Task 2: Verify the app in the Fabric portal

1.  In the Fabric portal, open your workspace **Rayfin-Fabric-Todoapp@lab.LabInstance.Id**. Notice the **to-do-app** App item and its **SQL database** and **SQL analytics endpoint**, which were created by **npx rayfin up**. Select **to-do-app**.

![](./media/image59.png)

**Note:** The workspace also contains the **To do_App** item that you created in the portal in Exercise 2. The CLI deploys the project to its own item, **to-do-app**.

2.  The app opens inside Fabric with your tasks.

![](./media/image60.png)

3.  Select the check box next to **Review Rayfin project structure**.

![](./media/image61.png)

4.  Select **Delete** next to **Complete Fabric Apps lab setup**.

![](./media/image62.png)

5.  Enter +++Explore the app+++ and select **Add**.

![](./media/image63.png)

6.  Verify the result: three tasks, with the counter showing **2 of 3 open**.

![](./media/image64.png)

**Useful deploy commands**

| **Command**                    | **Purpose**                                         |
|--------------------------------|-----------------------------------------------------|
| npx rayfin up                  | Deploy everything: settings, database, and frontend |
| npx rayfin up --dry-run        | Preview the changes without applying them           |
| npx rayfin up db apply         | Apply schema changes only                           |
| npx rayfin up staticapp deploy | Deploy the frontend only                            |
| npx rayfin up status           | Show the deployment state                           |
| npx rayfin login               | Sign in again (fixes 401 and 403 errors)            |

## Troubleshooting

| **Symptom**                                     | **Cause and fix**                                                                                                                    |
|-------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| No rayfin/data folder                           | This template keeps entities in **packages/data/src**. Follow Exercise 6.                                                            |
| Dialect is required when Data module is enabled | Add **dialect: mssql** under **services.data** in **rayfin.yml**.                                                                    |
| Map keys must be unique at line 16 ... data:    | **data:** is indented under **auth:**. Move it back so it lines up with **auth:** (two spaces in).                                   |
| Yellow or red underline on import { Todo }      | You edited the wrong **index.ts**. Check the breadcrumb: the shared contract belongs in **packages/shared/src/index.ts**.            |
| White dot on an editor tab                      | The file isn't saved. Select **File \> Save All**.                                                                                   |
| useAuth underlined in red                       | Import it from the file that exports it (see the tip in Exercise 7).                                                                 |
| Signed-in user id not found                     | Press **F12 \> Console**, look at the logged **Session object**, and update **getUserId()** to use the field that holds the user ID. |
| Internal server error after a successful deploy | A **@text** field is missing **max**. Add a bound and run **npx rayfin up** again.                                                   |
| 401 or 403 from the CLI                         | Run **npx rayfin login**, and then run **npx rayfin up** again.                                                                      |
| DeprecationWarning or react-swc message         | Harmless. You can ignore it.                                                                                                         |
| App item missing from **New item**              | Ask the administrator to enable **Fabric apps (preview)**, and then wait a few minutes.                                              |

## Clean up resources

1.  In the Fabric portal, select the **Rayfin-Fabric-Todoapp@lab.LabInstance.Id** workspace from the navigation pane.

![](./media/image65.png)

2.  Select the **...** option next to the workspace name, and then select **Workspace settings**.

![](./media/image66.png)

3.  Navigate to the bottom of the **General** tab, select **Remove this workspace**, and then confirm the deletion.

![](./media/image67.png)

![](./media/image68.png)

![](./media/image69.png)

4.  If you no longer need the local project, delete the folder **C:\LabFiles\FabricApps\to-do-app**.

**Summary**

In this lab, you built the **Zava Training Team Tracker** as a Microsoft Fabric app, by hand and without an AI coding agent. You created a workspace and an **App (preview)** item, scaffolded a Rayfin project with the CLI, and explored its structure. You deployed the starter app and ran it locally. You then enabled the data service, defined a secured **Todo** entity in which each signed-in user sees only their own rows, registered it in the data and shared packages, and deployed the table to Fabric. Finally, you replaced the starter page with a React screen that reads and writes tasks through the typed Rayfin client, published it to its Fabric hosting URL, and verified it in the Fabric portal.

**References**

- [Create your first Fabric apps project](https://learn.microsoft.com/fabric/apps/create-app)

- [Project structure](https://learn.microsoft.com/fabric/apps/project-structure)

- [Define data models](https://learn.microsoft.com/fabric/apps/data-models)

- [Define data permissions](https://learn.microsoft.com/fabric/apps/data-permissions)

- [Read and write data with GraphQL](https://learn.microsoft.com/fabric/apps/read-write-data-graphql)

- [Deploy to Fabric](https://learn.microsoft.com/fabric/apps/deploy-app)
