
# Lab 04: Translate Text and Audio with Azure AI Translator

### Estimated Duration: 60 Minutes

### Overview

One of the driving forces that has enabled human civilization to develop is the ability to communicate with one another. Communication is key in most human endeavors.

Artificial Intelligence (AI) can simplify communication by translating text or speech between languages, removing barriers to communication across countries and cultures.

To test the Translator service's capabilities, we will use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, like websites or phone apps.

### Lab Objectives

You will be able to complete the following tasks:

  - **Task 1:** Run Cloud Shell
  - **Task 2:** Configure and Run a Client Application

### Task 1: Run Cloud Shell

To test the translation service's capabilities, we will use a simple command-line application that runs in the Cloud Shell on Azure. 

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/analyze-images-computer-vision-service/powershell-portal-guide-1(1).png)

1. When you open the Cloud Shell for the first time, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.

### Task 2: Configure and Run a Client Application

Now that you have a custom model, you can run a simple client application that uses the translation service.

1. The required files are downloaded to a folder named **ai-search** in the previous lab. Now, we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell: 

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below: 

    ![The code editor.](../media/powershell-portal-guide-4.png)

1. In the **Files** pane on the left, expand **ai-search** and select **translator.ps1**. This file contains some code that uses the translator service:

    ![The editor containing code to use the Translator service](../media/translate-code-4b.png)

1. Don't worry too much about the details of the code. The important thing is that it needs the region/location and either of the keys for your Azure AI Services resource. Copy the values of **KEY 1 (2)** and **Location/Region (3)** from the **Keys and Endpoint (1)** page for your resource from the Azure portal and paste them into the code editor.

    ![Find the key and endpoint tab in your Azure AI Services resource's left hand pane.](../media/lab4b-1.png)

    > **Note:** The Translator service does not require the use of the Azure AI services endpoint, so there is no need to modify the Translator service endpoint. Instead, a dedicated global endpoint is available specifically for the Translator service. 

1. At the top right of the editor pane, click the **...** button to open the menu. Select **Save** to save your changes. Then open the menu again and choose **Close Editor**.

    The sample client application will use the Translator service to do several tasks:
    - Translate text from English into French, Italian, and Chinese.
    - Translate audio from English into French text.

    Use the below link to hear the input audio, which will be processed by this application and translated:
   
       https://www.microsoft.com/videoplayer/embed/RWORN0

    >**Note**: Copy the above link to your browser and listen to the audio file. Do not use the Lab VM browser.

    >**Note**: A real application could accept the input from a microphone and send the response to a speaker, but in this simple example, we will use pre-recorded input in an audio file.
    
1. In the Cloud Shell pane, enter the following command to run the code:

    ```PowerShell
    cd ai-search
    ./translator.ps1
    ```

1. Review the output. Did you see the translation from the text in English to French, Italian, and Chinese?  Did you see the English audio "hello" translated into French text?

### Summary

In this lab, you have covered the following:
  - Run Cloud Shell.
  - Configured and run a client application.

### Learn more

This simple app shows only some of the Translator service's capabilities. To learn more about what you can do with this service, see the [Translator page](https://learn.microsoft.com/en-us/azure/ai-services/translator/).

### You have successfully completed the lab.
