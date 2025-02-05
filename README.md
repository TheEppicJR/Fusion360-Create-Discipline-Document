# Create Discipline Document

When working as a team, with imported AnyCAD data, or on multiple manufacturing processes, it is useful to insert an existing source document into a new related document so that work can be done in the new related document and not change or lock the source document. This add-in creates a Design Workspace command that produces a new related document from a pre-configured list of saved start document that reference the source document. The source document must be the active document and must be saved before this add-in's command can be used.

When creating the new related document you may select from a configurable list of workflows. Each workflow allows a pre-saved start document to be used to create the new related document. It also pre-populates the name of the new related document to capture workflow and the name of the source document. This makes finding and understand why the new related document was created easier.

Here are a few examples for how you might use this add-in:

### Manufacture a native Fusion 3D Design

You have an existing Fusion Design and you want another team member to work on manufacturing setups and programming. By creating a related document that references the current design your team member can work in their own document without permission conflicts. In addition when you share your design you no longer have to share your private manufacturing information as it is store in its own document.

In this graph we see how the add-in help create the Manufacturing Document referencing the Fusion Design you as **User A** working **Fusion 3D Design**. Tour team member as **User B** can work on the **Manufacturing Document** in parallel.

```mermaid
%%{
init: {
'theme':'base',
'themeVariables': {
'primaryColor': '#f0f0f0',
'primaryBorderColor': '#454F61',
'lineColor': '#59cff0',
'tertiaryColor': '#e1ecf5',
'fontSize': '14px'
}
}
}%%

graph TD

a(Manufacturing Document)
b(Fusion 3D Design)

c((User A))
d((User B))

d -.-> a --"X-Ref"--> b
c -.-> b
```

### AnyCAD

You have an existing SolidWorks document that you upload using Fusion 360 Team to your hub.

You can open this SolidWorks part in Fusion 360 Desktop client and then use this add-in to create a new referencing document for manufacturing and CNC programming.

```mermaid
%%{
init: {
'theme':'base',
'themeVariables': {
'primaryColor': '#f0f0f0',
'primaryBorderColor': '#454F61',
'lineColor': '#59cff0',
'tertiaryColor': '#e1ecf5',
'fontSize': '14px'
}
}
}%%

graph TD

A(SolidWorks Part)
B(Manufacturing Document)

B --"X-Ref"--> A

```

Any changes to the SolidWorks part can saved and your manufacturing document will associatively update when you update the the new version.

> Note: AnyCAD workflows are only available when using a Team Hub and a Commercial, Education or Start Entitlement. Personal ( aka Free aka Hobby ) Entitlements do not have access to AnyCAD Workflows.

Later you may need to run FEA simulations on the same SolidWorks design. Using the add-in you can create a new related document referencing the SolidWorks AnyCAD part.

```mermaid
%%{
init: {
'theme':'base',
'themeVariables': {
'primaryColor': '#f0f0f0',
'primaryBorderColor': '#454F61',
'lineColor': '#59cff0',
'tertiaryColor': '#e1ecf5',
'fontSize': '14px'
}
}
}%%

graph TD

A(SolidWorks Part)
B(Manufacturing Document)
C(Simulation Document)

i((User A))
j((User B))

B --"X-Ref"--> A
C --"X-Ref"--> A

i -.-> B
j -.-> C

```

Two related documents have been created. One for Manufacturing and one for Simulation.
Each document can have a unique user working on it in parallel. This is very useful for teams to ensure you can have different discipline working on a design concurrently.

### Render designs with a common render setup

Using the start document's ability to pre store information. You open a new empty document and activate the rendering workspace.
You can define new emissive bodies as lights and store several different options for floors. In addition you setup the render defaults for exposure, HDRI Environment map, background, and camera focal settings. Defining these in the start part ensure that as you make future renderings you can have consistent look and feel to you renderings.

```mermaid
%%{
init: {
'theme':'base',
'themeVariables': {
'primaryColor': '#f0f0f0',
'primaryBorderColor': '#454F61',
'lineColor': '#59cff0',
'tertiaryColor': '#e1ecf5',
'fontSize': '14px'
}
}
}%%

graph TD

A(Fusion Design)
B(Render Document)

B --"X-Ref"--> A

```

## Multiple Start Documents

To best use this add-in, pre-create and save start documents that have information already defined in them. This allows you to automate and make smart start parts that can reduce setup for your intended workflows.
You can create a specific start document for each workflow and set the default naming scheme as you configure this add-in for your specific hub, team and use cases.

You can choose to save as much or as little information inside each start document.

**TIP:** It its always useful to save a generic empty start document and configure a plain assembly or default start document option so you can always get a simple new reference document when needed.

### User interface.

Once the Add-in is installed and running, you find the command in the Design Workspace -> Create Panel at the bottom of the menu.

![Command](assets/000-CDD.png)

You can add this command to your toolbar or "S Key" shortcuts.

Start the **Create Related Document** command. There will be a pause for a second or so as the list of documents is retrieved from your cloud Team Hub.

![Command Dialog](assets/001-CDD.png)

To select from the different start documents choose from the **Type** drop-down in the dialog displayed.

![Start Documents](assets/002-CDD.png)

The new related document is auto-named by default. You can uncheck the **Auto-Name** option and define a name of your own.

Click **OK** and the new related document is created and your source document is inserted.

## How to find your own documents URNs

Open the document you want to use as a start part.

From the application menu turn on the Text Command Pallet
![Text Command Pallet](assets/003-CDD.png)

Next, make sure you have "Py" turned on at the far right bottom of the **Text Commands** pallet.
![Py settings](assets/004-CDD.png)

In the text command pallet type:

```
app=adsk.core.Application.get()
```

Next type:

```
project=app.data.activeProject
```

Now type:

```
project.id
```

Fusion will return the urn for the open active hub. Select this string and copy it using the Right Mouse Click menu. Copy the entire string "a.XXXXXX"

Now we get the folder for the open document. This is the folder where all your start documents should be stored. You only need to ensure on start document is open and the active tab.

In the **Text Commands** pallet type:

```
doc = app.activeDocument.dataFile.parentFolder.id
```

Then type:

```
doc
```

Fusion will return the urn for the folder for active active document. Select this string and copy it using the Right Mouse Click menu. Copy the entire string "urn:XXXXXX"

You should see something like this in the **Text Commands** pallet:

```
app=adsk.core.Application.get()

project=app.data.activeProject

project.id
a.1234ABCDERgh9101234ABCDERgh910

doc = app.activeDocument.dataFile.parentFolder.id

doc
urn:adsk.wipprod:fs.folder:co.1234ABCDERgh910
```

Open [data.json](./data.json).

Edit this and paste in your PROJECT_ID and FOLDER_ID as shown:

```
{"PROJECT_ID":"_Paste your Project ID here_","FOLDER_ID":"_Paste your Folder ID here_"}
```

Your Json file should look like this:

```
{"PROJECT_ID":"a.1234ABCDERgh9101234ABCDERgh910","FOLDER_ID":"urn:adsk.wipprod:fs.folder:co.1234ABCDERgh910"}
```

> Note: Instead of "1234ABCDERgh910" you have your own values. **Save the JSON file** and ensure you provide it to your team members.

You are now ready to use the Related Documents Add-In. You can add documents to the folder you chose to store the start documents. Each time the command is run it will index the folder's documents and update. This check can take a second or so.

**TIP:** If you save your start documents with the workspace you want to default to, this workspace will automatically be active when the document opens.
