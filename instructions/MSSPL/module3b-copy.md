# Lab 02: Detect Objects in Images with Custom Vision

### Estimated Duration: 60 Minutes

## Overview

In this exercise, you will use the Custom Vision service to train an *object detection* model that can detect and locate three classes of fruit (apple, banana, and orange) in an image.

## Objectives

In this lab, you will complete the following tasks:

+ **Task 1:** Create a Custom Vision project
+ **Task 2:** Add and tag images
+ **Task 3:** Use the Training API to upload images
+ **Task 4:** Train and test a model
+ **Task 5:** Publish the object detection model
+ **Task 6:** Use the image classifier from a client application

## Architecture diagram

![](./images/mod9-ex2-dia.png)

## Task 1: Create a Custom Vision project

In this task, you will learn how to create a Custom Vision project using the Custom Vision portal to train an object detection model based on your training resource.

To train an object detection model, you need to create a Custom Vision project based on your training resource. To do this, you'll use the Custom Vision portal.

1. In a new browser tab, open the Custom Vision portal at `https://customvision.ai`, and sign in using the Microsoft account associated with your Azure subscription.

2. Create a new project with the following settings:
    - **Name**: Detect Fruit **(1)**
    - **Description**: Object detection for fruit. **(2)**
    - **Resource**: Select **Vision-<inject key="DeploymentID" enableCopy="false"/> (3)**
    - **Project Types**: Object Detection **(4)**
    - **Domains**: General **(5)**
    - Click on **Create Project (6)**

      ![Upload apple with apple tag](./images/l7e1-1.png)

3. Wait for the project to be created and opened in the browser.

## Task 2: Add and tag images

In this task, you will learn how to add and tag images for training an object detection model by uploading images and defining bounding boxes for each object instance.

To train an object detection model, you need to upload images that contain the classes you want the model to identify, and tag them to indicate bounding boxes for each object instance.

1. Go to Visual Studio Code, view the training images in the **18-object-detection/training-images** folder where you cloned the repository. This folder contains images of fruit.

    ![](./images/objdet(1).png)

2. Download and extract the training images from https://aka.ms/fruit-objects. The extracted folder contains a collection of images of fruit.

    ![](./images/objdet(2).png)

1. In the window, click **Extract (1)**, then select **Extract all (2)**. In the **Extract Compressed (Zipped) Folders** window, confirm the location is **`C:\Users\azureuser\Downloads\object\_training` (3)**, and click **Extract (4)**.
    
    ![](./images/objdet(3).png)

3. In the Custom Vision portal, open your object detection project and click **+ Add images (1)**. In the Open window, navigate to **`C:\Users\azureuser\Downloads\object\_training` (2)**, press **Ctrl+A (3)** to select all images, then click **Open (4)** to upload them.

    ![](./images/objdet(4).png)

1. In the **Image upload** window, click **Upload 33 files**.

    ![](./images/objdet(5).png)

4. After the images have been uploaded, select the first one to open it.

5. Hold the mouse over any object in the image until an automatically detected region is displayed, like the image below. Then select the object, and if necessary, resize the region to surround it.

     ![The default region for an object](./images/object-region.jpg)

   Alternatively, you can simply drag around the object to create a region.

6. When the region surrounds the object, add a new tag with the appropriate object type (*apple*, *banana*, or *orange*) as shown here:

     ![A tagged object in an image](./images/object-tag.jpg)

7. Select and tag each other object in the image, resizing the regions and adding new tags as required.

     ![Two tagged objects in an image](./images/object-tags.jpg)

8. Use the **>** link on the right to go to the next image, and tag its objects. Then just keep working through the entire image collection, tagging each apple, banana, and orange.

9. When you have finished tagging the last image, close the **Image Detail** editor and on the **Training Images** page, under **Tags**, select **Tagged** to see all of your tagged images:

     ![](./images/objdet(6).png)

## Task 3: Use the Training API to upload images

In this task, you will learn how to use the Custom Vision *Training API* to upload images with tags and object region data, allowing you to automate the image tagging process and integrate it into your workflow.

You can use the graphical tool in the Custom Vision portal to tag your images, but many AI development teams use other tools that generate files containing information about tags and object regions in images. In scenarios like this, you can use the Custom Vision training API to upload tagged images to the project.

>**Note:** In this exercise, you can choose to use the API from either the **C#** SDK. In the steps below, perform the actions appropriate for the language.

1. Click the **Settings** (&#9881;) icon at the top right of the **Training Images** page in the Custom Vision portal to view the project settings.

    ![](./images/objdet(7).png)

2. Under **General** (on the left), note the **Project Id** that uniquely identifies this project.

    ![](./images/objdet(8).png)

3. On the right, under **Resources**, note that the details for the *training* resource, including its **key and endpoint**, are shown (you can also obtain this information by viewing the resource in the Azure portal).

    ![](./images/objdet(9).png)

4. In Visual Studio Code, under the **18-object-detection (1)** folder, expand the **C-Sharp (2)** folder. Right-click the **train-detector (3)** folder and select **Open an Integrated Terminal (4)**.

    ![](./images/objdet(10).png)

5.  Then install the Custom Vision Training package by running the appropriate command for your language preference:

    **C#**

    ```
    dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training --version 2.0.0
    ```

6. View the contents of the **train-detector** folder, and note that it contains a file for configuration settings:

    - **C#**: appsettings.json

1. Open the configuration file and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *training* resource, and the **project ID** for the object detection project you created previously. Save your changes.

    ![](./images/objdet(11).png)

7. In the **train-detector** folder, open **tagged-images.json** and examine the JSON it contains. The JSON defines a list of images, each containing one or more tagged regions. Each tagged region includes a tag name, and the top and left coordinates and width and height dimensions of the bounding box containing the tagged object.

    > **Note**: The coordinates and dimensions in this file indicate relative points on the image. For example, a *height* value of 0.7 indicates a box that is 70% of the height of the image. Some tagging tools generate other formats of file in which the coordinate and dimension values represent pixels, inches, or other units of measurement.

8. Note that the **train-detector** folder contains a subfolder in which the image files referenced in the JSON file are stored.

    ![](./images/objdet(12).png)

9. Note that the **train-detector** folder contains a code file for the client application:

    - **C#**: Program.cs

        ![](./images/objdet(13).png)

1. Open the code file and review the code it contains, noting the following details:
    - Namespaces from the package you installed are imported
    - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionTrainingClient**, which is then used with the project ID to create a **Project** reference to your project.
    - The **Upload_Images** function extracts the tagged region information from the JSON file and uses it to create a batch of images with regions, which it then uploads to the project.

10. Return the integrated terminal for the **train-detector** folder, and enter the following command to run the program:
    
    **C#**

    ```
    dotnet run
    ```
    
11. Wait for the program to end. Then return to your browser and view the **Training Images** page for your project in the Custom Vision portal (refreshing the browser if necessary).

12. Verify that some new tagged images have been added to the project.

    ![](./images/objdet(14).png)

## Task 4: Train and test a model

In this task, you will learn how to train and test an object detection model in Custom Vision using the tagged images you uploaded, enabling you to evaluate its performance and accuracy.

Now that you've tagged the images in your project, you're ready to train a model.

1. In the Custom Vision project, click **Train (1)** to train an object detection model using the tagged images. Select the **Quick Training (2)** option and then click on **Train (3)**.

    ![](./images/objdet(15).png)

2. Wait for training to complete (it might take ten minutes or so), and then review the *Precision*, *Recall*, and *mAP* performance metrics - these measure the prediction accuracy of the classification model, and should all be high.

    ![](./images/objdet(16).png)

3. At the top right of the page, click **Quick Test**.
    
    ![](./images/objdet(18).png)

4. Then in the **Image URL** box, enter `https://aka.ms/apple-orange` and view the prediction that is generated. Then close the **Quick Test** window.

   ![](./images/objdet(17).png)

## Task 5: Publish the object detection model

In this task, you will learn how to publish your trained object detection model, making it accessible for integration into client applications for real-time object detection.

Now you're ready to publish your trained model so that it can be used from a client application.

1. In the Custom Vision portal, on the **Performance** page,  click **&#128504; Publish (1)** to publish the trained model with the following settings:

    - **Model name**: fruit-detector **(3)**
    - **Prediction Resource**: *The **prediction** resource you created previously (<u>not</u> the training resource)*. **(3)**
    - Click **Publish (4)**

       ![](./images/objdet(19).png)

2. At the top left of the **Project Settings** page, click the *Projects Gallery* (&#128065;) icon to return to the Custom Vision portal home page, where your project is now listed.

    ![](./images/objdet(20).png)

3. On the Custom Vision portal home page, at the top right, click the *settings* (&#9881;) icon to view the settings for your Custom Vision service. Then, under **Resources**, find your *prediction* (evident by the **-prediction** suffix) resource (<u>not</u> the training resource) to determine its **Key (1)** and **Endpoint (2)** values (you can also obtain this information by viewing the resource in the Azure portal).

    ![](./images/objdet(21).png)

## Task 6: Use the image classifier from a client application

In this task, you will learn how to use the published object detection model from a client application, enabling it to detect objects in images programmatically using **C#**.

Now that you've published the image classification model, you can use it from a client application. Once again, you can choose to use **C#**.

1. In Visual Studio Code, browse to the **18-object-detection (1)** folder and in the folder, select **C-Sharp (2)**. Right-click the **test-detector (3)** folder and select **Open in Integrated Terminal (4)**.

    ![](./images/objdet(22).png)

1. Then enter the following SDK-specific command to install the Custom Vision Prediction package:

    **C#**

    ```
    dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction --version 2.0.0
    ```

3. Open the configuration file for your client application (**appsettings.json** for C#) and update the configuration values it contains to reflect the **endpoint** and **key** for your Custom Vision *prediction* (evident by the **-prediction** suffix) resource, the **project ID** for the object detection project, and the name of your published model (which should be *fruit-detector*). Save your changes.

    ![](./images/objdet(23).png)

4. Open the code file for your client application (*Program.cs* for C#) and review the code it contains, noting the following details:
    - Namespaces from the package you installed are imported
    - The **Main** function retrieves the configuration settings and uses the key and endpoint to create an authenticated **CustomVisionPredictionClient**.
    - The prediction client object is used to get object detection predictions for the **produce.jpg** image, specifying the project ID and model name in the request. The predicted tagged regions are then drawn on the image, and the result is saved as **output.jpg**.

5. Return to the integrated terminal for the **test-detector** folder, and enter the following command to run the program:

    **C#**

    ```
    dotnet run
    ```

    ![](./images/objdet(24).png)

6. After the program has completed, view the resulting **output.jpg** file to see the detected objects in the image.

   ![](./images/objdet(25).png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
>
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.


## Summary
In this lab, you have completed:

+ Created a Custom Vision project
+ Added and tagged images
+ Used the Training API to upload images
+ Trained and tested a model
+ Published the object detection model
+ Used the image classifier from a client application
   
### You have successfully completed the Hands-on lab!