# Module 01: Explore Cognitive Services

Azure Cognitive Services encapsulate common AI functionality that can be categorized into four main pillars: vision, speech, language, and decision services. In this exercise, you will take a look at one of the decision services to get a general sense of how to provision and use a cognitive services resource in a software application.

The specific cognitive service you will explore in this exercise is **Anomaly Detector**. Anomaly Detector is used to analyze data values over time and to detect any unusual values that might indicate a problem or an issue for further investigation. For example, a sensor in a temperature-controlled storage facility might monitor the temperature every minute and log the measured values. You can use the Anomaly Detector service to analyze the logged temperature values and flag any that fall significantly outside of the normal range of expected temperatures.

To test the capabilities of the Anomaly Detection service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, such as websites or phone apps.

> **Note :**
> The goal of this exercise is to get a general sense of how cognitive services are provisioned and used. Anomaly Detector is used as an example, but you are not expected to gain a comprehensive knowledge of anomaly detection in this exercise!

## Exercise 1: Create an Anomaly Detector resource

### Task 1: Create an Anomaly Detector resource

Let us start by creating an **Anomaly Detector** resource in your Azure subscription:

1. In the Azure Portal click the **&#65291;Create a resource** button.

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img1.png)
   
1. In the Marketplace page search for **Anomaly Detector (1)** and Select **Anomaly Detector (2)**
   
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img2.png)
    
1. On Anomaly Detector Page Click on **Create**. 

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img3.png)
     
1. Create an **Anomaly Detector** resource with the following settings:
     
    - **Subscription**: Use existing subscription **(1)**
    - **Resource group**: Select **AI-900-Module-01-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - **Region**: Select **<inject key="location" enableCopy="false"/> (3)**
    - **Name**: Enter **myanomalydetector-<inject key="DeploymentID" enableCopy="false"/> (4)**
    - **Pricing tier**: Free F0 **(5)**
    - Click **Review + create (6)** 

       ![Picture1](media/ai900mod1img4.png)
   
   >**Note:** Wait for deployment to complete, and then go to the deployed resource.

1. View the **Keys and Endpoint** page for your Anomaly Detector resource. 
   
     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img5.png)
   
      >**Note :** 
      >You will need the endpoint and keys to connect from client applications. 

### Task 2: Run Cloud Shell

To test the capabilities of the Anomaly Detector service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/powershell-portal-guide-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.  

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img6.png)
 
1. If you are prompted to create storage for your Cloud Shell, ensure your subscription is selected and click on **Show advanced settings**.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img7.png)
 
1. Please make sure you have selected your resource group **AI-900-Module-01-<inject key="DeploymentID" enableCopy="false"/> (1)** and enter **blob<inject key="DeploymentID" enableCopy="false"/> (2)** for the **Storage account Name** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/> (3)** for the **File share Name**, then click on **Create Storage (4)**.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img8.png)

1. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img9.png)

1. Wait for PowerShell to start. You should see the following screen in the Azure portal:  

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img10.png)
     
### Task 3: Configure and run a client application

Now that you have a Cloud Shell environment, you can run a simple application that uses the Anomaly Detector service to analyze data.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```
    
    >**Note :** 
    >To Paste the Code right click inside the cloud Shell and then Click on Paste.

1. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

     ```PowerShell
    code .
    ```
    
    Notice how this opens an editor like the one in the image below: 

     ![The code editor.](media/powershell-portal-guide4c.png)

1. In the **Files** pane on the left, expand **ai-900 (1)** and select **detect-anomalies.ps1 (2)**. This file contains some code that uses the Anomaly Detection service, as shown here:

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img11.png)

1. Don't worry too much about the details of the code, the important thing is that it needs the endpoint URL and either of the keys for your Anomaly Detector resource. Copy these from the **Keys and Endpoints** page for your resource (which should still be in the top area of the browser) and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_ENDPOINT** placeholder values, respectively.

    > **Tip :**
    > You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img5.png)
  
    After pasting the key and endpoint values, the first two lines of code should look similar to this:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```
    
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img12.png)

1. Click at the top right of the editor pane **(1)**, use the button to open the menu, and select **Save (2)** to save your changes. Then open the menu again and select **Close Editor (3)**.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](media/ai900mod1img13.png)
      
      >**Note**: If you will be not able to find this option, you can use also Shortcut Keys **ctrl + S** to save and **ctrl + Q** to close the editor.
      
    Remember, anomaly detection is an artificial intelligence technique used to determine whether values in a series are within expected parameters. The sample client application will use your Anomaly Detector service to analyze a file containing a series of date/times and numeric values. The application should return results indicating at each time point, whether the numeric value is within expected parameters.

1. In the PowerShell pane, enter the following commands to run the code:

    ```PowerShell
    cd ai-900
    ```
    ```PowerShell
    .\detect-anomalies.ps1
    ```

1. Review the results, noting that the final column in the results is **True** or **False** to indicate if the value recorded at each date/time is considered an anomaly or not. Consider how we could use this information in a real-life situation. What action could the application trigger if the values were of fridge temperature or blood pressure and anomalies were detected?
     
      ![Picture1](media/Ai900-mod1-img9.png)
      
  **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:

  > - Navigate to the Lab Validation tab, from the upper right corner in the lab guide section.
  > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
  > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
  > - If you need any assistance, please contact us at labs-support@spektrasystems.com.


## Learn more

This simple app shows only some of the capabilities of the Anomaly Detector service. To learn more about what you can do with this service, see the [Anomaly Detector page](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/).
