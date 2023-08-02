# Module 4a: Explore Speech

To build software that can interpret audible speech and respond appropriately, you can use the **Speech** cognitive service, which provides a simple way to transcribe spoken language into text and vice-versa.

For example, suppose you want to create a smart device that can respond verbally to spoken questions, such as "What time is it?" The response should be the local time.

To test the capabilities of the Speech service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## Exercise 1: Create a Cognitive Services resource

## Task 1: Create a Cognitive Services resource

1. Click the **&#65291;Create a resource** button, search for *Cognitive Services*, and create a **Cognitive Services** resource with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: **AI-900-Module-04a-<inject key="DeploymentID" enableCopy="false"/>**
    - **Region**: Select **<inject key="location" enableCopy="false" />**
    - **Name**: enter **ai900cognitive-<inject key="DeploymentID" enableCopy="false"/>**
    - **Pricing tier**: Standard S0
    - **By checking this box I acknowledge that I have read and understood all the terms below**: Selected.

1.  Click on **Review and create**.
   
1. After successfully completing the validation process, click on the **Create** button located in the lower left corner of the page.
### Get the Key and Location for your Cognitive Services resource

1. Wait for deployment to complete. Then go to your Cognitive Services resource, and on the **Overview** page, click the link to manage the keys for the service. You will need the endpoint and keys to connect to your Cognitive Services resource from client applications.

1. View the **Keys and Endpoint** page for your resource. You will need the **location/region** and **key** to connect from client applications.

## Task 2: Run Cloud Shell

To test the capabilities of the Speech service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/analyze-receipts/powershell-portal-guide-01.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.  

1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected and click on **Show advanced settings**. Please make sure you have selected your resource group AI-900-Module-04a-<inject key="DeploymentID" enableCopy="false"/> and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** for the  **File share** , then click on **Create Storage**.
    
    ![Screenshot of the cloud shell in the Azure portal.](media/stoarge-up.png)
   
1. Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.

    ![How to find the left hand drop down menu to switch to PowerShell](media/analyze-receipts/powershell-portal-guide-03.png)

1. Wait for PowerShell to start. You should see the following screen in the Azure portal:  

    ![Wait for PowerShell to start.](media/analyze-receipts/powershell-prompt05.png)

## Task 3: Configure and run a client application

Now that you have a custom model, you can run a simple client application that uses the Speech service.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```


2. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below:

    ![The code editor.](media/analyze-receipts/powershell-portal-guide-04.png)

3. In the **Files** pane on the left, expand **ai-900** and select **speaking-clock.ps1**. This file contains some code that uses the Speech service to recognize and synthesize speech:

    ![The editor containing code to use the Speech service](media/recognize-synthesize-speech/speaking-clock-code.png)

4. Don't worry too much about the details of the code, the important thing is that it needs the region/location and either of the keys for your Cognitive Services resource. Copy the values of **KEY 1** and **Location/Region** value from *Keys and Endpoints* page for your resource from the Azure portal and paste them into the code editor.

    ![Find the key and endpoint tab in your Cognitive Services resource's left hand pane.](media/lab4b-1.png)

     > **Tip:**
    > You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.

    >**Note:** Region should be written in small letters, for example : eastus2
    
    > After pasting the key and endpoint values, the first two lines of code should look similar to this:

    
     > $key="1a2b3c4d5e6f7g8h9i0j...."    
     > $region="somelocation"

6. After making the changes to the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor.

    The sample client application will use your Speech service to transcribe spoken input and synthesize an appropriate spoken response. A real application would accept the input from a microphone and send the response to a speaker, but in this simple example, we'll use pre-recorded input in a file and save the response as another file.

   Use the below link to hear the input audio which will be processed by this application:
   
       https://www.microsoft.com/videoplayer/embed/RWMAvi

    >**Note**: Copy the above link to your browser, to listen to the audio file. Do not use the Lab VM browser.

    >**Note**: A real application could accept the input from a microphone and send the response to a speaker, but in this simple example, we'll use pre-recorded input in an audio file.


7. In the PowerShell pane, enter the following command to run the code:

    ```PowerShell
    cd ai-900
    ```
    
    ```PowerShell
    ./speaking-clock.ps1
    ```

8. Use the following link to hear the spoken output generated by the application in your **local machine**:
   
           https://www.microsoft.com/videoplayer/embed/RWMSIU
       

    **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Click Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
    > - Hit the Validate button for the corresponding task.  
    > - If you receive a success message, you can proceed to the next task.If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

## Learn more

This simple app shows only some of the capabilities of the Speech service. To learn more about what you can do with this service, see the [Speech page](https://azure.microsoft.com/services/cognitive-services/speech-services/).

### You have successfully completed this lab.
