# Analyze Images with Computer Vision

In this lab you will explore the Computer Vision cognitive service to analyze images.

## Exercise 1 : Analyze images with the Computer Vision service

The  _Computer Vision_  cognitive service uses pre-trained machine learning models to analyze images and extract information about them.

For example, suppose the fictitious retailer  _Northwind Traders_  has decided to implement a "smart store", in which AI services monitor the store to identify customers requiring assistance, and direct employees to help them. By using the Computer Vision service, images taken by cameras throughout the store can be analyzed to provide meaningful descriptions of what they depict.

In this lab, you'll use a simple command-line application to see the Computer Vision service in action. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

### Task 1 : Create a  _Cognitive Services_  resource

You can use the Computer Vision service by creating either a  **Computer Vision**  resource or a  **Cognitive Services**  resource.

If you haven't already done so, create a  **Cognitive Services**  resource in your Azure subscription.

1.  In another browser tab, open the Azure portal at  [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
    
2.  Click the  **＋Create a resource**  button, search for  _Cognitive Services_ and select it 

    ![](../media/AI-900-m1-1.png)
    
3.  Click on **Create** drop down and select **Cognitive Services**.    

    ![](../media/AI-900-m3-01.png)
 
5.  Create a **Cognitive Services** resource with the following settings:
    
    -   **Subscription**:  _Your Azure subscription_.
    -   **Resource group**:  Select  existing resource group .
    -   **Region**:  _Choose any available region_:
    -   **Name**:  Cognitive-[DeploymentID]
    -   **Pricing tier**: Standard S0
    -   **By checking this box I acknowledge that I have read and understood all the terms below**: Selected.
   
    **Note**:  Replace [DeploymentID] with your DeploymentId, which can be obtained from the Lab Environment Details page.
    
3.  Click on **Review + create**, and wait for deployment to complete. Then go to the deployed resource.
    
4.  View the  **Keys and Endpoint**  page for your Cognitive Services resource. You will need the endpoint and keys to connect from client applications.
    
### Task 2 : Run Cloud Shell

To test the capabilities of the Computer Vision service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1.  In the Azure portal, select the  **[>_]**  (_Cloud Shell_) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/powershell-portal-guide-1.png)
    
2.  The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (_Bash_  or  _PowerShell_). Select  **PowerShell**. If you do not see this option, skip the step.
    
3. When prompted, select **Show advanced settings** and then select **Use existing** and choose existing resource group. Then select **Create new** against Storage account as well as File Share and provide a unique value in both of the fields and then click on **Create storage**, and wait for the Azure Cloud Shell to initialize.
  
    ![Create storage by clicking confirm.](../media/Ai-900p1.png)
    
4.  Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to  _PowerShell_. If it is  _Bash_, switch to  _PowerShell_  by using the drop-down menu.
    
    ![How to find the left hand drop down menu to switch to PowerShell](../media/powershell-portal-guide-3.png)
    
5.  Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![Wait for PowerShell to start.](../media/powershell-prompt.png)
    
### Task 3 : Configure and run a client application

Now that you have a Cloud Shell environment, you can run a simple application that uses the Computer Vision service to analyze an image.

1.  In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    
    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
2.  The files are downloaded to a folder named  **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:
    
    ```
    code .
    
    ```
    
    Notice how this opens up an editor like the one in the image below:
    
    ![The code editor.](../media/powershell-portal-guide-4.png)
    
3.  In the  **Files**  pane on the left, expand  **ai-900**  and select  **analyze-image.ps1**. This file contains some code that uses the Computer Vision service to analyze an image, as shown here:
    
    ![The editor containing code to analyze an image](../media/analyze-image-code.png)
    
4.  Don't worry too much about the code, the important thing is that it needs the endpoint URL and either of the keys for your Cognitive Services resource. Copy these from the  **Keys and Endpoints**  page for your resource from the Azure portal and paste them into the code editor, replacing the  **YOUR_KEY**  and  **YOUR_ENDPOINT**  placeholder values respectively.
    
    Tip
    
    You may need to use the separator bar to adjust the screen area as you work with the  **Keys and Endpoint**  and  **Editor**  panes.
    
    After pasting the key and endpoint values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    
    ```
   
    >**Note** : `In the next part of the exercise we will be analysing the features of the images. We will however not be seeing the images inside the users's lab environment. But you can view the images in the integrated lab guide.`
   
5.  Click at the top right of the editor pane, to open the menu and select  **Save**  to save your changes. Then open the menu again and select  **Close Editor**.

    ![](../media/AI-900-m3-03.png) .
    
6.  The sample client application will use your Computer Vision service to analyze the following image, taken by a camera in the Northwind Traders store:
    
    ![An image of a parent using a cellphone camera to take a picture of a child in in a store](../media/store-camera-1.jpg)
    
    In the PowerShell pane, enter the following commands to run the code:
    
    ```
    cd ai-900
    ./analyze-image.ps1 store-camera-1.jpg
    
    ```
    
7.  Review the results of the image analysis, which include:
    
    -   A suggested caption that describes the image.
    -   A list of objects identified in the image.
    -   A list of "tags" that are relevant to the image.
    
8.  Now let's try another image:
    
    ![An image of person with a shopping basket in a supermarket](../media/store-camera-2.jpg)
    
    To analyze the second image, enter the following command:
    
    ```
    ./analyze-image.ps1 store-camera-2.jpg
    
    ```
    
9.  Review the results of the image analysis for the second image.
    
10. Let's try one more:
    
    ![An image of person with a shopping cart](../media/store-camera-3.jpg)
    
    To analyze the third image, enter the following command:
    
    ```
    ./analyze-image.ps1 store-camera-3.jpg
    
    ```
    
11. Review the results of the image analysis for the third image.
    

## Learn more

This simple app shows only some of the capabilities of the Computer Vision service. To learn more about what you can do with this service, see the  [Computer Vision page](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
