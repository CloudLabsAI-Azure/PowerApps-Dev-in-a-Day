# Lab 01 - Getting started with Powerapps

## Estimated Duration: 45 minutes

Working as part of the Prioritz fusion team you will be setting up your Power Platform development environment. You will import and review the current solution and explore the current state of the Prioritz apps, flows and tables. You will be also adding a column to a table and modifying the app to use it.

## Lab Objectives

You will be able to complete the following :

1. Exercise 1 - Import and review solution components

   - Task 1: Import, review solution components and run flow

   - Task 2: Test the apps

2. Exercise 2 – Add a column for My Notes

   - Task 1: Add a new column

   - Task 2: Update admin app

3. Exercise 3 – Verify the pre-installed Visual Studio Code Installer and Power Platform CLI Extension

   - Task 1: Test the Power Platform CLI

## Exercise 1 - Import, and review solution components

In this exercise, you will import the current solution into the pre-created dev environment and review the components of the solution. You will also run a flow that will add sample data to your environment, and test the applications in the solution.

>**Note**: The Dev environment is already pre-created as a part of the prerequisites.

### Task 1: Import, Review solution components and run flow

1. In the JumpVM, click on the **Power Apps** portal shortcut of the Microsoft Edge browser that is available on the desktop.

   ![azure portal.](images/L01/PAportal.png)

1. On the **Sign in** window, you will see the login screen, enter the following username **(1)** and click on **Next** **(2)**.

   * Email/Username: <inject key="AzureAdUserEmail"></inject>

   ![](/images/L01/signin.png)

1. Now enter the following password **(1)** and click on **Sign in** **(2)**. 

   * Password: <inject key="AzureAdUserPassword"></inject>
   
   ![](/images/L01/signinp.png)

1. If you see the pop-up **Stay Signed in?**, click **No**.

1. Once logged in, click on **Environment (1)** and select the pre-created dev environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)**.   

     ![](images/L01/dev11.png)

2. Now, click on **Solutions(1)** from the left hand side menu and click **Import Solution(2)**.

      ![](images/L01/importsolution1.png)

3.  Click **Browse**.
    
     ![](images/L01/browse1.png)
     
1. Navigate to this path `C:\LabFiles\Developer-in-a-day\Student\L01 - Getting started\Resources` in file explorer , select the **Prioritz_1_0_0_7.zip** file, and click **Open**.

1. Make sure the **Prioritz(1)** file is selected and Click **Next(2)**.
    
     ![](images/L01/next1.png)
     
1. Click **Next** again on the import solution blade.

1. Under the **Connections** section, click on the ellipsis button **...(1)** next to **Microsoft Dataverse Priority**.

1. Ensure that  **odl_user(2)** you are using  is selected.

1. Click on **Import(3)**.

    ![](images/L01/connection1.png)
    
1. Wait until the solution import to complete.

     ![](images/L01/solutionsuccess.png)
     
1. You should now see the solution you imported in the list of solutions.

1. Open the **Prioritz** solution you imported.

4. Expand **Tables (1)** and select the **PrioritZ Topic (2)** table.
   
     ![](images/L01/L01-table1.png)

5. Select the **Columns** under Schema and review the columns of the **PrioritZ Topic** table.

   >**Info**: The standard columns are built-in, and all tables have them. The custom columns were created by the team for this application.
 
   ![](images/L01/L01-coulumn.png)

6. Select the **Relationships** tab from the Columns dropdown and review how this table is related to other tables.
 
    ![](images/L01/L01-relation.png)
 
    ![](images/L01/L01-relation1.png)

1. Select **Cloud flow (1)** and open the **Import sample data – Topics (2)** flow.
 
    ![](images/L01/L01-cloud1.png)

9. Click on **Edit** button to review the flow.
  
    ![](images/L01/edit21.png)

10. Expand the **Parse JSON** step and review the data this flow will create.

    ![](images/L01/L01-parse1.png)
    
    >>**Note**:If you're unable to expand the step, click on the ellipsis (...), then select Settings, and click Cancel.
    
12. Expand the **Apply to each topic** step.
    
    ![](images/L01/L01-topic1.png)

13. Expand the **Apply to each topic item** step.
   
     ![](images/L01/L01-eachtopic1.png)

14. The **Apply to each** step should look like the image below. This is the logic for the automation.
 
    ![](images/L01/image111.png)

15. Click on the **<- back** button.
 
    ![](images/L01/image121.png)

16. Click on the flow name to open the flow details screen.

     ![](images/L01/EX1-T1-141.png)

17. Click on **Run** to run the flow.
   
     ![](images/L01/image131.png)

18. Click the **Run flow** button on the Run flow blade.

     ![](images/L01/L01-new1.png)

     > **Note**: If you receive this error `Error from the token exchange: Permission denied due to missing connection` while running the flow, this is because the **Dataverse connection** is not being added correctly. Delete the imported solution and try to re-import the solution by performing the **Steps 6-14** of this task again, then try to trigger the flow again.

19. Click **Done** and wait for the flow run to complete.

     ![](images/L01/EX1-T1-181.png)

20. The flow should run successfully. If you want, you can click on the run row and it will show you
    the details of what the flow did.
   
      ![](images/L01/image141.png)

### Task 2: Test the apps

1. Navigate back to **PrioritZ** solution by clicking on **Cloud flows**. Alternatively, you can also open the **Power Apps** maker portal by using this URL `https://make.powerapps.com` if not already open. Make sure the development environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** is selected.
       
   ![](images/L01/cloud1u.png)

1. Navigate to **Solutions** blade by clicking on **Back to Solutions** **(<-)** button.

   ![](images/L01/solutions.png)
   
2. Select **Apps (1)** from the left-hand side menu of Power Apps,  you should see two applications named **PrioritZ Ask** and **PrioritZ Admin (2)**. 

     >**Info:** **PrioritZ Admin** app is used to manage topics being asked about and **PrioritZ Ask** app allows users to respond.

    ![](images/L01/EX1-T2-2_1_1u.png)

3. Launch the **PrioritZ Admin** application by clicking on **play** symbol.
    
    ![](images/L01/L01-adminu1.png)

4. You should see the below four topics.

    ![](images/L01/EX1-T2-4-2u.png)

5. Click to open **Event banner** topic.

6. You should see the topic details with some topic items.

    ![](images/L01/EX1-T2-6-1u.png)

7. Click on the **<** back button.

    > **Note**: You should go back to the home screen.

9. Now, click on the **+** button to add a new topic.
    
    ![](images/L01/image16u.png)

10. Provide the below information and click **add a picture** that is present below **Respond By** field.
     
     1. **Topic**: Enter `Change Taco Tuesday to some other food`
     
     1. **Details**: Enter `People are tired of tacos, what should we have instead of tacos?`
     
     1. **Respond By**: Select **today's date**.
     
     ![](images/L01/image17u.png)

11. Navigate to this path C:\LabFiles in file explorer, select **image.png** and click open.

12. Type **Tamale Tuesday** on the Choice field and click **add a picture** that is present below the Choice field.
     
      ![](images/L01/image18u.png)

11. Navigate to this path `C:\LabFiles` in File Explorer, select **image.png** and click open.

13. Click **+** to add the choice.
     
      ![](images/L01/image191.png)

14. Add a couple more choices by repeating **steps 12-14**.
       
       1. **Choice 1** : Enter `Steak Tuesday`
       
       2. **Choice 2**: Enter `Cheese and Wine Tuesday`

15. Click on **Save** button to save the topic.
    
    ![](images/L01/image20u.png)

16. The new topic should be saved, and you should be navigated back to the main screen.

17. You should see the topic you added to the list of topics.

     ![](images/L01/L01-tacou.png)

18. Close the PrioritZ Admin application by closing the browser tab in which the PrioritZ Admin application is open.

19. Select **Apps (1)** from the left-hand side menu of Power Apps and launch the **PrioritZ Ask (2)** application by clicking on play symbol.
     
     ![](images/L01/L01-prioritzasku.png)

20. You should see a list of topics. Open the **Change Taco Tuesday to some other food** topic that you created in the previous steps.

     ![](images/L01/L01-listu.png)

21. Click on the **up/down** icons order the items in the order you prefer them and click **Vote**.
     
      ![](images/L01/L01-choiceuu.png)

22. You should be navigated back to the main screens, and you should see a notification message.   
      ![](images/L01/TVU.png)
    
23. Close the PrioritZ Ask app by closing the browser tab in which the PrioritZ Ask application is open.

## Exercise 2 – Add a column for My Notes

In this exercise, you will add a new column **My Notes** to the topic table and update the PriortZ Admin
application.

### Task 1: Add a new column

1. Navigate to the Power Apps maker portal by using the below URL if not already open. Make sure the development environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** is selected.
    ```
    https://make.powerapps.com
   ```
2. Select **Solutions (1)** from the left-hand side menu of Power Apps and open the **PrioritZ (2)** solution.

   ![](images/L01/EX2-T1-2-1u.png)

3. Expand **Tables(1)** and select the **PrioritZ Topic(2)** table.

4. Select the **Columns** tab that is present under **+ New(3)** and click **column(4)**.

    ![](images/L01/EX2-T1-4u.png)

5. Enter the below value in the Display name field.

   ```
   My Notes
   ```
1. Now, search for **Plain text (1)** under Data type then select the one that comes under **Multiline Text (2)** , and click **Save (3)**.

    ![](images/L01/L01-notesu.png)
   
   > **Note**: Do not navigate away from this page.

### Task 2: Update the admin app

1. Make sure you are still in the **PrioritZ** solution. Select **Apps (1)** under **Objects** and select the **PrioritZ Admin (2)** application and click on **Edit (3)**.
    
    ![](images/L01/L01-admineditu.png)

2. Select the **Add Topic Screen(1)**.

3. Click **+ Insert(2)** and select **Text input(3)**.
   
     ![](images/L01/tinputu.png)

4. Double-click on the newly added **Text input** and enter the below value to rename the text input.

    ```
    Notes textbox
    ```
    
     ![](images/L01/image27u.png)

5. Make the add picture control smaller if needed, and move the **Respond By and label textbox** down and place the **Notes textbox** between the Details control and the Respond by label.
   
    ![](images/L01/image28u.png)

6. Select **Notes textbox** and then **HintText** from the properties dropdown.

    ![](images/L01/hintextu.png)

7. Change the **HintText** value of the Notes textbox to the below value. 

    ```
    My notes
    ```
   
    ![](images/L01/image29u.png)

8. Select the **Mode** from the properties dropdown and change its value by entering the below text.

    ```
    TextMode.MultiLine
    ```

    ![](images/L01/L01-modeu.png)

9. Select **Save topic icon** under **Add Topics Screen** section.
     
     ![](images/L01/image30u.png)

10. Replace the **OnSelect** formula of the **Save topic icon** with the formula below. The Patch creates
    a new row in the Dataverse table.
     
     ![](images/L01/image31u.png)

    ```
    Set(newTopic,Patch('Prioritz Topics',Defaults('Prioritz Topics'),{'My Notes': 'Notes textbox'.Text,Topic:'Topic name textbox'.Text,Details:'Topic details textbox'.Text,'Respond By':'respond by date picker'.SelectedDate,Photo:AddTopicImage.Image}));ForAll(colAddChoices,Patch('Prioritz Topic Items',Defaults('Prioritz Topic Items'),{Choice:ThisRecord.choice,'PrioritZ Topic':newTopic,Photo:ThisRecord.photo}));Back()
    ```
11. Select the **View Topic Screen (1)** from the **Screens** tab.

12. Click **+ Insert(2)** tab and select **Text label(3)**.

    ![](images/L01/tlabelu.png)

13. Double-click on the newly added label and enter the below value to rename the label you just added.

     ```
     Notes label
     ```
     
    ![](images/L01/L01-labelu.png)

14. Change the **Text** value of the Notes label with the below text.

     ```
     'Topics gallery'.Selected.'My Notes'
     ```

15. Rearrange the controls and move the **Notes label** between the details label and the Topic items
    gallery.

16. Select the **Home Screen(1)** and click **Preview the app(2)**.
      
      ![](images/L01/image34u.png)

17. Click on the **+** button to add a new topic.

      ![](images/L01/L01-taco-1_1u.png)

18. Fill out the form by providing the below information and click **add a picture** that is present below the **Respond By** field.

       1. Topic: `Test Notes` (1)
       
       2. Details: `Testing the notes` (2)
       
       3. Text input: `Prioritz Admin topic` (3)
       
       4. Respond By: **Today's date** (4)
      
      
19. Navigate to this path C:\LabFiles in file explorer, select **image.png** and click open.

20. Type **Test One** on the Choice field and click **add a picture** that is present below the Choice field.
     
      ![](images/L01/image18uu.png)

21. Navigate to this path `C:\LabFiles` in File Explorer, select **image.png** and click open.

22. Click **+** to add the choice.
     
      ![](images/L01/image19u.png)

23. Add one more choice by repeating **steps 20-22** of this task.
       
       1. **Choice 1** : Enter `Test Two`
24. After adding all the Choices and topic details, your screen should look like the below screenshot.

   ![](images/L01/L01-testnotesu.png)
      
25. Now, click on the **Save** button. The new topic should be **saved**.

26. Click to open the **Test Notes** topic that you just created.

27. The notes **Prioritz Admin topic** that you added earlier should now be visible.
 
     ![](images/L01/image36.1.png)

28. Close the app **preview**.

29. Click **Publish**.

    ![](images/L01/publish.png)

30. Select Publish this version and wait for the publishing to complete.

     ![](images/L01/NewUipublish1u.png)

 31. You may close the **app designer**.

## Exercise 3 – Test the Power Platform CLI

In this exercise, you will review and test the Power Platform CLI extension in Visual Studio Code.

>**Note**: Visual studio code and Power platform CLI installation are already done as a part of the prerequisites.

1. Navigate to the Power Platform admin center by using the below URL and select **Environments**.
      ```
        https://admin.powerplatform.microsoft.com/environments
      ```

1. If you see the pop-up **Stay Signed in?**, click **No**.

2. Click to open your dev environment named **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />**.

3. Right-click on the **Environment URL** value and paste the value in Notepad.
 
    >**Note**: Make sure the Environment URL value is copied along with the **https**. Your copied value should look like this `https://orgxxxxxx.crm.dynamics.com/`

    ![](images/L01/image37u.png)

4. In the JumpVM, start **Visual Studio Code** using the shortcut available on the desktop.

   ![](images/L04/vscode1.png)
   
6. Click **Terminal** and select **New Terminal**.

    ![](images/L01/image42u.png)

7. Run the below command in the terminal.
   ```
   pac
   ```
   
   > **Info:** If you encounter an error after using the **pac** command, download the Power Platform CLI from the **https://aka.ms/PowerAppsCLI** link, 
     open   the installer, and complete the installation. Then, try the step again.

8. Replace `<your environment URL>` in the below command with the value of the environment URL that you copied earlier then run the command. 
   ```
   pac auth create --name DevAuth --url <your environment URL>
   ```

   > **Info:** After adding the environment URL, the command will look like this: `pac auth create --name DevAuth--url https://org32172839283.crm.dynamics.com/`
  
    ![](images/L01/Eeditpac.png)

1.  Complete the **Sign in** process, using the below credentials.

      * Email/Username: <inject key="AzureAdUserEmail"></inject>
      * Password: <inject key="AzureAdUserPassword"></inject>

9. Select **Power Platform (1)** tool, you should now have at least one **auth profile (2)**. If you have more than one profile, make sure the profile you created is selected
   
    ![](images/L01/L01-authu.png)

    > **Note** : If you are able to see the **Universal Profile** instead of **DeVAuth** profile, it is because of adding the incorrect **Environment URL** value in the **pac auth create** command in Step 9. To fix this issue, follow the below steps:

      1. Delete the **Universal Profile** from Visual Studio Code by clicking on the delete button.
      2. Copy the correct **Environment URL** value by following **Step 5** of this task. 
      3. Perform the **Step 9** of this task again to create the auth profile.

10. Click **Terminal** and select **New Terminal** if not already open.

     ![](images/L01/image42.png)

11. Run the command below to see a list of solutions.

      ```
      pac solution list
      ```
12. You should see a list of solutions installed on your environment.
    
    ![](images/L01/sollistu.png)

## Summary
In this lab , you learned to import and execute a starting solution, customize it by adding a new column and updating the admin app, and verify functionality using the Power Platform CLI.

## You have successfully completed the lab

# Click on Next >> to procced with next lab.
