# Module 02a: Explore Computer Vision

## Lab overview

The ***Computer Vision*** Azure AI service uses pre-trained machine learning models to analyze images and extract information about them.

For example, suppose the fictitious retailer *Northwind Traders* has decided to implement a "smart store", in which AI services monitor the store to identify customers requiring assistance and direct employees to help them. By using the Computer Vision service, images taken by cameras throughout the store can be analyzed to provide meaningful descriptions of what they depict.

In this lab, you'll use a simple command-line application to see the Computer Vision service in action. The same principles and functionality apply to real-world solutions, such as websites or phone apps. This includes:
- Creating Azure AI Services and Azure Storage Accounts
- Configuring and running a client application

## Lab objectives
In this lab, you will perform:
- Task 1: Create a *Azure AI Services* resource
- Task 2: Run Cloud Shell
- Task 3: Configure and run a client application
  
## Estimated timing: 60 minutes

## Architecture Diagram

![](media/Aimodule2b.png)
 
### Exercise 1: Create a *Azure AI Services* resource

## Task 1: Create a *Azure AI Services* resource

You can use the Computer Vision service by creating either a **Computer Vision** resource or a **Azure AI Services** resource.

1. In the Azure Portal, select the **&#65291;Create a resource** button, search for *Azure AI Services*, and create a **Azure AI Services** resource with the following settings:
   
    - **Subscription**: *Your Azure subscription*
    - **Resource group**: Select **AI-900-Module-03-<inject key="DeploymentID" enableCopy="false"/>**
    - **Region**:  **<inject key="location" enableCopy="false"/>**
    - **Name**: *Enter **ai900cognitive-<inject key="DeploymentID" enableCopy="false"/>***
    - **Pricing tier**: Standard S0
    - **By checking this box, I acknowledge that I have read and understood all the terms below**: Selected

1. Click **Review + create**.
   
1. After successfully completing the validation process, click on the **Create** button located in the lower-left corner of the page.
   
1. Wait for deployment to complete (it can take a few minutes), and then click on the **Go to resource** button; this will take you to your Azure AI Services.

1. View the **Keys and Endpoint** page from the left pane under Resource Management for your Azure AI Services resource. You will need the endpoint and keys to connect to client applications.

   > **Note**: Copy and save the **KEY 1** and **Endpoint** values to Notepad for future reference to connect from client applications.

   ![](media/key.png)

### Task 2: Run Cloud Shell

To test the capabilities of the Computer Vision service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/analyze-images-computer-vision-service/powershell-portal-guide-1(1).png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **Powershell**.  

1. Within the Getting Started pane, select **Mount storage account**, select your **Storage account subscription** from the dropdown and click **Apply**.

   ![](media/cloudshell-getting-started.png)

1. Within the **Mount storage account** pane, select **I want to create a storage account** and click **Next**.

   ![](media/cloudshell-mount-strg-account.png)


1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected, Please make sure you have selected your resource group **AI-900-Module-03-<inject key="DeploymentID" enableCopy="false"/>** and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account name** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** For the **File share name**, then click on **Create**.

    ![Create storage by clicking confirm.](media/cloudshell-advanced-settings.png "Create storage advanced settings")

1. Wait for PowerShell terminal to start.

### Task 3: Configure and run a client application

Now that you have a Cloud Shell environment, you can run a simple application that uses the Computer Vision service to analyze an image.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **Tip:**
    > If you already used this command in another lab to clone the *ai-900* repository, you can skip this step.

1. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

    ```PowerShell
    code .
    ```

   > **Note**: If you get a popup on **Switch to Classic Cloud Shell** while running **code .** command click on **Confirm** and make sure you are in the correct project path.

   ![](media/classic-cloudshell-prompt.png)  

    Notice how this opens up an editor like the one in the image below:

    ![The code editor.](media/analyze-images-computer-vision-service/powershell-portal-guide-4(2).png)

1. In the **Files** pane on the left, expand **ai-900** and select **analyze-image.ps1**. This file contains some code that uses the Computer Vision service to analyze an image, as shown here:

    ![The editor containing code to analyze an image](media/analyze-images-computer-vision-service/analyze-image-code1.png)

1. Don't worry too much about the code; the important thing is that it needs the endpoint URL and either of the keys for your Azure AI Services resource. Copy these from the **Keys and Endpoints** page for your resource from the Azure portal and paste them into the code editor, replacing the **YOUR_KEY** with *KEY 1* and **YOUR_ENDPOINT** with *Endpoint* placeholder values respectively.

    > **Tip**: You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
   After pasting the key and endpoint values, the first two lines of code should look similar to this:

    
     > $key="1a2b3c4d5e6f7g8h9i0j...."    
     > $endpoint="https..."

1. After making the changes to the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor.

1. The sample client application will use your Computer Vision service to analyze the following image, taken by a camera in the Northwind Traders store:

    ![An image of a parent using a cellphone camera to take a picture of a child in in a store](media/analyze-images-computer-vision-service/store-camera-1.jpg)

    In the PowerShell pane, enter the following commands to run the code:

    ```PowerShell
    cd ai-900
    ```
    
    ```PowerShell
    ./analyze-image.ps1 store-camera-1.jpg
    ```

1. Review the results of the image analysis, which include:
    - A suggested caption that describes the image.
    - A list of objects identified in the image.
    - A list of "tags" that are relevant to the image.

1. Now let's try another image:

    ![An image of person with a shopping basket in a supermarket](media/analyze-images-computer-vision-service/store-camera-2.jpg)

    To analyze the second image, enter the following command:

    ```PowerShell
    ./analyze-image.ps1 store-camera-2.jpg
    ```

1. Review the results of the image analysis for the second image.

1. Let's try one more:

    ![An image of person with a shopping cart](media/analyze-images-computer-vision-service/store-camera-3.jpg)

    To analyze the third image, enter the following command:

    ```PowerShell
    ./analyze-image.ps1 store-camera-3.jpg
    ```

1. Review the results of the image analysis for the third image.

## validation

<validation step="b213015b-b660-4aa0-810b-ab70b5a7f7f9" />

>**Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:

 > - Click the Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation tab.
 > - Hit the Validate button for the corresponding task.
 > - If you receive a success message, you can proceed to the next task. If not, carefully read the error message and retry the step, following the instructions in the lab guide.
 > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
   
### Learn more

This simple app shows only some of the capabilities of the Computer Vision service. To learn more about what you can do with this service, see the [Computer Vision page](https://azure.microsoft.com/services/cognitive-services/computer-vision/).

### Review
In this lab, you have completed:
- Create an Azure AI Services resource
- Run Cloud Shell
- Configure and run a client application
  
## You have successfully completed this lab.
  
