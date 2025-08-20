# Lab 04 - Application lifecycle management

## Estimated Duration: 130 Minutes

Working as part of the PrioritZ fusion team, you will be configuring GitHub Actions with the Power Platform Build Tools to automate and streamline the team’s deployments. This involves setting up continuous integration and continuous deployment (CI/CD) pipelines to ensure seamless and efficient delivery of updates to Power Platform applications, while also managing version control, testing, and deployment processes to enhance collaboration and maintain high-quality standards across the team’s projects.

Lab Objectives 

- Exercise 1: Configure a Service Principal
- Exercise 2: Promote Solution to Test Environment 
- Exercise 3: Create GitHub Repo 
- Exercise 4: Export and Branch 
- Exercise 5: Release to Test 

## Exercise 1 – Configure a Service Principal

In this exercise, you will create a service principal. The service principal will be used by the workflow actions, so they do not execute under your individual user identity.

### Task 1: Create app registration

1. Navigate back to the browser tab in which the Azure Portal is open. If not already open, navigate to the Azure Portal using the URL below.

   ```
   https://portal.azure.com/
   ```

1. From Azure Portal home page, search for **Microsoft Entra ID** **(1)** in the search bar and select **Microsoft Entra ID** **(2)** from the suggestions.

   ![](images/dev3.png)
   
1. Select **App registrations** **(1)** from the side blade under Manage and click on **+ New registration** **(2)**. This application registration will be used for the connector to access the protected API.

   ![](images/L05/8-8-25-l4-1.png)

1. Please provide the following details and click on **Register** **(3)**.
   
   - Name: **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** **(1)**

   - Supported account types: **Accounts in this organizational directory only (Azure HOL - xxxxxx only - Single tenant)** **(2)**

     ![](images/L05/8-8-25-l4-2.png)
   
1. Copy the **Application (client) ID (1)**, **Directory (Tenant) ID (2)**, and save it in a notepad as you need it for later use.
     
   ![](images/9-8-25-l4-1.png)

1. Select **Certificates & secrets (1)** from the side blade and click on **+ New client secret (2)**.

   ![](images/L05/8-8-25-l4-4.png)

1. Enter **GitHub client secret<inject key="DeploymentID" enableCopy="false" />** **(1)** as description, set expiry to **90 days (3 months)** **(2)**, and click on **Add** **(3)**.
   
   ![](images/L05/8-8-25-l4-5.png)
   
1. Copy the **value** and save it in a notepad as you need it for later use.

   ![](images/L05/8-8-25-l4-6.png)

   >**Note:** Make sure to copy and paste the correct **Application (client) ID**, **Directory (Tenant) ID** and **Secret** value. Copying the incorrect value will result in issues in the next steps/tasks.

### Task 2: Create a new Dataverse

In this task, you will create a new test Dataverse environment.

1. Open a new browser window or tab and navigate to the Power Platform Admin Center using the URL below.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```

1. Click on **+ New** to create a new Dataverse.         

   ![](images/L05/task2-st2.png)

1. In the **New environment** tab.
   
   - Name: **DEV_ENV_TEST (1)**.
   
   - Make this a Managed Environment: **Enable Yes (2)**.
   
   - Group: **None (3)**. and scroll down.
   
   - Type: **Developer (4)** and click on **Next (5)**.
   
   - Deploy sample apps and data?: **Enable Yes (6)** and click on **Save (7)**.
   
      ![](images/L05/8-8-25-l4-8.png)
   
      ![](images/L05/8-8-25-l4-9.png)
   
      ![](images/L05/8-8-25-l4-10.png)

1. You can now see the new Dataverse, **DEV_ENV_TEST**, that you created.

   ![](images/L05/newtask5.png)

### Task 3: Create an app user in Dataverse

In this task, you will be registering the app you created in Microsoft Entra ID into the dev and test
Dataverse environments. You will also be assigned a security role that will allow the service principal to
deploy solutions.

1. Open a new browser window or tab and navigate to the Power Platform Admin Center using the URL below.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```

1. Click on **Environments** **(1)** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** **(2)**.

   ![](images/L05/8-8-25-l4-11.png)
   
1. From your environment page, click on **Settings**.

   ![](images/L05/8-8-25-l4-12.png)
   
1. Expand **Users + permissions** **(1)** and select **Application users** **(2)**.
    
   ![](images/L05/8-8-25-l4-13.png)

1. In the Application users page, click on **+ New app user**.

   ![](images/L05/8-8-25-l4-14.png)
   
1. In Create a new app user tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** **(1)** application registration which you created earlier and click on **Add** **(2)**.

   ![](images/L05/8-8-25-l4-15.png)

1. Type **org** and select your **Business unit** **(1)** and in **Security roles** click on **edit symbol (2)** and select **System administrator (3)** then click on **Create (4)**.

   ![](images/L05/diad5l14u.png)

    > **Note:** If the **#** symbol is still visible before GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, click on it and refresh the pane to remove it.
   
1. Again go back to **Environments** **(1)** in the side blade and select your **DEV_ENV_TEST** **(2)**.

   ![](images/L05/diad5l17u.png)
   
1. From your test environment page, click on **Settings**.

   ![](images/L05/8-8-25-l4-16.png)

1. Expand **Users + permissions** **(1)** and select **Application users** **(2)**.
    
   ![](images/L05/8-8-25-l4-17.png)
   
1. In the Application users page, click on **+ New app user**.

   ![](images/L05/8-8-25-l4-18.png)
   
1. On the **Create a new app user** tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** **(1)** application registration which you created earlier and click on **Add** **(2)**.

   ![](images/L05/8-8-25-l4-19.png)

1. Type **org** and select your **Business unit** **(1)** and In **Security roles** click on **edit symbol (2)** and select **System administrator (3)** then click on **Create (4)**.

   ![](images/L05/diad5l14u.png)

    > **Note:** If the **#** symbol is still visible before GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, click on it and refresh the pane to remove it.
   
1. Click on **Environments** **(1)** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** **(2)**.

   ![](images/L05/envu.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/8-8-25-l4-20.png)
   
1. Again go back to **Environments** **(1)** in the side blade and select  **DEV_ENV_TEST (2)** environment.

   ![](images/L05/diad5l17uu.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/8-8-25-l4-21.png)

## Exercise 2 – Promote Solution to Test Environment

In this exercise, you will export the Contoso Badges connector solution from the Dev environment and import it to the Test environment.

### Task 1: Export solution

1. Navigate to the Power Apps  maker portal and make sure you are in your dev environment **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />**

   ```
   https://make.powerapps.com
   ```

2. Go to **Solutions (1)**, select **Contoso Badges connector (2)**, and click **Export Solution (3)**.
   
     ![](images/L03/L03-EX4-exporta.png)

3. On the **Before you export** blade, click **Publish** and wait for the publishing to complete.

    ![](images/2138.png)

4. Once published, click on **Next**.

5. Select **Managed** and click **Export**.

6. Wait for the solution to be exported.

7. Click the **Download** button at the top-right corner of the screen, then select **Download Solution**
 
    ![](images/L03/SolutionDown1.png)

### Task 2: Import solution

1. Navigate to the Power Apps maker portal if not already open and select your **Test** environment, click on **Environment (1)** and select the pre-created dev environment named **DEV_ENV_TEST (2)**. 

    ```
    https://make.powerapps.com
    ```

    ![](images/9-8-25-l4-2.png)

2. Click **Import Solution**.
    
     ![](images/L03/L03-EX5.png)
     
     >**Note:** Try refreshing the browser if solutions are not opened.

3. Click **Browse**.

4. Select the solution you exported from the Dev environment and click **Open**.

5. Click **Next**.

6. Click **Import** and wait for the import to complete.

7. The solution should be imported successfully. **Do not** navigate away from this page.

### Task 3: Test connector

1. Click on **Solution (1)** then **All (2)** and then select **Contoso Badge Connector (3)** to open the solution you just imported.

      ![](images/L05/8-8-25-l4-22.png)

2. Click to open the **Badges connector**.
  
    ![](images/L05/8-8-25-l4-23.png)

    >**Note:** If you receive the error message as **could not retrieve the connector data**, wait for a few mins (5-10 mins) to get the connector data updated. If that doesn't work, you can delete the imported connector and perform the **steps 1-7** in the **Task 2: Import solution** task again, then try to open the connector.

3. Click **Edit**.

4. Select the **Test** tab from the dropdown.
   
     ![](images/L05/8-8-25-l4-24.png)

5. Click **+ New connection**. A new browser tab will be opened to create a connection.

      ![](images/L05/8-8-25-l4-25.png)

6. Start a new browser window or tab and navigate to the URL below to open the Contoso Coffee Badges API.

   ```
   https://contosobadgestest.azurewebsites.net/
   ```

7. Click on the **Get an API Key** link.
  
     ![](images/L05/8-8-25-l4-26.png)

8. Copy the **API Key** value.

9. Go back to the connector editor, paste the API Key you copied in the previous step and click **Create connection**. Now, close the browser tab by clicking on **X**.
   
     ![](images/L05/8-8-25-l4-27.png)

10. Click **Refresh** connections.
     
      ![](images/L03/image62.png)

11. Go to the **Operations (1)** section and select the **AddCredit (2)** operation.

      ![](images/L03/image62a.png)

12. Provide your email for **recipientid (1)** , provide a **name (2)** , enter **1** for **points (3)** , and click **Test operation (4)**.
     
     ![](images/9-8-25-l4-3.png)

13. The test should succeed, and the response should look like the image below.
      
      ![](images/L03/image64u.png)

## Exercise 3 – Create GitHub Repo

In this exercise, you will create a GitHub repository and add repository secrets.

### Task 1: Create a repository

1. Navigate to the URL below and sign in using your GitHub credentials.

   ```
   https://github.com/
   ```

2. Click on your profile icon and select **Your repositories**.

   ![](images/L05/github1u.png)

3. Click **New repository** to create a repository.

   ![](images/L05/github2u.png)

4. Enter **PrioritZ (1)** for Repository name, select **Public (2)** , check the **Add a README file (3)**.

   ![](images/L05/github3.png)

5. Click **Create repository** to create it.

   ![](images/L05/github4.png)

6. Click **Settings (1)** to open the settings tab.

7. Go to the **Security** section, expand **Secrets and variables(2)** and select **Actions (3)**.

    > **Note:** The values you provide will not be visible after you create the item, so take your time to get the values correct. 
      
     ![](images/L05/actionpermissionuuu-1.png)
   
8. Click **New repository secret** to add a secret.

     ![](images/L05/github7uu.png)

9. Enter **PowerPlatformAppID (1)** for Name and paste the odl username: **<inject key="AzureAdUserEmail"></inject> (2)** and **click Add Secret (3)** 

     ![](images/L05/github8u.png)

10. Click **New repository secret** again.

11. Enter **PowerPlatformClientSecret (1)** for Name and paste the password: **<inject key="AzureAdUserPassword"></inject> (2)** and click **Add Secret (3)** 

     ![](images/L05/github9u.png)

12. Click **New repository secret** again.

13. Enter **PowerPlatformTenantID (1)** for Name and paste the secret **Tenant ID (2)** from your notepad that you noted earlier in **`Exercise 1 -> Task 1 -> Step 5`** in the **Value** field and click **Add secret (3)**.

     ![](images/L05/github10u.png)

14. Click **New repository secret** again.

15. Enter **PowerPlatformDevUrl (1)** for Name and paste the secret **Dev environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 3 -> Step 17`** in the **Value** field and click **Add secret (3)**.

    >**Note:** Make sure you are pasting the dev environment URL named **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />** that you copied in the **`Exercise 1 -> Task 3 -> Step 17`**
   
     ![](images/L05/github11u.png) 
  
16. Click **New repository secret** one more time.

17. Enter **PowerPlatformTestUrl (1)** for Name and paste the **Test Environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 3 -> Step 19`** in the **Value** field and click **Add secret (3)**.

    >**Note:** Make sure you are pasting the test environment URL named **DEV_ENV_TEST** that you copied in the **`Exercise 1 -> Task 3 -> Step 19`**
 
    ![](images/L05/L05-testurla.png)
 
18. You should now have **5** repository secrets.
     
    ![](images/L05/Images20uu5u.png)

19. Do not navigate away from this page.

## Exercise 4 – Export and Branch

In this exercise, you will set a workflow action and add steps that will export the solution from the dev environment and create a new branch.

### Task 1: Export and branch

In this task, you will create the workflow definition using the YAML provided. The action YAML uses two-space indentation, so follow that carefully as you build the workflow definition. If in doubt, review the
indentation shown in the images.

1. Select the **Actions** tab and click on **Set up a workflow yourself** to create a new workflow.
 
   ![](images/L05/Imagesworku.png)
   
1. Change the file name to it **export-and-branch.yml**
       
1. Remove everything from the workflow file.
  
   ![](images/L05/diad5l32.png)

1. Navigate to `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/export-and-branch.yml` URL, copy the full content of the file and paste it into the **export-and-branch.yml** workflow.

   ![](images/L05/diad5l28u.png)

1. Click **Commit changes**.
    
   ![](images/L05/commit1.png)

1. Then click **Commit changes**.

   ![](images/L05/Images202uuu.png)

1. Click On **Settings (1)**, Go to the **Actions (2)** tab on the left side, then select **General (3)**.

   ![](images/L05/github6u-1.png)

1. In the **Workflow Permission** section, make sure **Read and write permission (1)** is selected, then click **Save (2)**.  

   ![](images/9-8-25-l4-4.png)

1. Select the **Actions** **(1)** tab and select the **workflow** **(2)** you created.

   ![](images/L05/diad5l27.png)
   
1. Click on **Run workflow**.
      
   ![](images/L05/Images2027u.png)
   
1. Click **Run workflow** again and wait for the workflow run to complete.
      
   ![](images/L05/Images2028u.png)
   
1. Select the **Code** **(1)** tab and click on **Branches** **(2)**. You should see two branches.
   
   ![](images/L05/diad5l29u-1.png)
   
1. Click to open the branch that was created by the workflow action named Prioritz-XXXXXXXX-XXXX.
   
   ![](images/L05/changesbranchu.png)

1. On the Prioritz-XXXXXXXX-XXXX branch, you should be able to see the solution folder.
      
   ![](images/L05/diad5l30u.png)
   
1. Click on **Contribute** **(1)** button and select **Open pull request** **(2)**.
        
   ![](images/9-8-25-l4-5.png)
   
1. Add a description if you like, and then click **Create pull request**.

     ![](images/L05/pr1u.png)
   
1. You should now see the pull request summary. Confirm that the branch has no conflicts with the main branch and that the changes can be merged into the main branch automatically.
   
1. Click on the chevron button next to the **Merge pull request (1)** button and select **Squash and** **merge (2)**.
      
    ![](images/9-8-25-l4-6.png)

1. Click **Squash and merge**.
   
1. Click **Confirm squash and merge**.
   
1. The pull request should get merged successfully.
   
     ![](images/L05/prdoneu.png)

## Exercise 5 – Release to Test

In this exercise, you will create a workflow action and add steps that will release the solution you exported to the test environment.

### Task 1: Create workflow

1. Now navigate to the **Actions (1)** tab.
   
1. Click **New workflow (2)**.

    ![](images/L05/nwwfu.png)
   
1. Now on **Choose a workflow** page, click **set up a workflow yourself**.
     
    ![](images/L05/Images2033u.png)
   
1. Change the file name to **release-to-test.yml**
   
    ![](images/L05/fnameu.png)
     
1. Remove everything from the workflow file.

1. Navigate to `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/release-to-test.yml` URL in the browser and copy the full content of the file and paste it into the **release-to-test.yml** workflow file.
 
   ![](images/L05/cntnu.png)
      
1. Click **Commit changes** and then click **Commit changes**.

    ![](images/L05/commit1.png)
   
1. Select the **Actions** tab and monitor the workflow.

1. The release should be completed successfully.
    
     ![](images/L05/relecomplu.png)
     
1. Navigate back to the PowerApps portal and ensure you are in the **DEV_ENV_TEST** environment.

      ![](images/L05/lastu.png)

1. Select the **solutions (1)** tab from the left side and click on **Managed (2)**. You should see the solution deployed with the name of **Prioritz (3)**.

    ![](images/L05/lastuu.png)
    
## Summary
In this lab, you learned to promote a solution to a test environment, configure a service principal, and manage your solution using GitHub for version control and workflow automation.

## You have successfully completed the Hands-on lab!
