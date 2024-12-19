# Lab 03: Analyze Images with Azure AI Face API

## Estimated Duration: 60 Minutes

## Overview

Computer vision solutions often require an artificial intelligence (AI) solution to detect human faces. For example, suppose the retail company Northwind Traders wants to locate where customers are standing in a store to assist them best. One way to accomplish this is to determine if there are any faces in the images and, if so, to identify the bounding box coordinates around the faces.

To test the capabilities of the Face service, we will use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, like websites or phone apps.

## Lab Objectives

You will be able to complete the following tasks:

  - **Task 1:** Create a Face API Resource
  - **Task 2:** Configure and Run a Client Application

## Task 1: Create a Face API Resource

You can use the Face service by creating a **Face** resource. (Face API is no longer available in Azure AI services.)

If you haven't already done so, create a **Face API** resource in your Azure subscription.

1. Click the **&#65291;Create a resource** button, search for *Face*, and create a **Face** resource with the following settings:
    - **Subscription**: *Use existing Azure subscription*.
    - **Resource group**: ai-service-<inject key="DeploymentID" enableCopy="false"/>
    - **Region**:  **<inject key="Region" enableCopy="false"/>**
    - **Name**: Enter **aiface-<inject key="DeploymentID" enableCopy="false"/>**
    - **Pricing tier**: **Standard S0**

1.  Click on **Review and create**.
   
1. After successfully completing the validation process, click on the **Create** button in the page's lower left corner.
   
1. Wait for deployment to complete (it can take a few minutes), and then click on the **Go to resource** button. This will take you to your Face API.

1. Select **Keys and Endpoint** under **Resource Management** for your Face resource. Click on **Show keys**. You will need the endpoint and keys when connecting from client applications.

      >**Note:** 
      > Copy and save the **KEY 1** and **Endpoint** values to NotePad for future reference to connect from client applications. 

## Task 2: Configure and Run a Client Application

To test the capabilities of the Face service, we will use a simple command-line application that runs in the Cloud Shell on Azure. 

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal. 

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/create-face-solutions/ai900_03c-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.      

1. Ensure the shell type indicated on the top left of the Cloud Shell pane is *Switch to Bash*. If it is *Switch to PowerShell*, select it to Switch into PowerShell.

    ![How to find the left hand drop down menu to switch to PowerShell](../media/analyze-images-computer-vision-service/azure-ai-search-lab1-3.png) 

1. Wait for PowerShell to start. You should see the following screen in the Azure portal:  

    ![How to find the left hand drop down menu to switch to PowerShell](../media/analyze-images-computer-vision-service/azure-ai-search-lab1-3.png)

1. Now that you have a custom model, you can run a simple client application that uses the Face service.`

1. The files are downloaded to a folder named **ai-search** in the previous lab. Now, we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

     ```PowerShell
    code .
    ```

    Notice how this opens up an editor like the one in the image below: 

    ![The code editor.](../media/create-face-solutions/ai900_03c-5.png) 

1. In the **Files** pane on the left, expand **ai-search** and select **find-faces.ps1**. This file contains some code that uses the Face service to detect and analyze faces in an image, as shown here:

    ![The editor containing code to detect faces in an image](../media/create-face-solutions/ai900_03c-6.png)

1. Don't worry too much about the details of the code. The important thing is that it needs the endpoint URL and either of the keys for your Face resource. Copy these from the **Keys and Endpoints** page for your resource (Task 1, Step 5) and paste them into the code editor, replacing the **YOUR_KEY** with *KEY 1* and **YOUR_ENDPOINT** with *Endpoint* placeholder values, respectively.

    > **Tip**: You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint**, and **Editor** panes.

    After pasting the key and endpoint values, the first two lines of code should look similar to this:

    
    > $key="1a2b3c4d5e6f7g8h9i0j...."    
    > $endpoint="https..."
    

1. After changing the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor.

    The sample client application will use your Face service to analyze the following image, taken by a camera in the Northwind Traders store:

    ![An image of a parent using a cellphone camera to take a picture of a child in in a store](../media/create-face-solutions/ai900_03c-7.jpg)

1. In the PowerShell pane, enter the following commands to run the code:

    ```PowerShell
    cd ai-search
    ```

     ```PowerShell
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. Review the returned information, which includes the location of the face in the image. The location of a face is indicated by the top-left coordinates and the width and height of a *bounding box*, as shown here:
    
    ![An image of a person with their face outlined](../media/create-face-solutions/ai900_03c-8.jpg)
    ![An image of a person with their face outlined](../media/resultai-9003c.png)
    >**Note:**
    >Face service capabilities that return personally identifiable features are restricted. See https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/ for details.

1. Now let's try another image:

    ![An image of person with a shopping basket](../media/create-face-solutions/ai900_03c-9.jpg)

    To analyze the second image, enter the following command:

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. Review the results of the face analysis for the second image.

1. Let's try one more:

    ![An image of person with a shopping cart](../media/create-face-solutions/ai900_03c-10.jpg)

    To analyze the third image, enter the following command:

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. Review the results of the face analysis for the third image.

## Summary

In this lab, you have covered the following:
  
  - Created a Face API resource
  - Configured and run a client application

## Learn more

This simple app shows only some of the capabilities of the Face service. To learn more about what you can do with this service, see the [Face API page](https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/overview-identity).

### You have successfully completed the lab. Click on next from the bottom right corner.
