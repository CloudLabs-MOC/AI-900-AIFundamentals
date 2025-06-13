
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

1. Close the code editor, if you have it already open.

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

1. If the code editor is not opened, please re enter the below commnad **(1)** then you will be to see the code editor **(2)**. 

    ```PowerShell
    code .
    ```

     ![](../media/analyze-images-computer-vision-service/gt10.png)     

1. In the **Files** pane on the left, expand **ai-search (1)** and select **translator.ps1 (2)**. This file contains some code that uses the Translator service:

    ![The editor containing code to use the Translator service](../media/gt55.png)

1. Don't worry too much about the details of the code, the important thing is that it needs the region/location and either of the keys for your Azure AI Services resource. 

    - Navigate to **aiservice-<inject key="DeploymentID" enableCopy="false"/> (1)** AI Service resource. Go to **Keys and Endpoints (2)**, Copy the values of **KEY 1 (3)** and **Location/Region (4)** value from **Keys and Endpoints** page. Paste them into the code editor.

      ![Find the key and endpoint tab in your Azure AI Services resource's left hand pane.](../media/gt56.png)

    > **Note:** The Translator service does not require the use of the Azure AI Service endpoint, so there is no need to modify the Translator service endpoint. Instead, a dedicated global endpoint is available specifically for the Translator service. 

1. Replace **YOUR_KEY** with **KEY1** value and **YOUR_LOCATION** with **Location/Region** value, respectively.

    ![The editor containing code to use the Translator service](../media/gt57.png)

1. Hit **Ctrl+S** to save the changes that are done. Press **Ctrl+Q** to close the code editor window.

1. Make sure your are in **ai-search** folder if not run the below command to move into the folder.

    ```PowerShell
    cd ai-search
    ```
    
1. In the Cloud Shell pane, enter the following command to run the code:

    ```PowerShell
    ./translator.ps1
    ```

1. Review the output. Did you see the translation from the text in English to French, Italian, and Chinese?  Did you see the English audio "hello" translated into text in French?

    ![The editor containing code to use the Translator service](../media/translator-ps1.png)

### Summary

In this lab you have covered the following:
  - Configured and run a client application

### Learn more

This simple app shows only some of the capabilities of the Translator service. To learn more about what you can do with this service, see the [Translator page](https://learn.microsoft.com/en-us/azure/ai-services/translator/).

### You have successfully completed the lab.