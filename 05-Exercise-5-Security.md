# Exercise 5: Security


## Overview

**Security** is one of the most important aspects of any architecture. It provides the **Confidentiality**, **Integrity**, **Availability** assurances against deliberate attacks and abuse of your valuable data and systems.

Losing these assurances can negatively affect your business operations and revenue, and your organization's reputation. Cloud architectures can help simplify the complex task of securing an enterprise estate through **specialization** and **shared responsibilities**:

 * **Specialization**: Specialist teams at cloud providers can develop advanced capabilities to operate and secure systems on behalf of organizations. This approach is preferable to numerous organizations, individually developing deep expertise in managing and securing common elements.
 
 * **Shared Responsibility Model**: Organizations can reduce focus on activities that aren't core business competencies by shifting these responsibilities to a cloud service. Depending on the specific technology choices, some security protections will be built into the particular service, while addressing others will remain the customer's responsibility. To ensure that proper security controls are provided, organizations must carefully evaluate the services and technology choices.

In this exercise, we will apply security principles to your architecture to protect against attacks on the data and systems.


## Task 1: Identity and access management

User directories and other authentication functions are complex to develop and critically important to security assurances. You use capabilities like Azure Active Directory (Azure AD), Azure AD B2B, Azure AD B2C, or third-party solutions to authenticate and grant permission to users, partners, customers, applications, services, and other entities. In this task, we'll be enabling multi-factor authentication. Multi-factor authentication is a process where a user is prompted during the sign-in process for an additional form of identification which increases the level of security.

1. In your JumpVM launch browser, visit `https://AKA.ms/proofup` and if asked to log in then log in using the following credentials:

   - Username: **<inject key="AzureAdUserEmail" />**
   - Password: **<inject key="AzureAdUserPassword" />**
  
2. On a prompt saying "Help us protect your account" click on **Next**.

   ![](media/mfa-00.png)

3. Download the **Microsoft Authenticator** app on your Mobile from App Store. After installing the app, select **Next**.

   ![](media/EX5-task1-step3.png)
   
4. In the Microsoft Authenticator app, set up your account by adding a work or school account. After adding an account select **Next**.

   ![](media/EX5-task1-step4.png)
   
5. To connect the Microsoft Authenticator app with your account, **Scan the QR code (1)** and select **Next (2)**.

   ![](media/EX5-task1-step5.png)
   
6. A Notification to Approve will pop-up on your mobile. Approve that and select **Next**.

   ![](media/EX5-task1-step6.png)
   
7. Once Success! Great job! You have successfully set up your security info. Choose **Done** to continue signing in.

   ![](media/EX5-task1-step7.png)

8. Click on **Security info** tab from left side menu. In Security info page click on **+ Add sign-in method**.

   ![](media/EX5-task1-step8.png)

9. For Add a method pop-up, from the drop-down select **phone**, and click on **Add**.

   ![](media/EX5-task1-step9.png)

10. Enter the details required to set up MFA.

    - **Country Code**: Select the country code of your mobile number **(1)**
    - **Mobile Number**: Enter the number which you want to use for the MFA **(2)**
    - **Method**: Select the preferred method of authentication **(3)**
    - Click on **Next (4)**.

    ![](media/EX5-task1-step101.png)

11. Authenticate the login according to the authentication method you have chosen in the previous step to complete the verification.

    ![](media/EX5-task1-step11.png)
  
12. Now after a few seconds the status will change to **registered successfully**, click on **Done** to finish the MFA registration.

    ![](media/EX5-task1-step12.png)

13. You can verify the regirested **Phone** and **Microsoft Authenticator** on the Security info page

    ![](media/EX5-task1-step13.png)

## Task 2: Infra protection

In this task, you will learn how to control access to the Azure resources that you deploy. You will also create a diagnostic setting to send the Activity log to Azure **Storage account** for cheaper, long-term archiving to get an insight into subscription-level events and to audit all the changes to the infrastructure.

**Azure role-based access control** (Azure RBAC) is a system that provides fine-grained access management of Azure resources. Using Azure RBAC, you can segregate duties within your team and grant only the amount of access to users that they need to perform their jobs.

**Role Assignment to Resources**

1. In the Azure Portal, navigate to the **Resource Group** named **wafdev**.

   ![](./media/wag-rg.png)
   
2. Select **Access Control (IAM)** in the left-hand menu, select **+ Add (1)** above `Role assignments` and select **Add role assignment (2)**.
   
   ![](./media/Iam-role.png)
   
3. In the add role assignment form, search for **Reader** in Role and select it. Click on **Next**.

   ![](./media/reader.png)
   
4. In the Members pane, for **Assign access to** select **User, group, or service principle** and for **Members** click on **+Select Members**.
   
   ![](./media/select-member.png)
   
5. A Select members pane appears, search and add the member and **select**.
  
   ![](./media/select-member1.png)
   
6. After adding the Member, then select **Review + Assign**.

   ![](./media/review-assign.png)
 
**Creating a Diagnostic Setting**
 
7. Navigate to the **wafdev** resource group pane and click on **Activity log**. 

   ![](./media/activity1.png)
   
8. Then from the filter menu, click on **Timespan (1)** filter and then select **last month (2)**. Click on **Apply (3)** to apply the filter.
   
   ![](./media/activity2.png)
   
9. Once the filter is applied and you are able to see all the operations from last month, then select **Export Activity Logs**.

   ![](./media/export-activity.png)

10. On the **Diagnostic Settings** page, make sure your subscription is selected and then click on **+ Add diagnostic setting**.

    ![](./media/add-diag.png)
    
11. Fill the **Diagnostic Settings** page with the following details:

    * **Diagnostic setting name**: `dev-log` **(1)**
    *  **Logs**: Make sure you have selected all the categories **(2)**
    *  **Destination details**: Check **Archive to a storage account (3)** 
    *  **Subscription:** Leave to default **(4)**
    *  **Storage account:** Select the storage account that has **storage** as suffix, such as **wafdevxxxxstorage**
    *  Click on **Save (5)**.

    ![](./media/activity-save.png)
   
12. Navigate back to the **wafdev** resource group and select the storage account **wafdevxxxx**.

    ![](./media/storage-select.png)
    
13. From the left navigation pane, under the **Data storage** section, select **Containers**.

     ![](./media/container.png)
    
14. You will be able to see a container with the name **insights-activity-logs**. Click on it.

    ![](./media/container2.png)
    
    > **Note:** The container might take up to 2 minutes to get created. Click on the **Refresh** button once in a few seconds until you are able to see the container.
    
15. Go through the folder names **resourceId=** and observe that each event is stored in the **PT1H.json** file with the following format that uses a common top-level schema as shown in the below screenshot.

   `{ "time": "2020-06-12T13:07:46.766Z", "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MV-VM-01", "correlationId": "0f0cb6b4-804b-4129-b893-70aeeb63997e", "operationName": "Microsoft.Resourcehealth/healthevent/Updated/action", "level": "Information", "resultType": "Updated", "category": "ResourceHealth", "properties": {"eventCategory":"ResourceHealth","eventProperties":{"title":"This virtual machine is starting as requested by an authorized user or process. It will be online shortly.","details":"VirtualMachineStartInitiatedByControlPlane","currentHealthStatus":"Unknown","previousHealthStatus":"Unknown","type":"Downtime","cause":"UserInitiated"}}}`
   
   
   ![](./media/container4.png)


## Task 3: App Security
   
   Inconsistent configurations for applications can create security Risks. Azure App Configuration provides a service to centrally manage application settings and feature flags, which helps mitigate this risk. Defender for Cloud, continuously monitors the configuration of your virtual machines scale sets to identify potential security vulnerabilities and recommends actions to mitigate them. In this task, we will be adding log analytics to the virtual machine scale set using **Microsoft Defender for cloud**.

1. In the Azure portal, click on **Show portal menu (1)** and select **Resource groups (2)**.

   ![](./media/costopt-01.png)
   
2. Open **wafprod** resource group and open the Virtual machine scale set **srv**.

   ![](./media/ex5-task3-01.png)
   
3. From the left navigation pane, under the **Settings** section, select **Microsoft Defender for Cloud**. Click on the recommendation **Log Analytics agent should be installed on the virtual machine scale sets**.

   ![](./media/ex5-task3-02.png)
   
4. On the top section, notice the following:

   - Title of the recommendation: **Log Analytics agent should be installed on the virtual machine scale sets**
   - Top menu controls: **View policy definition** and **Open query**
   - Severity indicator: **High**
   - Freshness interval: **24 Hours** 
   
    ![](./media/ex5-task3-03.png)
   
5. Under **Remediation steps**, click on **Quick fix logic** and go through the **Automatic remediation script content**.  After that click on **Fix**.

   ![](./media/ex5-task3-04.png)
   
6. This will open a new window **Fixing resources**. Select an existing log analytics workspace for **Workspace ID** and click on **Fix 1 resources**.

   ![](./media/ex5-task3-05.png)
   
7. Wait for the notification: ✅ **Remediation successful**. 
 

## Task 4: Data encryption and sovereignty

In this task, you will create your own encryption key to protect the data in your storage account using a **customer-managed key**. When you specify a customer-managed key, that key is used to protect and control access to the key that encrypts your data. Customer-managed keys offer greater flexibility to manage access controls. 

**Data encryption** is the method of translating data into another form or code so that access to the data is limited to only those with the correct decryption key (or password). **Data sovereignty** is a corporate or government standard that makes your data reside within a certain country, usually within the country where your corporation resides. In this task, we will be using the Azure policy to implement data sovereignty.


**Data encryption**

1. Navigate back to the **wafdev** resource group and select the storage account **wafdevxxxx**.

   ![](./media/storage-select.png)
   
2. From the left navigation pane, under the **Security + Networking** section, Select **Encryption**.

   ![](./media/ex5-task4-01.png)
   
3. On the **Encryption** pane, select **`customer-managed keys`** for **Encryption Type (1)** and **`Select from key vault`** option for **Encryption key (2)**. Click on the **select a key vault and key (3)** option next to the key vault and key.

   ![](./media/ex5-task4-02.png)
   
4. On the **Select a key** page, select **key vault** for keystore type and click on the **create new key vault** option next to the key vault.

   ![](./media/ex5-task4-03.png)
   
5. You will be redirected to **Create a key vault** page. Provide the following information for the key vault:

   - **Subscription**: Select your subscription **(1)**
   - **Resource group**: wafdev **(2)**
   - **Key vault name**: Enter waf-keyvault<inject key="DeploymentID" enableCopy="false"/> **(3)**
   - **Region**: East US **(4)** 
   - click on **Next (5)**
  
    ![](./media/keyvault.png)
  
6. On the **Access policy** pane, check all the options under **Enable access to:** and click on **Review + create**.

   ![](/media/EX5-task4-step6.png)
  
7. Once the deployment is successfully finished, you will be redirected to **Select a key** page, and click on the **create a new key** option next to the key.

   ![](./media/ex5-task4-05.png)
  
8. On the **Create a key** page, fill the following details:

   - **Options**: Generate **(1)**
   - **Name**: waf-key **(2)**
   - **Key type**: RSA **(3)**
   - **RSA key size**: 2048 **(4)**
   - Leave all other values to default and click on **Create (5)**.

    ![](./media/ex5-task4-06.png)
  
9. Once the key is generated successfully, you will be redirected to **Select a key** page, and click on **Select**.

   ![](./media/ex5-task4-07.png)
   
10. Click on **Save** and wait until the changes to **Encryption** gets updated. Visit the encryption pane and observe the updated keys.

    ![](./media/ex5-task4-08.png)
   
**Data sovereignty**

11. Type **policy** in the search box located on the top of the Azure Portal page and click on it.

    ![](./media/ex5-task4-10.png)
   
12. From the left navigation pane, under the **Authoring** section, select **Definitions**.

    ![](./media/ex5-task4-11.png)
   
13. On the **Policy Definition (1)** page, type **location (2)** in the **search** bar and click on **Allowed locations (3)**.

    ![](./media/ex5-task4-12.png)
   
14. On the **Allowed locations** page, go through the policy details and click on **assign**.

    ![](./media/ex5-task4-13.png)
    
15. Click on **...** on the **Basics** tab of **assign policy** page and select your subscription as the scope.

    ![](/media/EX5-task4-step15.png)
    
    ![](./media/ex5-task4-15.png)
    
16. Click on **Next**, select your choice of location in the **Allowed locations** bar and click on **Review + create**.  

    ![](/media/EX5-task4-step16.png)
 
17. At last, click on **create** to assign the policy to the subscription.
    
    ![](/media/EX5-task4-step17q.png)
   
## Task 5: Security operations

In this task, we are going to perform integrated security monitoring for your workload using **Microsoft Defender for Cloud**. We will enable **Just-in-time** access to the Virtual machine, which locks down and limits the ports of the Azure virtual machine in order to overcome malicious attacks on the virtual machine, therefore only providing access to a port for a limited amount of time.


**Exploring Secure Score**

1. Type **Microsoft Defender** in the search box located on the top of the Azure Portal page and click on **Microsoft Defender for Cloud** to open it.

   ![](./media/ex5-task5-01.png)
   
2. The Microsoft Defender for Cloud Overview page provides a unified view for security professionals. This page contains detailed insights on the security posture on its dedicated dashboard and includes multiple independent cloud security pillars such as- **Secure Score**, **Regulatory Compliance** and **Microsoft Defender for Cloud**.

   ![](./media/ex5-task5-03.png)
   
3. On the **Overview** page, and look at the **Security posture** tile, you can see your current score along with the number of **Completed controls and Completed recommendations**. Clicking on this tile will redirect you to drill down view across subscriptions.

    ![](./media/ex5-task5-04.png)
   
  > ⭐ Good to know: <br>
  > The higher the score, the lower the identified risk level.

4. The bottom section lists the subscriptions and their current secure scores. To view the recommendations behind the score, click on **View recommendations**.

    ![](./media/ex5-task5-05.png)
   
**Exploring Security Controls and Recommendations**
   
5. On the Recommendations page, pay attention to the first part of the page. It includes the current Secure Score, progress on the Recommendations status(both completed security controls and recommendations), and Resource health (by severity).

    ![](./media/ex5-task5-06.png)
   
6. Under Recommendations, Click on **Secure management ports** and select **Management ports of virtual machines should be protected with just-in-time network access control**.

    ![](./media/ex5-task5-02.png)
   
7. On the top section, notice the following:

    - Title of the recommendation: **Management ports of virtual machines should be protected with just-in-time network access control**
    - Top menu controls: **View policy definition** and **Open query**
    - Severity indicator: **High**
    - Freshness interval: **24 Hours** 
    - Tactics and techniques: **Initial Access**

     ![](./media/ex5-task5-07.png)
   
8. The next important part is the Remediation Steps which contains the remediation logic where you can remediate the selected resource/s. To simplify remediation and improve your environment's security and increase your secure score, many recommendations include a Fix option. Fix helps you quickly remediate a recommendation on multiple resources.

9. Under **Affected resources**, select the virtual machine with the name **wafdevXXX** on the Unhealthy resources and click on **Fix**. This will automatically apply the remediation to the selected resource.

   ![](./media/ex5-task5-08.png)
  
10. This will open a new window **Just-in-time VM access configuration**, review the implications for this remediation, and click on **Save**.

    ![](./media/ex5-task5-09.png)
   
11. Wait for a notification: ✅ **Just-in-time VM access enabled** - which successfully blocks all inbound traffic at the network level to the virtual machine **wafdevxxx**. 
    
    > **Note**: It can take up to 5 minutes for the VM access to get enabled. 

12. Go back to **Recommendations (1)**, click on **Apply system updates (2)**, and select **Log Analytics agent should be installed on virtual machines (3)**.

    ![](./media/ex5-task5-10.png)
   
13. On the top section, notice the following:

    - Title of the recommendation: **Log Analytics agent should be installed on virtual machines**
    - Top menu controls: **View policy definition** and **Open query**
    - Severity indicator: **High**
    - Freshness interval: **24 Hours** 

     ![](./media/ex5-task5-11.png)
   
14. Under **Affected resources**, select the virtual machine with the name **wafproXXX** on the Unhealthy resources and click on **Fix**. 

    ![](./media/ex5-task5-12.png)
    
15. This will open a new window **Fixing resources**, select an existing log analytics workspace for **Workspace ID**, and click on **Fix 2 resources**.

    ![](./media/ex5-task5-13.png)
    
16. Wait for a notification: ✅ **Remediation successful**. 

     > **Note**: It can take up to 5 minutes for the remediation to get successful. 

17. Go to the **Recommendations** page and explore through other recommendations.




