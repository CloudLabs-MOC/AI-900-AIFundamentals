
# Lab 04: Translate text and audio with Azure AI Translator

## Estimated Duration: 60 minutes

## Overview

One of the driving forces that has enabled human civilization to develop is the ability to communicate with one another. In most human endeavors, communication is key.

Artificial Intelligence (AI) can help simplify communication by translating text or speech between languages, helping to remove barriers to communication across countries and cultures.

To test the capabilities of the Translator service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, such as websites or phone apps.

## Lab Objectives

You will be able to complete the following tasks:

  - Task 1: Configure and run a client application

## Task 1: Configure and run a client application

To test the capabilities of the Translation service, we'll use a simple command-line application that runs in the Cloud Shell on Azure. 

1. Switch back to the browser tab containing the Azure portal, where the **Cloud shell** (**[>_]**) is already opened.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/analyze-images-computer-vision-service/powershell-portal-guide-1(1).png)

1. If you see Cloud Shell timed out window, select **Reconnect** otherwise proceed with the next Task.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab4-2.png)

    Now that you have a custom model, you can run a simple client application that uses the Translation service.

1. The required files are downloaded to a folder named **ai-search** in the previous lab. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell: 

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below: 

    ![The code editor.](../media/powershell-portal-guide-4.png)

1. In the **Files** pane on the left, expand **ai-search** and select **translator.ps1**. This file contains some code that uses the Translator service:

    ![The editor containing code to use the Translator service](../media/translate-code-4b.png)

1. Don't worry too much about the details of the code, the important thing is that it needs the region/location and either of the keys for your Azure AI Services resource. Copy the values of **KEY 1** and **Location/Region** value from *Keys and Endpoints* page for your **aiservice-<inject key="DeploymentID" enableCopy="false"/>*** resource from the Azure portal and paste them into the code editor.

    ![Find the key and endpoint tab in your Azure AI Services resource's left hand pane.](../media/azure-ai-search-lab4-3.png)

    > **Note:** The Translator service does not require the use of the Azure AI Service endpoint, so there is no need to modify the Translator service endpoint. Instead, a dedicated global endpoint is available specifically for the Translator service. 

1. Hit **Ctrl+S** to save the changes that are done. Press **Ctrl+Q** to close the code editor window.

    The sample client application will use the Translator service to do several tasks:
    - Translate text from English into French, Italian, and Chinese.
    - Translate audio from English into text in French

    Use the below link to hear the input audio which will be processed by this application and translates:
   
       https://www.microsoft.com/videoplayer/embed/RWORN0

    >**Note**: Copy the above link to your browser, and listen to the audio file. Do not use the **Lab VM browser**.

    >**Note**: A real application could accept the input from a microphone and send the response to a speaker, but in this simple example, we'll use pre-recorded input in an audio file.
    
1. In the Cloud Shell pane, enter the following command to run the code:

    ```PowerShell
    ./translator.ps1
    ```

    >**Note**: Make sure your are in **ai-search** folder if not run **cd ai-search** command to move into the folder.

1. Review the output. Did you see the translation from the text in English to French, Italian, and Chinese?  Did you see the English audio "hello" translated into text in French?

### Summary

In this lab you have covered the following:
  - Configured and run a client application

### Learn more

This simple app shows only some of the capabilities of the Translator service. To learn more about what you can do with this service, see the [Translator page](https://learn.microsoft.com/en-us/azure/ai-services/translator/).

### You have successfully completed the lab.