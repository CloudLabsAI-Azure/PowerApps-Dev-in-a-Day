# Lab 02 - Build a code component

## Estimated Duration: 90 minutes

Working as part of the PrioritZ fusion team you have been asked to create a Power Apps code component to allow drag and drop priority ranking of items in the PrioritZ Ask Power App. You will build
a code component using the React JavaScript framework. A code component approach is used to
address the requirement because there isn’t a similar control already built in.

You have collaborated with the app makers to identify the following properties to allow them to
configure the code component in the app:

- BackgroundColor
- DragBackgroundColor
- ItemHeight
- FontSize
- FontColor

The PrioritZ Ask app will prepare a collection of the items to rank that will be bound as the dataset for
the code component. When an item is dragged and dropped the code component will raise an OnSelect
event that will be handled by the hosting app. The hosting app will update the collection items with
their new rank. The code component will be stateless.

## Lab Objectives

- Exercise 1: Build Code Component 
- Exercise 2: Use Code Component 
- Exercise 3: Add Code Component to Solution 

## Exercise 1 – Build Code Component

In this exercise, you will build the code component.

### Task 1: Create the code component

1. Start **Visual Studio Code** if not already open using the shortcut available on the desktop.

   ![](images/L04/vscode1.png)
   
2. Select the **Power Platform (1)** tab and make sure your **Dev Auth (2)** profile is selected. 
    
   >**Note**: The Power Platform tab is already installed.
    
    ![](images/L02/L01-authuu.png)

3. Click **Terminal (1)** and select **New Terminal (2).**
     
     ![](images/L02/L02-terminaluu.png)

4. In the Terminal window, make a new directory by running the command below.

    ```
    md PrioritZDnDRanking
    ```
5. Run the command below to switch to the PrioritZDnRanking directory you created.

    ```
    cd PrioritZDnDRanking
    ```
6. You should now be in the directory you created. Create a new component project and install dependencies by running the below command.
    
     ```
     pac pcf init -ns ContosoCoffee --name PrioritZDnDRanking --template dataset --framework react --run-npm-install
     ```
     
     ![](images/L02/image3.png)

7. The component framework project should be created successfully.

    ![](images/L02/image4.png)

8. Run the command below to open the project.
    ```
    code -a.
    ```

1. If you are presented with the below pop-up, click on **Yes** to trust the authors of the files.

    ![](images/L02/image4.1.png)

9. Review the created code component files by selecting the **Explorer** tab.
    
     ![](images/L02/L02-explorer.png)

10. Expand the **PrioritZDnDRanking** folder and then expand the sub-folder **generated**.

11. Open the **ControlManifest.Input.xml** file. The manifest is the metadata file that defines a
    component including the properties exposed to the hosting app.

      ![](images/L02/image6.png)

12. Locate **data-set** XML element in **line number 21** in the **ControlManifest.Input.xml** file.

      ![](images/L02/image7.png)

13. Change the **name** to **items** and the **display-name-key** to **items**. This defines the property the app will bind to a collection of items.

      ![](images/L02/image8.png)

14. Add the following properties in between the closing tag of the data-set element `</data-set>` and the opening tag of the `<resources>` element.

    > Add the following properties after **line number 26** in the **ControlManifest.Input.xml** file.

    ```
    <property name="BackgroundColor" display-name-key="Background color" usage="input" of-type="SingleLine.Text" default-value="#F3F2F1"/>
    <property name="DragBackgroundColor" display-name-key="Drag background color" usage="input" of-type="SingleLine.Text" default-value="lightgreen"/>
    <property name="ItemHeight" display-name-key="Item height" usage="input" of-type="Whole.None" default-value="32"/>
    <property name="FontSize" display-name-key="Font size" usage="input" of-type="Whole.None" default-value="12"/>
    <property name="FontColor" display-name-key="Font color" usage="input" of-type="SingleLine.Text" default-value="#333333"/>
    ```

     ![](images/L02/image9.png)

15. Locate `<resources>` section and add the below code after **code path** to add **css** resource. This will ensure that our styles will be bundled with the code component when it is deployed.

    ```
    <css path="css/PrioritZDnDRanking.css" order="1" />
    ```
    
     ![](images/L02/image10.png)
 
      >**Note**: Please make sure not to uncomment the **resx path** as you will be facing an issue in the next task while building the code component if it's uncommented.
      
 16. Notice the following two resources. This declares the component’s dependency on these two
    libraries. This is a result of specifying –framework React on initialization.
     ```
     <platform-library name="React" version="16.8.6" />
     <platform-library name="Fluent" version="8.29.0" />
     ```
     ![](images/L02/image11.png)
    
17. Click **File** and select **Save All** to save your changes.
18. Make sure you still have the **ControlManifest.Input.xml** file selected and then click **New Folder**.

      ![](images/L02/image12.png)

19. Name the new folder ass **css**.
20. Select the new **css** folder you created and then click **New File**
 
     ![](images/L02/image13.png)
 
 21. Name the new file **PrioritZDnDRanking.css**
 22. Paste the following CSS into the **PrioritZDnDRanking.css** file.
    
       ```
        .prioritydnd-scroll-container {
        box-sizing: border-box;
        padding: 2px;
        overflow-y: auto;
        overflow-x: hidden;
        position: relative;
        }
        .prioritydnd-item-container {
        user-select: none;
        display: flex;
        align-items: center;
        }
        .prioritydnd-item-column {
        margin: 8px;
        }
        ```
23. The file should now look like the following.

     ![](images/L02/image14.png)

24. Click **File** and select **Save All** to save your changes.

### Task 2: Implement the component logic

1. Select the **HelloWorld.tsx** component file, right-click on it and select **Delete** to remove the component file as it is automatically created and we won’t be using it. 
     
     ![](images/L02/L02-EX1-T2-1.png)
    
2. Navigate to this path `C:\LabFiles\Developer-in-a-day\Student\L02 - Build a code component\Resources` in file explorer.
    
3. Drag the **PriorityComponent.tsx** file and drop it in the **PrioriZDnDRanking** folder.
 
4. The **PriorityComponent.tsx** file should now be in the **PrioriZDnDRanking** folder.

     ![](images/L02/image16.png)

5. Click **File** and save your changes.
    
6. Open the **PriorityComponent.tsx** and review the contents. This implements the React
    component that will be rendered to represent our draggable items.
    
7. Notice line 9 `from react-beautiful-dnd` has a red underline. This is a npm package the
    component uses that we haven’t referenced.

     ![](images/L02/image17.png)
 
 8. Run the following command in a terminal window to add a reference to react-beautiful-dnd.

    ```
    npm install react-beautiful-dnd
    ```
    >**Note** : If you receive this error **npm is not recognised**, then perform the below steps:

      1. Open PowerShell then run this command `choco install -y --force nodejs`.
      2. Once the command execution is done, close the Visual Studio Code and open it again.
      3. Perform **Step 8** of this task again to install the **react-beautiful-dnd** package.

9. Run the following command for the type definitions.

    ```
    npm i --save-dev @types/react-beautiful-dnd
    ```

10. Notice the red underline in line 9 has been resolved.
    
11. Open the **index.ts** file.
    
12. Remove the following line (line number 2 in Index.ts file) as we are no longer using HelloWorld

    ```
    import { HelloWorld, IHelloWorldProps } from "./HelloWorld";
    ```
    
    ![](images/L02/image18.png)
 
 13. Add the below code to the **index.ts** file after **line number 1**. This will reference the PriorityComponent.
    
        ```
        import { PriorityComponent, PriorityComponentProps } from './PriorityComponent';
        ```
        ![](images/L02/image19.png)  
   
 
 15. Locate the **Export** class in **line number 7**.
      
     ![](images/L02/image20.png)
 
 16. Add the following code below inside the **export** class. This defines some working variables you
    will be using in the class logic.
   
        ```
         private context: ComponentFramework.Context<IInputs>;
         private items: ComponentFramework.PropertyTypes.DataSet;
         private state: ComponentFramework.Dictionary;
        ```
        ![](images/L02/image21.png)
 

 17. Locate the **init** function.
 
      ![](images/L02/init.png)
        
 18. Paste the code below inside the **init** function. This logic initializes our class variables from the
    runtime values and enables resize notification.    
    
        ![](images/L02/init1.png)
   
       ```
        this.context = context;
        context.mode.trackContainerResize(true);
       ```
 
19. Locate the **updateView** function.

     ![](images/L02/imageUpdateView.png)
 
20. Replace the **updateView** function with the function below. This logic creates the React Element
    from the PriorityComponent and adds it to the virtual DOM.
  
     ```   
    public updateView(context: ComponentFramework.Context<IInputs>): React.ReactElement {
        const dataset = context.parameters.items;
        return React.createElement(PriorityComponent, {
            width: context.mode.allocatedWidth,
            height: context.mode.allocatedHeight,
            itemHeight: context.parameters.ItemHeight.raw,
            fontSize: context.parameters.FontSize.raw,
            fontColor: context.parameters.FontColor.raw,
            dataset: dataset,
            onReorder: this.onReorder,
            backgroundColor: this.context.parameters.BackgroundColor.raw,
            dragBackgroundColor:
            this.context.parameters.DragBackgroundColor.raw,
        } as PriorityComponentProps);
    }
    ```

    ![](images/L02/image24.png)
 
21. Add the below code after the **destroy** function. This logic handles the onReorder event from
    the PriorityComponent and identifies the involved items to the hosting app as selected items.
       
    ```
    onReorder = (sourceIndex: number, destinationIndex: number): void => {
    const dataset = this.context.parameters.items;
    const sourceId = dataset.sortedRecordIds[sourceIndex];
    const destinationId = dataset.sortedRecordIds[destinationIndex];
    // raise the OnSelect event
    this.context.parameters.items.openDatasetItem(dataset.records[sourceId].getNamedReference());
    // set the SelectedItems property
    this.context.parameters.items.setSelectedRecordIds([sourceId, destinationId]);
    };
    ```
  
    ![](images/L02/image25.png)
 
     > **Note** : **Destroy** function will be present at the end of the **PrioritZDnDRanking** class.

22. After completing all the steps, your `index.ts` file should contain the following code.

    ```
    
    import { IInputs, IOutputs } from "./generated/ManifestTypes";
    import { PriorityComponent, PriorityComponentProps } from './PriorityComponent';
    import * as React from "react";


     export class PrioritZDnDRanking implements ComponentFramework.ReactControl<IInputs, IOutputs> {
         private context: ComponentFramework.Context<IInputs>;
         private items: ComponentFramework.PropertyTypes.DataSet;
         private state: ComponentFramework.Dictionary;
         private theComponent: ComponentFramework.ReactControl<IInputs, IOutputs>;
         private notifyOutputChanged: () => void;

       /**
       * Empty constructor.
       */
       constructor() { }

       /**
       * Used to initialize the control instance. Controls can kick off remote server calls and other initialization actions here.
       * Data-set values are not initialized here, use updateView.
       * @param context The entire property bag available to control via Context Object; It contains values as set up by the 
         customizer  mapped to property names defined in the manifest, as well as utility functions.
       * @param notifyOutputChanged A callback method to alert the framework that the control has new outputs ready to be retrieved 
         asynchronously.
       * @param state A piece of data that persists in one session for a single user. Can be set at any point in a controls life 
         cycle  by calling 'setControlState' in the Mode interface.
       */
        public init(
        context: ComponentFramework.Context<IInputs>,
        notifyOutputChanged: () => void,
        state: ComponentFramework.Dictionary
        ): void {
        this.context = context;
        context.mode.trackContainerResize(true);
        this.notifyOutputChanged = notifyOutputChanged;
      }

      /**
     * Called when any value in the property bag has changed. This includes field values, data-sets, global values such as container 
      height and width, offline status, control metadata values such as label, visible, etc.
     * @param context The entire property bag available to control via Context Object; It contains values as set up by the customizer 
     mapped to names defined in the manifest, as well as utility functions
     * @returns ReactElement root react element for the control
     */
     public updateView(context: ComponentFramework.Context<IInputs>): React.ReactElement {
        const dataset = context.parameters.items;
        return React.createElement(PriorityComponent, {
            width: context.mode.allocatedWidth,
            height: context.mode.allocatedHeight,
            itemHeight: context.parameters.ItemHeight.raw,
            fontSize: context.parameters.FontSize.raw,
            fontColor: context.parameters.FontColor.raw,
            dataset: dataset,
            onReorder: this.onReorder,
            backgroundColor: this.context.parameters.BackgroundColor.raw,
            dragBackgroundColor:
            this.context.parameters.DragBackgroundColor.raw,
        } as PriorityComponentProps);
       }

       /**
       * It is called by the framework prior to a control receiving new data.
       * @returns an object based on nomenclature defined in manifest, expecting object[s] for property marked as "bound" or "output"
       */
       public getOutputs(): IOutputs {
        return { };
       }

       /**
       * Called when the control is to be removed from the DOM tree. Controls should use this call for cleanup.
       * i.e. cancelling any pending remote calls, removing listeners, etc.
       */
        public destroy(): void {
        // Add code to cleanup control if necessary
        }
        onReorder = (sourceIndex: number, destinationIndex: number): void => {
        const dataset = this.context.parameters.items;
        const sourceId = dataset.sortedRecordIds[sourceIndex];
        const destinationId = dataset.sortedRecordIds[destinationIndex];
        // raise the OnSelect event
        this.context.parameters.items.openDatasetItem(dataset.records[sourceId].getNamedReference());
        // set the SelectedItems property
        this.context.parameters.items.setSelectedRecordIds([sourceId, destinationId]);
        };
        }
    
    ```
23. Open the **package.json** file.
    
24. Locate the **dependencies** JSON object.

      ![](images/L02/image26.png)
 
25. Replace **dependencies** with the JSON below.

    ```
    "dependencies": {
    "@fluentui/react": "8.29.0",
    "eslint-config-prettier": "^8.5.0",
    "eslint-plugin-prettier": "^4.0.0",
    "eslint-plugin-react": "^7.29.4",
    "eslint-plugin-react-hooks": "^4.4.0",
    "eslint-plugin-sonarjs": "^0.13.0",
    "prettier": "^2.6.1",
    "react": "16.8.6",
    "react-beautiful-dnd": "^13.1.0",
    "react-dom": "16.8.6"
    },
    ```
    
25. Navigate to **.eslintric.json(1)** file from left navigation to add the new lint rule. Locate **rules(2)** in **line number 21** and paste the rules below.
   
      ```
      "no-unused-vars": ["off"],
      "no-undef": ["off"]
     ```
   
      ![](images/L02/eslint.png)
   
   
26. Click **File** and save all your changes.

27. Click **Terminal** and select **New Terminal**.
     
      ![](images/L02/image27.png)

28. Run the command below. This will build your component and identify any problems.

    ```
    npm run-script build
    ```

      > **Note**: If the build operation fails with this error **`Root element is missing`**, please make sure that **resx path** is commented in the Manifest.Xml file and try to build the component again.
 
 29. The build should succeed. If any errors, resolve them before proceeding.
      
       ![](images/L02/image28.png)
 
 30. Run the command below to start the test harness.
    
        ```
        npm start
        ```

31. The test harness should start, if not then copy the address and paste it in a new browser window. Try dragging the items and see if the behaviour functions as expected.

      ![](images/L02/imagee29u.png)
 
     > **Note**: If the test harness didn't start as expected you are not able to see the expected output as mentioned. Please verify that you have followed the previous instructions and added the code correctly in the **Manifest and Index** files. 

32. Close the test harness by closing the browser tab.

33. Stop the run by holding the **[CONTROL]** key + **C**.
 
34. Type **Y** and [ENTER].
     
      ![](images/L02/image30.png)

 
35. Run the command below to push the component to your environment.
    ```
    pac pcf push --publisher-prefix contoso
    ```
    
    > **Note** : 
    
     1. If the push operation fails with the error **`Sorry, the app encountered a non-recoverable error and will need to terminate`**, please make sure that you have followed the previous instructions and added the code correctly in **Manifest and Index** files. 
        
        Additionally, you can find the **Manifest and Index** files in the location `C:\LabFiles`, you can compare your code with these files and fix the issues if there are any then retry to push the component by running the  **pac push command** again.

     2. If the run fails with a Nuget package error, run the below command in PowerShell and try running the above command again.
    
        ```
        dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org --configfile $env:APPDATA\NuGet\NuGet.Config
        ```

    
36. Wait for the solution to be imported and published to your environment.

      ![](images/L02/image31uu.png)
 
### Task 3: Confirm the control was added to the environment

1. Navigate to the Power Apps maker portal by using the below URL if not already open. Make sure the development environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** is selected.

   ```
    https://make.powerapps.com/
    ```
    
2. Select **Solutions** and open the **PowerAppsTools** solution.
    
    ![](images/L02/image32u.png)

 3. Confirm that the custom control is in this solution.
     
      ![](images/L02/image33u.png)
 
## Exercise 2 – Use Code Component

In this exercise, you will use the code component you created in the PrioritZ Ask canvas application.

### Task 1: Allow Power Apps component framework

In this task, you will allow the publishing of canvas apps with code components for your environment.

1. Navigate to the Power Platform admin center by using the below URL and select environments.
     ```
        https://admin.powerplatform.microsoft.com/environments
     ```

2. Open the dev environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />** that you are using for this lab.

3. Click **Settings** from top menu.
    
     ![](images/L02/settingsu.png)

4. Expand **Products(1)** and select **Features(2)**.
    
     ![](images/L02/featureu.png)

 5. Turn on **Allow publishing of canvas apps with code components** and Scroll down click **Save**.
     
      ![](images/L02/image35.2u.png)

      ![](images/L02/image35.2uu.png)

 ### Task 2: Edit canvas app

In this task, you will edit the PrioritZ Ask canvas application to use the code component you created.

1. Navigate to the Power Apps maker portal by using the below URL if not already open. Make sure the development environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />** is selected.
   ```   
   https://make.powerapps.com/
   ```
2. Select **Solutions** and open the **PrioritZ** solution.
    
3. Select **Apps (1)** , select the **PrioritZ Ask (2)** app and click **Edit (3)**.

     ![](images/L02/L02-editu.png)

4. Select the **Components** tab, click on the backward arrow  to **Import**
    **components**.

     ![](images/L02/image38u.png)
 
1. Select the **Code (1)** tab, select the code component **(2)** you created and click **Import (3)**.
    
     ![](images/L02/L02-codeuu.png)
 
7. Select the **Screens** tab.

8. Expand **votescreen (1)** and Select the **Votes gallery (2)**.

     ![](images/L02/L02-votescreenu.png)

1. Select the **Width** from the properties dropdown.    

     ![](images/L02/L02-votescreen1u.png)

9. Set the **Width** value of the Votes gallery to **570**.

     ![](images/L02/L02-widthu.png)
    
10. The screen should now look like the image below.
     
      ![](images/L02/image40u.png)

11. Select the **Votes Screen** and click **+ Insert**.
      
     ![](images/L02/image41u.png)
 
12. Select the component **PrioritZDnDRanking** under **Code Components**.
      
     ![](images/L02/image42uuu.png)
 
13. Go to the Tree view tab and select the **PrioritZDnDRanking** you just added.

14. Set the **Items** value of the **PrioritZDnDRanking** component to the formula below.

    ```
    'Votes gallery'.AllItems
    ```
     ![](images/L02/L02-voteitemu.png)
    
15. Select the **PrioritZDnDRanking**, go to the **Properties** pane that is present at the right side of the screen, set **Item Height** 160 and click **Edit Fields**.

      ![](images/L02/image43u.png)

16. Click on **+ Add field** to add a new field.
    
17. Select **Rank (1)** and click on **Add (2)**.
     
      ![](images/L02/image44.1uu.png)
 
18. The rank should now show on the control, but it is sorted descending.
      
19. Select the **Votes gallery**, then select the **Items** property from the property dropdown and change the sort order to **Ascending**.
     
      ![](images/L02/image46.png)
 
 20. The rank should now get sorted ascending.

21. Select the **PrioritZDnDRanking** component then **X** property from the property dropdown.
 
      ![](images/L02/image47.1.png)
      
22. Set the **X** value of the **PrioritZDnDRanking** component to the below formula.

    ```
    'Votes gallery'.Width
    ```
23. Select the **Width** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value to **60**.
    
24. Select the **Height** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value with the below formula.

    ```
    'Votes gallery'.Height
    ```
25. Select the **ItemHeight** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value with the below formula

    ```
    'Votes gallery'.TemplateHeight
    ```
26. Select the **BackgroundColor** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value to **"LightBlue"**
    
27. Select the **DragBackgroundColor** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value to **"#A70202"**

28. Select the **Y** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value with the below formula.

    ```
    'Votes gallery'.Y
    ```
    
29. Select the **OnSelect** property  of the **PrioritZDnDRanking** component from the property dropdown and set its value with the below formula.

    ```
    With(
        {
            sourceRank: First(Self.SelectedItems).Rank,
            destinationRank: Last(Self.SelectedItems).Rank
        },
        If(
            sourceRank < destinationRank,
     // Moving Up
            UpdateIf(
                colVotes,
                Rank >= sourceRank && Rank <= destinationRank,
                {
                    Rank: If(
                        Rank <> sourceRank,
                        Rank - 1,
                        destinationRank
                    )
                }
            );
        );
        If(
            sourceRank > destinationRank,
     // Moving Down
            UpdateIf(
                colVotes,
                Rank >= destinationRank && Rank <= sourceRank,
                {
                    Rank: If(
                        Rank <> sourceRank,
                        Rank + 1,
                        destinationRank
                    )
                }
            );
        );

    );

    ```

30. Select the **Home Screen** and click **Play**.

31. Select one of the **topics**.

32. You can view how it looks on a phone screen by using the emulator.
     
     ![](images/L02/image48up.png)

33. Drag one of the topic items and drop it in a different location.
     
     ![](images/L02/image49.png)
 
34. The drag/drop should work as expected.
35. Close the preview.
1. Click **Publish**.

    ![](images/L02/publish.png)

38. Select **Publish this version** and wait for the publish to be completed.
39. You may **close** the canvas app studio.


## Exercise 3 – Add Code Component to Solution

In this exercise, you will add the code component you created to the PrioritZ solution.

### Task 1: Add the component to the solution

1. Navigate to the Power Apps maker portal by using the below URL if not already open. Make sure the development environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** is selected.

   ```
    https://make.powerapps.com/
    ```
2. Select **Solutions** and open the **PrioritZ** solution.
3. Click **Add existing** and select **More | Developer | Custom control**.
      
      ![](images/L02/image50.1.png)
 
 4. Select **contoso_ContosoCoffee.PrioritZDnDRanking (1)** and click **Add (2)**.
     
      ![](images/L02/image51.1.png)
 
 5. Click **Publish all customizations** and wait for the publishing to complete.

    ![](images/L02/L02-EX3.png)

## Summary

In this lab,you learned to create a code component, implement its logic, integrate it into a canvas app, and add it to a solution within the Power Platform.

## You have successfully completed the lab

