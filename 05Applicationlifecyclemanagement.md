# Lab 04 - Application lifecycle management

## Estimated Duration: 90 minutes

Working as part of the PrioritZ fusion team, you will be configuring GitHub Actions with the Power Platform Build Tools to automate and streamline the team’s deployments. This involves setting up continuous integration and continuous deployment (CI/CD) pipelines to ensure seamless and efficient delivery of updates to Power Platform applications, while also managing version control, testing, and deployment processes to enhance collaboration and maintain high-quality standards across the team’s projects.

Lab Objectives 

- Exercise 1: Configure a Service Principal 
- Exercise 2: Create GitHub Repo 
- Exercise 3: Export and Branch 
- Exercise 4: Release to Test 

## Exercise 1 – Configure a Service Principal

In this exercise, you will create a service principal. The service principal will be used by the workflow
actions, so they do not execute under your individual user identity.

### Task 1: Create app registration

1. Navigate back to the browser tab in which Azure Portal is open. If not already open, navigate to Azure Portal using the below URL.

   ```
   https://portal.azure.com/
   ```

1. From Azure Portal home page, search for **Microsoft Entra ID** ***(1)*** in the search bar and select **Microsoft Entra ID** ***(2)*** from the suggestions.

   ![](images/dev3.png)
   
1. Select **App registrations** ***(1)*** from the side blade under Manage and click on **+ New registration** ***(2)***. This application registration will be used for the connector to access the protected API.

   ![](images/L05/diad5l2uu.png)

1. Please provide the following details and click on **Register** ***(3)***.
   
   - Name: **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)***
   - Supported account types: **Accounts in this organizational directory only (OTU WA HOL - <inject key="Deployment ID" enableCopy="false" /> only - Single tenant)** ***(2)***

     ![](images/L05/diad5l3uup.png).
   
1. Copy the **Application (client) ID**, **Directory(Tenant) ID**, and save it in a notepad as you need it for later use.
     
   ![](images/L05/diad5l4uu.png)

1. Select **Certificates & secrets** from the side blade and click on **+ New client secret**.

   ![](images/L05/diad5l5uu.png)

1. Enter **GitHub client secret<inject key="DeploymentID" enableCopy="false" />** ***(1)*** as description, set expiry to **3 months** ***(2)***, and click on **Add** ***(3)***.
   
   ![](images/L05/diad5l6uu.png)
   
1. Copy the **value** and save it in a notepad as you need it for later use.

   ![](images/L05/diad5l7uu.png)

   >**Note**: Make sure to copy and paste the correct **Application (client) ID**, **Directory(Tenant) ID** and **Secret** value. Copying the incorrect value will result in issues in the next steps/tasks.

### Task 2: Create a new Dataverse

In this task, you will  a new test Dataverse environments.

1. Open a new browser window or tab and navigate to the Power Platform Admin Center using the below URL.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```
1. Click on **+New** to create a new Dataverse.         

   ![](images/L05/newtask1.png)

1. In the **New environment** tab.
   
   - Name: **DEV_ENV_TEST(1)**.
   
   - Make this a Managed Environment: **Enable Yes(2)**.
   
   - Group: **None(3)**. and scroll down.
   
   - Type: **Developer(4)** and click on **Next(5)**.
   
   - Deploy sample apps and data?: **Enable Yes(6)** and click on **Save(7)**.
   
   ![](images/L05/newtask2.png)

   ![](images/L05/newtask3.png)

   ![](images/L05/newtask4.png)

1. You can now see the new Dataverse, **DEV_ENV_TEST**, that you created.

   ![](images/L05/newtask5.png)

### Task 3: Create an app user in Dataverse

In this task, you will be registering the app you created in Microsoft Entra ID into the dev and test
Dataverse environments. You will also be assigned a security role that will allow the service principal to
deploy solutions.

1. Open a new browser window or tab and navigate to the Power Platform Admin Center using the below URL.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```

1. Click on **Environments** ***(1)*** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** ***(2)***.

   ![](images/L05/env1u.png)
   
1. From your environment page, click on **Settings**.

   ![](images/L05/diad5l9u.png)
   
1. Expand **Users + permissions** **(1)** and select **Application users** **(2)**.
    
   ![](images/L05/diad5l10u.png)

1. In the Application users page, click on **+ New app user**.

   ![](images/L05/diad5l11u.png)
   
1. In Create a new app user tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application registration which you created earlier and click on **Add** **(2)**.

   ![](images/L05/diad5l13u.png)

1. Type **org** and select your **Business unit** **(1)** and in **Security roles** click on **edit symbol (2)** and select 
   **System administrator (3)** then click on **Create (4)**.

   ![](images/L05/diad5l14u.png)

   **Note:** If the **#** symbol is still visible before GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, click on it and refresh the pane to remove it.
   
1. Again go back to **Environments** ***(1)*** in the side blade and select your **DEV_ENV_TEST** ***(2)***.

   ![](images/L05/diad5l17u.png)
   
1. From your test environment page, click on **Settings**.

   ![](images/L05/diad5l18u.png)

1. Expand **Users + permissions** ***(1)*** and select **Application users** ***(2)***.
    
   ![](images/L05/diad5l19u.png)
   
1. In the Application users page, click on **+ New app user**.

   ![](images/L05/diad5l11uu.png)
   
1. On the **Create a new app user** tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application registration which you created earlier and click on **Add** ***(2)***.

   ![](images/L05/diad5l13uu.png)

1. Type **org** and select your **Business unit** **(1)** and In **Security roles** click on **edit symbol (2)** and select **System administrator (3)** then click on **Create (4)**.

   ![](images/L05/diad5l14u.png)

   **Note:** If the **#** symbol is still visible before GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, click on it and refresh the pane to remove it.
   
1. Click on **Environments** ***(1)*** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** ***(2)***.

   ![](images/L05/envu.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/diad5l211u.png)
   
1. Again go back to **Environments** ***(1)*** in the side blade and select  **DEV_ENV_TEST(2)** environment.

   ![](images/L05/diad5l17uu.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/diad5l22u.png)
   
## Exercise 2 – Create GitHub Repo

In this exercise, you will create a GitHub repository and add repository secrets.

### Task 1: Create a repository

1. Navigate to the below URL and sign in using your GitHub credentials.

   ```
   https://github.com/
   ```
1. Click on your profile icon and select **Your repositories**.

   ![](images/L05/github1u.png)

3. Click **New repository** to create a repository.

   ![](images/L05/github2u.png)

4. Enter **PrioritZ (1)** for Repository name, select **Public (2)** , check the **Add a README file (3)**.

   ![](images/L05/github3.png)

1. Click **Create repository** to create it.

   ![](images/L05/github4.png)

5. Click **Settings** to open the settings tab.
    
     ![](images/L05/Imagessettinguu.png)

6. Go to the **Security** section, expand **Secrets and variables(1)** and select **Actions (2)**.

    > **Note:** The values you provide will not be visible after you create the item so take your time to get the values correct. 
      
     ![](images/L05/github6u.png)
   
7. Click **New repository secret** to add a secret.

     ![](images/L05/github7uu.png)

8. Enter **PowerPlatformAppID (1)** for Name and paste the odl username: **<inject key="AzureAdUserEmail"></inject> (2)** and **click Add Secret (3)** 

     ![](images/L05/github8u.png)

9. Click **New repository secret** again.

10. Enter **PowerPlatformClientSecret (1)** for Name and paste the password: **<inject key="AzureAdUserPassword"></inject> (2)** and **click Add Secret (3)** 

     ![](images/L05/github9u.png)

11. Click **New repository secret** again.

12. Enter **PowerPlatformTenantID (1)** for Name and paste the secret **Tenant ID (2)** from your notepad that you noted earlier in **`Exercise 1 -> Task 1 -> Step 5`** in the **Value** field and click **Add secret (3)**.

     ![](images/L05/github10u.png)

13. Click **New repository secret** again.

14. Enter **PowerPlatformDevUrl (1)** for Name and paste the secret **Dev environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 3 -> Step 21`** in the **Value** field and click **Add secret (3)**.

    >**Note**: Make sure you are pasting the dev environment URL named **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />** that you copied in the **`Exercise 1 -> Task 3 -> Step 17`**
   
     ![](images/L05/github11u.png) 
  
15. Click **New repository secret** one more time.

16. Enter **PowerPlatformTestUrl (1)** for Name and paste the **Test Environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 3 -> Step 18`** in the **Value** field and click **Add secret (3)**.

    >**Note**: Make sure you are pasting the test environment URL named **DEV_ENV_TEST** that you copied in the **`Exercise 1 -> Task 3 -> Step 23`**
 
    ![](images/L05/L05-testurla.png)
 
17. You should now have **5** repository secrets.
     
    ![](images/L05/Images20uu5u.png)

18. Do not navigate away from this page.

### Exercise 3 – Export and Branch

In this exercise, you will set a workflow action, and add steps that will export the solution from the dev
environment and create a new branch.

### Task 1: Export and branch

In this task, you will create the workflow definition using the YAML provided. The action YAML uses two-space indentation so follow that carefully as you build the workflow definition. If in doubt, review the
indentation shown in the images.

1. Select the **Actions** tab and click on **Set up a workflow yourself** to create a new workflow.
 
   ![](images/L05/Imagesworku.png)
   
1. Change the file name to it **export-and-branch.yml**
       
1. Remove everything from the workflow file.
  
   ![](images/L05/diad5l32.png)

1. Navigate to `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/export-and-branch.yml` URL, Copy the full content of the file and paste it into the **export-and-branch.yml** workflow.

   ![](images/L05/diad5l28u.png)

1. Click **Commit changes**.
    
   ![](images/L05/commit1.png)

1. Then click **Commit changes**.

   ![](images/L05/Images202uuu.png)

1. Click On Settings, Go to the **Actions (1)** tab on the left side, then select **General (2)**.

   ![](images/L05/actionpermissionuuu.png)

1. In the **Workflow Permission** section, make sure **read and write permission** is selected, then click **save**.  

   ![](images/L05/workflowpermissionuuu.png)

1. Select the **Actions** **(1)** tab and select the **workflow** ***(2)*** you created.

   ![](images/L05/diad5l27.png)
   
1. Click on **Run workflow.**
      
   ![](images/L05/Images2027u.png)
   
1. Click **Run workflow** again and wait for the workflow run to complete.
      
   ![](images/L05/Images2028u.png)
   
1. Select the **Code** ***(1)*** tab and click on **Branches** ***(2)***. You should see two branches
   
   ![](images/L05/diad5l29u.png)
   
1. Click to open the branch that was created by the workflow action named Prioritz-XXXXXXX.
   
   ![](images/L05/changesbranchu.png)

1. On the Prioritz-XXXXXXX. branch, you should be able to see the solution folder.
      
   ![](images/L05/diad5l30u.png)
   
1. Click on **Contribute** ***(1)*** button and select **Open pull request** ***(2)***.
        
   ![](images/L05/L05-t1-1u.png)
   
23. Add a description if you like and then click **Create pull request**.

     ![](images/L05/pr1u.png)
   
24. You should now see the pull request summary. Confirm that the branch has no conflicts with the main branch and that the changes can be merged into the main branch automatically.
   
25. Click on the chevron button next to the **Merge pull request** button and select **Squash and**
    **merge**.
      
    ![](images/L05/Images2032u.png)

26. Click **Squash and merge**.
   
27. Click **Confirm squash and merge**.
   
28. The pull request should get merged successfully.
   
     ![](images/L05/prdoneu.png)


### Exercise 4 – Release to Test

In this exercise, you will create a workflow action and add steps that will release the solution you
exported to the test environment.

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
      
16. Click **Commit changes** and then click **Commit changes**.

    ![](images/L05/commit1.png)
   
17. Select the **Actions** tab and monitor the workflow.

18. The release should be completed successfully.
    
     ![](images/L05/relecomplu.png)
     
24. Navigate back to the PowerApps portal and  Ensure you are in the test PowerApps environment.

      ![](images/L05/lastu.png)

25. Select the **solutions (1)** tab from the left side and click on **Managed (2)** you should see the solution deployed with the 
    name of  **Prioritz (3)**.

    ![](images/L05/lastuu.png)
    
## Summary
In this lab, you learned to promote a solution to a test environment, configure a service principal, and manage your solution using GitHub for version control and workflow automation.

## You have successfully completed the lab
