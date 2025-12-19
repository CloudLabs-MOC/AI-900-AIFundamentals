# Lab 01: Classify Images with Custom Vision

### Estimated Duration: 60 Minutes

## Overview

The **Custom Vision** service enables you to create computer vision models that are trained on your own images. You can use it to train *image classification* and *object detection* models, which you can then publish and consume from applications.

In this exercise, you will use the Custom Vision service to train an image classification model that can identify three classes of fruit (apple, banana, and orange).

## Objectives

In this lab, you will complete the following tasks:

+ **Task 1:** Open the cloned folder in Visual Studio Code
+ **Task 2:** Create Custom Vision resources
+ **Task 3:** Create a Custom Vision project
+ **Task 4:** Test the model
+ **Task 5:** View the project settings
+ **Task 6:**  Use the *training* API
+ **Task 7:** Publish the image classification model
+ **Task 8:** Use the image classifier from a client application

## Architecture diagram

![](./images/mod9-ex1-dia.png)

## Task 1: Open the cloned folder in Visual Studio Code

In this task, you will learn how to open the cloned folder in **Visual Studio Code**, allowing you to view and edit the project files within the IDE.

1. In the Lab-VM desktop, double-click on the **Visual Studio Code**.

    ![Visual Studio Code Icon](./images/vscode(1).png)

1. Open a file, from the top-left options, click on **Explorer (1)-> Open Folder (2)** and navigate to **C:\AllFiles (3)**, choose **AI-102-AIEngineer-stage (4)** folder and click **Select folder (5)**.

    ![](./images/botframe1(6).png)

    >**Note:** Do you trust the authors of the files in this folder? prompt, select **Yes, I trust the authors**.

3.  Wait while additional files are installed to support the C# code projects in the repo.

## Task 2: Create Custom Vision resources

In this task, you will learn how to create Custom Vision resources in Azure for training and prediction, allowing you to manage access and costs for these workloads separately.

1. In the Azure portal, from the top search bar, search for **Custom vision (1)**, select **Custom vision (2)** from the results.

   ![Upload apple with apple tag](./images/custvision(1).png)

1. From the **AI Foundry | Custom vision** blade, click on **+ Create**.

    ![](./images/custvision(2).png)

1. Create a **Custom Vision** resource with the following settings and then click on **Review + Create**.

    - **Create options**: Both **(1)**
    - **Subscription**: Your Azure subscription **(2)**
    - **Resource group**: **Ai-102-<inject key="DeploymentID" enableCopy="false"/>** **(3)**
    - **Region**: **<inject key="Region" enableCopy="false"/>** **(4)**
    - **Name**: **Vision-<inject key="DeploymentID" enableCopy="false"/>** **(5)**
    - **Training pricing tier**: F0 **(6)**
    - **Prediction pricing tier**: F0 **(7)**

        ![](./images/custvision(3).png)

        > **Note:** If you already have an F0 custom vision service in your subscription, select **S0** for this one.

3. Now navigate to the **Review + create** tab, and then select **Create**.

    ![](./images/custvision(4).png)

4. Wait for the resources to be created, and then view the deployment details and note that two Custom Vision resources are provisioned; one for training, and another for prediction. You can view these by navigating to the resource group where you created them.

    > **Important:** Each resource has its own *endpoint* and *keys*, which are used to manage access from your code. To train an image classification model, your code must use the *training* resource (with its endpoint and key); and to use the trained model to predict image classes, your code must use the *prediction* resource (with its endpoint and key).

## Task 3: Create a Custom Vision project

In this task, you will learn how to create a Custom Vision project using the Custom Vision portal, which will be used to train an image classification model based on your training resource.

To train an image classification model, you need to create a Custom Vision project based on your training resource. To do this, you'll use the Custom Vision portal.

1. In Visual Studio Code, view the training images in the **17-image-classification/training-images**. This folder contains subfolders of apple, banana, and orange images.

    ![](./images/custvision(5).png)

1. In a new browser tab, open the Custom Vision portal at `https://customvision.ai`. 

1. If prompted, click on **Sign in** using the Microsoft account associated with your Azure subscription. 

    ![](./images/custvision(6).png)

1. In the **Terms of Service** dialog, select the **checkbox (1)** to accept the terms, then click **I Agree (2)**.

    ![](./images/custvision(7).png)

4. In the Custom Vision portal, select **NEW PROJECT**.

    ![](./images/custvision(8).png)

1. Create a new project with the following settings, then click on **Create Project**:

    - **Name**: Classify Fruit **(1)**
    - **Description**: Image classification for fruit **(2)**
    - **Resource**: Select **Vision-<inject key="DeploymentID" enableCopy="false"/> (3)**
    - **Project Types**: Classification **(4)**
    - **Classification Types**: Multiclass (Single tag per image) **(5)**
    - **Domains**: Food **(6)**

      ![Upload apple with apple tag](./images/l7e1-6.png)
      
5. In the new project, click **\[+] Add images (1)**, then navigate to the **`C:\AllFiles\AI-102-AIEngineer-stage\17-image-classification\training-images\apple` (2)** folder you viewed earlier. Press **Ctrl+A (3)** to select all images, and click **Open (4)**.

    ![](./images/custvision(9).png)

1. Then upload the image files, specifying the tag **apple (1)**, and click **Upload 15 files (2)**.

   ![](./images/custvision(10).png)

1. Click **Done** in the **Image upload** dialog. 

    ![](./images/custvision(11).png)

6. Repeat the previous step to upload the images in the **banana** folder with the tag *banana*, and the images in the **orange** folder with the tag *orange*.

7. Explore the images you have uploaded in the Custom Vision project - there should be 15 images of each class, like this:

   ![Tagged images of fruit - 15 apples, 15 bananas, and 15 oranges](./images/fruit.jpg)
    
8. In the Custom Vision project, above the images, click **Train (1)** to train a classification model using the tagged images. Select the **Quick Training (2)** option, then click on **Train (3)**, and then wait for the training iteration to complete (this may take a minute or so).

   ![](./images/custvision(12).png)

9. When the model iteration has been trained, review the **Precision**, **Recall**, and **AP** performance metrics - these measure the prediction accuracy of the classification model, and should all be high.

    ![](./images/custvision(13).png)

    >**Note**: The performance metrics are based on a probability threshold of 50% for each prediction (in other words, if the model calculates a 50% or higher probability that an image is of a particular class, then that class is predicted). You can adjust this at the top-left of the page.

## Task 4: Test the model

In this task, you will learn how to test the trained Custom Vision model to evaluate its performance and accuracy in classifying images.

Now that you've trained the model, you can test it.

1. Above the performance metrics, click **Quick Test**.

    ![](./images/custvision(14).png)

2. In the **Image URL** box, type `https://aka.ms/apple-image`**(1)** and click &#10132; **(2)**.

    ![](./images/custvision(15).png)

3. View the predictions returned by your model - the probability score for *apple* should be the highest, like this:

     ![An image with a class prediction of apple](./images/test-apple.jpg)

4. Close the **Quick Test** window.

## Task 5: View the project settings

In this task, you will learn how to view the project settings in Custom Vision.

1. Click the **settings (&#9881;)** icon at the top right of the **Performance** page to view the project settings.

    ![](./images/custvision(16).png)

2. Under **General** (on the left), note the **Project Id** that uniquely identifies this project.

    ![](./images/custvision(17).png)

3. On the right, under **Resources** note that the **key and endpoint** are shown. These are the details for the *training* resource (you can also obtain this information by viewing the resource in the Azure portal).

    ![](./images/custvision(18).png)

## Task 6: Use the *training* API

In this task, you will learn how to use the Custom Vision *training* API to automate the process of uploading images, tagging them, and training models without relying on the portal's user interface.

The Custom Vision portal provides a convenient user interface that you can use to upload and tag images and train models. However, in some scenarios, you may want to automate model training by using the Custom Vision training API.

1. In Visual Studio Code, in the **Explorer** pane, browse to the **17-image_classification (1)** folder and expand the **C-Sharp/Python (2)**. Right-click the **train-classifier (3)** folder and select **Open in Integrated Terminal (4)**. 

    ![](./images/custvision(19).png)

    <details>
    <summary><strong>C#</strong></summary>

    1. Then install the Custom Vision Training package by running the appropriate command for your language preference:

        ```
        dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training --version 2.0.0
        ```

    1. View the contents of the **train-classifier** folder, and note that it contains a file for configuration settings:

        **C#**: appsettings.json

            ![](./images/custvision(20).png)

    1. Open the configuration file and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *training* resource, and the **project ID** for the classification project you created previously. Save your changes by pressing **Ctrl+S**.

            ![](./images/custvision(21).png)

    1. Note that the **train-classifier** folder contains a code file for the client application:

        **C#**: Program.cs

            ![](./images/custvision(22).png)

    1. Open the code file and review the code it contains, noting the following details:
        - Namespaces from the package you installed are imported
        - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionTrainingClient**, which is then used with the project ID to create a **Project** reference to your project.
        - The **Upload_Images** function retrieves the tags that are defined in the Custom Vision project and then uploads image files from correspondingly named folders to the project, assigning the appropriate tag ID.
        - The **Train_Model** function creates a new training iteration for the project and waits for training to complete.

    1. Return the integrated terminal for the **train-classifier** folder, and enter the following command to run the program:

        ```
        dotnet run
        ```

        ![](./images/custvision(23).png)

    </details>

    <details>
    <summary><strong>Python</strong></summary>

    1. Then install the Custom Vision Training package by running the appropriate command for your language preference:

        ```
        pip install azure-cognitiveservices-vision-customvision==3.1.0
        ```

    1. View the contents of the **train-classifier** folder, and note that it contains a file for configuration settings:

        **Python**: .env

        ![](./images/custvision(20).png)

    1. Open the configuration file and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *training* resource, and the **project ID** for the classification project you created previously. Save your changes by pressing **Ctrl+S**.

        ![](./images/custvision(21).png)

    1. Note that the **train-classifier** folder contains a code file for the client application:

        **Python**: test-classification.py

        ![](./images/custvision(22).png)

    1. Open the code file and review the code it contains, noting the following details:
        - Namespaces from the package you installed are imported
        - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionTrainingClient**, which is then used with the project ID to create a **Project** reference to your project.
        - The **Upload_Images** function retrieves the tags that are defined in the Custom Vision project and then uploads image files from correspondingly named folders to the project, assigning the appropriate tag ID.
        - The **Train_Model** function creates a new training iteration for the project and waits for training to complete.

    1. Return the integrated terminal for the **train-classifier** folder, and enter the following command to run the program:

        ```
        python test-classifier.py
        ```

        ![](./images/custvision(23).png)

    </details>

1. Wait for the program to end. Then return to your browser and view the **Training Images** page for your project in the Custom Vision portal (refreshing the browser if necessary).

    ![](./images/custvision(24).png)

7. Verify that some new tagged images have been added to the project. Then view the **Performance** page and verify that a new iteration has been created.

## Task 7: Publish the image classification model

In this task, you will learn how to publish your trained image classification model, making it accessible for use in client applications.

Now you're ready to publish your trained model so that it can be used from a client application.

1. In the Custom Vision portal, on the **Performance** page,  click **&#128504; Publish (1)** to publish the trained model with the following settings:
    - **Model name**: fruit-classifier **(2)**
    - **Prediction resource**: *The **prediction** resource you created previously, which ends with "-Prediction" (<u>not</u> the training resource)*. **(3)**
     - Click **Publish (4)**

    ![](./images/custvision(25).png)

2. At the top left of the **Project Settings** page, click the *Projects Gallery* (&#128065;) icon to return to the Custom Vision portal home page, where your project is now listed.

    ![](./images/custvision(26).png)

3. On the Custom Vision portal home page, at the top right, click the *settings* (&#9881;) icon to view the settings for your Custom Vision service. Then, under **Resources**, find your *prediction* resource, which ends with "-Prediction"  (<u>not</u> the training resource), to determine its **Key (1)** and **Endpoint (2)** values (you can also obtain this information by viewing the resource in the Azure portal).

   ![](./images/custvision(27).png)

## Task 8: Use the image classifier from a client application

In this task, you will learn how to use the published image classification model from a client application, enabling it to analyze and classify images programmatically.

Now that you've published the image classification model, you can use it from a client application. Once again, you can choose to use **C#/Python** as per your prefernce.

1. In Visual Studio Code, in the **17-image-classification** folder, in the subfolder for your preferred language (**C-Sharp/Python**), right- the **test-classifier (1)** folder and **Open in Integrated Terminal (2)**. 

    ![](./images/custvision(28).png)

    <details>
    <summary><strong>C#</strong></summary>
    1. Then enter the following SDK-specific command to install the Custom Vision Prediction package:

    ```
    dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction --version 2.0.0
    ```

    2. Expand the **test-classifier** folder to view the files it contains, which are used to implement a test client application for your image classification model.

    3. Open the configuration file for your client application (**appsettings.json** for C#) and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *prediction* resource, the **project ID** for the classification project, and the name of your published model (which should be *fruit-classifier*). Save your changes.

        ![](./images/custvision(29).png)

    4. Open the code file for your client application (**Program.cs** for C#) and review the code it contains, noting the following details:

        - Namespaces from the package you installed are imported

        - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionPredictionClient**.

        - The prediction client object is used to predict a class for each image in the **test-images** folder, specifying the project ID and model name for each request. Each prediction includes a probability for each possible class, and only predicted tags with a probability greater than 50% are displayed.

    5. Return the integrated terminal for the **test-classifier** folder, and enter the following SDK-specific command to run the program:

        ```
        dotnet run
        ```
    6. View the label (tag) and probability scores for each prediction. You can view the images in the **test-images** folder to verify that the model has classified them correctly.

    ![](./images/custvision(30).png)

    </details>
    
    <details>
    <summary><strong>Python</strong></summary>

    1. Then enter the following SDK-specific command to install the Custom Vision Prediction package:

        ```
        pip install azure-cognitiveservices-vision-customvision==3.1.0
        ```

    2. Expand the **test-classifier** folder to view the files it contains, which are used to implement a test client application for your image classification model.

    3. Open the configuration file for your client application (**.env** for python) and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *prediction* resource, the **project ID** for the classification project, and the name of your published model (which should be *fruit-classifier*). Save your changes.

        ![](./images/custvision(29).png)

    4. Open the code file for your client application (**Program.cs** for C#) and review the code it contains, noting the following details:

        - Namespaces from the package you installed are imported

        - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionPredictionClient**.

        - The prediction client object is used to predict a class for each image in the **test-images** folder, specifying the project ID and model name for each request. Each prediction includes a probability for each possible class, and only predicted tags with a probability greater than 50% are displayed.

    5. Return the integrated terminal for the **test-classifier** folder, and enter the following SDK-specific command to run the program:

        ```
        python test-classifier.py
        ```
    6. View the label (tag) and probability scores for each prediction. You can view the images in the **test-images** folder to verify that the model has classified them correctly.

    ![](./images/custvision(30).png)

    </details>
## Summary
In this lab, you have completed:

+ Opened the cloned folder in Visual Studio Code
+ Created Custom Vision resources
+ Created a Custom Vision project
+ Tested the model
+ Viewed the project settings
+ Used the *training* API
+ Published the image classification model
+ Used the image classifier from a client application
   
### You have successfully completed the lab, click on Next >>.


![](./images/nextpage(3pages).png)
