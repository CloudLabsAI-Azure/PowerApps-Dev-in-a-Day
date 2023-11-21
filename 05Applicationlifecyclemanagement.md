# Lab 05 - Application lifecycle management

Duration: 95 mins

## Table of Contents


Lab Scenario 

1. Exercise 1 – Configure a Service Principal 

   - Task 1: Create app registration 

   - Task 2: Create an app user in Dataverse 

2. Exercise 2 – Create GitHub Repo 

   - Task 1: Create repository 

3. Exercise 3 – Export and Branch 

   - Task 1: Export and branch 

4. Exercise 4 – Release to Test 

   - Task 1: Create workflow

### Lab Scenario

Working as part of the PrioritZ fusion team you will be configuring GitHub Actions using the Power
Platform Build Tools to automate the team’s deployments.

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
   
1. Select **App registrations** ***(1)*** from the side blade and click on **+ New registration** ***(2)***. This application registration will be used for the connector to access the protected API.

   ![](images/L05/diad5l2.png)

1. Please provide the following details and click on **Register** ***(3)***.
   
   - Name: **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)***
   - Supported account types: **Accounts in this organizational directory only (OTU WA AIW [SUFFIX] only - Single tenant)** ***(2)***

   ![](images/L05/diad5l3.png).
   
1. Copy the **Application (client) ID**, **Directory(Tenant) ID**, and save it in a notepad as you need it for later use.
     
   ![](images/L05/diad5l4.png)

1. Select **Certificates & secrets** from the side blade and click on **+ New client secret**.

   ![](images/L05/diad5l5.png)

1. Enter **GitHub client secret<inject key="DeploymentID" enableCopy="false" />** ***(1)*** as description, set expiry to **3 months** ***(2)***, and click on **Add** ***(3)***.
   
   ![](images/L05/diad5l6.png)
   
1. Copy the **value** and save it in a notepad as you need it for later use.

   ![](images/L05/diad5l7.png)

    >**Note**: Make sure to copy and paste the correct **Application (client) ID**, **Directory(Tenant) ID** and **Secret** value. Copying the incorrect value will result in issues in the next steps/tasks.

 
### Task 2: Create an app user in Dataverse

In this task, you will be registering the app you created in Microsoft Entra ID into the dev and test
Dataverse environments. You will also be assigned a security role that will allow the service principal to
deploy solutions.


1. Open a new browser window or tab and navigate to the Power Platform Admin Center using the below URL.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```

1. Click on **Environments** ***(1)*** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** ***(2)***.

   ![](images/L05/env.png)
   
1. From your environment page, click on **Settings**.

   ![](images/L05/diad5l9.png)
   
1. Expand **Users + permissions** ***(1)*** and select **Application users** ***(2)***.
    
   ![](images/L05/diad5l10.png)

1. In the Application users page, click on **+ New app user**.

   ![](images/L05/diad5l11.png)
   
1. In Create a new app user tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application registration which you created earlier and click on **Add** **(2)**.

   ![](images/L05/diad5l13.png)

1. Select your **Business unit** ***(1)*** from the drop down and click on **Create** ***(2)***. There should only be one Business unit unless you create more.

   ![](images/L05/diad5l14.png)

1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application user you just created and click on **Edit security roles** ***(2)***.
    
   ![](images/L05/diad5l15.png)

1. On the **Edit security roles** blade, scroll down and select **System administrator** ***(1)*** and click on **Save** ***(2)***.

   ![](images/L05/diad5l16.png)
   
1. Again go back to **Environments** ***(1)*** in the side blade and select your **test environment** ***(2)***.

   ![](images/L05/diad5l17.png)
   
1. From your test environment page, click on **Settings**.

   ![](images/L05/diad5l18.png)

1. Expand **Users + permissions** ***(1)*** and select **Application users** ***(2)***.
    
   ![](images/L05/diad5l19.png)
   
1. In the Application users page, click on **+ New app user**.

   ![](images/L05/diad5l11.png)
   
1. On the **Create a new app user** tab, click on **+ Add an app**.
      
   ![](images/L05/diad5l12.png)
   
1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application registration which you created earlier and click on **Add** ***(2)***.

   ![](images/L05/diad5l13.png)

1. Select your **Business unit** ***(1)*** from the drop down and click on **Create** ***(2)***. There should only be one Business unit unless you create more.

   ![](images/L05/diad5l14.png)

1. Select the **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** application user you just created and click on **Edit security roles** ***(2)***.
    
   ![](images/L05/diad5l20.png)
   
1. Select **System administrator** ***(1)*** and click on **Save** ***(2)***.

   ![](images/L05/diad5l16.png)
   
1. Click on **Environments** ***(1)*** from the side blade and select your **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />'s environment** ***(2)***.

   ![](images/L05/env.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/diad5l211.png)
   
1. Again go back to **Environments** ***(1)*** in the side blade and select your **OTU XXXXXX (default) (2)** environment.

   ![](images/L05/diad5l17.png)
   
1. Copy the **Environment URL** and save it in a notepad, you will be using this URL in future steps.
    
   ![](images/L05/diad5l22.png)
   
## Exercise 2 – Create GitHub Repo

In this exercise, you will create a GitHub repository and add repository secrets.

### Task 1: Create a repository

1. Navigate to the below URL and sign in using your GitHub credentials.

   ```
   https://github.com/
   ```
1. Click on your profile icon and select **Your repositories**.

   ![](images/L05/github1.png)

3. Click **New repository** to create a repository.

   ![](images/L05/github2.png)

4. Enter **PrioritZ (1)** for Repository name, select **Public (2)** , check the **Add a README file (3)**.

   ![](images/L05/github3.png)

1. Click **Create repository** to create it.

   ![](images/L05/github4.png)

5. Click **Settings** to open the settings tab.
    
     ![](images/L05/Images%20(13).png)

6. Go to the **Security** section, expand **Secrets (1)** and select **Actions (2)**.

    > **Note:** The values you provide will not be visible after you create the item so take your time to get the values correct. 
      
     ![](images/L05/github6.png)
   
7. Click **New repository secret** to add a secret.

     ![](images/L05/github7.png)

8. Enter **PowerPlatformAppID (1)** for Name and paste the **Application (client) ID (2)** of **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** that you noted earlier in **`Exercise 1 -> Task 1 -> Step 5`** from your notepad in the **Value** field and click **Add secret (3)**.

     ![](images/L05/github8.png)

9. Click **New repository secret** again.
10. Enter **PowerPlatformClientSecret (1)** for Name and paste the **secret Value (2)** from your notepad that you noted earlier in **`Exercise 1 -> Task 1 -> Step 8`** in the **Value** field and click **Add secret (3)**.

     ![](images/L05/github9.png)

11. Click **New repository secret** again.
12. Enter **PowerPlatformTenantID (1)** for Name and paste the secret **Tenant ID (2)** from your notepad that you noted earlier in **`Exercise 1 -> Task 1 -> Step 5`** in the **Value** field and click **Add secret (3)**.

     ![](images/L05/github10.png)

13. Click **New repository secret** again.
14. Enter **PowerPlatformDevUrl (1)** for Name and paste the secret **Dev environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 2 -> Step 21`** in the **Value** field and click **Add secret (3)**.

    >**Note**: Make sure you are pasting the dev environment URL named **Testodl_user_<inject key="DeploymentID" enableCopy="false" />** that you copied in the **`Exercise 1 -> Task 2 -> Step 21`**
   
     ![](images/L05/github11.png) 
  
15. Click **New repository secret** one more time.
16. Enter **PowerPlatformTestUrl (1)** for Name and paste the **Test Environment URL (2)** from your notepad that you copied in the **`Exercise 1 -> Task 2 -> Step 23`** in the **Value** field and click **Add secret (3)**.

     >**Note**: Make sure you are pasting the test environment URL named **OTU XXXXXX (default)** that you copied in the **`Exercise 1 -> Task 2 -> Step 23`**
 
     ![](images/L05/L05-testurl.png)
   
 
17. You should now have **5** repository secrets.
     
       ![](images/L05/Images%20(15).png)

18. Do not navigate away from this page.

### Exercise 3 – Export and Branch

In this exercise, you will set a workflow action, and add steps that will export the solution from the dev
environment and create a new branch.

### Task 1: Export and branch

In this task, you will create the workflow definition using the YAML provided. The action YAML uses two-space indentation so follow that carefully as you build the workflow definition. If in doubt, review the
indentation shown in the images.

1. Select the **Actions** tab and click on **Set up a workflow yourself** to create a new workflow.
 
   ![](images/L05/Images%20(16).png)
   
1. Change the file name to it **export-and-branch.yml**
       
1. Remove everything from the workflow file.
  
   ![](images/L05/diad5l32.png)

1. Navigate to `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/export-and-branch.yml` URL, Copy the full content of the file and paste it into the **export-and-branch.yml** workflow.

   ![](images/L05/diad5l28.png)

1. Click **Start commit** and then click **Commit new file**.
    
   ![](images/L05/Images%20(26).png)

1. Select the **Actions** **(1)** tab and select the **workflow** ***(2)*** you created.

   ![](images/L05/diad5l27.png)
   
1. Click on **Run workflow.**
      
   ![](images/L05/Images%20(27).png)
   
1. Click **Run workflow** again and wait for the workflow run to complete.
      
   ![](images/L05/Images%20(28).png)
   
1. Select the **Code** ***(1)*** tab and click on **Branches** ***(2)***. You should see two branches
   
   ![](images/L05/diad5l29.png)
   
1. Click to open the branch that was created by the workflow action named Prioritz-XXXXXXX.
   
   ![](images/L05/changesbranch.png)

1. On the Prioritz-XXXXXXX. branch, you should be able to see the solution folder.
      
   ![](images/L05/diad5l30.png)
   
1. Click on **Contribute** ***(1)*** button and select **Open pull request** ***(2)***.
        
   ![](images/L05/L05-t1-1.png)
   
23. Add a description if you like and then click **Create pull request**.

     ![](images/L05/pr1.png)
   
24. You should now see the pull request summary. Confirm that the branch has no conflicts with the
    main branch and that the changes can be merged into the main branch automatically.
   
25. Click on the chevron button next to the **Merge pull request** button and select **Squash and**
    **merge**.
      
    ![](images/L05/Images%20(32).png)

26. Click **Squash and merge**.
   
27. Click **Confirm squash and merge**.
   
28. The pull request should get merged successfully.
   
     ![](images/L05/prdone.png)


### Exercise 4 – Release to Test

In this exercise, you will create a workflow action and add steps that will release the solution you
exported to the test environment.

### Task 1: Create workflow

1. Now navigate to the **Actions** tab.

    ![](images/L05/act.png)
   
1. Click **New workflow**.

    ![](images/L05/nwwf.png)
   
1. Now on **Choose a workflow** page, click **set up a workflow yourself**.
     
    ![](images/L05/Images%20(33).png)
   
1. Change the file name to **release-to-test.yml**
   
    ![](images/L05/fname.png)
     
1. Remove everything from the workflow file.
  
   ![](images/L05/diad5l32.png)

1. Navigate to `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/release-to-test.yml` URL in the browser and copy the full content of the file and paste it into the **release-to-test.yml** workflow file.
 
   ![](images/L05/cntn.png)
      
16. Click **Start commit** and then click **Commit new file**.
     
     ![](images/L05/Images%20(46).png)

17. Select the **Code** tab.
   
18. Go to the **Releases** section and click **Create new release**.
     
     ![](images/L05/Images%20(47).png)    
   
19. Click on the **Choose a tag** button, enter **v1.0.0** , and select **+ Create new tag on publish**.
      
     ![](images/L05/Images%20(48).png)  

20. Click **Publish release**.
   
21. Select the **Actions** tab and monitor the workflow.
      
     ![](images/L05/Images%20(49).png)

22. The release should be completed successfully.
    
     ![](images/L05/relecompl.png)
     
23. Navigate back to the PowerApps portal and select the solutions tab from the left side and you should see the solution deployed with the name of Prioritz. Make sure that you are in the test PowerApps environment.

      ![](images/L05/last.png)

