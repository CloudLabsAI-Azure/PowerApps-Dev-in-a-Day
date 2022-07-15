# Lab 01 - Getting Started


## Table of Contents

**Lab Scenario**

1. Exercise 1 - Create Dev environment 

   - Task 1: Create dev environment 

2. Exercise 2 - Import starting solution 

    - Task 1: Import solution 

3. Exercise 3 - Review solution components 

   - Task 1: Review solution components and run flow 

   - Task 2: Test the apps 

4. Exercise 4 – Add a column for My Notes 

   - Task 1: Add a new column 

   - Task 2: Update admin app 

5. Exercise 5 – Install Visual Studio Code Installer and Power Platform CLI Extension 

   - Task 1: Install Visual Studio Code Installer and Msbuild

   - Task 2: Test the Power Platform CLI 


### Lab Scenario


Working as part of the Prioritz fusion team you will be setting up your Power Platform development
environment. You will import the current solution and explore the current state of the Prioritz apps,
flows and tables. You will be also adding a column to a table and modifying the app to use it.

## Exercise 1 - Create Dev environment

In this exercise, you will create a Power Platform developer environment. You will do all the
development work for this course in this environment. You will also be assigned a test environment for
use in the ALM labs.

**Note:** Do not use an existing environment.

### Task 1: Create dev environment

1. Navigate to Power Apps Developer Plan by using below link.
```
https://powerapps.microsoft.com/en-in/developerplan/
```
2. Select **Add a dev environment**.
    
      ![](images/L01/image1.png)

3. Sign in if prompted.
4. Select your country and click **Accept**.
5. You should be navigated to the new **dev environment created for you** . Select the new
    **environment** and stay sign in.

    ![](images/L01/image2.png)

6. Expand **Dataverse**   and select **Tables** .
7. You should see several Dataverse tables that are created with every environment.
 
    ![](images/L01/image3.png)

## Exercise 2 - Import starting solution

In this exercise, you will import a solution into your dev environment. This solution contains the current
Prioritz apps, flows and Dataverse tables.

### Task 1: Import solution

1. Navigate to Power Apps maker portal by using below URL. Make sure the development environment is selected.
   ```        
    https://make.powerapps.com
   ```
2. Select **Solutions** and click **Import**.
3. Click **Browse**.
4. Go to the lab resources folder C:\LabFiles\Developer-in-a-day\Student\L01 - Getting started\Resources, select the **Prioritz_1_0_0_7.zip** file, and click **Open**.
5. Click **Next** .
   
    ![](images/L01/image4.png)

6. Click **Next**  again on the Import solution blade.
7. Click on the **Select a connection** dropdown and the select **+ New connection**.
  
    ![](images/L01/image5.png)

8. Click **Create**.
9. Sign in if prompted.
10. Close the connections browser window or tab.
11. Click **Refresh**.
12. Click **Import** and wait for the solution import to complete.
     
     ![](images/L01/image6.png)

13. You should now see the solution you imported in the list of solutions.
14. Do not navigate away from this page.

## Exercise 3 - Review solution components

In this exercise, you will review the components of the solution you imported, run a flow that will add
sample data to your environment, and test the applications in the solution.

### Task 1: Review solution components and run flow

1. Review data model diagram
    
     ![](images/L01/image7.png)

2. Open the **PrioritZ** solution you imported.
    
     ![](images/L01/image8.png)

3. Expand **Tables** and select the **PrioritZ Topic** table.
   
     ![](images/L01/NewUi1.png)

4. Select the **Columns** under Schema and review the columns of the PrioritZ Topic table. The standard columns are built-in, and all tables have them.
 The custom columns were created by the team for this application.
 
   ![](images/L01/NewUi2.png)

5. Select the **Relationships** tab from Columns dropdown and review how this table is related to other tables.
 
    ![](images/L01/NewUi3.png)
 
    ![](images/L01/NewUi4.png)

6. Select **Cloud flow**.
7. Open the **Import sample data – Topics** flow.
 
    ![](images/L01/image10.png)

8. Click **Edit**.
  
    ![](images/L01/NewUiImport.png)

9. Expand the Parse JSON step and review the data this flow will create.
10. Expand the **Apply to each topic** step.
11. Expand the **Apply to each topic item** step.
12. The apply to each step should look like the image below. This is the logic for the automation.
 
     ![](images/L01/image11.png)

13. Click on the **<- back** button.
 
     ![](images/L01/image12.png)

14. Click on the flow name to open the flow detail screen.
15. Click Run to run the flow.
   
     ![](images/L01/image13.png)

16. Click the **Run flow** on the Run flow blade.
17. Click **Done** and wait for the flow run to complete.
18. The flow should run successfully. If you want, you can click on the run row and it will show you
    the details of what the flow did.
   
      ![](images/L01/image14.png)

### Task 2: Test the apps

1. Navigate to Power Apps maker portal by using below URL. Make sure the development environment is selected.
   ```
       https://make.powerapps.com
   ```
2. Select **Apps**. You should see two applications. **PrioritZ Ask** and **PrioritZ Admin**. PrioritZ Admin
    app is used to manage topics being asked about. PrioritZ Ask app allows users to respond.
3. Launch the **PrioritZ Admin** application.
    
    ![](images/L01/image15.png)

4. You should see at least four topics.
5. Click to open one of the topics.
6. You should see the topic details with some topic items.
7. Click on the **<** back button
8. You should go back to the home screen.
9. Click on the **+** button.
    
    ![](images/L01/image16.png)

10. Provide a topic, details, respond by date and click add a picture.
     
     ![](images/L01/image17.png)

11. Select any image from your computer.
12. Type something on the Choice filed and click add a picture.
     
      ![](images/L01/image18.png)

13. Click **+** to add the choice.
     
      ![](images/L01/image19.png)

14. Add couple more choices.
15. Click **Save**.
    
    ![](images/L01/image20.png)

16. The new topic should be saved, and you should be navigated back to main screen.
17. You should see the topic you added in the list of topics.
18. Close the PrioritZ Admin application.
19. Launch the **PrioritZ Ask** application.
     
     ![](images/L01/image21.png)

20. You should see a list of topics. Open the topic you created.
21. Click on the up/down icons and order the items in the order you prefer them and click **Vote**.
     
      ![](images/L01/image22.png)

22. You should be navigated back to the main screens, and you should see a notification message.
      
      ![](images/L01/image23.png)

23. Close the PrioritZ Ask app.

## Exercise 4 – Add a column for My Notes

In this exercise, you will add a new column **My Notes** to the topic table and update the PriortZ Admin
application.

### Task 1: Add a new column

1. Navigate to Power Apps maker portal by using below URL. Make sure the development environment is selected.
    ```
    https://make.powerapps.com
   ```
2. Select **Solutions** and open the **PrioritZ** solution.
3. Expand **Tables** and select the **PrioritZ Topic** table.
4. Select the **Columns** tab and click **+ Add column**.


5. Enter **My Notes** for Display name, select **Multiline Text** for Data type, and click **Done**.

    ![](images/L01/NewUi5.png)

6. Click **Save Table**.
7. Do not navigate away from this page.

### Task 2: Update admin app

1. Make sure you are still in the **PrioritZ** solution.
2. Select **Apps** and click to open the **PrioritZ Admin** application.
    
    ![](images/L01/image25.png)

3. Select the **Add Topic Screen**.
4. Select the **Insert** tab, click **Text** , and then select **Text input**.
   
     ![](images/L01/image26.png)

5. Rename the text input **Notes textbox**.
    
     ![](images/L01/image27.png)

6. Make the add picture control smaller if needed and move the respond by and label textbox
    down and place the Notes textbox between the Details control and the Respond by label.
   
    ![](images/L01/image28.png)


7. Select Notes **textbox**.
8. Change the **HintText** value of the Notes textbox to **My notes**.
   
    ![](images/L01/image29.png)

9. Change the **Mode** to **TextMode.MultiLine**.
10. Select **Save topic icon**.
     
     ![](images/L01/image30.png)

11. Replace the **OnSelect** formula of the **Save topic icon** with the formula below. The Patch creates
    the new row in the Dataverse table.
     
     ![](images/L01/image31.png)



      ```
      Set(newTopic,Patch('Prioritz Topics',Defaults('Prioritz Topics'),{'My Notes':
      'Notes textbox'.Text,Topic:'Topic name textbox'.Text,Details:'Topic details
      textbox'.Text,'Respond By':'respond by date
      picker'.SelectedDate,Photo:AddTopicImage.Image}));ForAll(colAddChoices,Patch('P
      rioritz Topic Items',Defaults('Prioritz Topic
      Items'),{Choice:ThisRecord.choice,'PrioritZ
      Topic':newTopic,Photo:ThisRecord.photo}));Back()
      ```
12. Select the **View Topic Screen**.
13. Go to the **Insert** tab and click **Label**.
14. Rename the label you just added **Notes label**.
15. Change the **Text** value of the Notes label to **'Topics gallery'.Selected.'My Notes'**
  
      ![](images/L01/image32.png)

16. Rearrange the controls and move the **Notes label** between the details label and Topic items
    gallery.
   
      ![](images/L01/image33.png)


17. Select the **Home Screen** and click **Preview the app**.
      
      ![](images/L01/image34.png)

18. Click **+**.
19. Fill out the form, add some choices, and then click **Save**.
 
      ![](images/L01/image35.png)

20. The new topic should be **saved**.
21. Click to open the topic you just created.
22. The notes should now be shown.
 
     ![](images/L01/image36.png)

23. Close the app **preview**.
24. Click **File** and select **Save**.
25. Click **Publish**.
26. Select Publish this version and wait for the publishing to complete.

     ![](images/L01/NewUipublish.png)

 27. You may close the **app designer**.

## Exercise 5 – Install Visual Studio Code and Install  Power Platorm CLI

In this exercise, you will install Visual Studio Code and Power Platform CLI. These tools are used in the
labs in this course.


### Task 1: Install Visual Studio Code and MSbuild 

1. Download and Install Visual Studio tool Installer by navigating to the below URL

      ```
      https://visualstudio.microsoft.com/downloads/

      ```
2. Click Free download and Open.

    ![](images/L01/Vscode.png)

    ![](images/L01/Vscode1.png)

3. Select the Check box for **ASP.NET & Web Development** and **Azure Development** in Workloads page. Then Click on **Install while downloading** button.

   ![](images/L01/VscodeMsb.png)
   
4. To set an Environment Variable for MSbuild. Search Environment variables in windows search bar.

     ![](images/L01/envvar.jpg)

5. Click Environment Variable 

     ![](images/L01/Env.png)
   
     ![](images/L01/Env1.png)

6. In System Variables Section select **Path** and click **Edit** to Paste Msbuild Path
      ```
      C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin
      ```

    ![](images/L01/Env2.png)
 
7. Click Ok and again Click Ok.

8. Navigate to Visual Studio Marketplace Power Platform Tools by using the below URL.
      ```
        https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode
      ```
   
9. Click **Install**.
10. Click Continue.
11. Wait for the Power Platform Tools to be installed.

### Task 2: Test the Power Platform CLI

1. Navigate to Power Platform admin center by using below URL and select **Environments**.
      ```
        https://admin.powerplatform.microsoft.com/environments
      ```
2. Click to open your dev environment you created.
3. Copy the Environment URL and keep it in your clipboard or on notepad.
 
    ![](images/L01/image37.png)

4. Open Visual Studio Code.
5. Click terminal  new terminal
6. Run this command and paste environment URL your copyed before. 
   ```
   pac auth create --name DevAuth --url 
   ```
 
     ![](images/L01/Eeditpac.png)


11. Go to the Environments & Solutions pane and expand the environment you are using for this
    lab.
12. You should see all the solutions in the environment.
    
     ![](images/L01/image41.png)

13. Click **Terminal** and select **New Terminal**.

     ![](images/L01/image42.png)

14. Run the command below to see list of solutions.

      ```
      pac solution list
      ```
15. You should see list of solutions installed on your environment.
    
    ![](images/L01/image43.png)

